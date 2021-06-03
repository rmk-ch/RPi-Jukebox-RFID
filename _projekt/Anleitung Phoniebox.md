# Anleitung Phoniebox
<img src="Mechanik/phoniebox-final.png" alt="Phoniebox" width="100%" />

## Beschreibung
- Abspielen von vielen Quellen:
  - Spotify
  - Youtube
  - MP3 ab SD-Karte
- Sound wird mit Aufsetzen eines Tonie gestartet
  - Tonie hat Seriennummer, wird über Webinterface mit Aktion/Sound verknüpft
- Stopp mit Tonie entfernen oder über Play/Pause Knopf (Lauttärkeregler drücken)
- Lautstärkeschritte, maximale Lautstärke im Webinterface einstellbar
- Ausschalten: Einschaltknopf 2-3 Sekunden drücken
- Augenbrauen leuchten, sobald Linux gestartet bis ausgeschaltet
- Zunge leuchtet, wenn Musikdienst bereit ist (bis System ausgeschaltet)
- Aufladen des Akkus und gleichzeitiger Betrieb ist nicht möglich
- Empfehlung: Konfiguriert euch einen NFC-Chip, der die IP-Adresse vorliest
- Hochladen von MP3s auf SD-Karte über Windows-Freigabe (Benutzername/Passwort gleich wie Kommandzeilenzugang) oder Webinterface
- Viel Spass mitm Chasperli!

## Webinterface
1. Herausfinden, welche IP-Adresse die Phoniebox hat. Möglichkeiten:
    - Router-Einstellungen (Clients suchen)
    - Suchen mit Windows-Tools (z.B. Advanced IP Scanner)
    - Mit RFID Tag, der IP-Adresse vorliest (muss aber zuerst manuell konfiguriert werden!)
2. IP-Adresse in Browser eingeben (z.B. 192.168.254.104)
3. Webinterface öffnest sich für Konfiguration und Steuerung


## Spotify Zugangsdaten eingeben
1. Phoniebox abschalten
2. Mopidy ggn. Spotify authentifizieren (client_id, client_secret anfordern): https://mopidy.com/ext/spotify/#authentication
3. SD-Karte aus Raspi entfernen und in PC einstecken
4. Datei mopidy.conf von SD-Karte im Editor (z.B. Notepad++, nicht mit Windows Notepad) öffnen und folgende Zeilen anpassen:
    ```
    [spotify]
    username = BENUTZERNAME
    password = PASSWORT
    client_id = ... client_id Wert von mopidy.com ...
    client_secret = ... client_secret Wert von mopidy.com ...
    ```
    Benutzername und PW muss von Spotify sein, also nicht Facebook.
5. Speichern (unbedingt mit UNIX-Zeilenende, nicht mit Windows-Zeilenende!)
6. SD-Karte wieder in Raspi einsetzen und starten
7. Falls nicht funktioniert, nochmals überprüfen, ob UNIX-Zeilenenden gespeichert wurde


## Neues WLAN konfigurieren
1. Wenn im WLAN eingeloggt und Webinterface verfügbar, über Webinterface unter Einstellungen neue SSID und Passwort speichern.
2. Über SD-Karte
    1. Phoniebox abschalten
    2. SD-Karte aus Raspi entfernen und in PC einstecken
    3. Datei wpa_supplicant_BEISPIEL.conf.bak von SD-Karte auf lokale Festplatte kopieren
    4. Datei wpa_supplicant_BEISPIEL.conf.bak umbenennen nach wpa_supplicant.conf
    5. wpa_supplicant.conf im Editor (z.B. Notepad++, nicht mit Windows Notepad) öffnen und ssid und psk (Passwort) anpassen:
        ```
        ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
        update_config=1
        country=CH

        network={
            ssid="MEIN_WLAN"
            psk="SUPER_PASSWORT"
        }
        ```
        Achtung, bei ssid und psk sind Gross-/Kleinschreibung relevant
    6. Speichern (unbedingt mit UNIX-Zeilenende, nicht mit Windows-Zeilenende!!)
    7. Datei von lokaler Festplatte auf SD-Karte kopieren
    8. SD-Karte wieder in Raspi einsetzen und starten
    9. Raspi übernimmt Datei und löscht sie von SD-Karte! Wenn also etwas nicht funktioniert hat, kann die Datei von der lokalen Festplatte geprüft und geändert werden und dann wieder auf die SD-karte geschrieben werden.

## Login
Über SSH (WLAN, Ethernet) oder mit Bildschirm (HDMI) und Tastatur oder über Netzwerkfreigabe (Windows Explorer \\\\192.168.xxx.xxx)

Benutzername: pi, Passwort: CncLaserLinuxli

Wichtige Kommandos:
```
sudo pip3 install mopidy-Iris mopidy_spotify mopidy --upgrade
sudo systemctl restart mopidy
cd RPi-Jukebox-RFID; git pull
sudo systemctl restart phoniebox-gpio-control.service
sudo journalctl -u phoniebox-gpio-control.service
```


## Verdrahtung / Pinout
Beschreibung                      | Modul Pin | Raspi HW Pin  | Raspi Pin name | Farbe
:-------------------------------- | :---------| :------------ | :------------- | :-
Hifiberry PCM CLK                 | 32 | 32 | GPIO12,PWM0     | -
Hifiberry PCM FS                  | 35 | 35 | GPIO19,PCM_FS   | -
Hifiberry PCM DIN                 | 38 | 38 | GPIO20,PCM_DIN  | -
Hifiberry PCM DOUT                | 40 | 40 | GPIO21,PCM_DOUT | -
Volume CLK                        | 1  | 29 | GPIO5  | ws
Volume DT                         | 2  | 31 | GPIO6  | br
Volume SW (Taster)                | 3  | 13 | GPIO27 | or
Volume 3V3                        | 4  |    | 3V3    | rt
Volume GND                        | 5  |    | GND    | sw
Taster weiter +                   |    | 16 | GPIO23 | vi
Taster weiter -                   |    | 14 | GND    | bl
Taster zurück +                   |    | 15 | GPIO22 | gn
Taster zurück -                   |    | 9  | GND    | ge
RFID Karte aktiv                  |    | 22 | GPIO25 | gr
LED Ready (rot, Zunge)            |    | 18 | GPIO24 | vi
LED Ready (rot, Zunge) GND        |    | 20 | GND    | gr
LED Boot (weiss, Augenbrauen)     |    | 33 | GPIO13 | gn
LED Boot (weiss, Augenbrauen) GND |    | 39 | GND    | bl
Shutdown Taster                   |    | 11 | GPIO17 | sw/ws

<img src="Elektronik/Verdrahtung.jpg" width="100%" />

## Basis
Idee von www.phoniebox.de, 
Code basiert auf https://github.com/MiczFlor/RPi-Jukebox-RFID