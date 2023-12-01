---
title: TOP50 Docker Befehle die du als Entwickler kennen musst
layout: post
cover: assets/images/blog.png
categories:
- entwickler
date: '2023-09-24 21:37:00'
---

Docker ist eine Plattform, die es Entwicklern ermöglicht, Anwendungen und ihre Abhängigkeiten in leichtgewichtigen, isolierten Containern zu verpacken, um eine konsistente Ausführung über verschiedene Umgebungen hinweg zu gewährleisten.  
Diese Container können schnell bereitgestellt, skaliert und mit minimalen Ressourcen betrieben werden.  
Docker hat den Markt wie im Sturm erobert und immer mehr Prozesse und Unternehmen bauen auf Container Lösungen.  
  
Ich laufe immer in das Problem, dass mir gewisse Befehle, wenn ich sie brauche, einfach nicht einfallen.  
Daher habe ich eine Top50 Liste, der (für mich) wichtigsten Befehle zusammengestellt.  
Ich nutze diese Liste schon seid einiger Zeit als eine art quick-reference.  


Bevor wir mit den Befehlen starten, ist es wichtig, den Unterschied zwischen Docker Images und Docker Containern zu verstehen.  
Ein Docker Image setzt sich aus einer Docker Datei und allen notwendigen Abhängigkeiten zusammen.  
Ein Docker Container ist im Wesentlichen eine gestartete Instanz eines Docker Images.  
Es ist jedoch möglich, dass mehrere Container auf der Grundlage desselben Images ausgeführt werden.

__Here we go:__  

`docker run` Startet einen Container aus einem Image.
```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

`docker ps` Zeigt Informationen zu laufenden Containern an.
```bash
docker ps [OPTIONS]
```

`docker images` Zeigt die Liste der verfügbaren Images an.
```bash
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

`docker build` Erstellt ein Docker-Image aus einem Dockerfile.
```bash
docker build [OPTIONS] PATH | URL | -
```

`docker stop` Stoppt einen laufenden Container.
```bash
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

`docker start` Startet einen gestoppten Container.
```bash
docker start [OPTIONS] CONTAINER [CONTAINER...]
```

`docker restart` Startet einen gestoppten Container neu.
```bash
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

`docker exec` Führt einen Befehl in einem laufenden Container aus.
```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

`docker rm` Löscht einen oder mehrere Container.
```bash
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

`docker rmi` Löscht ein oder mehrere Images.
```bash
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

`docker network` Verwaltet Docker-Netzwerke.
```bash
docker network [OPTIONS] COMMAND
```

`docker volume` Verwaltet Docker-Volumes.
```bash
docker volume [OPTIONS] COMMAND
```

`docker logs` Zeigt die Logs eines Containers an.
```bash
docker logs [OPTIONS] CONTAINER
```

`docker inspect` Gibt ausführliche Informationen zu Ressourcen aus.
```bash
docker inspect [OPTIONS] NAME|ID [NAME|ID...]
```

`docker buildx` Erweiterte Funktionen für den Build-Prozess.
```bash
docker buildx [OPTIONS] COMMAND
```

`docker info` Zeigt Systeminformationen an.
```bash
docker info [OPTIONS]
```

`docker login` Authentifiziert einen Benutzer mit einem Docker-Registry.
```bash
docker login [OPTIONS] [SERVER]
```

`docker version` Zeigt die Docker-Versionen für Client und Server an.
```bash
docker version [OPTIONS]
```

`docker save` Speichert ein Image als tar-Archiv.
```bash
docker save [OPTIONS] IMAGE [IMAGE...]
```

`docker load` Lädt ein gespeichertes Image aus einem tar-Archiv.
```bash
docker load [OPTIONS]
```

`docker cp` Kopiert Dateien/Folder zwischen dem Dateisystem des Hosts und des Containers.
```bash
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
```

`docker rename` Benennt einen Container um.
```bash
docker rename [OPTIONS] CONTAINER NEW_NAME
```

`docker pause` Pausiert einen laufenden Container-Prozess.
```bash
docker pause CONTAINER [CONTAINER...]
```

`docker unpause` Setzt die Ausführung eines pausierten Containers fort.
```bash
docker unpause CONTAINER [CONTAINER...]
```

`docker top` Zeigt die laufenden Prozesse in einem Container an.
```bash
docker top CONTAINER [ps OPTIONS]
```

`docker events` Zeigt Docker-Ereignisse in Echtzeit an.
```bash
docker events [OPTIONS]
```

`docker stats` Zeigt Live-Statistiken für Ressourcenverbrauch von Containern an.
```bash
docker stats [OPTIONS] [CONTAINER...]
```

`docker system prune` Entfernt nicht mehr benötigte Daten wie gestoppte Container und unbenutzte Images.
```bash
docker system prune [OPTIONS]
```

`docker-compose` Verwaltet Docker-Anwendungen mit mehreren Containern.
```bash
docker-compose [OPTIONS] COMMAND [ARGS...]
```

`docker-compose up` Startet Dienste gemäß der docker-compose.yml-Konfiguration.
```bash
docker-compose up [OPTIONS] [SERVICE...]
```

`docker-compose down` Stoppt und entfernt Dienste gemäß der docker-compose.yml-Konfiguration.
```bash
docker-compose down [OPTIONS]
```

`docker-compose ps` Zeigt den Status der Dienste in der docker-compose.yml-Konfiguration an.
```bash
docker-compose ps [OPTIONS] [SERVICE...]
```

`docker-compose logs` Zeigt die Logs der Dienste in der docker-compose.yml-Konfiguration an.
```bash
docker-compose logs [OPTIONS] [SERVICE...]
```

`docker-compose exec` Führt Befehle in Diensten der docker-compose.yml-Konfiguration aus.
```bash
docker-compose exec [OPTIONS] SERVICE COMMAND [ARGS...]
```

`docker-compose build` Baut Dienste gemäß der docker-compose.yml-Konfiguration neu.
```bash
docker-compose build [OPTIONS] [SERVICE...]
```

`docker-compose pull` Lädt Docker-Images für die Dienste in der docker-compose.yml-Konfiguration herunter.
```bash
docker-compose pull [SERVICE...]
```

`docker-compose up -d` Startet Dienste im Hintergrund (detach mode) gemäß der docker-compose.yml-Konfiguration.
```bash
docker-compose up -d [SERVICE...]
```

`docker-compose logs -f` Zeigt die Echtzeit-Logs der Dienste in der docker-compose.yml-Konfiguration an.
```bash
docker-compose logs -f [SERVICE...]
```

`docker-compose stop` Stoppt Dienste gemäß der docker-compose.yml-Konfiguration.
```bash
docker-compose stop [SERVICE...]
```

`docker-compose start` Startet zuvor gestoppte Dienste gemäß der docker-compose.yml-Konfiguration.
```bash
docker-compose start [SERVICE...]
```

`docker-compose restart` Startet Dienste in der docker-compose.yml-Konfiguration neu.
```bash
docker-compose restart [SERVICE...]
```

`docker-compose pause` Pausiert Dienste in der docker-compose.yml-Konfiguration.
```bash
docker-compose pause [SERVICE...]
```

`docker-compose unpause` Setzt die Ausführung pausierter Dienste in der docker-compose.yml-Konfiguration fort.
```bash
docker-compose unpause [SERVICE...]
```

`docker-compose down -v` Stoppt und entfernt Dienste und zugehörige Volumes gemäß der docker-compose.yml-Konfiguration.
```bash
docker-compose down -v
```

`docker-compose ps -q` Zeigt die IDs der Container der Dienste in der docker-compose.yml-Konfiguration an.
```bash
docker-compose ps -q [SERVICE...]
```

`docker-compose logs -t` Zeigt die Zeitstempel in den Logs der Dienste in der docker-compose.yml-Konfiguration an.
```bash
docker-compose logs -t [SERVICE...]
```

`docker-compose scale` Ändert die Anzahl der Container für einen Dienst in der docker-compose.yml-Konfiguration.
```bash
docker-compose scale SERVICE=NUM [SERVICE...]
```

`docker-compose config` Überprüft und zeigt die Zusammenführung von docker-compose.yml-Dateien an.
```bash
docker-compose config [OPTIONS]
```

`docker-compose events` Zeigt Echtzeitereignisse von Diensten in der docker-compose.yml-Konfiguration an.
```bash
docker-compose events [SERVICE...]
```