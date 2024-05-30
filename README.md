[Todolistenverwaltung.yaml](https://github.com/IlyaSavchenkoIFA22/LF9/blob/main/apiserver/Todolistenverwaltung.yaml)
[Python-Server](https://github.com/IlyaSavchenkoIFA22/LF9/blob/main/apiserver/beispiel-server.py)
[![MIT License](https://img.shields.io/badge/license-MIT-red.svg?style=flat)](https://github.com/IlyaSavchenkoIFA22/LF9/blob/main/LICENSE)

# Todolistenverwaltung

## Endpunkte 


```markdown
1. /todo-list/{list-id}
	1. GET: Gibt die Todo-Liste mit der angegebenen ID zurück.
	2. DELETE: Löscht die Todo-Liste mit der angegebenen ID.
2. /todo-list
	1. POST: Erstellt eine neue Todo-Liste.
3. /todo-list/{list_id}/entry
	1. POST: Fügt einen neuen Eintrag zur angegebenen Todo-Liste hinzu.
4. /todo-list/{list-id}/entry/{entry-id}
	1. PATCH: Aktualisiert einen bestehenden Eintrag in der angegebenen Todo-Liste.
	2. DELETE: Löscht einen Eintrag aus der angegebenen Todo-Liste.
```


> [!info]
	>Dies ist ein Python-basierter Server, der auf einem Raspberry Pi OS in einem Container bereitgestellt wird und als Todolistenverwaltungs-Tool mit mehreren Listen und Einträgen fungiert. Der Server nutzt OpenAPI und bietet die folgenden Endpunkte zur Verwaltung von Todo-Listen und -Einträgen.

## Anforderungen

```markdown
1. Netzwerkkonfiguration: Der Raspberry Pi muss mit einer statischen IP-Adresse im lokalen Subnetz konfiguriert werden
2. Zwei lokale Nutzer: 
	1. Normaler Nutzer für den allgemeinen Zugriff
	2. Nutzer mit sudo-Rechten für den SSH-Zugriff
3. SSH-Dienst: Muss aktiviert sein, damit sudo-Nutzer administrative Rechte hat
4. Deployment: Die Todolistenverwaltung wird als Container auf dem Raspberry Pi bereitgestellt
```

>[!info]
>Dem Prinzip nach, kann man die Applikation mittels Container auf anderen Systemen aufsetzen und die Nutzer in zwei simple Gruppen unterteilen.

## Einrichtung

### Netzwerkkonfiguration

1. **Statische IP-Adresse einstellen**:  
		    `sudo nano /etc/dhcpcd.conf`                                                  

    `interface eth0 static ip_address=192.168.1.100/24 static routers=192.168.1.1 static domain_name_servers=192.168.1.1`
    
2.    **Neustart des Raspberry Pi um Änderungen wirksam zu machen**:
        
    `sudo reboot`
    

### Benutzer einrichten

1. **Normaler Nutzer**:
        
    `sudo adduser normaluser`
    
2. **Benutzer mit sudo-Rechten**:
    
    `sudo adduser adminuser sudo usermod -aG sudo adminuser`
    

### SSH-Dienst einrichten

1. **SSH-Dienst aktivieren**:
    
    `sudo systemctl enable ssh sudo systemctl start ssh`


2. **Zugriff für den Admin-Benutzer erlauben**: 
    
    `sudo nano /etc/ssh/sshd_config`
    
 3.   **Folgende Zeilen müssen vorhanden sein**:
    
    `PermitRootLogin no AllowUsers adminuser`
    
4.  **Neustart des SSH-Diensten um Änderung wirksam zu machen**:

    `sudo systemctl restart ssh`

### Deployment

**Container erstellen und starten**: Erstellen einer [`Dockerfile`](https://github.com/IlyaSavchenkoIFA22/LF9/blob/09730546ca34c4b824097deb53473452004f4b71/Dockerfile.txt) für den Python-Server und einer [`docker-compose.yml`](https://github.com/IlyaSavchenkoIFA22/LF9/blob/09730546ca34c4b824097deb53473452004f4b71/docker-compose.yml), um den Container zu starten.

#### Deployment mit Docker

    docker build https://github.com/IlyaSavchenkoIFA22/LF9.git -t verandb
    docker run -p 5000:5000 -d verandb

#### Deployment mit Docker Compose

    git clone https://github.com/IlyaSavchenkoIFA22/LF9.git
    docker compose up -d

## Nutzung

Nach der Einrichtung und dem Start des Containers ist der Server über die statische IP-Adresse des Raspberry Pi im lokalen Netzwerk zugänglich. Mit den oben beschriebenen Endpunkten, kann die Applikation zum Verwalten von Todolisten genutzt werden.