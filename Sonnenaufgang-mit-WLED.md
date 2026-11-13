# Sonnenaufgang mit WLED und SK6812 RGBW (130 LEDs)

Der SK6812 RGBW ist für eine Sonnenaufgangssimulation der richtige Strip-Typ: Das echte Weiß liefert den warmen Endbereich, und die Pixel dimmen im unteren Bereich besser als eine günstige WLAN-Birne.

## 1. Hardware

Verwendet wird ein fertiger GLEDOPTO WLED Controller mit ESP32 und getestetem 5-V-Netzteil. Verkabelung, Einspeisung und Pegelwandlung sind damit erledigt.

- **Controller:** GLEDOPTO WLED Controller ESP32, WLED vorinstalliert
- **Strip:** SK6812 RGBW, 130 LEDs, 5 V
- **WLED-Version:** 16.0.1
- **Datenpin:** Voreinstellung des Controllers beibehalten, nicht ändern
- **Mikrofon** des Controllers: für den Sunrise-Effekt nicht nötig

## 2. WLED-Einstellungen (Config → LED Preferences)

- **LED Type / Length:** `SK6812 RGBW` und `130` prüfen, der Controller kann ab Werk einen anderen Typ voreingestellt haben.
- **Color Order:** meist `GRBW`, bei SK6812 je nach Hersteller aber anders. Testen: Farbe Rot wählen, es muss auch rot leuchten.
- **Auto White Mode:** auf `None` stellen. Sonst mischt WLED das Weiß aus dem RGB-Anteil heraus und die Farben werden blass. So kannst du Weiß gezielt über den Weißkanal steuern.
- **Gamma correction:** an (Color und Brightness). Das ist der wichtigste Punkt für sanftes Dimmen.
- **Maximum Current:** passend zum Netzteil begrenzen.
- **Dithering:** aktivieren, falls die installierte Version es anbietet. Das verbessert das Dimmen bei niedriger Helligkeit spürbar.

## 3. Sonnenaufgang einrichten

1. **Time & Macros:** NTP an, Zeitzone `CET/CEST` einstellen, dann einen Wecker (z. B. 06:30, Mo-Fr) anlegen, der ein Preset startet.
2. **Preset** mit Effekt "Sunrise" anlegen. Die Dauer wird über den Speed-Regler eingestellt (bei langer Dauer: Speed ganz nach links, Intensität nach Geschmack, Dauer in der Praxis ausprobieren).
3. Beim Test die Helligkeit niedrig lassen, den Effekt einmal komplett durchlaufen lassen und beobachten, wo die Farbe kippt.

### Alternative: Nightlight mit eigener Startfarbe

Wenn dir der Effektverlauf nicht passt (z. B. zu früh zu weiß), kannst du die Nightlight-Funktion nutzen, bei der du die Farbe vorher selbst setzt:

```json
{"on":true,"bri":1,"seg":[{"col":[[255,30,0,0]]}],"nl":{"on":true,"dur":20,"mode":3,"tbri":255}}
```

Dann wird nur die Helligkeit hochgefahren. Für einen Farbverlauf stößt du zusätzlich zwei bis drei Presets zeitversetzt an. Die Mode-Nummern bitte in der WLED-Doku zur eigenen Version prüfen.

## 4. Diffusor

Bei 130 Pixeln sieht man ohne Abdeckung einzelne Punkte. Besser: Alu-Profil mit Opalabdeckung, wenigstens aber Backpapier oder Diffusorfolie mit Abstand zum Strip.
