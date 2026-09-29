# site/ — Website liara-manifestation.com

Die öffentliche Website. **Nicht zu verwechseln mit `web/`** — das ist das
Flutter-Web-Scaffold und wird von `flutter build web` überschrieben. Dieser
Ordner hier wird von Flutter nicht angefasst.

## Inhalt

| Datei | Zweck | Pflicht wegen |
|---|---|---|
| `index.html` | Startseite | Marketing-URL (optional), Ziel der Footer-Links |
| `impressum.html` | Anbieterkennzeichnung | § 5 DDG |
| `datenschutz.html` | Datenschutzerklärung | App Store Connect (Pflichtfeld), Art. 13 DSGVO |
| `nutzungsbedingungen.html` | EULA + Abobedingungen + Gesundheitshinweis | Apple Schedule 2 |
| `support.html` | Kontakt + FAQ | App Store Connect (Pflichtfeld) |
| `style.css` | gemeinsames Stylesheet; Farben/Radien/Schatten 1:1 aus `lib/theme/app_theme.dart` | — |
| `assets/wordmark.png` | Wortmarke im Seitenkopf — **lokale Kopie** von `assets/images/wordmark.png` | — |
| `assets/favicon.png` | Tab-Icon und Apple-Touch-Icon (180 px, aus dem App-Icon) | — |
| `assets/og-image.png` | Vorschaubild beim Teilen (1200 × 630, Wortmarke auf Ivory) | — |
| `assets/screen-*.png/.jpg` | drei Produkt-Screenshots auf der Startseite (Auswahl aus den App-Store-Motiven) — siehe „Screenshots erneuern" | — |

## Die eine Regel

**Keine externen Ressourcen, kein JavaScript, keine Cookies.** Keine Google
Fonts, kein CDN, kein Analytics. Das ist kein Stilentscheid, sondern die
Grundlage dafür, dass die Zusagen in `datenschutz.html` (Abschnitt 3 und 8)
wahr sind — und dass die Seite ohne Consent-Banner auskommt.

Prüfung vor jedem Upload:

```bash
grep -nE '(src|<link[^>]*href)="https?:' *.html   # muss leer sein
grep -n '<script' *.html                          # muss leer sein
```

Erlaubte Ausnahmen sind genau zwei, und beide laden im Browser des Besuchers
nichts nach:

1. `<a href="https://…">`-Textlinks (derzeit zwei, beide in den
   Nutzungsbedingungen: Apples Erstattungsformular in Abschnitt 6 und Apples
   Standard-EULA in Abschnitt 11). Ein Link überträgt nichts, solange niemand
   ihn anklickt.
2. `<meta property="og:image" content="https://liara-manifestation.com/…">` — eine
   absolute URL ist hier Pflicht, damit Messenger und soziale Netze das
   Vorschaubild auflösen können. Sie wird **serverseitig vom Crawler** geholt,
   nie von der Seite selbst. **Achtung:** diese URL ist die einzige Stelle, an
   der die Domain fest verdrahtet ist — bei einem Domainwechsel mit anpassen.

Die Prüf-Greps oben schlagen auf beide bewusst nicht an.

## Die eingetragenen Angaben

Alle Platzhalter sind gefüllt. Die Regel bleibt: offene Angaben werden in
doppelte eckige Klammern gesetzt, und der Upload ist erst erlaubt, wenn das hier
nichts mehr findet (diese Datei schreibt die Klammern deshalb bewusst nirgends
wörtlich — sonst schlägt der Check auf sich selbst an):

```bash
grep -rn '\[\[' .
```

| Angabe | Wert | Steht in |
|---|---|---|
| Anbieter | Julian Schäfer, Einzelunternehmer | Impressum, Datenschutz § 1, Nutzungsbedingungen § 1 |
| Anschrift | c/o Impressumservice Dein-Impressum, Stettiner Str. 41, 35410 Hungen | Impressum, Datenschutz § 1 |
| Kontakt | `support@liara-manifestation.com` | Impressum, Datenschutz § 1 + § 2, Support |
| Umsatzsteuer | Kleinunternehmer nach § 19 UStG (keine USt-IdNr.) | Impressum |
| Hosting | GitHub Pages (GitHub, Inc., USA) | Datenschutz § 3 |
| Supabase-Region | Frankfurt am Main, Deutschland (EU) | Datenschutz § 7 |
| Testphase | 3 Tage — wortgleich zum Badge „3 TAGE GRATIS“ in `lib/screens/paywall/steps/step_subscription.dart` | Nutzungsbedingungen § 5 |

Zwei Punkte hängen an Entscheidungen, die noch nicht gefallen sind:

- **Abo-Preis.** Nutzungsbedingungen § 5 ist bewusst **preisneutral** formuliert
  („Laufzeit, Preis und Abrechnungszeitraum werden dir vor dem Kauf im App Store
  angezeigt“). Das ist zulässig, weil Apple den Preis unmittelbar vor dem Kauf
  verbindlich ausweist — der Absatz darunter sagt genau das. Sobald das Produkt
  in App Store Connect steht, kann der konkrete Preis dort ergänzt werden; er
  muss dann **wortgleich** zu App Store Connect sein.
- **Das Postfach muss existieren.** `support@liara-manifestation.com` ist
  Pflichtkontakt nach § 5 DDG und Support-URL-Kontakt für App Store Connect —
  eine Weiterleitung auf ein gelesenes Postfach genügt, eine tote Adresse ist
  abmahnfähig.

**Zur c/o-Anschrift:** § 5 DDG verlangt eine ladungsfähige Anschrift. Genau dafür
ist ein Impressumsservice gemacht — Voraussetzung ist, dass der Dienst zur
Entgegennahme von Post und Zustellungen beauftragt ist und der Vertrag läuft.
Endet er, fällt die Grundlage der Angabe weg und die Adresse muss ersetzt werden.

## Lokal ansehen

```bash
python3 -m http.server 8080    # in diesem Ordner, dann http://localhost:8080
```

Im Netzwerk-Tab darf ausschließlich `localhost` auftauchen.

## Veröffentlichen

Ziel ist **GitHub Pages** (so steht es auch in der Datenschutzerklärung,
Abschnitt 3 — ein Hosterwechsel muss dort nachgezogen werden). Es gibt keinen
Build-Schritt, die Dateien werden 1:1 veröffentlicht.

Die **Custom Domain `liara-manifestation.com`** ist Pflicht, nicht Kosmetik: die
`og:image`-URL in jedem `<head>` ist absolut auf diese Domain verdrahtet und
zeigt unter einer `*.github.io`-Adresse ins Leere. Einzurichten über
Repository → Settings → Pages → Custom domain (legt die `CNAME`-Datei selbst an),
dazu der DNS-Eintrag beim Domain-Anbieter und „Enforce HTTPS“.

Die Links sind relativ (`datenschutz.html`), damit die Seite lokal und auf
jedem Host funktioniert. Viele Hoster liefern zusätzlich saubere URLs ohne
Endung aus (`/datenschutz`); beide Formen führen dann ans Ziel.

## Screenshots erneuern

Die drei Bilder der Startseite sind eine **Auswahl aus den Rohaufnahmen der
App-Store-Screenshots** — ohne Headline und Karte, weil die Seite den nackten
Screenshot mit eigenem Rahmen zeigt (`style.css`, `.shot`). Aufgenommen werden
sie ausschließlich über `tools/store_shots/capture.sh` (iPhone 17 Pro Max,
1320 × 2868, Rohaufnahmen in `build/store_shots/raw/`). Wie das Skript hinter
das Router-Gate kommt und warum es dafür temporär in `lib/` schreibt, steht in
`tools/store_shots/README.md`. **DON'T** dafür kein zweites Handverfahren
pflegen — die beiden Beschreibungen liefen sonst auseinander.

```bash
tools/store_shots/capture.sh              # alle Motive, oder z. B. `capture.sh categories`
node tools/store_shots/render.mjs         # Store-Bilder gleich mit nachziehen
```

Danach auf 580 px Breite skalieren (ergibt 580 × 1260 — so steht es in den
`width`/`height`-Attributen von `index.html`):

| Rohaufnahme | Website-Datei |
|---|---|
| `02-streak.png` | `assets/screen-home.png` |
| `03-categories.png` | `assets/screen-categories.jpg` |
| `04-category.png` | `assets/screen-category.jpg` |

Der flächige Home-Screen bleibt PNG (`sips --resampleWidth 580 <quelle> --out <ziel>`),
die beiden bildlastigen werden JPEG (zusätzlich `-s format jpeg -s formatOptions 80`)
— als PNG wären sie um ein Vielfaches größer. Bei neuem Bildinhalt die
`alt`-Texte in `index.html` mitziehen: sie beschreiben, was tatsächlich zu sehen
ist.

## Verdrahtung in der App

Die drei Rechtstext-URLs liegen an **einer** Stelle: `lib/config/legal_config.dart`
(`LegalConfig.privacyPolicy` / `.terms` / `.imprint`, alle unter `baseUrl`).
`test/legal_links_test.dart` prüft Schema und Domain — ein Domainwechsel ist
eine Zeile, und eine zurückgebliebene Adresse fällt im Test auf.

Erledigt:

- `backup_prompt_screen.dart` nutzt `LegalConfig.privacyPolicy`. Der frühere
  Platzhalter zeigte auf `https://google.com` — ausgerechnet auf dem Screen, der
  die DSGVO-Einwilligung einholt. **DON'T** den Link dort nie wieder hinter eine
  `isEmpty`-Bedingung legen: ohne einsehbare Erklärung ist die Einwilligung
  nicht informiert.
- Profil → Abschnitt „Rechtliches" mit allen drei Links
  (`profile_screen.dart`, `_buildLegalCard`).

Noch offen:

- `lib/screens/update/force_update_screen.dart` → `kAppStoreUrl` bleibt leer,
  bis die App gelistet ist (der Screen sperrt auch ohne Button).
- RevenueCat-Dashboard → Paywall-Footer: Datenschutz- und Terms-Link
- App Store Connect → Privacy-Policy-URL, Support-URL, Lizenzvereinbarung,
  Länderverfügbarkeit DE/AT/CH
