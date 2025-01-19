# Verbinden en installeren van de ATOM S3 setup

> [!TIP]
> [De gemakkelijkste optie is om een aangepaste m5stack grove kabel met CN105 stekker van @fonkse te gebruiken](https://tweakers.net/aanbod/3767796/m5stack-atom-voor-besturing-via-cn105-mitsubishi-electric-warmtepomp.html)

@fonkse heeft grove bekabeling aangepast en het is eigenlijk plug en play. Kabels zijn in diverse lengte te verkrijgen maar 1 mtr is vaak voldoende om de Atom S3 module buiten de warmtepomp te hangen ivm wifi bereik.

![image](https://github.com/gekkekoe/esphome-ecodan-hp/blob/main/img/m5stack_cn105.jpg?raw=true)


### Verbinden en instellen
Deze gids is geschreven voor mensen zonder ervaring met ESPHome en gaat uit van alleen een basiskennis van Home Assistant.

Gebruikte afkortingen:
FTC Flow Temperature Controller (hoofdcontroller die normaal op de binnenunit is gemonteerd)

## Hoe te installeren:

1. De grove kabel moet worden aangesloten op de hoofdprintplaat van de warmtepomp binnenunit op de bruine connector CN105. Open het deksel (scharniert naar boven) met de 2 schroeven (aan de onderkant van de voorkant) te verwijderen

> [!WARNING]
> Voordat je de unit opent, zorg ervoor dat er GEEN spanning naar de unit gaat.)
> Schakel de warmtepomp eerst uit en zet vervolgens de zekeringautomaten van de warmtepomp uit.

![image](https://github.com/gekkekoe/esphome-ecodan-hp/blob/main/img/connection_FTC.jpg?raw=true)

2. Verbind de grove kabel met de warmtepomp op de bruine connector genaamd CN105 en hang de Atom S3 lite buiten de unit ivm wifi signaal.

![image](https://github.com/gekkekoe/esphome-ecodan-hp/blob/main/img/m5stack_installed.jpg?raw=true)

3. Als de Atom vooraf geflasht is, ben je eigenlijk klaar. Sluit de unit en herstel de spanning. De Atom gaat in WiFi-hotspotmodus zodat je hem kunt verbinden met je thuisnetwerk. Zie https://esphome.io/components/captive_portal.html

4. Ga nu naar Home Assistant. Met de Atom op je WiFi-netwerk verschijnt deze als een nieuw apparaat en kan op de gebruikelijke manier worden toegevoegd.

## Zelf flashen.
> [!WARNING]
> Sluit NOOIT een usb-kabel aan op de ATOM S3 lite ESP WANNEER deze is aangesloten op de warmtepomp..
> 
> Zet de warmtepomp UIT en zet vervolgens de zekeringautomaten van de warmtepomp uit en verwijder eerst de ESP. Indien je de kabel aan de CN105 poort losmaakt, let dan op het klipje wat ingedrukt dient te worden voordat de stekker los gaat.

Als je het zelf wilt flashen, installeer dan de ESPHome-integratie als een add-on in Home Assistant.
1. Kies in esphome builder voor nieuw apparaat en druk op doorgaan
2. Geef het een naam, bijvoorbeeld ecodan-esphome
3. Kies bijvoorbeeld esp32-s3
4. Er wordt een configuratie toegevoegd
> [!IMPORTANT]
> KIES GEEN ENCRYPTIE-SLEUTEL (overslaan)

5. Je moet de (wifi) secrets invullen (in de rechterbovenhoek van het esphome-dashboard).
```
# Your Wi-Fi SSID and password
wifi_ssid: "wifi network id"
wifi_password: "wifi password"
```

6. Klik op opslaan.
7. Ga nu naar het nieuwe apparaat en kies BEWERKEN.
8. Kopieer de ruwe code van githubs [ecodan-esphome.yaml](ecodan-esphome.yaml) in dit nieuwe apparaat en overschrijf ALLE bestaande code.
9. Voor een Atom S3 lite vul `esp32s3.yaml` in als configuratie in `ecodan-esphome.yaml`.
Je kan eventueel andere extra opties nog uitcommenten met de # te verwijderen. Bijvoorbeeld andere keuze van de taal.

> [!TIP]
> Voorbeeld pakketten code met esp32s3-proxy2.yaml:
```
packages:
  remote_package:
    url: https://github.com/gekkekoe/esphome-ecodan-hp/
    ref: main
    refresh: always
    files: [ 
            confs/base.yaml,        # required
            confs/esp32s3.yaml,     # confs/esp32.yaml, for regular board
            confs/zone1.yaml,
            ## enable if you want to use zone 2
            #confs/zone2.yaml,
            ## enable label language file
            confs/ecodan-labels-en.yaml,
            #confs/ecodan-labels-nl.yaml,
            #confs/ecodan-labels-it.yaml,
            #confs/ecodan-labels-fr.yaml,
            #confs/ecodan-labels-es.yaml
            confs/server-control.yaml,
            #confs/debug.yaml,
            ## enable this to monitor WiFi status with ESP in-built LED
            #confs/status_led.yaml,
            ## enable this to monitor status with custom led colors, uses https://github.com/esphome/esphome/pull/5814
            #confs/status_led_rgb.yaml,
            confs/wifi.yaml
           ]
```

10. Klik op opslaan en installeren.
11. Kies handmatige download en de code wordt gecompileerd naar een ecodan-heatpump.bin bestand.
12. Kies Factory format wanneer het compileren is voltooid.
13. Je kunt het .bin-bestand voor de eerste keer flashen met een usb-c kabel die is aangesloten op de Atom S3 lite. (je moet de warmtepomp opnieuw uit te schakelen en de m5stack proxy te verwijderen om te flashen met een USB C kabel
14. Flash de atom aangesloten op een usb C-kabel met behulp van [Esphome Web](https://web.esphome.io/?dashboard_install)
15. Klik op verbinden en kies de juiste com-poort. En klik daarna op installeren.
16. Koppel los van de usb-kabel en installeer terug in de warmtepomp op de CN105 connector
17. De warmtepomp zekeringautomaten weer inschakelen
18. Klaar!

> [!TIP]
> De volgende keer kan de firmware direct via wifi-verbinding worden bijgewerkt vanuit esphome, omdat het apparaat wordt gedetecteerd en al is ingesteld in je netwerk, met dezelfde esphome-code.o power off the heatpump again and remove the m5stack proxy to flash it with USB cable)
14. Flash the atom connected to a usb cable by using  [Esphome Web](https://web.esphome.io/?dashboard_install)
15. Click connect and choose the correct com port. And then click install.
16. Disconnect from usb cable and install back inside the heatpump on CN105
17. All done!

> [!TIP]
> Next time, the firmware can be updated through wifi connection directly, from within esphome, as it detects the device and is already setup in your network, with the same esphome code.
