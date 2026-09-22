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
| `assets/screen-*.png/.jpg` | Produkt-Screenshots auf der Startseite — siehe „Screenshots erneuern" | — |

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

## Geltungsbereich DE · AT · CH

Die App erscheint im App Store in **Deutschland, Österreich und der Schweiz**.
Die Rechtstexte tragen alle drei Länder in *einem* deutschsprachigen Dokument —
bewusst ohne Länderversionen, die auseinanderlaufen würden.

**Österreich braucht keine eigenen Angaben.** Der Anbieter ist in Deutschland
niedergelassen, damit gilt das Herkunftslandprinzip der E-Commerce-Richtlinie
(in Österreich § 20 ECG): kein österreichisches ECG-/Mediengesetz-Impressum,
keine WKO- oder Gewerbeordnungs-Angaben. Die DSGVO gilt ohnehin identisch, und
„Beschwerde bei **einer** Datenschutz-Aufsichtsbehörde“ in Datenschutz
Abschnitt 2 deckt die österreichische DSB mit ab — **DON'T** das nicht zu einer
Behördenliste ausbauen, die Aufzählung wäre enger als die offene Formulierung.

**Die Schweiz ist nicht EU/EWR** — weder Herkunftslandprinzip noch DSGVO greifen
dort als maßgebliches Regime. Deshalb:

- `datenschutz.html` Abschnitt 13 ordnet die Bearbeitung nach **revDSG** ein
  (Rechte nach Art. 25/28/32, besonders schützenswerte Personendaten nach
  Art. 5 lit. c, Aufsicht EDÖB in Bern).
- Abschnitte 3 und 7 nennen neben dem EU-US auch das **Swiss-US Data Privacy
  Framework**. Die drei Stellen hängen zusammen — wird eine geändert, müssen
  die anderen mit.
- `nutzungsbedingungen.html` Abschnitt 12: **Art. 120 Abs. 2 IPRG** schließt bei
  Konsumentenverträgen eine Rechtswahl ganz aus. Der allgemeine Vorbehalt
  zwingender Verbraucherschutzvorschriften (Rom I Art. 6 Abs. 2, richtig für AT)
  trägt für die Schweiz nicht weit genug, deshalb steht das dort ausdrücklich.
- `nutzungsbedingungen.html` Abschnitt 8 führt die Notfallnummern **je Land**.
  `112` gilt in allen dreien, `0800 111 0 111` und `116 117` nur in Deutschland.
  Die Nummern stehen unabhängig von den freigeschalteten Store-Ländern da: die
  Website ist von überall erreichbar.

`impressum.html` bleibt für alle drei Länder unverändert — für die Schweiz
verlangt Art. 3 Abs. 1 lit. s UWG Identität und Kontaktadresse inkl. E-Mail, was
die vorhandenen Angaben erfüllen.

### Offen: Vertreter in der Schweiz (Art. 14 revDSG)

**Derzeit ist bewusst keiner bestellt.** Die Pflicht greift nur, wenn die
Bearbeitung *umfangreich* **und** *regelmässig* **und** *mit hohem Risiko*
verbunden ist — alle drei kumulativ. Gesundheitsdaten erfüllen das
Risiko-Kriterium sicher; „umfangreich“ zielt auf Massenbearbeitung und ist bei
einer jungen App plausibel nicht erfüllt.

**Neu bewerten, sobald die Schweizer Nutzerbasis spürbar wird** — das ist der
Zweck dieses Eintrags: die Entscheidung ist datierbar getroffen, nicht vergessen
worden. Wird ein Vertreter bestellt, gehören Name und Schweizer Adresse in
`datenschutz.html` Abschnitt 13.

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

`assets/screen-session.png`, `screen-home.png` und `screen-categories.jpg` sind
echte Aufnahmen aus dem iOS-Simulator (iPhone 17 Pro, 1206 × 2622, auf 580 px
Breite herunterskaliert).

Sie liegen **hinter dem Router-Gate** (Onboarding → Personalizing → Ready →
Paywall), und ein Seeding der SharedPreferences von außen funktioniert nicht.
Der Weg, der funktioniert — alle drei Eingriffe sind **temporär und dürfen nie
committet werden**:

1. In `lib/main.dart` direkt nach `SharedPreferences.getInstance()` einen
   fertigen `UserModel` als JSON unter `lucky_me_user` ablegen (alle Gate-Flags
   auf `true`, `isPremium: true`, Name, `goalCategories`, `streakDays`).
2. In `lib/router/router.dart` am Ende des `redirect` die Zielroute erzwingen:
   `const shotTarget = AppRoutes.session; return location == shotTarget ? null : shotTarget;`
   — **nicht** über `initialLocation`: iOS stellt die zuletzt besuchte Route
   wieder her, wodurch `initialLocation` still ignoriert wird (auch nach einem
   Hot Restart und nach einem kompletten Neustart der App).
3. `xcrun simctl status_bar <udid> override --time "9:41" --batteryState charged
   --batteryLevel 100 --cellularBars 4 --wifiBars 3` für eine saubere
   Statusleiste, danach `xcrun simctl io <udid> screenshot <datei>.png`.

Zwischen zwei Motiven genügt es, `shotTarget` zu ändern und im laufenden
`flutter run` ein Hot Restart (`R`) auszulösen. Danach `git checkout -- lib/`
und `xcrun simctl status_bar <udid> clear`.

Skalieren: `sips --resampleWidth 580 <quelle> --out <ziel>`. Die beiden flächigen
Screens bleiben PNG, das bildlastige Kategorie-Raster wird JPEG
(`-s format jpeg -s formatOptions 80`) — sonst ist die Datei um ein Vielfaches
größer.

## Danach

Die URLs müssen noch verdrahtet werden (bewusst noch nicht erledigt):

- `lib/screens/backup/backup_prompt_screen.dart` → `kPrivacyPolicyUrl` steht auf
  `https://google.com` — **Release-Blocker**
- Rechtslinks im Profil ergänzen (`lib/screens/profile/profile_screen.dart`)
- RevenueCat-Dashboard → Paywall-Footer: Datenschutz- und Terms-Link
- App Store Connect → Privacy-Policy-URL, Support-URL, Lizenzvereinbarung
