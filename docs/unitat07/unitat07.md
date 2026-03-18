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

<img width="563" height="232" alt="image" src="https://github.com/user-attachments/assets/529055c6-0d95-49bf-85ba-d0903b1f7112" />

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

```
#!/usr/bin/env python3
import sys
import os
from pathlib import Path

from PyQt5.QtCore import Qt, QThread, pyqtSignal
from PyQt5.QtGui import QIcon
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QWidget, QHBoxLayout, QVBoxLayout,
    QTabWidget, QTextEdit, QLabel, QPushButton, QCheckBox,
    QProgressBar, QSizePolicy
)

from modules.core_utils import log, format_gb, get_disk_usage
from modules.apt_cleaner import clean_apt
from modules.snap_cleaner import clean_snap
from modules.trash_cleaner import clean_trash
from modules.tmp_cleaner import clean_tmp
from modules.scan_large import scan_large_files

from ui.navigation_rail import NavigationRail
from ui.fab_button import FabButton
from ui.md3_snackbar import Snackbar
from ui.disk_chart import DiskChart
from ui.md3_cards import Md3Card


BASE_DIR = Path(__file__).resolve().parent
THEME_DIR = BASE_DIR / "theme"
ICON_DIR = THEME_DIR / "icons"


class CleanerWorker(QThread):
    progress = pyqtSignal(int)
    finished = pyqtSignal(float, str)
    message = pyqtSignal(str)

    def __init__(self, func, dry_run=False, label=""):
        super().__init__()
        self.func = func
        self.dry_run = dry_run
        self.label = label

    def run(self):
        try:
            self.message.emit(f"Iniciant {self.label.lower()}…")
            self.progress.emit(10)
            freed = self.func(dry_run=self.dry_run)
            self.progress.emit(100)
            self.finished.emit(freed, self.label)
        except Exception as e:
            self.message.emit(f"Error durant {self.label.lower()}: {e}")
            self.progress.emit(0)
            self.finished.emit(0.0, self.label)


class ScanWorker(QThread):
    progress = pyqtSignal(int)
    finished = pyqtSignal(list)
    message = pyqtSignal(str)

    def __init__(self, path="~", min_size_gb=1.0):
        super().__init__()
        self.path = os.path.expanduser(path)
        self.min_size_gb = min_size_gb

    def run(self):
        try:
            self.message.emit("Escanejant fitxers grans…")
            self.progress.emit(10)
            files = scan_large_files(self.path, self.min_size_gb)
            self.progress.emit(100)
            self.finished.emit(files)
        except Exception as e:
            self.message.emit(f"Error a l’escaneig: {e}")
            self.progress.emit(0)
            self.finished.emit([])


class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("Netejador d’espai — MD3")
        self.resize(1200, 720)

        self.dry_run = False
        self.current_worker = None

        self._load_theme(light=True)
        self._init_ui()

    def _load_theme(self, light=True):
        qss_file = THEME_DIR / ("md3_light.qss" if light else "md3_dark.qss")
        if qss_file.exists():
            with open(qss_file, "r", encoding="utf-8") as f:
                QApplication.instance().setStyleSheet(f.read())

    def _init_ui(self):
        central = QWidget()
        root_layout = QHBoxLayout(central)
        root_layout.setContentsMargins(0, 0, 0, 0)

        self.nav_rail = NavigationRail(
            items=[
                ("Neteja", "clean.svg"),
                ("Fitxers grans", "files.svg"),
                ("Informes", "reports.svg"),
                ("Configuració", "settings.svg"),
            ],
            icon_dir=ICON_DIR,
        )
        self.nav_rail.setObjectName("NavRail")
        self.nav_rail.currentChanged.connect(self.on_nav_changed)

        self.fab = FabButton(str(ICON_DIR / "fab_add.svg"), "Neteja ràpida")
        self.fab.setObjectName("FabButton")
        self.fab.clicked.connect(self.on_fab_clicked)

        self.tabs = QTabWidget()
        self.tabs.setDocumentMode(True)

        self.disk_chart = DiskChart()
        self.disk_chart.setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Fixed)
        self.disk_chart.setFixedHeight(220)

        self.progress = QProgressBar()
        self.progress.setRange(0, 100)
        self.progress.setValue(0)

        self.log_view = QTextEdit()
        self.log_view.setReadOnly(True)
        self.log_view.setMinimumHeight(120)

        self.tab_clean = QWidget()
        self._init_tab_clean()

        self.tab_large = QWidget()
        self._init_tab_large()

        self.tab_reports = QWidget()
        self._init_tab_reports()

        self.tab_settings = QWidget()
        self._init_tab_settings()

        self.tabs.addTab(self.tab_clean, "Neteja")
        self.tabs.addTab(self.tab_large, "Fitxers grans")
        self.tabs.addTab(self.tab_reports, "Informes")
        self.tabs.addTab(self.tab_settings, "Configuració")

        self.snackbar = Snackbar(self)
        self.snackbar.setObjectName("Snackbar")

        right = QVBoxLayout()
        right.setContentsMargins(16, 16, 16, 16)
        right.setSpacing(12)
        right.addWidget(self.disk_chart)
        right.addWidget(self.tabs)
        right.addWidget(self.progress)
        right.addWidget(self.log_view)

        rail_layout = QVBoxLayout()
        rail_layout.setContentsMargins(8, 16, 8, 16)
        rail_layout.setSpacing(12)
        rail_layout.addWidget(self.nav_rail)
        rail_layout.addStretch()
        rail_layout.addWidget(self.fab, 0, Qt.AlignHCenter | Qt.AlignBottom)

        root_layout.addLayout(rail_layout)
        root_layout.addLayout(right)

        self.setCentralWidget(central)
        self.nav_rail.setCurrentIndex(0)
        self.tabs.setCurrentIndex(0)
        self._append_log("Aplicació iniciada.")

    def _init_tab_clean(self):
        layout = QVBoxLayout(self.tab_clean)
        layout.setSpacing(12)

        title = QLabel("Neteja del sistema")
        title.setObjectName("PageTitle")
        desc = QLabel("Allibera espai eliminant fitxers innecessaris i memòria cau.")
        desc.setObjectName("PageDescription")

        layout.addWidget(title)
        layout.addWidget(desc)

        card = Md3Card()
        card.setMinimumHeight(220)

        # CORRECCIÓ IMPORTANT
        card_layout = card.inner.layout()

        self.chk_dry_run = QCheckBox("Mode segur (simulació, no s’esborra res)")
        self.chk_dry_run.stateChanged.connect(self.on_dry_run_changed)
        card_layout.addWidget(self.chk_dry_run)

        btn_apt = QPushButton("Neteja APT")
        btn_apt.clicked.connect(lambda: self.run_cleaner(clean_apt, "Neteja APT"))
        card_layout.addWidget(btn_apt)

        btn_snap = QPushButton("Neteja Snap")
        btn_snap.clicked.connect(lambda: self.run_cleaner(clean_snap, "Neteja Snap"))
        card_layout.addWidget(btn_snap)

        btn_trash = QPushButton("Buida paperera")
        btn_trash.clicked.connect(lambda: self.run_cleaner(clean_trash, "Buida paperera"))
        card_layout.addWidget(btn_trash)

        btn_tmp = QPushButton("Neteja /tmp")
        btn_tmp.clicked.connect(lambda: self.run_cleaner(clean_tmp, "Neteja /tmp"))
        card_layout.addWidget(btn_tmp)

        btn_all = QPushButton("Neteja completa")
        btn_all.clicked.connect(self.run_clean_all)
        card_layout.addWidget(btn_all)

        layout.addWidget(card)
        layout.addStretch()

    def _init_tab_large(self):
        layout = QVBoxLayout(self.tab_large)
        layout.setSpacing(12)

        title = QLabel("Fitxers grans")
        title.setObjectName("PageTitle")
        desc = QLabel("Escaneja i revisa fitxers grans per alliberar espai.")
        desc.setObjectName("PageDescription")

        layout.addWidget(title)
        layout.addWidget(desc)

        card = Md3Card()
        card.setMinimumHeight(220)

        # CORRECCIÓ IMPORTANT
        card_layout = card.inner.layout()

        btn_scan = QPushButton("Escaneja fitxers grans")
        btn_scan.clicked.connect(self.run_scan_large)
        card_layout.addWidget(btn_scan)

        self.large_files_view = QTextEdit()
        self.large_files_view.setReadOnly(True)
        card_layout.addWidget(self.large_files_view)

        layout.addWidget(card)
        layout.addStretch()

    def _init_tab_reports(self):
        layout = QVBoxLayout(self.tab_reports)
        layout.setSpacing(12)

        title = QLabel("Informes del disc")
        title.setObjectName("PageTitle")
        desc = QLabel("Consulta l’estat actual del disc i l’espai utilitzat.")
        desc.setObjectName("PageDescription")

        layout.addWidget(title)
        layout.addWidget(desc)

        card = Md3Card()
        card.setMinimumHeight(220)

        # CORRECCIÓ IMPORTANT
        card_layout = card.inner.layout()

        self.lbl_disk_info = QLabel("")
        card_layout.addWidget(self.lbl_disk_info)

        btn_refresh = QPushButton("Actualitza informació")
        btn_refresh.clicked.connect(self.update_disk_info)
        card_layout.addWidget(btn_refresh)

        layout.addWidget(card)
        layout.addStretch()

        self.update_disk_info()

    def _init_tab_settings(self):
        layout = QVBoxLayout(self.tab_settings)
        layout.setSpacing(12)

        title = QLabel("Configuració")
        title.setObjectName("PageTitle")
        desc = QLabel("Personalitza l’aparença i el comportament de l’aplicació.")
        desc.setObjectName("PageDescription")

        layout.addWidget(title)
        layout.addWidget(desc)

        card = Md3Card()
        card.setMinimumHeight(220)

        # CORRECCIÓ IMPORTANT
        card_layout = card.inner.layout()

        self.chk_theme = QCheckBox("Mode fosc")
        self.chk_theme.stateChanged.connect(self.on_theme_toggled)
        card_layout.addWidget(self.chk_theme)

        layout.addWidget(card)
        layout.addStretch()

    def on_nav_changed(self, index):
        self.tabs.setCurrentIndex(index)

    def on_fab_clicked(self):
        self.run_clean_all(quick=True)

    def on_dry_run_changed(self, state):
        self.dry_run = state == Qt.Checked
        msg = "Mode segur activat (no s’esborra res)." if self.dry_run else "Mode segur desactivat."
        self.snackbar.show_message(msg)
        self._append_log(msg)

    def on_theme_toggled(self, state):
        self._load_theme(light=(state != Qt.Checked))
        self.snackbar.show_message("Tema actualitzat.")

    def run_cleaner(self, func, label):
        if self.current_worker and self.current_worker.isRunning():
            self.snackbar.show_message("Ja hi ha una tasca en execució.")
            return

        self.progress.setValue(0)
        worker = CleanerWorker(func, dry_run=self.dry_run, label=label)
        self.current_worker = worker

        worker.progress.connect(self.progress.setValue)
        worker.message.connect(self._append_log)
        worker.finished.connect(self.on_cleaner_finished)

        worker.start()

    def run_clean_all(self, quick=False):
        if self.current_worker and self.current_worker.isRunning():
            self.snackbar.show_message("Ja hi ha una tasca en execució.")
            return

        def all_clean(dry_run=False):
            total = 0.0
            total += clean_apt(dry_run=dry_run)
            total += clean_snap(dry_run=dry_run)
            total += clean_trash(dry_run=dry_run)
            if not quick:
                total += clean_tmp(dry_run=dry_run)
            return total

        label = "neteja ràpida" if quick else "neteja completa"
        worker = CleanerWorker(all_clean, dry_run=self.dry_run, label=label)
        self.current_worker = worker

        self.progress.setValue(0)
        worker.progress.connect(self.progress.setValue)
        worker.message.connect(self._append_log)
        worker.finished.connect(self.on_cleaner_finished)

        worker.start()

    def run_scan_large(self):
        if self.current_worker and self.current_worker.isRunning():
            self.snackbar.show_message("Ja hi ha una tasca en execució.")
            return

        self.progress.setValue(0)
        worker = ScanWorker(path="~", min_size_gb=1.0)
        self.current_worker = worker

        worker.progress.connect(self.progress.setValue)
        worker.message.connect(self._append_log)
        worker.finished.connect(self.on_scan_finished)

        worker.start()

    def on_cleaner_finished(self, freed, label):
        self.current_worker = None
        msg = f"La {label.lower()} s’ha completat correctament. Has recuperat {format_gb(freed)}."
        self.snackbar.show_message(msg)
        self._append_log(msg)
        self.disk_chart.update_chart()

    def on_scan_finished(self, files):
        self.current_worker = None
        if not files:
            self.large_files_view.setPlainText("No s’han trobat fitxers grans.")
            self.snackbar.show_message("Escaneig completat.")
            return

        lines = [f"{path} — {size:.2f} GB" for path, size in files]
        self.large_files_view.setPlainText("\n".join(lines))
        self.snackbar.show_message("Escaneig completat.")

    def _append_log(self, text):
        self.log_view.append(text)
        log(text)

    def update_disk_info(self):
        used = get_disk_usage()
        msg = f"Espai utilitzat aproximat: {format_gb(used)}"
        self.lbl_disk_info.setText(msg)
        self.disk_chart.update_chart()
        self._append_log("Informació del disc actualitzada.")


def main():
    app = QApplication(sys.argv)
    icon = ICON_DIR / "clean.svg"
    if icon.exists():
        app.setWindowIcon(QIcon(str(icon)))

    win = MainWindow()
    win.show()
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()

```

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
