---
layout: default
title: "Unitat 6. LAMP a Linux Ubuntu"
---

## Part 1 - Instal·lació del servidor LAMP

---

1. Abans d'instal·lar qualsevol component, és fonamental actualitzar la llista de paquets disponibles i les versions dels paquets ja instal·lats per garantir la seguretat i estabilitat del servidor.
```bash
# Actualitzar la llista de paquets
sudo apt-get update

# Actualitzar els paquets instal·lats
sudo apt-get upgrade -y
``` 
![foto](fotos/web1.png)
![foto](fotos/web2.png)

2. Instal·larem el paquet 'apache2', que inclou el servidor web i les eines necessàries per al seu funcionament. Després de la instal·lació, iniciarem el servei per posar en marxa el servidor web.
```bash
# Instal·lar el servidor web Apache2
sudo apt-get install apache2 -y

# Iniciar el servei d'Apache
sudo systemctl start apache2
```
![foto](fotos/web3.png)
![foto](fotos/web4.png)

3. És crucial verificar que el servidor web Apache s'estigui executant correctament i habilitar-lo perquè s'iniciï automàticament cada vegada que s'iniciï el servidor.
```bash
# Verificar l'estat del servei Apache
sudo systemctl status apache2

# Habilitar Apache per iniciar automàticament
sudo systemctl enable apache2
```
4. En aquest pas, instal·larem el servidor de bases de dades MariaDB, que és una alternativa compatible i de codi obert a MySQL, clau per emmagatzemar la informació de les nostres aplicacions.
```bash
# Instal·lar el servidor de base de dades MariaDB
sudo apt install mariadb-server -y
```
![foto](fotos/web5.png)
![foto](fotos/web6.png)

5. Un cop instal·lat MariaDB, iniciarem el servei i executarem l'script de seguretat vital per eliminar configuracions insegures per defecte, com usuaris anònims o l'accés remot de root, i establirem una contrasenya per al root de la base de dades.
```bash
# Iniciar el servei de MariaDB
sudo systemctl start mariadb

# Executar l'script de configuració segura
sudo mysql_secure_installation

# Habilitar MariaDB per iniciar automàticament amb el sistema
sudo systemctl enable mariadb
```
![foto](fotos/web7.png)
![foto](fotos/web8.png)
![foto](fotos/web9.png)
![foto](fotos/web10.png)

6. 
![foto](fotos/web11.png)
![foto](fotos/web12.png)
![foto](fotos/web13.png)
![foto](fotos/web14.png)
![foto](fotos/web15.png)
![foto](fotos/web16.png)
![foto](fotos/web17.png)
![foto](fotos/web18.png)
![foto](fotos/web19.png)
![foto](fotos/web20.png)
![foto](fotos/web21.png)
![foto](fotos/web22.png)
![foto](fotos/web23.png)
![foto](fotos/web24.png)
![foto](fotos/web25.png)
![foto](fotos/web26.png)
![foto](fotos/web27.png)
![foto](fotos/web28.png)
![foto](fotos/web29.png)
![foto](fotos/web30.png)
![foto](fotos/web31.png)

8.

9.

10.

11.

12.

13.

14.

15.

16.

---
