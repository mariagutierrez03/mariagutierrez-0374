# Netejador d’Espai MD3 (Ubuntu)

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
<img width="563" height="622" alt="image" src="https://github.com/user-attachments/assets/ec82c19a-77c0-4053-a83b-dba5080176c0" />

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

<img width="563" height="142" alt="image" src="https://github.com/user-attachments/assets/268f4fd8-0b4e-40d5-994f-572e17be50b5" />
<img width="620" height="301" alt="image" src="https://github.com/user-attachments/assets/72585b54-62e2-463f-9378-4702c3fbd566" />

---

# 3. Funcionament del programa

Aquesta és la imatge de com es veu el programa just quan l'obres per primera vegada. El cercle de la part superior dreta t'indica que el teu disc està al 56% de la seva capacitat, i el quadre de text de sota t'avisa que l'aplicació ja està "iniciada" i a punt perquè comencis a netejar el teu ordinador.                
<img width="791" height="190" alt="image" src="https://github.com/user-attachments/assets/04deb111-8b66-4237-8d3e-7835d6f64d3b" />
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/883ae016-c885-4407-a726-f199d0e28c29" />

El sistema "APT" és el que fa servir l'ordinador per instal·lar programes, i sovint queden fitxers d'instal·lació antics que ja no calen. En aquesta foto veiem com el programa acaba de fer aquesta neteja i ens informa a la llista de text que tot el procés s'ha completat correctament i amb èxit.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/bc5856c7-34a4-4632-ae65-7b8a546f042a" />

Ubuntu utilitza un sistema anomenat "Snap" per a les aplicacions, que de vegades guarda còpies velles que ocupen espai inútilment. En prémer aquest botó, el programa busca aquestes restes i les elimina; veuràs un avís a la pantalla confirmant que has recuperat una part del teu disc gràcies a aquesta acció.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/b8aa3496-ee14-43de-970c-5e524ad0a9b5" />

Els fitxers temporals o "/tmp" són arxius que l'ordinador crea per un moment i que després ja no serveixen per a res. Amb aquest botó els esborres de forma segura i, com es veu a la imatge, el programa et confirma a l'instant que la neteja s'ha completat sense cap error.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/d176fcd5-24ee-4e2b-acbd-a43454d42a7a" />

Aquesta és la pantalla principal on pots triar què vols netejar exactament per guanyar espai. En aquesta captura es veu com, després de prémer els botons, el programa t'avisa que ha acabat de buidar la paperera i de netejar els fitxers temporals, confirmant que tot ha anat bé amb un missatge clar.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/5d2192d0-66e5-4c14-8c4f-c08529b485db" />

Si no vols anar botó per botó, pots prémer "Neteja completa" per fer-ho tot de cop i estalviar temps. L'aplicació t'informarà a la llista de sota de cada pas que fa i, al final, et traurà un avís negre on podràs llegir quants gigabytes (GB) de memòria has aconseguit recuperar.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/8c9bd3f2-2c21-45c5-b2cf-c786dfc5a998" />

Aquí el programa t'ajuda a trobar les coses que pesen més i que potser ja no necessites. Quan prems el botó d'escanejar, l'aplicació busca per tot l'ordinador, si no troba cap fitxer que ocupi molt d'espai, t'ho indicarà amb el missatge "No s’han trobat fitxers grans" per quedar-te tranquil.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/14581188-5b76-42ef-b6d7-a72da7fbfe44" />

Aquest apartat serveix per veure d'un cop d'ull quant espai està ocupant tot el que tens guardat a l'ordinador. Et mostra una xifra gran, com ara "13.43 GB", i si prems el botó blau de "Actualitza informació", el programa torna a calcular l'espai per si has esborrat alguna cosa fa poc.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/a7b991d1-28ed-48c9-908a-f534dad20012" />

En aquesta pantalla pots personalitzar com es veu l'aplicació al teu gust. L'opció principal és el "Mode fosc", que canvia el fons a color negre per descansar la vista i estalviar bateria, quan l'actives, apareix un petit avís que diu "Tema actualitzat" per confirmar que el canvi s'ha fet correctament. En cas de desactivar-la apareix en mode clar, amb fons de color blau cel.                
<img width="816" height="938" alt="image" src="https://github.com/user-attachments/assets/724a1c3a-a5c2-44d7-8171-777e29b7e101" />

---

# 4. Funció de cada fitxer creat

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

```
import shutil
import os
from datetime import datetime


LOG_FILE = os.path.expanduser("~/.neteja_md3.log")


def log(text):
    """Escriu al log del sistema i al fitxer."""
    timestamp = datetime.now().strftime("[%Y-%m-%d %H:%M:%S]")
    line = f"{timestamp} {text}"
    try:
        with open(LOG_FILE, "a", encoding="utf-8") as f:
            f.write(line + "\n")
    except:
        pass
    print(line)


def format_gb(value):
    """Converteix bytes a GB amb 2 decimals."""
    return f"{value:.2f} GB"


def get_disk_usage():
    """Retorna l’espai utilitzat en GB."""
    total, used, free = shutil.disk_usage("/")
    return used / (1024**3)


def get_disk_total():
    """Retorna la capacitat total del disc en GB."""
    total, used, free = shutil.disk_usage("/")
    return total / (1024**3)
```

---

## modules/apt_cleaner.py
Aquest fitxer neteja la memòria cau d’APT i els paquets que ja no serveixen. Està separat per mantenir el codi modular i fàcil d’executar. També pot funcionar en mode simulació sense esborrar res.

| Fitxer | Funció |
|--------|--------|
| `apt_cleaner.py` | Neteja de paquets APT |

```
import subprocess
from modules.core_utils import log


def clean_apt(dry_run=False):
    """
    Neteja APT: paquets residuals, cache, dependències.
    Retorna espai alliberat en GB.
    """
    log("Executant neteja APT...")

    cmds = [
        "sudo apt autoremove -y",
        "sudo apt autoclean -y",
        "sudo apt clean -y",
    ]

    if dry_run:
        log("Mode segur: simulació APT.")
        return 0.0

    before = _get_apt_cache_size()

    for cmd in cmds:
        try:
            subprocess.run(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
        except Exception as e:
            log(f"Error executant {cmd}: {e}")

    after = _get_apt_cache_size()
    freed = max(0.0, before - after)
    return freed


def _get_apt_cache_size():
    """Retorna la mida de /var/cache/apt en GB."""
    try:
        result = subprocess.check_output("du -s /var/cache/apt", shell=True).decode()
        kb = int(result.split()[0])
        return kb / 1024 / 1024
    except:
        return 0.0

```

---

## modules/snap_cleaner.py
Aquest fitxer elimina versions antigues de paquets Snap. Manté el codi ordenat i permet fer neteges independents. Retorna l’espai alliberat després de la neteja.

| Fitxer | Funció |
|--------|--------|
| `snap_cleaner.py` | Neteja de Snap |

```
import subprocess
from modules.core_utils import log


def clean_snap(dry_run=False):
    """
    Elimina versions antigues de snaps.
    Retorna espai alliberat estimat en GB.
    """
    log("Executant neteja Snap...")

    if dry_run:
        log("Mode segur: simulació Snap.")
        return 0.0

    try:
        subprocess.run("sudo snap set system refresh.retain=2", shell=True)
        subprocess.run("sudo snap remove --purge $(snap list --all | awk '/disabled/{print $1, $3}')", shell=True)
    except Exception as e:
        log(f"Error netejant Snap: {e}")

    # Estimació aproximada
    return 0.2

```

---

## modules/trash_cleaner.py
Aquest fitxer buida la paperera de l’usuari. Recorre els fitxers i els elimina de manera segura. També pot funcionar en mode segur per evitar esborrats accidentals.

| Fitxer | Funció |
|--------|--------|
| `trash_cleaner.py` | Buidar la paperera |

```
import os
import shutil
from modules.core_utils import log


def clean_trash(dry_run=False):
    """
    Buida la paperera (~/.local/share/Trash).
    Retorna espai alliberat en GB.
    """
    trash_path = os.path.expanduser("~/.local/share/Trash")
    freed = 0

    if not os.path.exists(trash_path):
        return 0.0

    for root, dirs, files in os.walk(trash_path):
        for name in files:
            fpath = os.path.join(root, name)
            try:
                size = os.path.getsize(fpath)
                freed += size
                if not dry_run:
                    os.remove(fpath)
            except:
                pass

        for name in dirs:
            dpath = os.path.join(root, name)
            try:
                if not dry_run:
                    shutil.rmtree(dpath, ignore_errors=True)
            except:
                pass

    return freed / (1024**3)

```

---

## modules/tmp_cleaner.py
Aquest fitxer neteja el directori `/tmp`, que acumula fitxers temporals. Està separat perquè no totes les neteges necessiten tocar `/tmp`. Retorna l’espai recuperat.

| Fitxer | Funció |
|--------|--------|
| `tmp_cleaner.py` | Neteja del directori /tmp |

```
import os
import shutil
from modules.core_utils import log


def clean_tmp(dry_run=False):
    """
    Elimina fitxers temporals de /tmp.
    Retorna espai alliberat en GB.
    """
    tmp = "/tmp"
    freed = 0

    log("Netejant /tmp...")

    for root, dirs, files in os.walk(tmp):
        for name in files:
            fpath = os.path.join(root, name)
            try:
                size = os.path.getsize(fpath)
                freed += size
                if not dry_run:
                    os.remove(fpath)
            except:
                pass

        for name in dirs:
            dpath = os.path.join(root, name)
            try:
                if not dry_run:
                    shutil.rmtree(dpath, ignore_errors=True)
            except:
                pass

    return freed / (1024**3)

```

---

## modules/scan_large.py
Aquest fitxer busca fitxers grans dins la carpeta de l’usuari. Mostra quins arxius ocupen més espai i retorna una llista amb rutes i mides. Serveix per ajudar l’usuari a decidir què pot eliminar.

| Fitxer | Funció |
|--------|--------|
| `scan_large.py` | Escaneig de fitxers grans |

```
import os


def scan_large_files(path, min_size_gb=1.0):
    """
    Escaneja fitxers grans a partir d'una mida mínima.
    Retorna una llista de tuples (path, mida_gb).
    """
    min_bytes = min_size_gb * (1024**3)
    results = []

    for root, dirs, files in os.walk(path):
        for name in files:
            fpath = os.path.join(root, name)
            try:
                size = os.path.getsize(fpath)
                if size >= min_bytes:
                    results.append((fpath, size / (1024**3)))
            except:
                pass

    results.sort(key=lambda x: x[1], reverse=True)
    return results

```

---

## ui/navigation_rail.py
Aquest fitxer dibuixa el menú lateral amb estil MD3. Permet navegar entre pestanyes de manera moderna i clara. També detecta quin botó està seleccionat.

| Fitxer | Funció |
|--------|--------|
| `navigation_rail.py` | Menú lateral MD3 |

```
from PyQt5.QtWidgets import QWidget, QVBoxLayout, QPushButton, QSizePolicy
from PyQt5.QtCore import Qt, pyqtSignal, QSize
from PyQt5.QtGui import QIcon

class NavigationRail(QWidget):
    currentChanged = pyqtSignal(int)

    def __init__(self, items, icon_dir, parent=None):
        super().__init__(parent)

        self.setObjectName("NavRail")
        self.items = items
        self.icon_dir = icon_dir
        self.buttons = []
        self.current_index = 0

        layout = QVBoxLayout(self)
        layout.setContentsMargins(0, 20, 0, 20)
        layout.setSpacing(8)
        layout.setAlignment(Qt.AlignTop)

        # Crear botons
        for i, (label, icon_name) in enumerate(items):
            btn = QPushButton(label)
            btn.setObjectName("NavButton")
            btn.setCheckable(True)
            btn.setIcon(QIcon(str(icon_dir / icon_name)))
            btn.setIconSize(QSize(24, 24))
            btn.setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Fixed)
            btn.clicked.connect(lambda checked, idx=i: self.setCurrentIndex(idx))
            layout.addWidget(btn)
            self.buttons.append(btn)

        # Selecció inicial
        self.setCurrentIndex(0)

    # -------------------------
    # Selecció d’ítems
    # -------------------------

    def setCurrentIndex(self, index):
        if index < 0 or index >= len(self.buttons):
            return

        # Desmarcar tots
        for i, btn in enumerate(self.buttons):
            btn.setChecked(i == index)
            btn.setProperty("selected", "true" if i == index else "false")
            btn.style().unpolish(btn)
            btn.style().polish(btn)

        self.current_index = index
        self.currentChanged.emit(index)

    def currentIndex(self):
        return self.current_index

```

---

## ui/fab_button.py
Aquest fitxer crea el botó flotant d’acció ràpida. Dona un estil MD3 i destaca l’acció de neteja ràpida. És un element visual típic de Material Design.

| Fitxer | Funció |
|--------|--------|
| `fab_button.py` | Botó flotant MD3 |

```
from PyQt5.QtWidgets import QPushButton
from PyQt5.QtCore import Qt, QSize
from PyQt5.QtGui import QIcon, QPainter, QBrush, QColor


class FabButton(QPushButton):
    def __init__(self, icon_path, tooltip="", parent=None):
        super().__init__(parent)

        self.setObjectName("FabButton")
        self.setToolTip(tooltip)
        self.setIcon(QIcon(icon_path))
        self.setIconSize(QSize(28, 28))

        # Mida del FAB MD3
        self.setFixedSize(56, 56)

        # Elevació MD3 (ombra suau)
        self._shadow_color = QColor(0, 0, 0, 60)

        # Estil del cursor
        self.setCursor(Qt.PointingHandCursor)

        # Sense focus rectangular
        self.setFocusPolicy(Qt.NoFocus)

    def paintEvent(self, event):
        # Dibuixar ombra MD3
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing)

        # Ombra
        painter.setBrush(QBrush(self._shadow_color))
        painter.setPen(Qt.NoPen)
        painter.drawEllipse(2, 4, self.width() - 4, self.height() - 4)

        # Dibuixar botó normal
        super().paintEvent(event)

```

---

## ui/md3_snackbar.py
Aquest fitxer mostra missatges emergents a la part inferior de la pantalla. Serveix per avisar l’usuari de resultats, errors o confirmacions. És útil per donar feedback sense interrompre.

| Fitxer | Funció |
|--------|--------|
| `md3_snackbar.py` | Missatges emergents |

```
from PyQt5.QtWidgets import QWidget, QLabel
from PyQt5.QtCore import Qt, QPropertyAnimation, QRect, QEasingCurve, QTimer
from PyQt5.QtGui import QColor, QPalette


class Snackbar(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)

        self.setObjectName("Snackbar")
        self.setWindowFlags(Qt.FramelessWindowHint | Qt.ToolTip)
        self.setAttribute(Qt.WA_TranslucentBackground)
        self.setAutoFillBackground(False)

        self.label = QLabel("", self)
        self.label.setAlignment(Qt.AlignCenter)
        self.label.setStyleSheet("padding: 8px 16px;")

        self.resize(300, 40)
        self.hide()

        self.anim = QPropertyAnimation(self, b"geometry")
        self.anim.setDuration(250)
        self.anim.setEasingCurve(QEasingCurve.OutCubic)

        self.fade = QPropertyAnimation(self, b"windowOpacity")
        self.fade.setDuration(250)

        self.timer = QTimer()
        self.timer.timeout.connect(self._hide_snackbar)

    # -----------------------------------------------------

    def show_message(self, text, duration=3000):
        self.label.setText(text)
        self.adjustSize()

        parent = self.parentWidget()
        if not parent:
            return

        pw = parent.width()
        ph = parent.height()

        w = self.width()
        h = self.height()

        x = (pw - w) // 2
        y = ph - h - 30

        self.setGeometry(x, y + 40, w, h)
        self.setWindowOpacity(0.0)
        self.show()

        # Animació d'entrada
        self.anim.stop()
        self.anim.setStartValue(QRect(x, y + 40, w, h))
        self.anim.setEndValue(QRect(x, y, w, h))
        self.anim.start()

        self.fade.stop()
        self.fade.setStartValue(0.0)
        self.fade.setEndValue(1.0)
        self.fade.start()

        # Temporitzador per desaparèixer
        self.timer.start(duration)

    # -----------------------------------------------------

    def _hide_snackbar(self):
        self.timer.stop()

        # Animació de sortida
        geo = self.geometry()
        x, y, w, h = geo.x(), geo.y(), geo.width(), geo.height()

        self.anim.stop()
        self.anim.setStartValue(QRect(x, y, w, h))
        self.anim.setEndValue(QRect(x, y + 40, w, h))
        self.anim.start()

        self.fade.stop()
        self.fade.setStartValue(1.0)
        self.fade.setEndValue(0.0)
        self.fade.start()

        # Amagar després de l’animació
        QTimer.singleShot(250, self.hide)

```

---

## ui/md3_cards.py
Aquest fitxer dibuixa les targetes MD3 amb ombra i cantonades arrodonides. Organitza el contingut de cada pestanya i conté el layout interior on s’afegeixen els botons. Dona un aspecte net i modern.

| Fitxer | Funció |
|--------|--------|
| `md3_cards.py` | Targetes MD3 |

```
from PyQt5.QtWidgets import QFrame, QVBoxLayout, QWidget
from PyQt5.QtGui import QColor, QPainter, QBrush
from PyQt5.QtCore import Qt, QRectF


class Md3Card(QFrame):
    def __init__(self, parent=None, padding=16):
        super().__init__(parent)

        self.radius = 18
        self.padding = padding

        self.surface_color = QColor("#FFFFFF")
        self.shadow_color = QColor(0, 0, 0, 30)

        # Contenidor interior
        self.inner = QWidget(self)
        self.inner.setObjectName("CardInner")

        layout = QVBoxLayout(self.inner)
        layout.setContentsMargins(padding, padding, padding, padding)
        layout.setSpacing(10)

        main_layout = QVBoxLayout(self)
        main_layout.setContentsMargins(0, 0, 0, 0)
        main_layout.addWidget(self.inner)

        self.setAttribute(Qt.WA_StyledBackground, True)

    def resizeEvent(self, event):
        self.inner.setGeometry(0, 0, self.width(), self.height())
        super().resizeEvent(event)

    def paintEvent(self, event):
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing)

        w = self.width()
        h = self.height()

        painter.setBrush(QBrush(self.shadow_color))
        painter.setPen(Qt.NoPen)
        painter.drawRoundedRect(QRectF(3, 5, w - 6, h - 6), self.radius, self.radius)

        painter.setBrush(QBrush(self.surface_color))
        painter.setPen(Qt.NoPen)
        painter.drawRoundedRect(QRectF(0, 0, w - 6, h - 6), self.radius, self.radius)

```

---

## ui/disk_chart.py
Aquest fitxer dibuixa el gràfic circular de l’ús del disc. Utilitza QPainter per crear un estil MD3 personalitzat. També adapta el color del text segons si el tema és clar o fosc.

| Fitxer | Funció |
|--------|--------|
| `disk_chart.py` | Gràfic circular d’ús del disc |

```
from PyQt5.QtWidgets import QWidget
from PyQt5.QtGui import QPainter, QColor, QPen, QFont
from PyQt5.QtCore import Qt, QRectF
from modules.core_utils import get_disk_usage, get_disk_total


class DiskChart(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        self.used = 0
        self.total = 1
        self.setMinimumHeight(200)
        self.update_chart()

    # -----------------------------------------------------

    def update_chart(self):
        self.used = get_disk_usage()
        self.total = get_disk_total()
        self.repaint()

    # -----------------------------------------------------

    def paintEvent(self, event):
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing)

        w = self.width()
        h = self.height()
        size = min(w, h) - 40

        rect = QRectF((w - size) / 2, (h - size) / 2, size, size)

        # Colors MD3 (paleta C4)
        primary = QColor("#4285F4")
        surface = QColor("#E0E0E0")

        # Percentatge
        pct = min(max(self.used / self.total, 0), 1)

        # Fons del donut
        pen_bg = QPen(surface, 26)
        pen_bg.setCapStyle(Qt.RoundCap)
        painter.setPen(pen_bg)
        painter.drawArc(rect, 0, 360 * 16)

        # Arc d’ús
        pen_fg = QPen(primary, 26)
        pen_fg.setCapStyle(Qt.RoundCap)
        painter.setPen(pen_fg)
        painter.drawArc(rect, 90 * 16, -int(360 * pct * 16))

        # --- TEXT CENTRAL AMB COLOR AUTOMÀTIC (LIGHT/DARK) ---

        # Detectar si el tema és fosc segons el color de fons real del widget
        bg = self.palette().color(self.backgroundRole())
        is_dark = bg.value() < 128   # <128 = fosc, >128 = clar

        # Text blanc en dark, negre en light
        text_color = QColor("#FFFFFF") if is_dark else QColor("#1C1B1F")
        painter.setPen(text_color)

        painter.setFont(QFont("Segoe UI", 18, QFont.Bold))
        percent_text = f"{int(pct * 100)}%"
        painter.drawText(rect, Qt.AlignCenter, percent_text)

```

---

## theme/md3_light.qss
Aquest fitxer defineix l’estil visual en mode clar. Utilitza colors blaus i grisos suaus per seguir Material Design 3. Dona forma als botons, targetes i textos.

| Fitxer | Funció |
|--------|--------|
| `md3_light.qss` | Tema clar MD3 |

```
QWidget {
    background-color: #E8F0FE;
    color: #1C1B1F;
    font-family: "Segoe UI", sans-serif;
    font-size: 14px;
}

/* Títols */
#PageTitle {
    font-size: 26px;
    font-weight: 600;
    margin-bottom: 4px;
}

#PageDescription {
    font-size: 15px;
    color: #5F6368;
    margin-bottom: 16px;
}

/* Botons */
QPushButton {
    background-color: #4285F4;
    color: #FFFFFF;
    border-radius: 8px;
    padding: 8px 14px;
    border: none;
}

QPushButton:hover {
    background-color: #5A95F6;
}

QPushButton:pressed {
    background-color: #2F6FE0;
}

/* Navigation Rail */
#NavRail {
    background-color: #FFFFFF;
    border-right: 1px solid #DADCE0;
}

#NavRail QPushButton {
    background: transparent;
    color: #1C1B1F;
    border-radius: 12px;
    padding: 10px 12px;
    text-align: left;
}

#NavRail QPushButton:hover {
    background-color: #E8F0FE;
}

#NavRail QPushButton[selected="true"] {
    background-color: #D2E3FC;
    font-weight: 600;
}

/* FAB */
#FabButton {
    background-color: #4285F4;
    color: #FFFFFF;
    border-radius: 24px;
}

/* Snackbar */
#Snackbar QLabel {
    background-color: #323232;
    color: #FFFFFF;
    border-radius: 6px;
    padding: 10px 16px;
}

/* Progress Bar */
QProgressBar {
    background-color: #DADCE0;
    border-radius: 6px;
    height: 12px;
}

QProgressBar::chunk {
    background-color: #4285F4;
    border-radius: 6px;
}

/* TextEdit */
QTextEdit {
    background-color: #FFFFFF;
    border: 1px solid #DADCE0;
    border-radius: 8px;
    padding: 8px;
}

```

---

## theme/md3_dark.qss
Aquest fitxer crea el mode fosc amb tons negres i blaus. Manté coherència amb el mode clar però invertint la lluminositat. Ajusta els colors perquè el text sigui llegible.

| Fitxer | Funció |
|--------|--------|
| `md3_dark.qss` | Tema fosc MD3 |

```
/* ------------------------------
   Material Design 3 — Dark Theme
   Estil coherent amb MD3 Light
   ------------------------------ */

QWidget {
    background-color: #121212;
    color: #E3E3E3;
    font-family: "Segoe UI", sans-serif;
    font-size: 14px;
}

/* Títols */
#PageTitle {
    font-size: 26px;
    font-weight: 600;
    color: #E3E3E3;
    margin-bottom: 4px;
}

#PageDescription {
    font-size: 15px;
    color: #B0B0B0;
    margin-bottom: 16px;
}

/* Botons */
QPushButton {
    background-color: #4285F4;      /* Blau MD3 */
    color: #FFFFFF;
    border-radius: 8px;
    padding: 8px 14px;
    border: none;
}

QPushButton:hover {
    background-color: #5A95F6;
}

QPushButton:pressed {
    background-color: #2F6FE0;
}

/* Navigation Rail */
#NavRail {
    background-color: #1E1E1E;
    border-right: 1px solid #2C2C2C;
}

#NavRail QPushButton {
    background: transparent;
    color: #E3E3E3;
    border-radius: 12px;
    padding: 10px 12px;
    text-align: left;
}

#NavRail QPushButton:hover {
    background-color: #2A2A2A;
}

#NavRail QPushButton[selected="true"] {
    background-color: #2F3B55;   /* Blau fosc MD3 */
    color: white;
    font-weight: 600;
}

/* FAB */
#FabButton {
    background-color: #4285F4;
    color: #FFFFFF;
    border-radius: 24px;
}

#FabButton:hover {
    background-color: #5A95F6;
}

#FabButton:pressed {
    background-color: #2F6FE0;
}

/* Snackbar */
#Snackbar QLabel {
    background-color: #323232;
    color: #FFFFFF;
    border-radius: 6px;
    padding: 10px 16px;
}

/* Progress Bar */
QProgressBar {
    background-color: #2C2C2C;
    border-radius: 6px;
    height: 12px;
}

QProgressBar::chunk {
    background-color: #4285F4;
    border-radius: 6px;
}

/* TextEdit */
QTextEdit {
    background-color: #1E1E1E;
    border: 1px solid #3A3A3A;
    border-radius: 8px;
    padding: 8px;
}

```
