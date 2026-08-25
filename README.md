---
title: ESP32 Räucherofensteuerung
type: project
status: active
tags: [esp32, elektronik, räuchern]
created: 2026-08-25
updated: 2026-08-25
---

# ESP32 Räucherofensteuerung

## Ziel
Steuerung für einen Räucherofen auf Basis eines ESP32 bauen:
- Temperaturfühler misst die Ofentemperatur
- Relais schaltet die Heizung ein/aus (Zweipunktregelung)
- Bedienung über eigene Web-UI auf dem ESP32
- Anbindung an Home Assistant per MQTT

## Status
Grobkonzept steht (Komponenten geklärt), Umsetzung noch nicht begonnen.

## Nächste Schritte
- [ ] Temperaturfühler-Typ festlegen (z. B. Thermoelement + MAX6675/MAX31855, oder PT100/PT1000, je nach erwarteten Räuchertemperaturen)
- [ ] Relais/SSR für Heizungsansteuerung auswählen (Schaltleistung passend zur Heizung)
- [ ] Regelstrategie definieren (einfache Hysterese vs. PID)
- [ ] Web-UI Grundgerüst auf dem ESP32 (Sollwert setzen, Ist-Temperatur anzeigen)
- [ ] MQTT-Anbindung an Home Assistant (Topics/Discovery)

## Offene Fragen
- ❓ Welche Art Räucherofen (Kalt-/Warm-/Heißräuchern)? Bestehender Ofen wird nachgerüstet oder Neubau?
- ❓ Erwarteter Temperaturbereich (relevant für Sensorwahl)?

## Notizen
Projekt angelegt aus Gespräch vom 2026-08-25.
Komponenten laut Gespräch vom 2026-08-25: Web-UI, Temperaturfühler, MQTT-Anbindung an Home Assistant, Relais zur Heizungssteuerung (an/aus).
