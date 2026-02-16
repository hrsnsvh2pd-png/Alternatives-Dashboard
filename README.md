# Alternatives-Dashboard
Grafana Dashboard auf Grundlage der von vieventlog erzeugten und fortgeschriebenen SQLite-Datenbank viessmann_events.db. 
# Grafana Dashboard Repository
Dieses Repository enthält ein Grafana-Dashboard als Classic-JSON-Datei.
Das Dashboard kann direkt in eine eigene Grafana-Instanz importiert werden.

Grafana
Das Dashboard basiert auf Grafana OSS, einem Produkt von Grafana Labs, lizenziert unter der GNU Affero General Public License v3.0 (AGPLv3).

Der vollständige Quellcode von Grafana ist verfügbar unter:
https://github.com/grafana/grafana

Die Lizenzbedingungen sind einsehbar unter:
https://www.gnu.org/licenses/agpl-3.0.txt

Dieses Dashboard nutzt Grafana unverändert, sodass eine private Nutzung ohne Offenlegungspflichten möglich ist.
Jeder Benutzer muss eine eigene Grafana-Instanz herunterladen und installieren.

Das für die Anbindung der viessmann_events-Datenbank erforderliche SQLite-Plugin ist Bestandteil der Grafana-Standard-Distribution und verursacht keine zusätzlichen Lizenzanforderungen.

vieventlog
Das Dashboard greift auf die Datei viessmann_events.db zu, die durch die Software vieventlog erzeugt und fortgeschrieben wird.

vieventlog ist ein Open-Source-Projekt von Matthias Schneider.
Ein besonderer Dank gilt ihm für die Entwicklung und Bereitstellung dieses Projekts für die Community.

Der vollständige Quellcode ist verfügbar unter:
https://github.com/mschneider82/vieventlog/releases

vieventlog ist unter der MIT-Lizenz lizenziert.
Lizenzbedingungen:
https://github.com/mschneider82/vieventlog/blob/main/LICENSE

Dieses Dashboard ist ein eigenständiges Community-Projekt und steht in keiner offiziellen Verbindung zu vieventlog oder dessen Autor.


## 1. Vorbemerkung
Matthias Schneider hat mit sehr viel Herzblut vientlog geschaffen – und mit dieser Anwendung die zentrale Lücke nach Abschaltung von VIGuide für Nutzer geschlossen. Es ist sicherlich auch ein Stück „Berufskrankheit“, die mich dazu veranlasst hat, ein alternatives Dashboard auf Grundlage von vieventlog (genau: der durch vieventlog fortgeschriebenen  Datenbank viessmann_events.db) zu schaffen. Mich treibt dabei, bei einer derartig herausragenden Leistung zu versuchen, das Maximum aus dieser Anwendung herauszuholen. 
Dies war meine Motivation. 
Die wesentlichen Änderungen betreffen neben einigen zusätzlichen für meine Bedürfnisse erforderlichen Feldern (insbes. Anzahl Kompressorstarts des lfd. Tages,Tage ohne Kompressorlaufzeit (ich nutze Solarthermie), Stromverbrauch pro Tag) insbesondere 3 wesentliche Punkte:
a) Integration der Events-Timeline in ein Gesamt-Dashboard mit Sensor-/Statistikwerten und Betriebsdaten/-verläufen 
b) Alertsystem mittels Ampellogik über Schwellwerte wo immer möglich und sinnvoll
c) ökonomische Effizienzeinschätzung mittels Ampellogik über Schwellwerte
    Der Vergleich der jährlichen laufenden Mehr-/Minderkosten einer Wärmepumpe wird wesentlich durch das    
    Verhältnis Strom- zu Gaspreis pro KW/h beeinflusst. Und dies auch, wenn man nicht wie ich, den 
    vorhandenen Gasbrenner als externe Heizquelle weiter nutzt. Die Gasanbieter rechnen den Verbrauch von 
    Kubikmeter in KW/h um. Abgerechnet wird am Zähler, sowohl Strom- als auch Gaspreis wird je KW/h berechnet;
    eine KW/h ist kostenmäßig eine KW/h. 
    Damit ist die Jahreszahl (Kalenderjahr) ein sehr guter Indikator für die ökonomische Effizienz einer 
    Wärmepumpe: Liegt die (ehrliche) JAZ über dem Arbeitspreis-Verhältnis von Strom- zu Gas (bei uns ab 
    1.1.2026 = 2,98 ist das ein sehr guter Indikator dafür, dass die Wärmepumpe wahrscheinlich kostengünstiger 
    als ein Gasbrenner ist.
d) Aus meiner beruflichen Erfahrung weiß ich, dass eigentlich jeder Nutzer seine eigenen Vorstellungen bezüglich Statistik und Dashboard hat. Deshalb habe ich das alternative Dashboard auf der Grafana-Plattform (OSS) entwickelt. Jeder Nutzer kann es auf dieser Plattform nach seinen Wünschen und Vorstellungn individuell ändern bzw. anpassen.

Ich habe dieses Dashboard nach meinen Vorstellungen mit den aus meiner Installation verfügbaren Daten entwickelt. Es setzt auf viebentlog (genauer auf der Datenbank viessmann_events.db) auf, setzt also zwingend voraus, dass vieventlog läuft und die Datenbank fortschreibt. Dem Sinn einer Community entsprechend möchte ich dieses Dashboard allen Mitgliedern anbieten. Matthias ist über dieses Vorgehen vorab informiert worden und hat empfohlen, das Dashboard ebenfalls auf GitHub zu veröffentlichen, um es mit vieventlog verlinken zu können. 


## 2. Mein Use Case

<img width="1330" height="940" alt="image" src="https://github.com/user-attachments/assets/97ad467d-27cf-46ca-91f8-932a08db7a9f" />



Das Dashboard ist von mir als Aufsatz auf vieventlog entwickelt worden und dabei so konzipiert, dass es als Einstieg in vieventlog fungieren kann. Der Aufruf dabei über localhost:3000 (Windows) bzw. über Synology_IP:3000 (sollte der Port 3000 bereits auf der Datenstation verwendet werden, muss im Grafana-Docker-Container ein Portmapping vorgenommen werden). Ich nutze es permanent als primäres Informationssystem über meine Wärmepumpe. Vieventlog selbst ist per Link aus dem Dashboard heraus errreichbar (separeate Linkbuttons für Events und Dashboard/Kältekreislauf). 

Vieventlog fungiert für mich unverzichtbar als Datenbank Engine (API-Calls → Fortschreiben der viessmann_events.db, Events undSnapshots) sowie punktuell für vertiefte und Detail-Informationen über Events, Sensorwerte, Temperaturwerte, Betriebsdaten und Statitikwerte. Es wird ohnehin zwingend zur Einstellung der API-Call-Intervalle (Event und Snapshots) benötigt. Unverzichtbar ist für mich auußerdem die Kältekreislauf-Visualisierung und Einordnung des Heizkreis-Vorlauf-Wertes bei herrschender Auißentemperaturn in der Heizkurve. Und da das Dashboard ausschließlich die Wärmepumpe im Fokus hat, ist es ohnehin für die Information bezüglich Smart Client-Geräte, Vitoevent-Systeme und Vitocharge-Geräte unabdingbar.

Desweiteren nutze ich VICare (Homeassistant) als Alternative zur VICare-App (auf Windows-PC) - ins besondere auch, wenn die VICare-App nicht verfügbar ist und ich Einstllungen bei meiner Wärmepumpe ändern muss. 

Überaus hilfreich - insbesondere, wenn man Änderungen am Dashboard vornehemen will - hat sich auch ein Zugriff auf die Datenbank (viessmann_events.db) erwiesen. Ich nutze für den Zugriff ein Dataenbankmanagement-Tool, das ebenfalls auf meiner Synology-Daenstation in einem Docker-Container läuft (coleifer/sql-web). Dieses Tool iat aus dem Dashboard über einen separaten Button verlinkt. Unter Windows nutze ich das ebenfalls kostenlose Tool DB-Browser für SQLite, das auch für macOS und die meisten Versionen von Linux und Unix verfügbar ist (https://sqlitebrowser.org/). 

Alle Links können nachI nstallation über die Grafana-Oberfläche angepasst oder ggf. gelöscht werden. Selbstverständlich können auch neue Links ergänzt werden. 

## 3. Vorbereitende Massnahmen: Erzeugen von 3 logischen Views in der Datenbank viessmann_events.db

Um das über den bereitgestellten JSON gelieferte Dashboard nutzen zu können, müssen 3 logische Views in der Datenbank viessmann_events.db erzeugt werden. Dies kann über eines der vorstehend genannten Datenbankmanagement-Tools ganz einfach über die Eingabe und Ausführung der nachsehenden SQL-Statements erfolgen. 

a) temperature_snapshot.stat

Diese View dient der Umstellung der Timestamps auf ISO 8601 und auf 'localtime', da ich andernfalls nicht die erforderliche Zeitzonenproblematik (arbeiten mit deutscher Zeit auch in einer anderen Zeitzone) hinbekommen habe und vorbereitende Brechnungen für Stromverbrauch, Wärmeleistung und Laufzeit.

 ![image](https://github.com/user-attachments/assets/4bbe46ce-44f6-4935-b7d4-bedb31dd03a2)

 Zum Kopieren:

CREATE VIEW IF NOT EXISTS 'temperature_snapshots_stat' as 
      SELECT datetime(substr(timestamp, 1, 19), 'localtime') 
      as 'timestamp_mez', 
      thermal_power * 1000 / 60 * sample_interval 
      as 'Wärmeleistung',
      compressor_power / 60 * sample_interval 
      as 'elektrische_Leistung',
      thermal_power * 1000 / compressor_power 
      as  'COP_calc',
      time('00:00', '+' || (compressor_active * sample_interval) || ' minutes') 
      as 'Laufzeit', 
      * 
      FROM temperature_snapshots 
      ORDER BY installation_id desc, 
      datetime(substr(timestamp, 1, 19), 'localtime' ) desc

b) temperature_snapshots_takte heute
Diese View dient der Erkennung von Ein-/Ausschaltung des Kompressors zur Berechnung der Kompressorstarts des laufenden Tages ("Vorlesen" des Kompressorzustandes im zeitlich nachgelegerten Datensatz und übertragung des kommenden Status in den gerade berarbeiteten Datensatz)

![image](https://github.com/user-attachments/assets/4e3edeef-7160-4080-91e3-63389c410421)
Zum Kopieren:

CREATE VIEW IF NOT EXISTS
 'temperature_snapshots_takte_heute' 
      as SELECT datetime ( substr(timestamp, 1, 19), 'localtime') 
      as timestamp_mez, 
      installation_id, 
      Compressor_active, 
      LAG ( compressor_active) over(order by installation_id desc, datetime ( substr(timestamp, 1, 19), 'localtime') desc) 
      as next_period, 
      LAG ( compressor_active) over(order by installation_id desc, 
      datetime ( substr(timestamp, 1, 19), 'localtime') desc )

c) event_betriebszustand_timeline
Aufbereitung der Events für die Timeline-Darstellung (Eintragung des Ende-Zeitpunkts eines Events und Mappingeinzelner Events auf bestimmte Betriebszustände

<img width="1160" height="651" alt="image" src="https://github.com/user-attachments/assets/759db04e-1501-4923-9a44-3cf81f917172" />

Zum Kopieren: 

CREATE VIEW 'event_betriebszustand_timeline' as

SELECT
    strftime   (
  '%F %T',
  event_timestamp) AS event_timestamp_start,
  -- Endzeit = vorherige Zeile (jüngerer Event) oder NOW für die erste Zeile
    CASE 
        WHEN LAG(event_timestamp) OVER (
                 PARTITION BY installation_id
                 ORDER BY event_timestamp DESC, error_code ASC
             ) IS NULL
        THEN strftime('%F %T', 'now')
        ELSE strftime(
                 '%F %T',
                 LAG(event_timestamp) OVER (
                     PARTITION BY installation_id
                     ORDER BY event_timestamp DESC, error_code ASC
                 )
             )
    END AS event_timestamp_end,
  strftime('%F %T', created_at) AS created_at,
  error_code,
  human_readable,
  CASE                                                                       
          WHEN error_code in ('S.118')                       THEN 'S.125'   
          WHEN error_code in ('S.127', 'S.135', 'S.176')     THEN 'S.128     
  END
  FROM events
WHERE installation_id = ${anlage_id}
  AND active = 1 and
  error_code <> 'S.187'    

  
## 4. Installation von Grafana

Das Grafana-Dashboard läuft bei mir ebenso wie vieventlog auf meiner Synology-Datenstation in einem Docker-Container. Diese Installation habe ich naturgeäß intensiv getestet. Ebenfalls habe ich Grafana und das Dashboard auf meinem Windowas-PC unter Windows 11 installiert. Ich habe aufgrund der Verbreitung im beiligenden pdf-Dokuement die Installation unter Windows ausführlich beeschrieben. Die Grafana-Einrichtung und das Laden des Dashboards läuft unter Windows und im Synology-Docker-Contaimer identisch ab. Die Installationshinweise für die anderen Umgebungen sind automatisiert erstellt worden und nicht getestet worden. 

Die ausführliche Installation unter Windows ist auch auf den Seten 14 - 25 in beiliegenden pdf-Dokument beschrieben

### Windows
1. Lade den Windows-Installer von der offiziellen Grafana-Website herunter: (https://grafana.com/get/)

Achtung: Auf der Downloadseite sind mehrere Grafana-Versionen verfügbar (Cloud und OSS). Nach klicken auf OSS erscheint eine Auswahl Enterprise und OSS. Bitte die kostenlose OSS-Version verwenden,es sein denn, jemand möchte eine andere kostenpflichtige Version erwerben. 

Hinweis: Grafana läuft in der OSS-Version ausschließlich lokal. Nach meinen Informationen werden keine Daten oder Nutzungsinformationen in irgend einer Form an GrafanaLabs übertragen.

2. Installiere Grafana wie gewohnt.
3. Starte grafana-server.exe
   --> dazu mit der rechten Maustaste unter C:\Programme\GrafanaLabs\grafana\bin die Datei grafana-server.exe klicken und diese unbedingt als Administrator ausführen
4. Starte Grafana über das Startmenü. Standardmäßig läuft Grafana unter `http://localhost:3000`.

<img width="517" height="513" alt="image" src="https://github.com/user-attachments/assets/7eb93da9-7ddd-4b5c-8d7b-c01760623a75" />


5. Standard-Login: Benutzername: `admin`, Passwort: `admin`
   Nach dem erstmaligen Aufruf muss das admin-Passowrt geändert werden.


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

Hinweis: Nach dem ersten Aufruf muss das Adinistrator-Passwort geändert werden.


Datenbank-Verwaltung: Viessmann-Datenbank z.B. /volume1/docker/viessmann_events.db, 
Tool: SQLite Browser auf Client oder coleifer/sql-web (mein Tool)


##  5. Import des Dashboards
1.	Lade die Dashboard-JSON-Datei herunter: Viessmann Wärmepumpe – All-in-One Dashboard.json
2.	Windows: Starte grafaner-server.exe als Administrator
3.	Starte Grafana
4.	Auswahl → Dashboard
5.	Einrichten der Datasource (Verbindung zur viessmann_events.db)
    Unter Connections --> Datasource auf Add "new connection klicken"
    <img width="301" height="398" alt="image" src="https://github.com/user-attachments/assets/93a2f160-40e5-4f43-9e2d-5ea274a1182a" />

    auf auf "new connection" klicken und SQLite eingeben
    <img width="1344" height="471" alt="image" src="https://github.com/user-attachments/assets/53081b3c-04e1-4324-ae69-d4d483ba31ce" />
    
    auf "SQLite" klicken
  	<img width="510" height="172" alt="image" src="https://github.com/user-attachments/assets/124acfd2-5367-47e5-81b2-9c4c99a57753" />

    auf "Install" klicken
  	<img width="1343" height="294" alt="image" src="https://github.com/user-attachments/assets/de0903e2-fa01-4782-ba6d-fe33baa4024f" />

    auf "Add new Datasource klicken
  	<img width="1343" height="193" alt="image" src="https://github.com/user-attachments/assets/ba589a78-4874-4bb5-ae9f-358d8e8d0655" />


    a) Pfad zur Datenbank eintragen
          → wo bei Euch die viesmann_events.db
               physisch gespeichert ist
           → C:\ ….\viesmann_events.db
                (Windows-Verzeichnis-Schreibweise)
           → alle übrigen Felder unverändert lassen
          
    b) auf Save & Test klicken
  	<img width="1015" height="723" alt="image" src="https://github.com/user-attachments/assets/0ab8a0ba-33b5-4b19-992c-da4c34334914" />

6. Dashboard laden und anlegen
   auf "Dashboard" klicken
   <img width="300" height="398" alt="image" src="https://github.com/user-attachments/assets/06e97afc-96da-49b1-a6ab-eb376fb94208" />

    auf "Import Dashboard" klicken
   <img width="1346" height="704" alt="image" src="https://github.com/user-attachments/assets/35a5c9d3-7dc4-42ac-98c5-9aaf3b2ba0c6" />
    
   auf "Upload JSON file" klicken
   <img width="638" height="648" alt="image" src="https://github.com/user-attachments/assets/8eaf026f-04a6-49d3-8e28-3c509f5569d4" />

    aus GitHub heruntergeladenen JSON auswählen
    
    Datenbank auswählen
   <img width="622" height="529" alt="image" src="https://github.com/user-attachments/assets/bd134ff4-5eb2-4e5a-b9bc-f63ab7270fc6" />

    auf "Frser-SQLite-datasource" klicken
    <img width="611" height="232" alt="image" src="https://github.com/user-attachments/assets/fa665997-7697-4507-afa4-7c4aba5184be" />

    auf "Import" klicken"
    <img width="613" height="126" alt="image" src="https://github.com/user-attachments/assets/4ef474c6-8721-484a-9da0-efa17b18c260" />
    

   nach dem Import  des JSON wird das Dashboard zunächst noch ohne Daten geladen


7. Anlagen in der Variable einstellen
    ingeöffneten Dashboard auf "Edit" (oben rechts) klicken
   <img width="262" height="88" alt="image" src="https://github.com/user-attachments/assets/67dab411-faa9-4e87-86b0-d858f5595dbf" />

   auf "Settings" klicken
   <img width="373" height="50" alt="image" src="https://github.com/user-attachments/assets/4ad7636b-0b79-4c14-acea-51c95d28c330" />

   auf "Variables" klicken
   <img width="689" height="183" alt="image" src="https://github.com/user-attachments/assets/2ae971ee-9311-44af-9b6c-9f94b2571206" />

   auf "anlage_id" klicken
   <img width="184" height="195" alt="image" src="https://github.com/user-attachments/assets/482d691d-e856-4b36-83d3-269abf881e60" />

   in Query eingeben:
   select installation_id from temperature_snapshots  
   group by  installation_id

   auf "Save Dashboard" klicken
   <img width="1338" height="695" alt="image" src="https://github.com/user-attachments/assets/aed2897e-8139-46d6-b934-a335f498d290" />

   zurück zum Dashboard
   Anlage auswählen
   <img width="174" height="26" alt="image" src="https://github.com/user-attachments/assets/cd7c43a6-a682-4983-9b37-0566f530d1a1" />


    Jetzt wird das Dashboard mit den in Deiner Datenbank vorhandenen Werten gefüllt
   

8. Links einstellen
   auf Edit --> Settings klicken
   auf Links klicken
   <img width="703" height="99" alt="image" src="https://github.com/user-attachments/assets/c25bdf79-535c-467a-a826-c5617e1300d6" />

   Links einstellen 
   <img width="1296" height="370" alt="image" src="https://github.com/user-attachments/assets/87b77771-deec-4723-b8ce-234901f0f23e" />

   Hinweis:
   Die Link-Adressen zu
    - Events (vieventlog-Events)
    - Kältekreislauf (vieventlog-Dashboard)
    - Datenbank (Datenbank-Verwaltungstool)
    - VICare (homeassistent)

    müssen selbst händisch eingfügt werden. sonein Link nicht genutzt werden soll, kann er einfach gelöscht werden; neue Links können beliebig hinzugefügt werden.

9. Übrige Variable einstellen
   Korrekturfaktor Strom
   Start Wirtschaftsjahr
   Zeitzone einstellen
   gewünschtes Intervall auswählen

10. Nutzer und Berechtigungen anlegen

    Grafana Haupt-Menü öffnen
    <img width="582" height="58" alt="image" src="https://github.com/user-attachments/assets/bc40e062-db82-4266-8c6d-c819082bd034" />

    auf User und Access klicken und User anlegen
    <img width="296" height="527" alt="image" src="https://github.com/user-attachments/assets/26fd6102-ccd7-41d2-81b1-f12151ccb386" />
    

##  6.Lizenz und Haftungsbestimmungen / License & Disclaimer
Deutschsprachige Version:

6.1 Nutzung von Grafana

Dieses Dashboard wurde für die Nutzung mit Grafana OSS entwickelt, einem Produkt der Grafana Labs.

Grafana OSS ist lizenziert unter der
GNU Affero General Public License v3.0 (AGPLv3).

Der Quellcode ist öffentlich verfügbar unter:
https://github.com/grafana/grafana

Die Lizenzbedingungen sind abrufbar unter:
https://www.gnu.org/licenses/agpl-3.0.txt

Dieses Repository enthält keine modifizierte Version von Grafana.
Jeder Nutzer ist verpflichtet, eine eigene Grafana-Installation bereitzustellen.

Eine Verpflichtung zur Offenlegung eigener Anpassungen entsteht nur im Rahmen der AGPL-Bestimmungen und betrifft ausschließlich Modifikationen an der AGPL-lizenzierten Software selbst.

6.2 Abhängigkeit von vieventlog

Das Dashboard greift auf die Datei viessmann_events.db zu, die durch die Software
vieventlog erzeugt und fortgeschrieben wird.

vieventlog ist ein Open-Source-Projekt von Matthias Schneider.

Der vollständige Quellcode ist verfügbar unter:
https://github.com/mschneider82/vieventlog

vieventlog steht unter der MIT License.

Dieses Projekt ist unabhängig von vieventlog und steht in keiner geschäftlichen oder organisatorischen Verbindung zu dessen Autor.

6.3 Lizenz dieses Repositories

Die in diesem Repository enthaltenen Grafana-Dashboard-JSON-Dateien stehen unter der
Creative Commons Attribution 4.0 International (CC BY 4.0).

Namensnennung des Urhebers ist erforderlich.

© 2026 Hans-Hermann Gröger

6.4 Haftungsausschluss

Die Bereitstellung der Dashboard-Dateien erfolgt unentgeltlich.

Eine Haftung für Sach- oder Rechtsmängel ist ausgeschlossen, sofern nicht Vorsatz oder grobe Fahrlässigkeit vorliegt.

Für Schäden, die aus der Nutzung oder Nichtnutzung der bereitgestellten Inhalte entstehen, wird – außer in Fällen zwingender gesetzlicher Haftung – keine Haftung übernommen.

Der Nutzer ist selbst verantwortlich für:
- Installation
- Konfiguration
- Datensicherung
- Systemsicherheit

6.5 Kein Support

Ein Anspruch auf Support, Wartung oder Weiterentwicklung besteht nicht.

6.6 Keine Verbindung zu Grafana Labs

Dieses Projekt steht in keiner Verbindung zu Grafana Labs und wird von diesem Unternehmen weder unterstützt noch zertifiziert.

Alle Produkt- und Markennamen sind Eigentum ihrer jeweiligen Inhaber.

Englischsprachige Version:

1. Grafana Dependency

This dashboard was developed for use with Grafana OSS, a product of Grafana Labs.

Grafana OSS is licensed under the
GNU Affero General Public License v3.0 (AGPLv3).

The full source code of Grafana is available at:
https://github.com/grafana/grafana

The AGPLv3 license text is available at:
https://www.gnu.org/licenses/agpl-3.0.txt

This repository does not contain, distribute, or modify Grafana itself.
Users must install and maintain their own Grafana instance.

Under the AGPL, disclosure obligations apply only to modifications of AGPL-licensed software. This repository contains configuration and dashboard files only and does not modify Grafana.

2. Database Dependency

The dashboard reads data from the file viessmann_events.db, which is generated and maintained by
vieventlog.

vieventlog is an open-source project developed by Matthias Schneider.

The source code is available at:
https://github.com/mschneider82/vieventlog

vieventlog is licensed under the MIT License.

This project is independent from vieventlog and is not affiliated with its author.

3. License of This Repository

All Grafana dashboard JSON files contained in this repository are licensed under:

Creative Commons Attribution 4.0 International (CC BY 4.0)

You are free to:
- Use
- Share
- Modify
- Redistribute

Attribution to the original author is required.

Full license text:
https://creativecommons.org/licenses/by/4.0/legalcode

© 2026 Hans-Hermann Gröger

4. Disclaimer

THE DASHBOARD FILES ARE PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NON-INFRINGEMENT.

TO THE MAXIMUM EXTENT PERMITTED BY APPLICABLE LAW, THE AUTHOR SHALL NOT BE LIABLE FOR ANY CLAIM, DAMAGES, OR OTHER LIABILITY, WHETHER IN CONTRACT, TORT, OR OTHERWISE, ARISING FROM OR IN CONNECTION WITH THE SOFTWARE OR ITS USE.

The user is solely responsible for:
- Installation
- Configuration
- Data backup
- System security

Compliance with applicable laws and licenses

5. No Affiliation

This project is not affiliated with, endorsed by, sponsored by, or otherwise associated with Grafana Labs.

All product names, trademarks, and registered trademarks mentioned are the property of their respective owners.
