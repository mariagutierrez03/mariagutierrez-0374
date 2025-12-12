
---
layout: default
title: "Unitat 3. Gestió de kernels i mòduls del sistema Linux"
---

## Fase 1: Configuració de la Base i Active Directory

1. Comencem per la instal·lació inicial del sistema operatiu Windows Server. Preparem la màquina virtual o física, assegurant-nos que els paràmetres de regió, teclat i idioma són els correctes, i confirmem que el sistema està preparada per rebre els rols d'infraestructura.
2. Seguidament, realitzem la configuració de xarxa. Assignem una adreça IP estàtica al servidor (per exemple, 192.168.1.10), definint la màscara de subxarxa i la porta d'enllaç, per tal d'establir una base sòlida per al Controlador de Domini.
3.  A continuació, instal·lem el rol de Servidor DNS i el rol de Serveis de Domini d'Active Directory (AD DS) a través de l'Administrador del Servidor. Seleccionem amb cura els components necessaris, preparant la màquina per a la promoció a Controlador de Domini.
4.  Un cop instal·lats els rols, promovem el servidor a Controlador de Domini. Triem l'opció de Crear un nou bosc i introduïm el nom del nostre domini (per exemple, nostredomini.local), establert la nova autoritat d'autenticació a la xarxa.
5.  Configurem les opcions de Controladora de Domini (DC) i els Nivells Funcionals del bosc i del domini. Introduïm la contrasenya del mode de restauració de serveis de directori (DSRM) i revisem les rutes dels fitxers de la base de dades d'Active Directory.
6.  Revisem el nom del NetBIOS i les opcions addicionals que ens ofereix l'assistent. Comprovem que la instal·lació està a punt per començar i, un cop es confirma, iniciem el procés de promoció, que culminarà amb el reinici del servidor.
7.  Després del reinici, obrim la consola Usuaris i Equips d'Active Directory (ADUC). Verifiquem que el domini s'ha creat correctament i que podem accedir als contenidors d'usuaris i equips per començar l'estructuració.
8.  Creem l'estructura d'Unitats Organitzatives (OUs) necessària per a la gestió jeràrquica de la nostra empresa (per exemple, OUs per "Usuaris", "Equips", "Administració", "Comercial"). Aquesta estructura ens facilitarà l'aplicació futura de Polítiques de Grup.

## Fase 2: Gestió d'Usuaris, Grups i GPOs

9.  Procedim a la creació dels Grups de Seguretat pertinents. Creem grups globals i locals de domini que utilitzarem per organitzar els usuaris amb permisos similars per a l'accés a recursos compartits.
10. Afegim els Usuaris de domini (per exemple, usuari.prova, tecnic.xarxa) a les seves OUs corresponents. Assignem-los la contrasenya inicial i configurem les opcions de canvi de contrasenya.
11. Un cop creats els usuaris, els afegim als Grups de Seguretat que hem definit al pas 9. Verifiquem les propietats de cada usuari per assegurar-nos que pertanyen als grups adequats per als seus rols.
12. Obrim la consola de Gestió de Polítiques de Grup (GPMC). Creem una nova GPO (per exemple, "Política de Seguretat") per aplicar configuracions de seguretat centralitzades a tots els usuaris o equips del domini.
13. Configurem els paràmetres de seguretat més importants dins de la nova GPO. Establirem els requisits de complexitat de contrasenya (llargada mínima, caducitat, històric) i les polítiques de bloqueig de comptes.
14. Enllacem la GPO de seguretat a les OUs que contenen els usuaris pertinents. Això assegura que, després d'una actualització de política, les noves regles de contrasenya s'aplicaran als usuaris de l'organització.
15. Creem una altra GPO (per exemple, "Mapeig d'Unitats") per automatitzar la connexió a les carpetes compartides de la xarxa. Definim les accions de "Actualitzar" o "Crear" la unitat de xarxa amb una lletra concreta.
16. Configurem l'element de preferència de la GPO de mapeig de unitats, indicant la ruta UNC a la carpeta compartida i aplicant una Item-Level Targeting per tal que el mapeig s'apliqui només a un grup de seguretat concret.

## Fase 3: Configuració del Servidor de Fitxers (Compartició)

17. Accedim al disc de dades (per exemple, D:\) del servidor de fitxers (que pot ser el DC o un servidor membre) i creem la carpeta que volem compartir (per exemple, D:\Dades_Empresa).
18. Configurem els Permisos NTFS d'aquesta carpeta. És important que desactivem l'herència de permisos i eliminem els grups que no han de tenir accés, assegurant la màxima restricció per defecte.
19. Apliquem els permisos NTFS detallats. Afegim els Grups de Seguretat de l'AD (per exemple, "Grup_Finances") amb permisos de "Modificar" i "Lectura i Execució" segons correspongui, garantint el control granular de l'accés.
20. Configurem la Compartició Avançada de la carpeta. Li assignem un nom de compartició clar (per exemple, DADES_EMPRESA$) i limitem el nombre d'usuaris concurrents si és necessari.
21. Establirem els Permisos de Compartició (Share Permissions). Per defecte, assignem el permís de "Control total" al grup "Usuaris Autenticats" o "Tothom", confiant en la superioritat restrictiva dels permisos NTFS per la seguretat final.
22. Verifiquem la ruta UNC de la carpeta compartida. Comprovem que podem accedir a la compartició des de l'Administrador del Servidor utilitzant la ruta (\NomServidor\NomCompartició) i revisem els permisos efectius.
23. Fem una prova de creació i eliminació de fitxers al servidor amb un usuari de domini per assegurar-nos que els permisos NTFS i de Compartició permeten o deneguen l'accés correctament.
24. Documentem la vista de l'Administrador del Servidor que mostra el Servei de Fitxers i Emmagatzematge i les comparticions actives, confirmant que el recurs està disponible per a la xarxa.

## Fase 4: Instal·lació i Configuració del Servidor VPN (RRAS)

25. Instal·lem el rol de Serveis d'Accés i Política de Xarxes (NPS) al servidor VPN. Aquest rol ens permetrà configurar l'Encaminament i Accés Remot (RRAS) i, opcionalment, gestionar l'autenticació amb RADIUS.
26. Seleccionem l'opció Encaminament i Accés Remot dins dels Serveis de Rol. Això instal·larà el component clau que permet establir connexions de xarxa privades virtuals (VPN) i rutejar el trànsit.
27. Un cop instal·lat el rol, obrim la consola Encaminament i Accés Remot. Iniciem l'assistent de configuració i seleccionem l'opció de Configurar i Habilitar Encaminament i Accés Remot.
28. Triem la configuració per a Accés Remot (Connexió de Marcatge o VPN) a l'assistent. Això permet que els usuaris externs es puguin connectar al servidor per accedir a la xarxa interna.
29. Seleccionem la Interfície de Xarxa Externa que utilitzarem per escoltar les peticions VPN entrants. És crucial que identifiquem correctament la interfície connectada a la xarxa pública.
30. Establirem el mètode d'assignació d'adreces IP per als clients VPN. Triem l'opció de "Un rang d'adreces especificat" i definirem un conjunt d'IPs (per exemple, de 192.168.1.200 a 192.168.1.250).
31. Definim el rang d'adreces IP que seran assignades als clients que es connectin per VPN. Introduïm la IP inicial i final del rang, assegurant-nos que estan dins de la subxarxa interna i no entren en conflicte amb DHCP.
32. Finalitzem l'assistent de configuració. Comprovem que el servei RRAS està iniciat i que la icona del servidor mostra un estel verd, indicant que està operatiu i esperant connexions.

## Fase 5: Seguretat i Protocols VPN

33. Accedim a les Propietats del servidor RRAS i anem a la pestanya de Seguretat. Verifiquem els mètodes d'autenticació que estem utilitzant, com el MS-CHAP v2, i ens assegurem que està activat el proveïdor d'autenticació d'Active Directory.
34. Configurem els Protocols VPN a la pestanya Ports. Ens assegurem que el protocol L2TP o IKEv2 està habilitat i desactivem protocols més antics i insegurs com el PPTP, millorant la seguretat de la connexió.
35. Obrim la consola Usuaris i Equips d'Active Directory (ADUC) i seleccionem un usuari de prova. Accedim a les seves Propietats i a la pestanya "Dial-in" (Accés telefònic).
36. Establirem el permís de connexió VPN per a l'usuari, seleccionant l'opció "Allow access" (Permetre accés). Això és necessari perquè RRAS permeti la connexió, independentment de la política de xarxa (NPS).
37. Configurem el tallafocs del servidor VPN. Afegim regles entrants per permetre el trànsit dels protocols VPN escollits, com ara el UDP 500 i UDP 4500 per a L2TP/IPSec.
38. Revisem el registre d'esdeveniments de RRAS per a les connexions fallides o reeixides. Això ens permet monitoritzar l'activitat del servei i detectar possibles problemes d'autenticació o configuració.
39. Configurem un Servei de Política de Xarxa (NPS) si volem un control més estricte. Definim les condicions i restriccions de les sol·licituds de connexió, afegint una capa addicional de seguretat abans de l'accés.
40. Documentem la vista del Visor d'Esdeveniments mostrant els registres d'inici del servei RRAS, verificant que tots els components han començat sense errors.

## Fase 6: Configuració i Prova del Client

41. Accedim a la màquina client (Windows 10 o similar) i configurem les propietats de xarxa. Ens assegurem que la seva configuració DNS apunten al Controlador de Domini per permetre la resolució de noms del domini.
42. Unim la màquina client al domini d'Active Directory. Introduïm el nom del domini i les credencials d'un usuari amb privilegis per unir equips, iniciant el procés d'incorporació a la xarxa corporativa.
43. Un cop reincorporada al domini, iniciem la sessió a la màquina client amb un usuari de domini estàndard (per exemple, usuari.prova). Això verifica que l'autenticació d'Active Directory funciona correctament.
44. Verifiquem que les GPOs s'han aplicat a la màquina client. Executem gpupdate /force i revisem algun element de política (per exemple, el fons d'escriptori o les restriccions del Panell de Control).
45. Creem la nova connexió VPN a la màquina client. Introduïm l'adreça IP o el nom de domini públic del servidor VPN i seleccionem el tipus de protocol que hem configurat (per exemple, L2TP/IPSec).
46. Establirem la connexió VPN introduint les credencials de l'usuari de domini. Observem el procés d'establiment del túnel, esperant el missatge de "Connectat".
47. Un cop connectats per VPN, executem ipconfig a la màquina client. Verifiquem que la màquina ha rebut una adreça IP dins del rang que hem definit al servidor RRAS (per exemple, 192.168.1.2xx).
48. Comprovem la resolució de noms i la connectivitat interna. Realitzem un ping al nom del servidor (per exemple, ping DC-SERVER) i a la seva IP interna, confirmant que el túnel VPN funciona i permet l'accés a la xarxa corporativa.

## Fase 7: Proves i Verificacions Finals

49. Accedim a la carpeta compartida utilitzant la ruta UNC (\NomServidor\DADES_EMPRESA$) mentre estem connectats per VPN. Això demostra que podem accedir als recursos de fitxers de l'empresa de manera segura des de fora.
50. Comprovem els permisos a la carpeta compartida. Intentem crear, modificar i eliminar un fitxer amb l'usuari de prova, verificant que els permisos NTFS i de Compartició es respecten a través del túnel VPN.
51. Documentem una captura de pantalla de l'Administrador de Servidor mostrant el Visor de Sessions actives de RRAS. Veiem la connexió de l'usuari remot, confirmant que el servei està monitoritzant l'accés.
52. Documentem una captura de pantalla del Monitor de Recursos del servidor, observant l'ús de la CPU, la memòria i la xarxa després de la connexió VPN. Això ens dóna una idea del rendiment del servidor.
53. Fem una prova de connexió VPN fallida amb un usuari que no té permisos de Dial-in. Documentem el missatge de rebuig per assegurar-nos que la seguretat d'Active Directory està protegint l'accés remot.
54. Executem la comanda net view \NomServidor des del client connectat per VPN. Veiem les carpetes compartides disponibles, confirmant que la navegació de la xarxa és funcional dins del túnel.
55. Finalitzem la sessió de l'usuari de domini i tanquem la connexió VPN. Documentem la captura de pantalla on es mostra que la connexió VPN ha estat desconnectada correctament.
56. Documentem una vista final del servidor amb el Tauler d'Administració del Servidor (Dashboard) que mostra tots els rols (AD DS, DNS, RRAS, Servidor de Fitxers) instal·lats i indicant un estat de salut correcte. Això conclou la documentació del projecte.
