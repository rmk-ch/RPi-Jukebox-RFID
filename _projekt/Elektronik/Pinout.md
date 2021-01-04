# Pinout
Beschreibung       | Modul Pin | Raspi HW Pin  | Raspi Pin name | Farbe
:----------------- | :---------| :------------ | :------------- | :-
Hifiberry PCM CLK  | 32 | 32 | GPIO12,PWM0     | -
Hifiberry PCM FS   | 35 | 35 | GPIO19,PCM_FS   | -
Hifiberry PCM DIN  | 38 | 38 | GPIO20,PCM_DIN  | -
Hifiberry PCM DOUT | 40 | 40 | GPIO21,PCM_DOUT | -
NFC SPI SDA        | 1  | 24 | GPIO8, CE0   | ws
NFC SPI SCK        | 2  | 23 | GPIO11, SCLK | vi
NFC SPI MOSI       | 3  | 19 | GPIO10, MOSI | gr
NFC SPI MISO       | 4  | 21 | GPIO9, MISO  | ge
NFC IRQ	           | 5  | 18 | GPIO24       | or
NFC GND	           | 6  |    |              | bl
NFC RST	           | 7  | 22 | GPIO25       | br
NFC 3.3V	       | 8  |    |              | rt
Volume CLK         | 1  | 29 | GPIO5  | ws
Volume DT          | 2  | 31 | GPIO6  | br
Volume SW (Taster) | 3  | 13 | GPIO27 | or
Volume 3V3         | 4  |    | 3V3    | rt
Volume GND         | 5  |    | GND    | sw
Taster weiter      |    | 16 | GPIO23 |
Taster zurück      |    | 15 | GPIO22 |
LED Augen          |    | 18 | GPIO24 | ws
LED Augen GND      |    | 20 | GND    | sw
LED Zungen         |    | |
Shutdown Taster    | OnOffShim | |
