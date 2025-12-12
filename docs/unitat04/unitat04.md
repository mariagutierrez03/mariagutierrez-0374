---
layout: default
title: "Unitat 4. RSAT, VPN i RDP"
---

---

## Part 1 - Configuració de la base i Active Directory

1. Comencem per la instal·lació inicial del sistema operatiu Windows Server. Preparem la màquina virtual, assegurant-nos que els paràmetres de regió, teclat i idioma són els correctes, i confirmem que el sistema està preparada per rebre els rols d'infraestructura. Afegim un nou disc de 20 GB, desactivem el firewall i canviem el nom de l'ordinador a MARIA. 
![foto](fotos/winVPN1.png)
![foto](fotos/winVPN2.png)
![foto](fotos/winVPN3.png)


3. Seguidament, realitzem la configuració de xarxa. Assignem al primer adaptador una adreça IP estàtica al servidor (per exemple, 192.168.2.101), definint la màscara de subxarxa i la porta d'enllaç, per tal d'establir una base sòlida per al Controlador de Domini. Al segon adaptador li assignem l'adreça ip 10.10.1.1, aquesta simula la xarxa externa.
![foto](fotos/winVPN4.png)
![foto](fotos/winVPN5.png)
![foto](fotos/winVPN6.png)


5. A continuació, instal·lem el rol de Serveis de Domini d'Active Directory (AD DS) a través de l'Administrador del Servidor. Seleccionem amb cura els components necessaris, preparant la màquina per a la promoció a Controlador de Domini.
![foto](fotos/winVPN7.png)
![foto](fotos/winVPN8.png)
![foto](fotos/winVPN9.png)
![foto](fotos/winVPN10.png)
![foto](fotos/winVPN11.png)
![foto](fotos/winVPN12.png)
![foto](fotos/winVPN13.png)
![foto](fotos/winVPN14.png)

7. Un cop instal·lats els rols, promovem el servidor a Controlador de Domini. Triem l'opció de Crear un nou bosc i introduïm el nom del nostre domini (per exemple, MARIA.local), establert la nova autoritat d'autenticació a la xarxa.
![foto](fotos/winVPN15.png)

8. Configurem les opcions de Controladora de Domini (DC) i els Nivells Funcionals del bosc i del domini. Introduïm la contrasenya del mode de restauració de serveis de directori (DSRM) i revisem les rutes dels fitxers de la base de dades d'Active Directory.
![foto](fotos/winVPN16.png)

9. Revisem el nom del NetBIOS i les opcions addicionals que ens ofereix l'assistent. Comprovem que la instal·lació està a punt per començar i, un cop es confirma, iniciem el procés de promoció, que culminarà amb el reinici del servidor.
![foto](fotos/winVPN17.png)
![foto](fotos/winVPN18.png)
![foto](fotos/winVPN19.png)
![foto](fotos/winVPN20.png)
![foto](fotos/winVPN21.png)
![foto](fotos/winVPN22.png)
![foto](fotos/winVPN23.png)

11. Després del reinici, verifiquem que el domini s'ha creat correctament i que podem accedir als contenidors d'usuaris i equips per començar l'estructuració.
![foto](fotos/winVPN24.png)

12. Creem l'estructura d'Unitats Organitzatives (OUs) necessària per a la gestió jeràrquica del nostre servidor (per exemple, OU de "ASIX").
![foto](fotos/winVPN25.png)

---

## Part 2 - Gestió d'usuaris

10. Afegim els Usuaris de domini (per exemple, user1, user2) a OU "ASIX". Assignem-los la contrasenya inicial i configurem les opcions de canvi de contrasenya.
![foto](fotos/winVPN26.png)
![foto](fotos/winVPN27.png)
![foto](fotos/winVPN28.png)
![foto](fotos/winVPN29.png)

---

## Part 3 - Configuració del servidor de fitxers (Compartició)

17. Accedim al disc de 20GB addicional (afegit prèviament), el formatem al "Administrador de discos" en el servidor.
![foto](fotos/winVPN42.png)
![foto](fotos/winVPN43.png)
![foto](fotos/winVPN44.png)
![foto](fotos/winVPN45.png)
![foto](fotos/winVPN46.png)

18. Seguidament creem dues carpetes una per a user1 i l'altra per a user2. Aquestes contenen fitxers dins, per fer proves d'accés posteriorment. 
![foto](fotos/winVPN49.png)
![foto](fotos/winVPN48.png)
![foto](fotos/winVPN47.png)

20. Apliquem els permisos NTFS detallats. Afegim els usuaris corresponents segons la carpeta (per exemple, "user1" a la capeta user1) amb permisos de "Modificar" i "Lectura i Execució" segons correspongui, garantint el control de l'accés.
![foto](fotos/winVPN50.png)
![foto](fotos/winVPN51.png)
![foto](fotos/winVPN52.png)

22. Configurem la Compartició Avançada de la carpeta. Li assignem un nom de compartició clar (per exemple, user1) i limitem el nombre d'usuaris concurrents si és necessari.
![foto](fotos/winVPN56.png)
![foto](fotos/winVPN57.png)

24. Seguidament, realitzarem el mateix procés en l'altra carpeta amb l'usuari user2. 
![foto](fotos/winVPN54.png)
![foto](fotos/winVPN58.png)

25. Verifiquem la ruta UNC de la carpeta compartida. Dins dels usuaris del Active Directory en Propietats, dins de l'apartat Perfil, clicarem en connectar, escollirem una lletra i utilitzant la ruta (\NomServidor\NomCompartició), els usuaris podran tindre acces al rescurs compartit.
![foto](fotos/winVPN.png)

NO ACABAT !!! ------------------------------> a partir d'aqui

26. Fem una prova de creació i eliminació de fitxers al servidor amb un usuari de domini per assegurar-nos que els permisos NTFS i de Compartició permeten o deneguen l'accés correctament.
![foto](fotos/winVPN.png)

27. Documentem la vista de l'Administrador del Servidor que mostra el Servei de Fitxers i Emmagatzematge i les comparticions actives, confirmant que el recurs està disponible per a la xarxa.
![foto](fotos/winVPN.png)

---

## Part 4 - Instal·lació i Configuració del Servidor VPN (RRAS)

25. Instal·lem el rol de Serveis d'Accés i Política de Xarxes (NPS) al servidor VPN. Aquest rol ens permetrà configurar l'Encaminament i Accés Remot (RRAS) i, opcionalment, gestionar l'autenticació amb RADIUS.
![foto](fotos/winVPN.png)

26. Seleccionem l'opció Encaminament i Accés Remot dins dels Serveis de Rol. Això instal·larà el component clau que permet establir connexions de xarxa privades virtuals (VPN) i rutejar el trànsit.
![foto](fotos/winVPN.png)

27. Un cop instal·lat el rol, obrim la consola Encaminament i Accés Remot. Iniciem l'assistent de configuració i seleccionem l'opció de Configurar i Habilitar Encaminament i Accés Remot.
![foto](fotos/winVPN.png)

28. Triem la configuració per a Accés Remot (Connexió de Marcatge o VPN) a l'assistent. Això permet que els usuaris externs es puguin connectar al servidor per accedir a la xarxa interna.
![foto](fotos/winVPN.png)

29. Seleccionem la Interfície de Xarxa Externa que utilitzarem per escoltar les peticions VPN entrants. És crucial que identifiquem correctament la interfície connectada a la xarxa pública.
![foto](fotos/winVPN.png)

30. Establirem el mètode d'assignació d'adreces IP per als clients VPN. Triem l'opció de "Un rang d'adreces especificat" i definirem un conjunt d'IPs (per exemple, de 192.168.1.200 a 192.168.1.250).
![foto](fotos/winVPN.png)

31. Definim el rang d'adreces IP que seran assignades als clients que es connectin per VPN. Introduïm la IP inicial i final del rang, assegurant-nos que estan dins de la subxarxa interna i no entren en conflicte amb DHCP.
![foto](fotos/winVPN.png)

32. Finalitzem l'assistent de configuració. Comprovem que el servei RRAS està iniciat i que la icona del servidor mostra un estel verd, indicant que està operatiu i esperant connexions.
![foto](fotos/winVPN.png)

---

## Part 5 - Seguretat i Protocols VPN

33. Accedim a les Propietats del servidor RRAS i anem a la pestanya de Seguretat. Verifiquem els mètodes d'autenticació que estem utilitzant, com el MS-CHAP v2, i ens assegurem que està activat el proveïdor d'autenticació d'Active Directory.
![foto](fotos/winVPN.png)

34. Configurem els Protocols VPN a la pestanya Ports. Ens assegurem que el protocol L2TP o IKEv2 està habilitat i desactivem protocols més antics i insegurs com el PPTP, millorant la seguretat de la connexió.
![foto](fotos/winVPN.png)

35. Obrim la consola Usuaris i Equips d'Active Directory (ADUC) i seleccionem un usuari de prova. Accedim a les seves Propietats i a la pestanya "Dial-in" (Accés telefònic).
![foto](fotos/winVPN.png)

36. Establirem el permís de connexió VPN per a l'usuari, seleccionant l'opció "Allow access" (Permetre accés). Això és necessari perquè RRAS permeti la connexió, independentment de la política de xarxa (NPS).
![foto](fotos/winVPN.png)

37. Configurem el tallafocs del servidor VPN. Afegim regles entrants per permetre el trànsit dels protocols VPN escollits, com ara el UDP 500 i UDP 4500 per a L2TP/IPSec.
![foto](fotos/winVPN.png)

38. Revisem el registre d'esdeveniments de RRAS per a les connexions fallides o reeixides. Això ens permet monitoritzar l'activitat del servei i detectar possibles problemes d'autenticació o configuració.
![foto](fotos/winVPN.png)

39. Configurem un Servei de Política de Xarxa (NPS) si volem un control més estricte. Definim les condicions i restriccions de les sol·licituds de connexió, afegint una capa addicional de seguretat abans de l'accés.
![foto](fotos/winVPN.png)

40. Documentem la vista del Visor d'Esdeveniments mostrant els registres d'inici del servei RRAS, verificant que tots els components han començat sense errors.
![foto](fotos/winVPN.png)

---

## Part 6 - Configuració i Prova del Client

41. Accedim a la màquina client (Windows 10 o similar) i configurem les propietats de xarxa. Ens assegurem que la seva configuració DNS apunten al Controlador de Domini per permetre la resolució de noms del domini i que el firewall està desactivat.
![foto](fotos/winVPN30.png)
![foto](fotos/winVPN31.png)
![foto](fotos/winVPN32.png)
![foto](fotos/winVPN33.png)

43. Unim la màquina client al domini d'Active Directory. Introduïm el nom del domini i les credencials d'un usuari amb privilegis per unir equips, iniciant el procés d'incorporació a la xarxa corporativa.
![foto](fotos/winVPN34.png)
![foto](fotos/winVPN35.png)
![foto](fotos/winVPN36.png)
![foto](fotos/winVPN38.png)
![foto](fotos/winVPN39.png)

45. Un cop reincorporada al domini, iniciem la sessió a la màquina client amb un usuari de domini estàndard (per exemple, user1). Això verifica que l'autenticació d'Active Directory funciona correctament. A més també comprovem que al servidor apareix l'ordinador client connectat. 
![foto](fotos/winVPN40.png)
![foto](fotos/winVPN41.png)

ME HE QDAT AQUI -----------------------------------------------------

47. Creem la nova connexió VPN a la màquina client. Introduïm l'adreça IP o el nom de domini públic del servidor VPN i seleccionem el tipus de protocol que hem configurat (per exemple, L2TP/IPSec).
![foto](fotos/winVPN.png)

48. Establirem la connexió VPN introduint les credencials de l'usuari de domini. Observem el procés d'establiment del túnel, esperant el missatge de "Connectat".
![foto](fotos/winVPN.png)

49. Un cop connectats per VPN, executem ipconfig a la màquina client. Verifiquem que la màquina ha rebut una adreça IP dins del rang que hem definit al servidor RRAS (per exemple, 192.168.1.2xx).
![foto](fotos/winVPN.png)

50. Comprovem la resolució de noms i la connectivitat interna. Realitzem un ping al nom del servidor (per exemple, ping DC-SERVER) i a la seva IP interna, confirmant que el túnel VPN funciona i permet l'accés a la xarxa corporativa.
![foto](fotos/winVPN.png)

---

## Part 7 - Proves i Verificacions Finals

49. Accedim a la carpeta compartida utilitzant la ruta UNC (\NomServidor\DADES_EMPRESA$) mentre estem connectats per VPN. Això demostra que podem accedir als recursos de fitxers de l'empresa de manera segura des de fora.
![foto](fotos/winVPN.png)

50. Comprovem els permisos a la carpeta compartida. Intentem crear, modificar i eliminar un fitxer amb l'usuari de prova, verificant que els permisos NTFS i de Compartició es respecten a través del túnel VPN.
![foto](fotos/winVPN.png)

51. Documentem una captura de pantalla de l'Administrador de Servidor mostrant el Visor de Sessions actives de RRAS. Veiem la connexió de l'usuari remot, confirmant que el servei està monitoritzant l'accés.
![foto](fotos/winVPN.png)

52. Documentem una captura de pantalla del Monitor de Recursos del servidor, observant l'ús de la CPU, la memòria i la xarxa després de la connexió VPN. Això ens dóna una idea del rendiment del servidor.
![foto](fotos/winVPN.png)

53. Fem una prova de connexió VPN fallida amb un usuari que no té permisos de Dial-in. Documentem el missatge de rebuig per assegurar-nos que la seguretat d'Active Directory està protegint l'accés remot.
![foto](fotos/winVPN.png)

54. Executem la comanda net view \NomServidor des del client connectat per VPN. Veiem les carpetes compartides disponibles, confirmant que la navegació de la xarxa és funcional dins del túnel.
![foto](fotos/winVPN.png)

55. Finalitzem la sessió de l'usuari de domini i tanquem la connexió VPN. Documentem la captura de pantalla on es mostra que la connexió VPN ha estat desconnectada correctament.
![foto](fotos/winVPN.png)

56. Documentem una vista final del servidor amb el Tauler d'Administració del Servidor (Dashboard) que mostra tots els rols (AD DS, DNS, RRAS, Servidor de Fitxers) instal·lats i indicant un estat de salut correcte. Això conclou la documentació del projecte.
![foto](fotos/winVPN.png)

---
