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

12. Un cop completat tot, es poden fer proves iniciant sessió amb un usuari del domini. Si tot funciona bé, Ubuntu crearà automàticament el directori personal de l’usuari i permetrà l’accés.    

13. A més, cal comprovar al servidor Windows que l’equip Ubuntu apareix dins de la llista d’ordinadors a Active Directory. Aquesta comprovació final confirma que la unió al domini s’ha realitzat correctament i que l’Ubuntu forma part de la xarxa del domini.      

---
