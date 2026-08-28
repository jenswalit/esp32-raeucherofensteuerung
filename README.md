---
title: ESP32 Räucherofensteuerung
type: project
status: active
tags: [esp32, elektronik, räuchern]
created: 2026-08-25
updated: 2026-08-28
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
- [x] Grundsatzentscheidung: Eigenbau mit ESP32 statt fertigem Auber-Controller (Gespräch vom 2026-08-28)
- [x] Temperaturfühler-Typ festgelegt: **PT1000**, passend zum Temperaturbereich 30–135 °C (Gespräch vom 2026-08-28)
- [ ] PT1000-Auswertung für ESP32 auswählen (z. B. passendes RTD-Amplifier-Modul/Breakout, da PT1000 einen geeigneten Messverstärker braucht)
- [ ] Relais/SSR für Heizungsansteuerung auswählen (Schaltleistung passend zur Heizung des Masterbuilt 140B)
- [ ] Regelstrategie definieren — Tendenz PID statt einfacher Hysterese, siehe Begründung unten
- [ ] Umbau des Masterbuilt 140B umsetzen: Zugriff auf Heizspirale über Revisions-/Serviceklappe (bestätigter Ansatz, Gespräch vom 2026-08-28), Anschlussleistung und Verdrahtung der Heizspirale vorher checken
- [ ] Vorhandenen Temperaturbegrenzer/Hochtemperatur-Schutzthermostat (schaltet bei ca. 350 °F / 177 °C ab) beim Umbau erhalten — bestätigt als Sicherheitsanforderung (Gespräch vom 2026-08-28)
- [ ] Web-UI Grundgerüst auf dem ESP32 (Sollwert setzen, Ist-Temperatur anzeigen)
- [ ] MQTT-Anbindung an Home Assistant (Topics/Discovery)

## Offene Fragen
- ✅ Welche Art Räucherofen? Bestehender **Masterbuilt 140B** (digitaler Controller) wird nachgerüstet, kein Neubau. Quelle: 00-Inbox-Ablage vom 2026-08-28, `Recherche/masterbuilt-140b-pid-recherche.pdf`.
- ✅ Erwarteter Temperaturbereich: **ca. 30–135 °C** (Werksangabe Masterbuilt 140B). Quelle: `Recherche/masterbuilt-140b-pid-recherche.pdf`.
- ⚠️ Widerspruch/Unklarheit: Die Recherche-Notiz nennt den **Auber AW-1520H** (WLAN-PID-Controller) als Referenzcontroller, das beigelegte Handbuch ist aber für den **Auber WSD-1500H-W** (ebenfalls PID, aber ohne WLAN laut Handbuch-Deckblatt — WLAN-Status-LED wird zwar im Handbuch erwähnt, Modellbezeichnung weicht dennoch ab). Bleibt offen, da eh selbst gebaut wird (siehe unten) — die Auber-Doku dient nur noch als technische Referenz, nicht als Kaufoption; Modellklärung daher nicht mehr entscheidungsrelevant.
- ✅ Fertiger Auber-Controller oder Eigenbau? **Eigenbau mit ESP32.** Die Auber-Doku dient nur als Vorbild für Funktionsumfang/Anschlussschema, es wird kein Auber-Controller gekauft. Sensorwahl: **PT1000** (wie beim Auber-Referenzcontroller). Quelle: Gespräch vom 2026-08-28.
- ❓ Soll das Kalträuchern mit externem Rauchgenerator (im Recherche-Dokument erwähnt) auch über die ESP32-Steuerung mit abgebildet werden (z. B. analog zum "Smoke Generator Output" des Auber-Controllers)?

## Erkenntnisse aus der Recherche (2026-08-28)
- Der originale Masterbuilt-140B-Controller regelt per Hysterese (Beispiel: Soll 110 °C, ein bei 105 °C, aus bei 115 °C) → spürbare Temperaturschwankungen, besonders unschön zwischen 80–120 °C. Ein PID-Ansatz für die ESP32-Steuerung ist daher sinnvoller als einfache Hysterese. Quelle: `Recherche/masterbuilt-140b-pid-recherche.pdf`.
- Umbau-Prinzip (von Auber Instruments für "Modify Masterbuilt Digital Smokers" dokumentiert): OEM-Steuerplatine wird umgangen, indem die Heizelement-Leitung direkt an den externen Controller (bzw. hier: ans ESP32-Relais/SSR) gelegt wird, statt über das digitale Bedienpanel zu laufen. Genaue Verkabelung (ein oder zwei Junction-Boxen am Boden, Steckverbinder-Farben) ist modellabhängig — vor dem eigenen Umbau mit dem konkreten Gerät abgleichen. Sicherheitsrelevant: Hochtemperatur-Schutzthermostat (Cutoff bei ~350 °F) bleibt in der Kette erhalten. Quelle: `Recherche/how-to-modify-masterbuilt-digital-smokers.pdf` (Auber Instruments, Version 2.0, Juni 2026).
- Auber-Referenzcontroller als Vorbild für den Funktionsumfang der eigenen ESP32-Lösung: PID-Regelung, Überwachung mehrerer Fühler (Garraum + Kerntemperatur), mehrstufige Garprogramme (bis 6 Schritte), Temperaturprotokollierung, automatisches Warmhalten nach Erreichen der Kerntemperatur, WLAN/App-Anbindung. Quelle: `Recherche/masterbuilt-140b-pid-recherche.pdf`, `Recherche/wsd-1500h-manual-referenz-controller.pdf`.
- WSD-1500H-Handbuch als technische Referenz (auch wenn Modell nicht 1:1 übernommen wird): Sensor PT1000 RTD, max. 15 A/120 V bzw. 12 A/220 V, Temperaturauflösung 1 °C, Genauigkeit ±1 °C, separater Ausgang für Rauchgenerator (3 A). Quelle: `Recherche/wsd-1500h-manual-referenz-controller.pdf`.

## Notizen
Projekt angelegt aus Gespräch vom 2026-08-25.
Komponenten laut Gespräch vom 2026-08-25: Web-UI, Temperaturfühler, MQTT-Anbindung an Home Assistant, Relais zur Heizungssteuerung (an/aus).

Recherche-Dokumente aus der Inbox vom 2026-08-28 einsortiert nach `Recherche/` (siehe Erkenntnisse oben).

Code/Repo: https://github.com/jenswalit/esp32-raeucherofensteuerung (angelegt 2026-08-25, am 2026-08-28 auf public gestellt, um es einem Kollegen zugänglich zu machen).
