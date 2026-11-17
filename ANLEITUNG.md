# Kegelkasse der Anrather Pegelkönige – eigene Web-App einrichten

Die App läuft im Browser auf Tablet, Handy und PC und braucht kein Claude-Konto. Die Daten liegen in einer eigenen, kostenlosen Firebase-Datenbank von Google und werden zwischen allen Geräten live abgeglichen. Wer etwas eintragen will, meldet sich mit E-Mail und Passwort an.

Zeitaufwand: etwa 15 Minuten, einmalig.

## Inhalt des Ordners

| Datei | Zweck |
|---|---|
| `index.html` | die App |
| `config.js` | Zugangsdaten zu deiner Firebase-Datenbank (in Schritt 4 ausfüllen) |
| `firestore.rules` | Sicherheitsregeln: nur angemeldete Nutzer dürfen lesen und schreiben |
| `manifest.webmanifest`, `sw.js`, `icon-*.png` | App-Symbol, Installation auf dem Startbildschirm, Offline-Start |

## 1. Firebase-Projekt anlegen

1. <https://console.firebase.google.com> öffnen und mit einem Google-Konto anmelden (am besten ein Vereinskonto).
2. **Projekt erstellen**, Name z. B. `pegelkoenige-kasse`. Google Analytics kannst du ausschalten.
3. Der kostenlose **Spark-Tarif** reicht. Es fallen keine Kosten an, es ist keine Kreditkarte nötig.

## 2. Datenbank anlegen

1. Links **Build → Firestore Database → Datenbank erstellen**.
2. Standort: **eur3 (Europa)** oder `europe-west3 (Frankfurt)`.
3. **Im Produktionsmodus starten** wählen.
4. Reiter **Regeln** öffnen, den Inhalt durch den Text aus `firestore.rules` ersetzen und **Veröffentlichen** klicken.

## 3. Anmeldung einrichten

1. **Build → Authentication → Jetzt starten**.
2. Unter **Anmeldemethode** den Anbieter **E-Mail-Adresse/Passwort** aktivieren und speichern.
3. Reiter **Nutzer → Nutzer hinzufügen**. Lege zum Beispiel an:
   - ein Konto für das Tablet, z. B. `tablet@pegelkoenige.de` (die Adresse muss es nicht wirklich geben), mit einem Passwort,
   - ein Konto für dich und bei Bedarf für den Kassenwart.

Neue Nutzer kannst du jederzeit dort anlegen oder löschen. Es gibt keine Registrierung über die App, Fremde kommen also nicht hinein.

## 4. App mit der Datenbank verbinden

1. In der Firebase-Konsole oben links auf **Projektübersicht**, dann auf das Symbol **`</>` (Web)** klicken.
2. App-Name z. B. `Kegelkasse` eingeben. Firebase Hosting ist **nicht** nötig. **App registrieren** klicken.
3. Angezeigt wird ein Block `const firebaseConfig = { apiKey: "...", ... }`.
4. Die Werte in `config.js` übernehmen (`apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`) und speichern.

Diese Werte sind kein Geheimnis. Geschützt werden die Daten durch die Anmeldung und die Regeln aus Schritt 2.

Falls du `config.js` nicht bearbeiten möchtest: Die App fragt beim ersten Start nach der Konfiguration. Das musst du dann aber auf jedem Gerät einzeln einfügen.

## 5. App ins Internet stellen (kostenlos)

**Einfachster Weg: Netlify Drop**
1. <https://app.netlify.com/drop> öffnen und ein kostenloses Konto anlegen.
2. Den ganzen Ordner `pegelkoenige-kegelkasse` auf die Seite ziehen.
3. Du bekommst sofort einen Link, z. B. `https://xyz-123.netlify.app`. Unter **Site configuration → Change site name** kannst du daraus z. B. `pegelkoenige.netlify.app` machen.
4. Für Updates ziehst du den Ordner später erneut auf die Seite unter **Deploys**.

Alternativen: GitHub Pages oder Firebase Hosting. Wichtig ist nur, dass die Seite über **https** erreichbar ist.

## 6. Daten aus der bisherigen App übernehmen

1. Die bisherige App in Claude öffnen: **Menü → Einstellungen → Allgemein → „Alle Daten kopieren“**.
2. Die neue App öffnen, anmelden und **„Daten importieren“** tippen (beim ersten Start direkt sichtbar, sonst unter Einstellungen → Allgemein).
3. Den kopierten Text einfügen und **Importieren** tippen.

Danach nur noch die neue App benutzen, sonst laufen die Daten auseinander.

## 7. Auf dem Tablet installieren

- **Android / Chrome:** Link öffnen, Menü **⋮ → App installieren** (oder „Zum Startbildschirm hinzufügen“).
- **iPad / Safari:** Link öffnen, **Teilen → Zum Home-Bildschirm**.

Die App startet dann im Vollbild mit eurem Logo als Symbol. Die Anmeldung bleibt gespeichert.

## Gut zu wissen

- **Kurz offline:** Ist das WLAN auf der Kegelbahn mal weg, kannst du weiter eintragen. Die Eingaben werden übertragen, sobald wieder Netz da ist. Die Anzeige oben rechts zeigt dann „Offline?“.
- **Datensicherung:** Unter Einstellungen → Allgemein → **„Backup herunterladen“** gibt es jederzeit eine Sicherungsdatei. Mit „Daten importieren“ lässt sie sich wiederherstellen.
- **Kosten:** Der kostenlose Tarif erlaubt 50.000 Lesezugriffe und 20.000 Schreibvorgänge pro Tag. Ein Kegelabend braucht nur einen Bruchteil davon.
