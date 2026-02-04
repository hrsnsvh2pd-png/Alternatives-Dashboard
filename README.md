# Alternatives-Dashboard
Grafana Dashboard auf Grundlage der von vieventlog erzeugten und fortgeschriebenen SQLite-Datenbank viessmann_events.db. 
# Grafana Dashboard Repository

Dieses Repository enthält ein Grafana-Dashboard als Classic-JSON-Datei. Das Dashboard können direkt in eine eigene Grafana-Instanz importiert werden.

Das Dashboard läuft unter Grafana OSS. Grafana OSS ist ein Produkt der Grafana OSS © Grafana Labs und ist lizenziert unter der GNU Affero General Public License v3.0 (AGPLv3). Der vollständige Quellcode von Grafana ist öffentlich verfügbar unter:

https://github.com/grafana/grafana

Dieses Produkt wird für das Alternative Dashboard unverändert genutzt. Somit ist eine private Nutzung ohne Offenlegungspflichten möglich. Die Lizenzbedingungen sind unter

https://www.gnu.org/licenses/agpl-3.0.txt

einsehbar.

Jeder Benutzer muss sich eine eigene Grafana-Anwendung herunterladen und installeren. 

Das für die Anbindung der viessmann_events-Datenbank erforderlich SQLite-Plug in ist Bestandteil der Grafana-Standard-Distrubution und wirft insoweit keine Lizenzprobleme auf.

Das Grafana Dashboard setzt auf der viessmann_events.db auf, die von der vieventlog-Anwendung erzeugt und fortgeschrieben wird.  vieventlog ist ein Produkt von mschneider82. Der vollständige Quellcode von vieventlog ist öffentlich verfügbar unter:

https://github.com/mschneider82/vieventlog/releases

vieventlog ist unter der MIT-Lizenz lizenziert. Die Lizenzbedingungen sind unter 

https://github.com/mschneider82/vieventlog/blob/main/LICENSE

einsehbar. 

----

## 1. Vorbemerkung



## 2. Installation von Grafana

Das Grafana-Dashboard läuft bei mir auf meiner Synology-Datenstation in einem Docker-Container. Nur diese Installation habe ich getestet. Die Installationshinweise für die anderen Umgebungen sind automatisiert erstellt worden und nicht getestet worden. 

### Windows
1. Lade den Windows-Installer von der offiziellen Grafana-Website herunter: [https://grafana.com/grafana/download](https://grafana.com/grafana/download)
2. Installiere Grafana wie gewohnt.
3. Starte Grafana über das Startmenü. Standardmäßig läuft Grafana unter `http://localhost:3000`.
4. Standard-Login: Benutzername: `admin`, Passwort: `admin`

**Datenbank-Verwaltung:**
- Viessmann-Datenbank: `viessmann_events.db`
- Kostenloses Tool: [DB Browser for SQLite](https://sqlitebrowser.org/)

### macOS (Apple)
1. Mit Homebrew:
bash
brew install grafana
brew services start grafana
2.	Alternativ: Download des macOS-Pakets von der Grafana-Website.
3.	Standard-Login: Benutzername: admin, Passwort: admin
4.  Datenbank-Verwaltung: Viessmann-Datenbank viessmann_events.db, Tool: DB Browser for SQLite

### Linux / Unix
1.	Debian/Ubuntu:
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update
sudo apt-get install grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
2.	Standard-Login: admin/admin
3.  Datenbank-Verwaltung: Viessmann-Datenbank viessmann_events.db, Tool: DB Browser for SQLite

### Raspberry Pi / ARM
1.	ARM-Paket herunterladen: https://grafana.com/grafana/download￼
2.	Installation per .deb:
    sudo dpkg -i grafana_<version>_armhf.deb
    sudo systemctl start grafana-server
    sudo systemctl enable grafana-server
3.	Standard-Login: admin/admin
4.  Datenbank-Verwaltung: Viessmann-Datenbank viessmann_events.db, Tool: DB Browser for SQLite

### Synology NAS
1. 	Installiere das Docker-Paket über das Synology-Paketzentrum (Image: z.B. grafana/grafana: latest

    ![image](https://github.com/user-attachments/assets/9e0e7734-56cf-428a-95be-cf6b4809d76b)


2. Grafana-Container erstellen

   ![image](https://github.com/user-attachments/assets/fcc33ea7-6f8f-4b8e-8ff5-3951fe06a183)


3.  Grafana über Browser starten : http://deine_synology_ip:öffentlicher Port gem. Conatiner (z.B. 3030)

    Standard-Login:
    User: admin
    Passwort: admin
    
4.  SQLite-Plugin für Grafana installieren (Main Menü --> Connections --> Datasources
    frser-sqlite-datasource laden
    frser-sqlite-datasource öffnen
    
6.  Pfad zur viessmann.events.db eintragen
   
    Achtung: Hier ist der Container-Pfad einzutragen, NICHT der physische Pfad, wo die Datenbank auf dee Synology tatsächlich
    liegt (vgl. 2. Grafana-Container erstelln). Grafana läuft im Container, dieser kennt nur die Volumes im Container.
    
    Der Path hängt davon ab, in welchem Verzeichnis sqlite im  Grafana-Container unter Volume eingerichtet (gemountet) wurde: Bei mir      auf der Synology (vgl. 2. Grafana-Container erstellen)

    /volume1/docker/vieventlog/db:/sqlite:rw

    Erklärung: 

    /volume1/docker/vieventlog/db   Ist das Verzeichnis, in dem die Datenbank physisch auf der Synology liegt (volume1 markiert dabei      nur das rout-Verzeichnis) 

    /sqlite   Ist das logische Verzeichnis, über das der Container auf die Datenbank zugreift. 

    rw   Ist die Berechtigung für die Datenbank (read/write) 

    ![image](https://github.com/user-attachments/assets/fa652483-a9a1-4bbc-b579-db98bfba9fe1)

7.  Aufruf
    Das Dashboards kann  auf zwei verschiedene Weisen aufgerufen werden:

    Regelfall: Aufruf im Kiosk-Mode
    http://my_synology_ip:3030/d/viessmann-all-in-one?orgId=1&kiosk
    Nutzer: individuell eingerichteter User

    Bei diesem Aufruf sieht man nichts von Grafana; Grafana funktioniert nur als Anzeige-Engine. Der Nutzer sollte nur
    Leserechte haben. 

    Aufruf im Edit-Mode
    http://my_synology_ip:3030
    Nutzer:  admin (oder alternativer Nutzer mit Admin-Rechten

    Diese Aufruf (notwendig mit Admin-Rechten) ermöglicht eine Veränderung des Dashboards. Die Änderungen sollten sicherheitshalber immer an einer Kopie erfolgen. 
 
    Achtung: Der Port 3030 ist der externe Port, den ich im Grafana-Container eingestellt habe (Port-Mapping). Grafana selbst erwartet     den Zugriff über Port 3000; der ist bei mir aberschon anders belegt. Auch dieses Port Mapping erfolgt auf der Synology über den
    Container Manager. 


Datenbank-Verwaltung: Viessmann-Datenbank z.B. /volume1/docker/viessmann_events.db, 
Tool: SQLite Browser auf Client oder coleifer/sql-web (mein Tool)


##  3. Logische views in der viessmann_events.db erzeugen
1.  Datenbank Verwaltungssoftware installieren (z.B. DB Browser for SQLite](https://sqlitebrowser.org/)
2.  Datenbank öffnen
3.  Beide logische Views auf die Tabelle temperature_snapshots erzeugen:
    a) temperature_snapshots_stat:

    ![image](https://github.com/user-attachments/assets/4bbe46ce-44f6-4935-b7d4-bedb31dd03a2)



    CREATE VIEW IF NOT EXISTS 'temperature_snapshots_stat' as
    SELECT datetime  (
    substr(timestamp, 1, 19),'localtime') as 'timestamp_mez',
    thermal_power * 1000 / 60 * sample_interval as 'Wärmeleistung',
    compressor_power / 60 * sample_interval as 'elektrische_Leistung',
    thermal_power * 1000 / compressor_power as 'COP_calc',
    time('00:00', '+' || (compressor_active * sample_interval) || ' minutes') as 'Laufzeit',
    *
    from temperature_snapshots order by installation_id desc,
    datetime(substr(timestamp, 1, 19), 'localtime' )

    b) temperature_snapshots_takte_heute:

    ![image](https://github.com/user-attachments/assets/4e3edeef-7160-4080-91e3-63389c410421)



    CREATE VIEW IF NOT EXISTS 'temperature_snapshots_takte_heute' as
    SELECT datetime  (
    substr(timestamp, 1, 19), 'localtime') as timestamp_mez,
    installation_id,
    Compressor_active,
    LAG (
    compressor_active) over(order by installation_id desc,  datetime (
    substr(timestamp, 1, 19),
    'localtime') desc) as next_period,
    LAG (
    compressor_active) over(order by installation_id desc,  datetime (
    substr(timestamp, 1, 19),
    'localtime') desc )


##  4. Import des Dashboards
1.	Lade die Dashboard-JSON-Datei herunter: Viessmann Wärmepumpe – All-in-One Dashboard.json￼
2.	Öffne Grafana → Dashboard → Import → Upload JSON file
3.	Datei auswählen -> Importieren

Hinweis: Die Link-Adressen zu
- Events (vieventlog-Events)
- Kältekreislauf (vieventlog-Dashboard)
- Datenbank (Datenbank-Verwaltungstool)
- VICare (homeassistent)

müssen selbst händisch eingfügt werden. sonein Link nicht genutzt werden soll, kann er einfach gelöscht werden; neue Links können beliebig hinzugefügt werden.

##  5. Lizenz
Die in diesem Repository enthaltenen Grafana-Dashboard-JSON-Dateien
stehen unter der **Creative Commons Attribution 4.0 International
(CC BY 4.0)** Lizenz.

Kurz gesagt:
- Nutzung, Weitergabe und Anpassung sind erlaubt
- Namensnennung des Autors ist erforderlich

Vollständiger Lizenztext:
https://creativecommons.org/licenses/by/4.0/legalcode
