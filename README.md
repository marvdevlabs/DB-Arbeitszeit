# DB Toolbox

Single-File-Webapp mit zwei Werkzeugen für DB-Techniker. Mobile-first, offline-fähig, **alles bleibt im Browser** — kein Server, kein Tracking, keine Datenübertragung.

**Live:** [marvdevlabs.github.io/arbeitszeit](https://marvdevlabs.github.io/arbeitszeit/) · [Vollständiges Changelog](CHANGELOG.md)

---

## Die zwei Tools

### 🕐 Tool 1 — Tageserfassung

Tägliche Erfassung der Arbeitszeit in **AE (Arbeitseinheiten, 1 AE = 10 min)**.

- **5-Step-Wizard**: Bundesland → Bereitschaft → Datum/Startzeit → SAP-Nummern → AE-Werte
- **Live-Berechnung** von Sollzeit, Pause, Plus-Stunden, 10h-Limit
- **Feiertage** für alle 16 Bundesländer (Gauß'sche Osterformel — funktioniert für jedes Jahr)
- **Wochenend-/Feiertags-Logik** — kein An-/Abfahrt-Abzug, eigene Pausen-Regeln
- **MONAmC-Screenshot-Import** via Tesseract.js — SAP-Nummern + Splits werden automatisch erkannt
- **Splits** (10, 11, 20, 29, 40, 50) + beliebige eigene Splits (1–999) pro Auftrag
- **LRE-Stufen** (1/2/3) + Custom-Start-Uhrzeit bei Bereitschaft
- **Aufträge umsortieren** mit ▲ ▼ Pfeilen
- **Verlauf** der letzten 14 Tage mit Restore-Modal beim Start
- **Export**: Kopieren · Drucken · JSON-Backup · QR-Code-Transfer (LZ-komprimiert)

### 💰 Tool 2 — Abrechnungs-Check

Bereitschafts-Abrechnung gegen die Bereitschafts-Liste prüfen — komplett clientseitig.

- **PDF-Parser** (pdf.js, SRI-gepruft) extrahiert LRE-Einsätze, Rufbereitschaftsstunden, Steuern + SV-Beiträge aus DB-Abrechnungsbescheinigungen
- **Bereitschafts-Liste** (DB eTime) wird automatisch ausgelesen — Gesamtsumme LRE1/2/3, Zulagenberechtigende Rufbereitschaftszeit
- **Layout-Check** beider PDFs (Pflicht), **Monat-Abgleich** + automatische Nachzahlungs-Block-Erkennung
- **Zweistufiger Wizard**: erst LRE-Vergleich (passt alles?), auf Klick die Geld-Prüfung (Brutto → Netto → tatsächlicher Auszahlungsbetrag)
- **Persönliche Daten** (Steuerklasse, Kinderfreibetrag, Konfession, Bundesland, KV-Zusatz) für Netto-Berechnung — nur lokal gespeichert
- **Auszahlungsbetrag** wird wenn möglich direkt aus Lohnart 977 („Überweisung") gelesen, sonst geschätzt
- **Alles offline**: PDFs bleiben im RAM, kein Upload, kein Server. Privacy-Banner verdeutlicht das.

---

## Privacy & Sicherheit

- **Keine Server-Calls** für persönliche Daten. PDFs werden ausschließlich im Browser-RAM verarbeitet.
- **Keine Tracker, kein Analytics, kein Indexing** (`robots: noindex,nofollow`).
- **Content Security Policy** restriktiv — `default-src 'self'`, gezielte CDN-Allowlist nur für pdf.js / Tesseract.js / lz-string / qrcode-generator.
- **Subresource Integrity (SHA-384)** auf allen externen Skripten — manipulierte CDN-Inhalte verweigert der Browser.
- **pdf.js-Worker** wird zusätzlich nach Download via `crypto.subtle.digest` manuell gegen den erwarteten SHA-384 geprüft — die `worker-src`-CSP bleibt eng auf `'self' blob:`.
- **Tiefe Schema-Validierung** auf alle externen Datenquellen (LocalStorage, JSON-Import, URL-Hash, OCR-Output).
- **Open Source** unter MIT-Lizenz — der gesamte Code ist in diesem Repo einsehbar.

---

## Technik

- **Single-File-HTML** — ein einziges `index.html` mit inline CSS + JS (~6.000 Zeilen).
- **Externe Libs**: alle lazy-loaded mit SRI-Hashes:
  - [Tesseract.js 5.0.5](https://github.com/naptha/tesseract.js) — OCR für MONAmC-Screenshots
  - [pdf.js 3.11.174](https://github.com/mozilla/pdf.js) — PDF-Parsing für Abrechnungen
  - [lz-string 1.5.0](https://github.com/pieroxy/lz-string) — Komprimierung für QR-Transfer
  - [qrcode-generator 1.4.4](https://github.com/kazuhikoarase/qrcode-generator) — QR-Code-Rendering
- **PWA** mit Service Worker (network-first für HTML, stale-while-revalidate für Assets) und Web App Manifest.
- **Light/Dark-Theme** mit System-Erkennung, Theme-Toggle im Header.
- **Mobile-first**, Notch-fest auf iPhone (Safe-Area-Insets).

---

## Installation

### Als Web-App
Einfach [marvdevlabs.github.io/arbeitszeit](https://marvdevlabs.github.io/arbeitszeit/) im Browser öffnen.

### Als PWA (Smartphone)
- **iOS**: Safari → Teilen → „Zum Home-Bildschirm"
- **Android**: Chrome zeigt automatisch einen Install-Banner an

### Selbst hosten
```bash
git clone https://github.com/marvdevlabs/arbeitszeit.git
cd arbeitszeit
# Statischer Server reicht — z.B.:
python -m http.server 8000
# Browser: http://localhost:8000
```

Keine Build-Step, keine Dependencies, kein Node nötig.

---

## Open Source

Marvin Duhn · [@marvdevlabs](https://github.com/marvdevlabs) · MIT-Lizenz

**Kein offizielles Tool der Deutschen Bahn AG.** Privates Open-Source-Projekt eines DB-Mitarbeiters für DB-Mitarbeiter. Tarif-Tabellen sind **nicht** im Code hinterlegt — das monatliche Grundgehalt trägst du für die Netto-Berechnung selbst ein.

Bug-Reports und Feature-Wünsche: [GitHub Issues](https://github.com/marvdevlabs/arbeitszeit/issues)
