# Documentació del projecte — Netejador d’Espai MD3 (Ubuntu)

## 1. Estructura de fitxers i carpetes
Vaig organitzar el projecte en carpetes separades per mantenir-lo ordenat i fàcil d’entendre. La carpeta `modules` contenia totes les funcions de neteja i càlcul, mentre que la carpeta `ui` guardava els components visuals com les targetes, el gràfic i el navigation rail. També vaig crear la carpeta `theme` per tenir els fitxers d’estil MD3 en mode clar i fosc.

### Esquema visual de l’estructura

| Carpeta / Fitxer | Contingut | Funció |
|------------------|-----------|--------|
| `modules/` | Funcions de neteja i càlcul | Treballar amb el sistema |
| `ui/` | Components visuals | Elements MD3 personalitzats |
| `theme/` | Fitxers QSS | Estils light i dark |
| `icons/` | Icones SVG | Icones del navigation rail i FAB |
| `main.py` | Arrel del projecte | Control de la interfície |

---

## 2. Permisos i paquets necessaris
Per crear l’aplicació només vaig necessitar Python 3 i el paquet PyQt5, que vaig instal·lar amb `apt` o `pip`. No vaig necessitar permisos especials, ja que totes les funcions funcionaven amb permisos normals d’usuari. També vaig utilitzar ordres d’Ubuntu com APT, Snap i la paperera, que es podien executar en mode simulació sense risc.

### Taula de requisits

| Element | Necessitat | Explicació |
|--------|------------|------------|
| Python 3 | Sí | Llenguatge principal del projecte |
| PyQt5 | Sí | Crear la interfície gràfica |
| Permisos root | No | Tot funcionava amb permisos normals |
| APT / Snap / Paperera | Sí | Ordres per netejar espai |

---

# 3. Funció de cada fitxer creat

## main.py
Aquest fitxer controla tota la interfície gràfica i la navegació entre pestanyes. Gestiona els botons, el navigation rail, el botó flotant i el snackbar. També crea i coordina els fils de treball perquè l’aplicació no es bloquegi durant les neteges.

| Fitxer | Tipus | Funció |
|--------|-------|--------|
| `main.py` | Interfície | Control de pestanyes, botons i fils |

---

## modules/core_utils.py
Aquest fitxer calcula l’espai utilitzat i total del disc i ofereix funcions bàsiques com el format de GB. També escriu missatges al registre intern. Serveix com a base per obtenir dades reals del sistema.

| Fitxer | Funció |
|--------|--------|
| `core_utils.py` | Càlcul d’espai i utilitats |

---

## modules/apt_cleaner.py
Aquest fitxer neteja la memòria cau d’APT i els paquets que ja no serveixen. Està separat per mantenir el codi modular i fàcil d’executar. També pot funcionar en mode simulació sense esborrar res.

| Fitxer | Funció |
|--------|--------|
| `apt_cleaner.py` | Neteja de paquets APT |

---

## modules/snap_cleaner.py
Aquest fitxer elimina versions antigues de paquets Snap. Manté el codi ordenat i permet fer neteges independents. Retorna l’espai alliberat després de la neteja.

| Fitxer | Funció |
|--------|--------|
| `snap_cleaner.py` | Neteja de Snap |

---

## modules/trash_cleaner.py
Aquest fitxer buida la paperera de l’usuari. Recorre els fitxers i els elimina de manera segura. També pot funcionar en mode segur per evitar esborrats accidentals.

| Fitxer | Funció |
|--------|--------|
| `trash_cleaner.py` | Buidar la paperera |

---

## modules/tmp_cleaner.py
Aquest fitxer neteja el directori `/tmp`, que acumula fitxers temporals. Està separat perquè no totes les neteges necessiten tocar `/tmp`. Retorna l’espai recuperat.

| Fitxer | Funció |
|--------|--------|
| `tmp_cleaner.py` | Neteja del directori /tmp |

---

## modules/scan_large.py
Aquest fitxer busca fitxers grans dins la carpeta de l’usuari. Mostra quins arxius ocupen més espai i retorna una llista amb rutes i mides. Serveix per ajudar l’usuari a decidir què pot eliminar.

| Fitxer | Funció |
|--------|--------|
| `scan_large.py` | Escaneig de fitxers grans |

---

## ui/navigation_rail.py
Aquest fitxer dibuixa el menú lateral amb estil MD3. Permet navegar entre pestanyes de manera moderna i clara. També detecta quin botó està seleccionat.

| Fitxer | Funció |
|--------|--------|
| `navigation_rail.py` | Menú lateral MD3 |

---

## ui/fab_button.py
Aquest fitxer crea el botó flotant d’acció ràpida. Dona un estil MD3 i destaca l’acció de neteja ràpida. És un element visual típic de Material Design.

| Fitxer | Funció |
|--------|--------|
| `fab_button.py` | Botó flotant MD3 |

---

## ui/md3_snackbar.py
Aquest fitxer mostra missatges emergents a la part inferior de la pantalla. Serveix per avisar l’usuari de resultats, errors o confirmacions. És útil per donar feedback sense interrompre.

| Fitxer | Funció |
|--------|--------|
| `md3_snackbar.py` | Missatges emergents |

---

## ui/md3_cards.py
Aquest fitxer dibuixa les targetes MD3 amb ombra i cantonades arrodonides. Organitza el contingut de cada pestanya i conté el layout interior on s’afegeixen els botons. Dona un aspecte net i modern.

| Fitxer | Funció |
|--------|--------|
| `md3_cards.py` | Targetes MD3 |

---

## ui/disk_chart.py
Aquest fitxer dibuixa el gràfic circular de l’ús del disc. Utilitza QPainter per crear un estil MD3 personalitzat. També adapta el color del text segons si el tema és clar o fosc.

| Fitxer | Funció |
|--------|--------|
| `disk_chart.py` | Gràfic circular d’ús del disc |

---

## theme/md3_light.qss
Aquest fitxer defineix l’estil visual en mode clar. Utilitza colors blaus i grisos suaus per seguir Material Design 3. Dona forma als botons, targetes i textos.

| Fitxer | Funció |
|--------|--------|
| `md3_light.qss` | Tema clar MD3 |

---

## theme/md3_dark.qss
Aquest fitxer crea el mode fosc amb tons negres i blaus. Manté coherència amb el mode clar però invertint la lluminositat. Ajusta els colors perquè el text sigui llegible.

| Fitxer | Funció |
|--------|--------|
| `md3_dark.qss` | Tema fosc MD3 |
