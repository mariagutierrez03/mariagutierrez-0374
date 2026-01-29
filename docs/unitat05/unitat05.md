---
layout: default
title: "Unitat 5. Automatització funcional adaptada al context d'ús Windows"
---

---

## Part 1 - Afegir el client Ubuntu amb script AD

1. El primer pas és obrir un editor de text amb permisos d’administrador i crear un fitxer que contindrà el script. Aquest fitxer serà el que automatitzarà tot el procés per unir Ubuntu al domini MARIA.local.        
![foto](fotos/winVPN1.png)
![foto](fotos/winVPN2.png)
![foto](fotos/winVPN3.png)

3. Un cop creat el fitxer, dins d’aquest es defineixen les variables d’entorn, com el nom del domini, la IP i el nom complet del servidor de domini, i l’usuari administrador. Aquestes variables permeten que totes les accions del script utilitzin la mateixa informació sense necessitat de canvis manuals.      

4. El següent pas comprova que l’usuari que executa el script té permisos d’administrador. Si no és així, el script s’atura i mostra un missatge d’error, ja que moltes de les accions necessiten privilegis elevats.      

5. Tot seguit, el script configura el DNS i el domini al sistema Ubuntu. Afegeix la IP del servidor de domini com a servidor DNS i defineix el domini principal, i després reinicia el servei de resolució de noms perquè aquests canvis tinguin efecte immediat. Això permet que Ubuntu pugui resoldre correctament els noms dels equips i serveis del domini.      

6. La següent secció actualitza la configuració del sistema per reconèixer millor el servidor de domini. Modifica els fitxers necessaris perquè Ubuntu consulti el DNS quan resol noms d’equips i comprova que existeixi una entrada amb la IP i el nom complet del servidor de domini, afegint-la si cal. Això assegura que qualsevol servei o comanda pugui localitzar correctament el controlador del domini.      

7. Després, el script comprova que hi hagi connectivitat amb el servidor de domini. Fa proves per verificar que l’Ubuntu pot comunicar-se amb el servidor i que el nom del domini es resol correctament. Si alguna comprovació falla, el script s’atura per evitar errors posteriors en la unió al domini.      

8. La següent secció descarrega i instal·la PBIS Open, el programari que permet que Ubuntu es pugui unir a un domini Windows. Aquesta part prepara el sistema i facilita la unió automàtica sense haver de fer configuracions manuals.      

9. Tot seguit, el script uneix l’ordinador al domini amb l’usuari administrador. Durant aquest pas, el sistema demana la contrasenya de l’usuari, i si tot és correcte, mostra un missatge indicant que l’equip s’ha unit amb èxit al domini.      

10. A continuació, el script configura PAM per permetre que els usuaris del domini puguin iniciar sessió a Ubuntu amb interfície gràfica. Aquesta configuració crea automàticament el directori personal dels usuaris quan inicien sessió per primera vegada, evitant errors i assegurant que cada usuari tingui el seu entorn complet.      

11. Després d’executar el script, el sistema mostra els usuaris del domini detectats, cosa que permet comprovar que Ubuntu ha reconegut correctament els comptes abans de provar-los a la interfície gràfica.

# Configuració de Servidor Web IIS amb Certificat SSL (SAN)

Aquesta guia detalla el procés pas a pas per configurar un servidor web a Windows Server 2022, la resolució de noms mitjançant DNS i la implementació de seguretat HTTPS amb plantilles personalitzades.

## 1. Instal·lació i preparació del servei IIS

El primer pas és instal·lar el rol de servidor **IIS (Internet Information Services)**. Durant l'assistent d'instal·lació, mantindrem totes les opcions predeterminades. Un cop instal·lat, el fitxer HTML per defecte es troba a la ruta oficial de Windows Server: `C:\inetpub\wwwroot`.

![foto](fotos/conf01.png)

### Neteja i personalització del lloc
Un cop som dins de la ruta esmentada, eliminarem els fitxers que el sistema crea per defecte per deixar directori net.

![foto](fotos/conf02.png)

A continuació, crearem un nou fitxer anomenat `index.html` i l'editarem amb el codi HTML necessari per personalitzar la nostra pàgina web.

![foto](fotos/conf03.png)
![foto](fotos/conf04.png)

### Verificació inicial
Un cop configurat el fitxer, el visualitzarem fent doble clic per assegurar-nos que el disseny és correcte.

![foto](fotos/conf05.png)

També comprovarem que el servei web respon correctament introduint l'adreça IP del propi servidor al navegador.

![foto](fotos/conf06.png)

## 2. Configuració del Servidor DNS

Si intentem accedir al lloc web mitjançant el nom de domini configurat a l'IIS (en aquest cas, `mariagutierrez`), veurem que no es reconeix. Això passa perquè cal configurar el servei **DNS** per resoldre el nom i associar-lo a la IP `192.168.2.101`.

![foto](fotos/conf07.png)

Dins de la consola de configuració del DNS, afegirem un nou **Host (A)** a la zona existent. Aquest registre apuntarà el nom `mariagutierrez` directament a la IP del servidor.

![foto](fotos/conf08.png)

### Proves de connectivitat DNS
Comprobarem que el domini respon correctament fent un `ping`. Així mateix, utilitzarem l'eina `nslookup` per confirmar que el servidor DNS retorna la IP correcta.

![foto](fotos/conf09.png)

## 3. Enllaços de lloc a l'IIS

Dins de la configuració de l'IIS, verificarem que existeix un enllaç (*binding*) actiu per al protocol **HTTP** a través del port **80** per a qualsevol adreça IP.

![foto](fotos/conf10.png)

Si tornem al navegador i cerquem `http://mariagutierrez`, ara podrem observar que el domini funciona correctament i visualitzem el nostre fitxer HTML.

![foto](fotos/conf11.png)

## 4. Instal·lació de l'Entitat de Certificació (CA)

Per implementar seguretat al lloc, instal·larem el servei de certificats al servidor.

![foto](fotos/conf12.png)

És fonamental marcar totes les opcions de configuració, sent l'**Entitat de certificació** la més important per a aquest procés.

![foto](fotos/conf13.png)

## 5. Creació de la Plantilla de Certificat SAN

Els navegadors actuals requereixen l'ús del paràmetre **SAN (Subject Alternative Name)**. Com que la plantilla bàsica del sistema no el contempla per defecte, haurem de crear-ne una de personalitzada duplicant la plantilla de "Servidor web".

![foto](fotos/conf14.png)
![foto](fotos/conf15.png)

Assignarem un nom identificatiu a la nova plantilla, com per exemple `WebServer-SAN`.

![foto](fotos/conf16.png)

Ajustarem la compatibilitat de l'entitat de certificació i del destinatari seguint els estàndards de **Windows Server 2016**.

![foto](fotos/conf17.png)

### Permisos de la plantilla
És imprescindible configurar correctament els permisos de seguretat; el grup **Equips del domini** ha de tenir permisos d'accés i lectura.

![foto](fotos/conf18.png)
![foto](fotos/conf19.png)
![foto](fotos/conf20.png)
![foto](fotos/conf21.png)
![foto](fotos/conf22.png)
![foto](fotos/conf23.png)

## 6. Generació del fitxer de sol·licitud (.inf)

Crearem un fitxer de configuració anomenat `web.inf` a l'arrel de la unitat `C:`.

![foto](fotos/conf24.png)

Aquest fitxer contindrà les comandes i paràmetres específics necessaris per generar el certificat amb el format correcte.

![foto](fotos/conf25.png)

## 7. Configuració final i emissió del certificat

Per completar la instal·lació, anirem a l'avís de configuració post-instal·lació (triangle groc) i iniciarem l'assistent.

![foto](fotos/conf26.png)
![foto](fotos/conf27.png)

Seleccionarem l'Entitat de Certificació i finalitzarem l'assistent prement "Següent" en totes les pantalles.

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
Dins de la consola `certsrv.msc`, sota el node `MARIA-MARIA-CA`, farem clic dret a "Plantillas de certificado" > "Nuevo" > "Plantilla de certificado que se va a emitir". Triarem la nostra plantilla `WebServer-SAN`.

![foto](fotos/conf38.png)
![foto](fotos/conf39.png)

## 8. Sol·licitud i instal·lació mitjançant CMD

Executarem el Símbol del Sistema (**CMD**) com a administrador per realitzar la sol·licitud formal utilitzant la plantilla creada.

![foto](fotos/conf40.png)

Enviarem la sol·licitud a l'entitat certificadora `MARIA-MARIA-CA`.

![foto](fotos/conf41.png)

Un cop rebuda la resposta, procedirem amb la instal·lació del certificat al magatzem local del servidor.

![foto](fotos/conf42.png)

## 9. Implementació d'HTTPS al lloc web

Finalment, tornarem a la consola d'administració de l'IIS. En l'apartat "Agregar enlace de sitio", seleccionarem el tipus **https**, port **443**, i assignarem el certificat SSL que acabem de generar per a `mariagutierrez`.

![foto](fotos/conf43.png)

### Comprovació final
Ara podem confirmar que el lloc web és totalment funcional sota el protocol segur **HTTPS**, garantint una connexió xifrada.

![foto](fotos/conf44.png)

13. Un cop completat tot, es poden fer proves iniciant sessió amb un usuari del domini. Si tot funciona bé, Ubuntu crearà automàticament el directori personal de l’usuari i permetrà l’accés.    

14. A més, cal comprovar al servidor Windows que l’equip Ubuntu apareix dins de la llista d’ordinadors a Active Directory. Aquesta comprovació final confirma que la unió al domini s’ha realitzat correctament i que l’Ubuntu forma part de la xarxa del domini.      

---
