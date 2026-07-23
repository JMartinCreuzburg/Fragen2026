### Frage: In ihrer Anwendung bekommt der User eine Bestätigungseite in HTML angezeigt über einen get Endpunkt. In dem Endpunkt wird addToworkBook aufgerufen und die HTML Bestätigungseite erstellt. Wie ist das Vorgehen für einen REST get-Endpoint zu werten?
 Benennt die Ursache im Entwurf — GET soll nach HTTP-Konvention **nur lesen** (sicher und idempotent), Änderungen gehören hinter POST; nennt als saubere Lösung eine Zwischenseite mit Bestätigungsknopf und POST; kennt weitere Auslöser desselben Effekts: Link-Vorschau in Messenger und Mail-Client, Browser-Prefetch, Safe-Links-Prüfungen, Archivierungssysteme.

### Frage: Welche weiteren HTTP Methods kennen sie und welche wäre besser geeignet?

### Frage: Sie verwenden eine REST API als Schnittstelle. Erklären sie was eine REST API ausmacht.

### Frage 2.1 — „Sie schreiben, die Anwendung entschlüsselt die UUID. Erklären Sie Schritt für Schritt, wie aus dem Hash im Link wieder die E-Mail-Adresse wird."

*Zweck: Die aussagekräftigste Einzelfrage der ganzen Arbeit. Sie prüft Kryptografie-Grundverständnis und zugleich die Eigenleistung an der Dokumentation.*

✔ **Erwartet:** Die Erkenntnis, dass **gar nichts entschlüsselt wird** — ein Hash ist eine Einwegfunktion und nicht umkehrbar. Der tatsächliche Ablauf: Die Anwendung liest alle bekannten E-Mail-Adressen aus der Master-Datei, berechnet für jede erneut `Hash(Adresse + Salt)` und vergleicht das Ergebnis mit dem Wert aus der URL. Bei Übereinstimmung ist die zugehörige Adresse gefunden. Also **Nachrechnen und Vergleichen**, nicht Entschlüsseln.

### Was ist der Unterschied zwischen Hashing, Verschlüsselung und Kodierung (z. B. Base64)?

## Frage 2.2 — „Warum SHA-1? Was spricht dagegen, und was würden Sie heute nehmen?"

✔ **Erwartet:** SHA-1 gilt seit 2017 als **gebrochen** (praktikable Kollisionsangriffe) und wird für neue Systeme nicht mehr empfohlen. Als Alternative mindestens SHA-256.

### Optional: Frage 2.3 — „Was passiert, wenn der Salt-Wert bekannt wird? Und wo liegt er?"

✔ **Erwartet:** Wer den Salt kennt, kann für jede E-Mail-Adresse einen gültigen Link selbst berechnen und damit für beliebige Kollegen bestätigen. Da Firmenadressen einem festen Muster folgen und damit aufzählbar sind, hängt die **gesamte Fälschungssicherheit allein an der Geheimhaltung dieses einen Wertes**. Dazu die Auskunft, wo er liegt (`settings.json` auf dem Server) und wer auf diese Datei Lesezugriff hat.

★ **Darüber hinaus:** Nennt den Begriffsunterschied korrekt — ein **globaler geheimer Wert ist ein Pepper**, ein **Salt** wird pro Datensatz vergeben und muss nicht geheim sein; sein Zweck ist, dass gleiche Eingaben verschiedene Hashes ergeben (Schutz gegen vorberechnete Tabellen). Erkennt, dass ein Wechsel des Wertes alle bereits versendeten Links ungültig macht, und nennt bessere Ablageorte (Umgebungsvariable, Rechte auf Dienstkonto beschränkt, Schlüsseltresor).

### Frage 8.1 — „Ist ein gehashter E-Mail-Wert nach DSGVO ein personenbezogenes Datum? Begründen Sie."

✔ **Erwartet:** **Ja.** Maßgeblich ist die Bestimmbarkeit: Mit dem Salt und der Adressliste lässt sich die Person eindeutig zuordnen — genau das tut die Anwendung bei jedem Klick selbst. Es handelt sich um **Pseudonymisierung**, nicht um Anonymisierung.


# Cluster 3 — Nebenläufigkeit: Queue, Lock-Datei, Race Conditions

### Frage 3.1 — „Zwei Nutzer klicken exakt gleichzeitig. Es wird geprüft ob eine Lock Datei vorhanden ist, wenn ja wird die Anwendung beendet. Was ist wenn beide gleichzeitig prüfen?

*Zweck: Der technisch anspruchsvollste Punkt der Arbeit.*

✔ **Erwartet:** Der Prüfling geht beide Abläufe verschränkt durch und erkennt die Lücke: Prozess A prüft — Lock existiert nicht. Prozess B prüft — Lock existiert ebenfalls nicht. A legt an, B legt an. **Beide** halten sich für den einzigen Bearbeiter und schreiben gleichzeitig in die Excel-Datei — genau der Fehler, den der Mechanismus verhindern sollte. Der Fachbegriff (TOCTOU, „time of check to time of use") ist nicht erforderlich, das Verständnis schon.

★ **Darüber hinaus:** Erklärt den Kern — „prüfen" und „handeln" sind zwei getrennte Schritte, und dazwischen liegt immer eine Lücke; nur eine **unteilbare (atomare)** Operation schließt sie. Nennt konkrete Abhilfen: `os.open(..., O_CREAT | O_EXCL)` (Anlegen schlägt fehl, wenn die Datei existiert — Prüfen und Anlegen in einem Schritt), Betriebssystem-Dateisperren (`msvcrt.locking`, `portalocker`) oder eine echte Datenbanktransaktion.


### Frage 4.1 — „Sie haben sich gegen eine Datenbank entschieden. Was hätte Ihnen SQLite gebracht — und wäre Ihre Queue dann noch nötig gewesen?"

*Zweck: Prüft Abwägungsfähigkeit und Kenntnis von Alternativen. Die stärkste Alternativfrage der Arbeit.*

✔ **Erwartet:** SQLite ist dateibasiert, braucht **keinen Server** und **keine Lizenz** und ist im Python-Standard enthalten — die genannten Gegenargumente treffen darauf also nicht zu. Es bringt Transaktionen und Sperrverwaltung von Haus aus mit: Gleichzeitige Schreibzugriffe werden vom Datenbanksystem geordnet. Damit wäre die selbstgebaute Queue mit Lock-Datei **weitgehend überflüssig** gewesen.

### Frage 4.2 Welche Eigenschaften von relationalen Datenbanken hätte ihnen hier geholfen? Nennen und erklären sie diese.

### Was ist eine atomare Operation, und warum ist „prüfen, dann handeln" ohne Atomarität riskant?

### Welche weiteren Eigenschaften neben atomar kennen sie?

### Frage 3.3 — „Ihr Stresstest unter Funktionstests startet Threads. Im Betrieb erzeugt CGI Prozesse. Warum ist das ein Unterschied für Ihren Test?"

✔ **Erwartet:** Threads laufen **innerhalb eines** Prozesses und teilen sich dessen Speicher; CGI erzeugt **getrennte** Prozesse. Der Test ruft die Funktion außerdem direkt auf und lässt Webserver, CGI und HTTP komplett aus. Damit prüft er genau die kritische Situation nicht: mehrere unabhängige Prozesse, die um dieselbe Lock- und Excel-Datei konkurrieren.

### Optional Frage 5.3 — „Ihr Programm bricht bei Empfänger 300 von 1.000 ab. Was tun Sie?"

✔ **Erwartet:** Erkennt das eigentliche Problem: Es wird **nirgends festgehalten, wer bereits eine Mail bekommen hat**. Ein einfacher Neustart würde die ersten 300 Empfänger doppelt anschreiben. Beschreibt einen gangbaren Notbehelf: Protokoll auswerten, Empfängerliste manuell ab der Abbruchstelle fortsetzen.

### Frage 6.2 — „In Anhang A8 steht `send_email` als Klasse modelliert. Was ist es im Code, und was gehört in ein Klassendiagramm?"

✔ **Erwartet:** `send_email` ist eine **Funktion beziehungsweise Methode**, keine Klasse. In ein Klassendiagramm gehören Klassen mit Attributen, Methoden, Sichtbarkeiten (`+` öffentlich, `-` privat) und die Beziehungen zwischen ihnen (Assoziation, Vererbung, Komposition) mit Kardinalitäten — **nicht** einzelne Funktionen als eigene Kästen.

### Frage: Wenn es eine Klasse wäre. Welche Beziehung hätte send_mail zu EmailUtils

### Frage 8.2 — „Sie schreiben, alles entspreche den internen Sicherheitsrichtlinien — wer hat das geprüft?"

✔ **Erwartet:** Eine ehrliche Auskunft: entweder eine konkrete Freigabe mit Ansprechpartner (IT-Sicherheit, Systemadministration, Freigabe des Servers und des Mail-Relays) — oder das Eingeständnis, dass es die eigene Einschätzung ohne formale Prüfung war.