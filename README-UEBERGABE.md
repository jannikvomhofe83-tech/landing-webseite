# Flowstate Landing Page – Übergabe

Fertige, self-contained One-Page-Landing-Page nach den drei Dokumenten (Struktur / Inhalt / Design).
Keine Build-Tools, keine Libraries, keine externen Requests → maximale Ladezeit & DSGVO-sicher.

## Dateien
- `index.html` – komplette Landing Page (S0–S13), inkl. Design-Tokens, Animationen, Formular-Logik
- `impressum.html` – Pflichtseite (§ 5 DDG)
- `datenschutz.html` – Pflichtseite (DSGVO), muss Meta Pixel + Hosting nennen

## Öffnen / testen
Einfach `index.html` im Browser öffnen (Doppelklick) oder lokal ausliefern:
```bash
cd "Landing Webseite" && python3 -m http.server 8080
# dann http://localhost:8080 öffnen
```

## Was Lukas noch ersetzen muss (alle Platzhalter)
Suche im Code nach `PLATZHALTER` bzw. `[XX]`:

0. **Hero-Video (S1):** Die Hero besteht aus zentrierter Headline + Erklärsatz + Video-Platzhalter + CTA.
   Der dunkle Video-Platzhalter (`.video-placeholder`) wird durch das echte Video ersetzt.
   Im Code steht der fertige `<video>`-Block als Kommentar bereit — selbst hosten (kein YouTube → DSGVO),
   Datei nach `Bilder/hero-video.mp4` + Poster `Bilder/video-poster.jpg`.
1. **Trust-Bar (S2):** `[XX]` Websites live · `[XX]` zufriedene Kunden · Ø `[XX]` Tage — nur echte Zahlen.
2. **Referenzen (S7): ERLEDIGT** – 5 echte Referenzen mit Hero-Screenshots eingebaut
   (Taxiizi, Fuchs Pools, Betthupferl, Projektbau Erding, Physiotherapie Schediwy).
   Bilder liegen als `Bilder/ref-*.webp` (16:10). Weitere Referenzen: Eintrag im Array `referenzen`
   unten in `index.html` ergänzen (`name, branche, ort, url, bild`) – Grid passt sich automatisch an (auto-fit).
3. **Über uns (S9):** echtes Gründerfoto (kein Stockbild) + persönlicher Satz.
4. **FAQ (S10):** „Wie lange dauert es…" → echte Wochen-Angabe.
5. **Ablauf (S8):** Schritt 2 & 3 → echte Tage-Angaben (`ca. [X] Tage`).
6. **Formular-Success + Footer:** Telefonnummer, Anschrift, E-Mail.
7. **Impressum / Datenschutz:** alle `[PLATZHALTER]` mit echten Angaben füllen.
8. **OG-Image:** 1200×630-Bild erzeugen und im `<head>` verlinken.

## Tracking aktivieren (Cookies / Consent)

Die Consent-Lösung ist **fertig gebaut und getestet** – sie ist nur noch nicht scharf geschaltet.
In `index.html` ganz oben im `<script>`:

```js
const GA4_ID        = '';   // Google Analytics 4  -> Kategorie „Statistik"
const META_PIXEL_ID = '';   // Meta-Pixel          -> Kategorie „Marketing"
```

**Beide leer = aktueller Zustand:** Die Seite setzt keine Cookies, es erscheint kein Banner.
Das ist rechtlich sauber und kostet keine Conversions.

**Sobald eine ID eingetragen ist:** Banner erscheint automatisch, nur die konfigurierte Kategorie
wird angezeigt, und der Dienst lädt **erst nach aktiver Zustimmung**. Widerruf über den
Footer-Link „Cookie-Einstellungen" und über die Datenschutzseite.

### Empfehlung für „wie viele Besucher und woher"
Dafür braucht ihr **keine Cookies**. Cookiefreie Alternativen laufen serverseitig bzw. ohne
Cookies und benötigen daher **kein Banner**:

- **Netlify Analytics** (~9 $/Monat) – ihr hostet ohnehin dort, komplett serverseitig
- **Plausible** / **Fathom** – EU-Hosting, cookiefrei

Dann `GA4_ID` leer lassen. In `datenschutz.html` liegt der passende Abschnitt als
auskommentierter Block bereit (**Variante A**). Bei Google Analytics stattdessen **Variante B**
einkommentieren – dort ist die Einwilligung zwingend.

> Wichtig: Den Abschnitt in der Datenschutzerklärung **erst einkommentieren, wenn das Tool
> wirklich läuft** – sonst beschreibt die Erklärung etwas, das es nicht gibt.

## Noch anzubinden (technisch)
- **Formular-Backend:** aktuell zeigt das Formular einen Inline-Success-State (Demo). In `index.html`
  im Submit-Handler `// Produktion: hier fetch() an das Backend/CRM anbinden` ersetzen (z. B. Formspree,
  eigenes PHP, CRM-Webhook, oder E-Mail-Versand).
- **Meta Pixel:** Hook liegt in `track()`. Pixel **erst nach Cookie-Einwilligung** laden.
  Events sind vorbereitet: PageView, ViewContent (Scroll bis Angebot), InitiateCheckout (erster
  Formular-Fokus), Lead (erfolgreiches Absenden).
- **Cookie-Banner:** erst nötig/aktiv, sobald das Meta Pixel eingebunden ist.
- **Schriften (Inter):** aktuell System-Font-Fallback. Für 100 % Marken-Look: Inter als WOFF2 (400/500/600/700)
  selbst hosten und per `@font-face` einbinden (kein Google-CDN — DSGVO!).

## Umgesetzte Design-Vorgaben (Dokument 3)
- LUNA-Palette als CSS-Tokens, 70/20/10-Regel (überwiegend weiß)
- Genau zwei dunkle Anker-Sektionen: Angebot (S5) + Formular (S11), Footer #011C40
- Preis 1 € / 100 € above the fold, auch auf 390 px
- Negative Letter-Spacings, tabular-nums, Marken-Schatten (rgba(1,28,64,…))
- 4-px-Abstands-Skala, Lucide-Outline-Icons, sichtbare Labels
- Animationen: Scroll-Reveal (gestaffelt), Header-Blur, Sticky-CTA, FAQ-Accordion, Stat-Count-up,
  Hover-Effekte — alle `transform/opacity`, `prefers-reduced-motion` respektiert
- Mobile-First, Sticky-CTA nur < 640 px ab Scroll 600 px
