---
layout: default
title: "Unitat 5. Automatització funcional adaptada al context d'ús Windows"
---

---

## Part 1 - Afegir el client Ubuntu amb script AD

1. El primer pas és obrir un editor de text amb permisos d’administrador i crear un fitxer que contindrà el script. Aquest fitxer serà el que automatitzarà tot el procés per unir Ubuntu al domini MARIA.local.        
![foto](fotos/linad9.png)
![foto](fotos/linad10.png)
![foto](fotos/linad8.png)
![foto](fotos/linad7.png)
![foto](fotos/linad1.png)

3. Un cop creat el fitxer, dins d’aquest es defineixen les variables d’entorn, com el nom del domini, la IP del controlador de domini i l’usuari administrador. També es detecta automàticament el nom de l'equip i s'identifiquen les targetes de xarxa interna i externa. Aquestes variables permeten que totes les accions del script utilitzin la mateixa informació sense necessitat de canvis manuals.      
```
#!/bin/bash

#############################################
# CONFIGURACIÓ DEL ENTORN
#############################################
DOMINI="MARIA.local"
DC_IP="192.168.2.101" 
AD_ADMIN="Administrador" 
# COGEMOS EL HOSTNAME ACTUAL DEL SISTEMA
ACTUAL_HOSTNAME=$(hostname)
INT_IF="enp0s3"  # INTERNA (DC)
EXT_IF="enp0s8"  # EXTERNA (Internet)
```
4. El següent pas comprova que l’usuari que executa el script té permisos d’administrador (root). Si no és així, el script s’atura i mostra un missatge d’error, ja que la instal·lació de paquets i la modificació de fitxers de sistema necessiten privilegis elevats.
```
if [[ $EUID -ne 0 ]]; then
   echo "❌ Executa aquest script com a root"
   exit 1
fi
```
5. Tot seguit, el script realitza una neteja de possibles unions prèvies i prepara la xarxa per a la instal·lació. Atura la targeta interna per evitar conflictes de rutes i configura un DNS extern (8.8.8.8) per garantir la sortida a Internet. Després, descarrega i instal·la el conjunt d'eines format per Realmd, SSSD i Adcli, que és el programari modern que permet que Ubuntu es pugui unir a un domini Windows i gestionar l'autenticació de manera segura.      
```
echo "🚀 Iniciant unió corregida a $DOMINI (Equip: $ACTUAL_HOSTNAME)"

# --- NETEJA PREVIA (Per evitar l'error de "Ja es va unir") ---
realm leave $DOMINI 2>/dev/null
rm -f /etc/krb5.keytab

#############################################
# 1. PREPARAR RED PARA INSTALACIÓN
#############################################
echo "📌 Prioritzant internet per $EXT_IF..."
ip link set $INT_IF down
echo "nameserver 8.8.8.8" > /etc/resolv.conf

apt-get update -o Acquire::ForceIPv4=true
apt-get install -y realmd sssd sssd-tools adcli samba-common-bin packagekit ntpdate
```
6. La següent secció restaura la connectivitat amb la xarxa interna per poder localitzar el controlador de domini. El script actualitza el fitxer de resolució de noms perquè apunti a la IP del servidor de domini i utilitza la comanda ntpdate per sincronitzar l'hora de l'Ubuntu amb la del servidor Windows. Aquesta sincronització és imprescindible perquè el protocol d'autenticació Kerberos funcioni correctament.       
```
#############################################
# 2. PREPARAR RED PARA UNIÓN
#############################################
echo "📌 Connectant a la xarxa interna i sincronitzant hora..."
ip link set $INT_IF up
sleep 3 # Donem una mica més de temps perquè la IP interna s'assigni

# Configurem DNS per veure el DC
echo -e "nameserver $DC_IP\nsearch $DOMINI" > /etc/resolv.conf

# Sincronització horària (fonamental per Kerberos)
ntpdate -u $DC_IP
```    
9. Tot seguit, el script executa la unió oficial de l’ordinador al domini utilitzant l'eina realm. Durant aquest pas, el sistema demanarà la contrasenya de l’administrador del domini Windows. S'inclou un control d'errors que atura el procés si la unió no s'ha pogut completar, assegurant que no es continuï amb una configuració defectuosa.
```
#############################################
# 3. UNIÓ AL DOMINI
#############################################
echo "📌 Intentant unir al domini com a $ACTUAL_HOSTNAME... INTRODUEIX CONTRASENYA:"
realm join --user=$AD_ADMIN --verbose $DOMINI

if [[ $? -ne 0 ]]; then
   echo "❌ ERROR: No s'ha pogut unir al domini."
   exit 1
fi
```
11. A continuació, el script configura el servei SSSD i el sistema PAM per gestionar l'inici de sessió. Es crea un fitxer de configuració que permet als usuaris entrar amb el seu nom d'usuari de Windows (sense haver d'escriure el domini complet) i s'activa la creació automàtica del directori personal (/home) per a cada usuari del domini quan accedeixi per primera vegada, ja sigui per consola o per interfície gràfica.      
```
#############################################
# 4. CONFIGURACIÓ FINAL
#############################################
echo "📌 Configurant SSSD i PAM..."
pam-auth-update --enable mkhomedir

cat <<EOF > /etc/sssd/sssd.conf
[sssd]
domains = $DOMINI
config_file_version = 2
services = nss, pam

[domain/$DOMINI]
ad_domain = $DOMINI
krb5_realm = ${DOMINI^^}
realmd_tags = manages-system joined-with-adcli 
cache_credentials = True
id_provider = ad
krb5_store_password_if_offline = True
default_shell = /bin/bash
ldap_id_mapping = True
use_fully_qualified_names = False
fallback_homedir = /home/%u
access_provider = ad
EOF

chmod 600 /etc/sssd/sssd.conf
systemctl restart sssd

echo "-------------------------------------------------------"
echo "✅ EQUIP $ACTUAL_HOSTNAME UNIT CORRECTAMENT A $DOMINI"
echo "🎉 Prova ara: id $AD_ADMIN"
echo "-------------------------------------------------------"
```
12. Després d’executar el script, el sistema mostra els usuaris del domini detectats, cosa que permet comprovar que Ubuntu ha reconegut correctament els comptes abans de provar-los a la interfície gràfica.         
![foto](fotos/linad3.png)
![foto](fotos/linad2.png)
![foto](fotos/linad4.png)
![foto](fotos/linad5.png)

14. Un cop completat tot, es poden fer proves iniciant sessió amb un usuari del domini. Si tot funciona bé, Ubuntu crearà automàticament el directori personal de l’usuari i permetrà l’accés.    
![foto](fotos/linad13.png)
![foto](fotos/linad12.png)
![foto](fotos/linad11.png)
![foto](fotos/linad14.png)

16. A més, cal comprovar al servidor Windows que l’equip Ubuntu apareix dins de la llista d’ordinadors a Active Directory. Aquesta comprovació final confirma que la unió al domini s’ha realitzat correctament i que l’Ubuntu forma part de la xarxa del domini.    
![foto](fotos/linad6.png)

---

# Part 2 - Configuració de Servidor Web IIS amb Certificat SSL (SAN)

Aquesta guia detalla el procés pas a pas per configurar un servidor web a Windows Server 2022, la resolució de noms mitjançant DNS i la implementació de seguretat HTTPS amb plantilles personalitzades.

## 1. Instal·lació i preparació del servei IIS

1. El primer pas és instal·lar el rol de servidor **IIS (Internet Information Services)**. Durant l'assistent d'instal·lació, mantindrem totes les opcions predeterminades. Un cop instal·lat, el fitxer HTML per defecte es troba a la ruta oficial de Windows Server: `C:\inetpub\wwwroot`.      
![foto](fotos/conf01.png)

### Neteja i personalització del lloc
2. Un cop som dins de la ruta esmentada, eliminarem els fitxers que el sistema crea per defecte per deixar directori net.      
![foto](fotos/conf02.png)

3. A continuació, crearem un nou fitxer anomenat `index.html` i l'editarem amb el codi HTML necessari per personalitzar la nostra pàgina web.      
![foto](fotos/conf03.png)
![foto](fotos/conf04.png)

### Verificació inicial
4. Un cop configurat el fitxer, el visualitzarem fent doble clic per assegurar-nos que el disseny és correcte.        
![foto](fotos/conf05.png)

5. També comprovarem que el servei web respon correctament introduint l'adreça IP del propi servidor al navegador.      
![foto](fotos/conf06.png)

## 2. Configuració del Servidor DNS

1. Si intentem accedir al lloc web mitjançant el nom de domini configurat a l'IIS (en aquest cas, `mariagutierrez`), veurem que no es reconeix. Això passa perquè cal configurar el servei **DNS** per resoldre el nom i associar-lo a la IP `192.168.2.101`.      
![foto](fotos/conf07.png)

2. Dins de la consola de configuració del DNS, afegirem un nou **Host (A)** a la zona existent. Aquest registre apuntarà el nom `mariagutierrez` directament a la IP del servidor.      
![foto](fotos/conf08.png)
![foto](fotos/conf09.png)

### Proves de connectivitat DNS
3. Comprobarem que el domini respon correctament fent un `ping`. Així mateix, utilitzarem l'eina `nslookup` per confirmar que el servidor DNS retorna la IP correcta.      
![foto](fotos/conf10.png)
![foto](fotos/conf11.png)
![foto](fotos/conf12.png)

## 3. Enllaços de lloc a l'IIS

1. Dins de la configuració de l'IIS, verificarem que existeix un enllaç (*binding*) actiu per al protocol **HTTP** a través del port **80** per a qualsevol adreça IP.    
![foto](fotos/conf13.png)

2. Si tornem al navegador i cerquem `http://mariagutierrez`, ara podrem observar que el domini funciona correctament i visualitzem el nostre fitxer HTML.      
![foto](fotos/conf14.png)

## 4. Instal·lació de l'Entitat de Certificació (CA)

1. Per implementar seguretat al lloc, instal·larem el servei de certificats al servidor.      
![foto](fotos/conf15.png)

2. És fonamental marcar totes les opcions de configuració, sent l'**Entitat de certificació** la més important per a aquest procés.      
![foto](fotos/conf16.png)

## 5. Creació de la Plantilla de Certificat SAN

1. Els navegadors actuals requereixen l'ús del paràmetre **SAN (Subject Alternative Name)**. Com que la plantilla bàsica del sistema no el contempla per defecte, haurem de crear-ne una de personalitzada duplicant la plantilla de "Servidor web".      
![foto](fotos/conf17.png)
![foto](fotos/conf18.png)

2. Assignarem un nom identificatiu a la nova plantilla, com per exemple `WebServer-SAN`.      
![foto](fotos/conf19.png)

3. Ajustarem la compatibilitat de l'entitat de certificació i del destinatari seguint els estàndards de **Windows Server 2016**.      
![foto](fotos/conf20.png)

### Permisos de la plantilla
4. És imprescindible configurar correctament els permisos de seguretat; el grup **Equips del domini** ha de tenir permisos d'accés i lectura.      
![foto](fotos/conf21.png)
![foto](fotos/conf22.png)
![foto](fotos/conf23.png)

## 6. Generació del fitxer de sol·licitud (.inf)

1. Crearem un fitxer de configuració anomenat `web.inf` a l'arrel de la unitat `C:`.      
![foto](fotos/conf24.png)

2. Aquest fitxer contindrà les comandes i paràmetres específics necessaris per generar el certificat amb el format correcte.      
![foto](fotos/conf25.png)

## 7. Configuració final i emissió del certificat
1. Per completar la instal·lació, anirem a l'avís de configuració post-instal·lació (triangle groc) i iniciarem l'assistent.      
![foto](fotos/conf26.png)
![foto](fotos/conf27.png)

2. Seleccionarem l'Entitat de Certificació i finalitzarem l'assistent prement "Següent" en totes les pantalles.      
![foto](fotos/conf28.png)
![foto](fotos/conf29.png)
![foto](fotos/conf30.png)
![foto](fotos/conf31.png)
![foto](fotos/conf32.png)
![foto](fotos/conf33.png)
![foto](fotos/conf34.png)
![foto](fotos/conf35.png)
![foto](fotos/conf36.png)
![foto](fotos/conf37.png)

### Publicació de la plantilla
3. Dins de la consola `certsrv.msc`, sota el node `MARIA-MARIA-CA`, farem clic dret a "Plantillas de certificado" > "Nuevo" > "Plantilla de certificado que se va a emitir". Triarem la nostra plantilla `WebServer-SAN`.      
![foto](fotos/conf39.png)
![foto](fotos/conf40.png)

## 8. Sol·licitud i instal·lació mitjançant CMD

1. Executarem el Símbol del Sistema (**CMD**) com a administrador per realitzar la sol·licitud formal utilitzant la plantilla creada.      
![foto](fotos/conf41.png)

2. Enviarem la sol·licitud a l'entitat certificadora `MARIA-MARIA-CA`.      
![foto](fotos/conf42.png)
![foto](fotos/conf38.png)

4. Un cop rebuda la resposta, procedirem amb la instal·lació del certificat al magatzem local del servidor.  
![foto](fotos/conf43.png)

## 9. Implementació d'HTTPS al lloc web

1. Finalment, tornarem a la consola d'administració de l'IIS. En l'apartat "Agregar enlace de sitio", seleccionarem el tipus **https**, port **443**, i assignarem el certificat SSL que acabem de generar per a `mariagutierrez`.      
![foto](fotos/conf44.png)

### Comprovació final
2. Ara podem confirmar que el lloc web és totalment funcional sota el protocol segur **HTTPS**, garantint una connexió xifrada.      
![foto](fotos/conf45.png)
![foto](fotos/conf46.png)
![foto](fotos/conf47.png)

  

---
