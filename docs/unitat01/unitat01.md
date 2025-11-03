---
layout: default
title: "Unitat 1. Creació entorn d'arrencada amb systemd Linux"
---

## Part 1 - Preparar l'entorn

1. El primer que vaig fer va ser afegir l'usuari "hacker".   
```bash
sudo adduser hacker
```
![foto](fotos/sistemes1.png)

2. Seguidament, vaig entrar en mode administrador i vaig crear el grup "cryptshare". A més vaig afegir els usuaris "maria" i "hacker" al grup prèviament creat. També és important afegir a l'usuari "hacker" al grup sudo. En acabar comprovo el resultat al fitxer /etc/group.    
```bash
groupadd cryptshare
```
```bash
usermod -aG cryptshare maria && usermod -aG cryptshare hacker && usermod -aG sudo hacker
```
```bash
cat /etc/group | grep sudo* && cat /etc/group | grep cryp*
```
![foto](fotos/sistemes2.png)

3. Tot seguit, vaig instal·lar el paquet gocryptfs. Aquest és important per encriptar les dades.
```bash
sudo apt update && sudo apt install -y gocryptfs
```
![foto](fotos/sistemes4.png)
![foto](fotos/sistemes5.png)

5. A l'usuari hacker vaig crear dues carpetes. La primera "dades_originals_backup" té la funció de fer una copia de les dades del usuari "maria". Les demés tenen la funció de guardar les dades de "maria" encriptades.    
```bash
sudo chown -R hacker:cryptshare /home/hacker
```
```bash
mkdir dades_originals_backup && sudo chown hacker:cryptshare dades_originals_backup && sudo chmod 770 dades_originals_backup
```
![foto](fotos/sistemes6.png)
![foto](fotos/sistemes7.png)

6. A continuació, crearem fitxers en multiples carpetes dins de l'usuari "maria". A més de canviar grup propietari a cryptshare i permisos 770.
```bash
ls -l
```
![foto](fotos/docs1.png)
![foto](fotos/docs2.png)

---

## Part 2 - Crear el target

1. El primer que vaig de fer és crear el arxiu a el directori /etc/systemd/system/.    
![foto](fotos/target1.png)

2. Aquí dins el que havia de fer és escriure les següent linies, que són molt paregudes a les del arxiu 'multi-user.target'.
```bash
[Unit]
Description=Punt de sincronització per a les carpetes xifrades de l'usuari Maria
Requires=multi-user.target graphical.target
After=multi-user.target graphical.target
# Permet utilitzar 'systemctl isolate maria-docs.target'
AllowIsolate=yes
```
![foto](fotos/target2.png)

---

## Part 3 - Crear el script

1. Per començar, vaig crear el script al directori /usr/local/bin/. A més de afegir permisos per executar-lo i que l'usuari i grup propietari sigui root.    
![foto](fotos/script01.png)

2. Aquest primer bloc configura el comportament del shell perquè pari si hi ha errors (errexit, nounset, pipefail) i assegura que la variable $USER tingui un valor (si no, la pren amb whoami). Això evita execucions inesperades i fa que les rutes que depenen de $USER funcionin correctament.       
![foto](fotos/script02.png)

3. Aquí es defineixen totes les rutes i fitxers importants: on estan les versions xifrades (a l’usuari hacker), on són els punts de muntatge de maria, els fitxers de contrasenya i la carpeta de backup. L’array FOLDERS agrupa la informació per iterar-la fàcilment. Finalment assegura que el directori de backups existeixi.        
![foto](fotos/script03.png)
![foto](fotos/script04.png)

4. Aquesta funció fa tot el treball greu per a una carpeta: crea directoris i passfile si cal, inicialitza gocryptfs si no està inicialitzat, fa un backup si hi ha fitxers, buida el directori, munta la carpeta xifrada i, si cal, restaura els fitxers al punt muntat. Els echo serveixen per saber en quin pas està i per depurar.       
![foto](fotos/script05.png)
![foto](fotos/script06.png)
![foto](fotos/script07.png)

5. Aquesta funció desmunta la carpeta (si cal), buida el contingut en clar i copia els fitxers xifrats des de la carpeta _encrypted de hacker cap al directori de maria, excloent els fitxers interns de gocryptfs. És la sincronització final que deixa la carpeta de Maria amb la versió xifrada.      
![foto](fotos/script08.png)

6. Aquí s’executa tot. El primer bucle crida process_folder() per a cada carpeta (fa backup, munta i restaura). El segon bucle crida sync_encrypted_to_mountpoint() per copiar la versió xifrada final a les carpetes de l’usuari. Els echo imprimen l’inici i final per tenir un registre clar.     
![foto](fotos/script09.png)

---

## Part 4 - Crear el servei

1. Per poder fer el servei havia de crear el fitxer al mateix directori que el target, però en la diferencia de ficar '.service' al final.    
![foto](fotos/service0.png)

2. Una vegada dins és molt important configurar que funcioni a partir del target que havia creat prèviament. A més de la ruta al script, per a que l'executi.    
```bash
[Unit]
Description=Muntar carpetes xifrades de l'usuari Maria
After=network.target multi-user.target
PartOf=maria-docs.target

[Service]
Type=oneshot
User=maria
Environment=HOME=/home/maria
ExecStart=/usr/local/bin/mount_encrypted.sh
ExecStop=/usr/bin/fusermount -u /home/maria/documents-desxifrats
RemainAfterExit=yes
```
![foto](fotos/service1.png)

3. Per últim, haurem d'actualitzar el sistema, habilitar el service i fer un start del target.    
```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl set-default maria-docs.target
```
```bash
sudo systemctl start mount-docs.service
```
![foto](fotos/service2.png)
![foto](fotos/service3.png)
![foto](fotos/service4.png)

---

## Part 5 - Resultats

1. Una vegada ja hem reiniciat el sistema podrem observar que dins de cada carpeta del usuari maria les dades estan encriptades.    
```bash
ls -l Baixades
```
![foto](fotos/resul2.png)

2. Seguidament, si entrem en l'usuari hacker, podrem observa que a la carpeta dades_originals_backup estan totes les dades en clar del usuari maria.     
```bash
ls -l dades_originals_backup/
```
![foto](fotos/resul3.png)

3. Per últim si observem els directoris *_encrypted podrem veure que les dades son exactament igual a les que estan actualment a maria, per tant la integritat de les dades encriptades es manté.
```bash
ls -l Baixades_encrypted/
```
![foto](fotos/resul1.png)

---
