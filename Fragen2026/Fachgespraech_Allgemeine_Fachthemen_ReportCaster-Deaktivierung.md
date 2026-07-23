# Allgemeine Fachthemen für das Fachgespräch
## Anknüpfend an die Projektdokumentation „Automatisiertes Deaktivierungssystem für redundante Business Report Jobs"

**Fachrichtung:** Fachinformatiker Anwendungsentwicklung · **Abschlussprüfung Sommer 2026**

**Zweck:** Ergänzung zu den projektbezogenen Fragen in `Fachgespraech_Themen_ReportCaster-Deaktivierung.md`. Die dortigen Fragen zielen auf das, was der Prüfling **konkret gebaut** hat. Die folgenden Themen kommen im Projekt ebenfalls vor, lassen sich aber **allgemein** abfragen — unabhängig von der konkreten Umsetzung. Damit lässt sich prüfen, ob hinter der Lösung breites Fachwissen steht.

**Hinweis zur Nutzung:** Pro Themenblock steht zuerst der Anknüpfungspunkt im Projekt (als Einstieg ins Gespräch), dann mögliche Fragen, dann eine kurze Orientierung, was eine gute Antwort enthalten sollte. Die Orientierung ist bewusst knapp und allgemeinverständlich gehalten, damit auch fachfremde Ausschussmitglieder die Antwort einordnen können.

**Verhältnis zu den anderen Dokumenten:**
- `Fachgespraech_Themen_…` → projektspezifisch, „was hast **du** gebaut und warum"
- `Fachgespraech_Erwartungshorizont_…` → Bewertungsraster zu genau diesen projektspezifischen Fragen
- **dieses Dokument** → allgemeines Fachwissen, „kannst du das Prinzip **unabhängig** vom Projekt erklären"

**Empfehlung:** Zwei bis drei Themenblöcke genügen. Besonders naheliegend sind hier die Blöcke 2, 3, 4, 6 und 9 — sie treffen die technischen Kernthemen der Arbeit (Kryptografie, Nebenläufigkeit, Datenhaltung, Web) und lassen sich gut mit den projektspezifischen Fragen verzahnen.

---

## 1. Web-Architektur und Ausführungsmodelle

**Anknüpfung:** Die Anwendung ist eine Python-Webanwendung, die über CGI auf einem vorhandenen Webserver betrieben wird; für jede Anfrage entsteht ein eigener Prozess.

**Fragen:**
- Erklären Sie das Client-Server-Prinzip am Beispiel eines Seitenaufrufs im Browser.
- Was ist der Unterschied zwischen einem Webserver und einer Webanwendung?
- Was macht CGI, und warum ist es ressourcenintensiv?
- Was versteht man unter einer Schnittstelle zwischen Webserver und Anwendung (Stichwort WSGI/FastCGI)?
- Was bedeutet es, dass eine Webanwendung „zustandslos" ist? Wie merkt sie sich dann einen angemeldeten Nutzer?
- Wann lohnt sich ein Framework, und wann ist es überdimensioniert?

**Gute Antwort:** Beschreibt den Ablauf Anfrage → Verarbeitung → Antwort und trennt sauber: Der Webserver nimmt Verbindungen entgegen, die Anwendung erzeugt den Inhalt. CGI startet **pro Anfrage** einen neuen Prozess samt Interpreter — das kostet Zeit und Speicher und teilt keinen Zustand zwischen Anfragen; FastCGI/WSGI halten die Anwendung dagegen dauerhaft bereit. Zustandslosigkeit heißt: Der Server erinnert sich zwischen zwei Anfragen an nichts; der Zusammenhang entsteht über Sitzungskennungen (Cookies) oder Werte in der Anfrage. Beim Framework sollte eine Abwägung fallen: Es nimmt viel Standardarbeit ab, ist für eine einzige Route aber schweres Werkzeug.

---

## 2. HTTP und Schnittstellen ★

**Anknüpfung:** Der Bestätigungslink löst über einen einzigen GET-Aufruf eine Aktion aus, die Daten schreibt; die Antwort ist je nach Fall eine von mehreren fertigen Seiten.

**Fragen:**
- Welche HTTP-Methoden kennen Sie und wofür stehen sie? Was heißt idempotent, was „sicher" (safe)?
- Warum sollte eine Aktion, die etwas verändert, nicht per GET ausgelöst werden?
- Was sagen die Statuscode-Gruppen 2xx, 3xx, 4xx und 5xx aus?
- Was macht eine Schnittstelle zu einer REST-Schnittstelle?
- Was ist der Unterschied zwischen einem Pfadparameter und einem Query-Parameter?
- Was bringt HTTPS gegenüber HTTP, und warum ist das gerade bei einem Link mit Kennung relevant?

**Gute Antwort:** Ordnet GET dem Lesen und POST dem Verändern zu und erklärt „sicher" als „verändert nichts" und idempotent als „mehrfach ausgeführt ändert sich nichts zusätzlich". Der Kernpunkt: GET-Links werden von Mail-Scannern, Vorschaudiensten und Browser-Prefetch **ungefragt** aufgerufen — eine schreibende Aktion darf deshalb nicht an GET hängen, sauber ist eine Bestätigungsseite mit anschließendem POST. 4xx = Fehler beim Aufrufer, 5xx = Fehler beim Server. Bei REST fallen Ressourcen, Zustandslosigkeit und die Nutzung der HTTP-Methoden. HTTPS schützt den Link samt Kennung auf dem Transportweg vor Mitlesen.

---

## 3. Kryptografie: Hashing, Verschlüsselung, Signaturen ★

**Anknüpfung:** Die Kennung im Link wird aus E-Mail-Adresse und einem geheimen Zusatzwert gebildet und mit einer Hashfunktion (SHA-1) berechnet; die Doku spricht davon, die Kennung werde „entschlüsselt".

**Fragen:**
- Was ist der Unterschied zwischen Hashing, Verschlüsselung und Kodierung (z. B. Base64)?
- Warum ist ein Hash **nicht** umkehrbar, und wie prüft man dann trotzdem einen Wert?
- Was ist ein Salt, was ein Pepper, und wozu dient welcher?
- Wann nutzt man symmetrische, wann asymmetrische Verschlüsselung?
- Was ist der Unterschied zwischen einem Kollisions- und einem Preimage-Angriff?
- Warum gilt eine Hashfunktion irgendwann als „gebrochen", und woran macht man die Auswahl heute fest?
- Wozu dient HMAC, und was ist der Unterschied zu einem einfachen Hash über „Geheimnis + Nachricht"?

**Gute Antwort:** Stellt klar: Verschlüsselung ist umkehrbar (mit Schlüssel), Hashing nicht (Einwegfunktion, nur neu berechnen und vergleichen), Kodierung hat gar keine Schutzwirkung. Salt = pro Datensatz, darf öffentlich sein, verhindert vorberechnete Tabellen; Pepper = ein globaler geheimer Wert. Symmetrisch = ein gemeinsamer Schlüssel (schnell), asymmetrisch = Schlüsselpaar (Austausch über unsichere Wege, Signaturen). Kollision = zwei Eingaben mit gleichem Hash, Preimage = aus dem Hash die Eingabe finden. „Gebrochen" heißt, ein Angriff ist praktikabel geworden (SHA-1: Kollisionen seit 2017); heute mindestens SHA-256, für „Geheimnis + Nachricht" HMAC — weil ein naiver Hash über beides strukturell angreifbar sein kann.

---

## 4. Nebenläufigkeit und Synchronisation ★

**Anknüpfung:** Weil mehrere Anfragen gleichzeitig in dieselbe Datei schreiben könnten, wurde eine Warteschlange mit einer Sperr-Datei gebaut, die sicherstellen soll, dass immer nur eine Verarbeitung läuft.

**Fragen:**
- Was ist der Unterschied zwischen einem Prozess und einem Thread?
- Worin unterscheiden sich synchron, asynchron und parallel?
- Was ist eine Race Condition? Nennen Sie ein Beispiel.
- Was versteht man unter einem kritischen Abschnitt, und wie schützt man ihn?
- Was ist eine atomare Operation, und warum ist „prüfen, dann handeln" ohne Atomarität riskant?
- Was ist ein Deadlock, was ein verwaistes Lock (stale lock)?
- Was macht der GIL in Python, und was folgt daraus für Threads?

**Gute Antwort:** Prozess = eigener Speicherbereich, Thread = Ausführungsstrang **innerhalb** eines Prozesses mit geteiltem Speicher. Trennt „asynchron = nicht warten" von „parallel = tatsächlich gleichzeitig". Race Condition = das Ergebnis hängt von der zufälligen Reihenfolge paralleler Zugriffe ab; klassisches Beispiel zwei, die einen Zähler gleichzeitig lesen, erhöhen und zurückschreiben. Kritischer Abschnitt = darf nur von einem gleichzeitig durchlaufen werden, Schutz durch Sperren. Der Kern: „Prüfen" und „Handeln" sind zwei Schritte, dazwischen liegt eine Lücke — nur eine unteilbare Operation schließt sie. Deadlock = wechselseitiges Warten; stale lock = eine Sperre bleibt nach Absturz liegen und blockiert alles Weitere. Der GIL serialisiert Python-Threads teilweise — echte Parallelität entsteht erst über getrennte Prozesse.

---

## 5. Relationale Datenbanken und Alternativen ★

**Anknüpfung:** Die Daten werden bewusst in Excel-Dateien gehalten statt in einer Datenbank; eine Migration wird als späterer Schritt genannt.

**Fragen:**
- Was leistet ein Datenbanksystem, das eine Datei nicht leistet?
- Was ist eine Transaktion? Wofür steht ACID?
- Was bedeuten Primär- und Fremdschlüssel, was referenzielle Integrität?
- Was versteht man unter Normalisierung (erste drei Normalformen)?
- Was ist der Unterschied zwischen einer dateibasierten Datenbank (z. B. SQLite) und einem Datenbankserver?
- Wann ist eine einfache Datei die richtige Wahl — und wann nicht mehr?

**Gute Antwort:** Nennt Transaktionen, Mehrbenutzerbetrieb, Sperren, Integritätsregeln, Indizes und Abfragesprache als das, was eine Datei nicht mitbringt. Transaktion = Klammer um mehrere Schritte, die nur ganz oder gar nicht wirken; ACID = Atomarität, Konsistenz, Isolation, Dauerhaftigkeit. Primärschlüssel identifiziert eindeutig, Fremdschlüssel verweist darauf. Normalisierung sinngemäß: atomare Werte, volle Abhängigkeit vom Schlüssel, keine Abhängigkeiten zwischen Nicht-Schlüsselfeldern — Ziel ist die Vermeidung von Redundanz. Wichtig ist die Einsicht, dass SQLite serverlos und lizenzfrei ist und deshalb der „zu aufwendig"-Einwand darauf nicht zutrifft.

---

## 6. Datenaustausch, Formate und E-Mail-Technik

**Anknüpfung:** Konfiguration liegt als JSON vor; Benachrichtigungen gehen als HTML-Mail über SMTP an über 1.000 Empfänger, mit selbst gebauter Platzhalter-Ersetzung.

**Fragen:**
- Wofür steht JSON, wie ist es aufgebaut, welche Vor- und Nachteile hat es gegenüber XML?
- Wie funktioniert der E-Mail-Versand grundsätzlich (Stichwort SMTP)?
- Was ist eine MIME-Multipart-Nachricht, und wozu dient ein Text-Alternativteil neben dem HTML-Teil?
- Was ist eine Template-Engine, und welchen Vorteil bietet automatisches Escaping?
- Was versteht man unter Injection allgemein, und wie entsteht HTML-Injection beim Zusammenbauen von Ausgaben?
- Worauf achtet man beim Versand großer Mengen an E-Mails (Rate Limits, Zustellbarkeit, Wiederholbarkeit)?

**Gute Antwort:** JSON als schlankes, gut lesbares Format aus Schlüssel-Wert-Paaren, kompakter als XML, aber ohne mitgeliefertes Prüfschema. SMTP als Protokoll zum Einliefern der Mail beim Server. Multipart/alternative liefert **denselben** Inhalt in mehreren Formaten, der Client wählt — der Textteil ist der Rückfall für Clients ohne HTML. Eine Template-Engine trennt Vorlage und Daten und maskiert Sonderzeichen automatisch, wodurch eingesetzter Fremdtext nicht als Markup interpretiert wird. Injection allgemein = Daten werden als Code/Markup ausgeführt, weil sie ungeprüft in eine Ausgabe geraten. Beim Massenversand sollten Drosselung, Umgang mit Unzustellbarkeit (Bounces) und Wiederaufnahme ohne Doppelversand fallen.

---

## 7. Softwaredesign und Entwurfsprinzipien

**Anknüpfung:** Die Anwendung ist in mehrere Module nach Verantwortlichkeiten aufgeteilt; es gab eine eigene Phase für Code-Review und Refactoring, und es werden UML-Diagramme verwendet.

**Fragen:**
- Was bedeuten enge und lose Kopplung? Was ist Kohäsion?
- Was besagt das Prinzip der einzigen Verantwortlichkeit (Single Responsibility)?
- Was ist eine zyklische Abhängigkeit zwischen Modulen, und warum ist sie problematisch?
- Was versteht man unter Refactoring — und warum ändert sich dabei das Verhalten nicht?
- Was gehört in ein Klassendiagramm, und wie unterscheidet es sich von einem Komponentendiagramm?
- Was ist ein Decorator (Wrapper), und wofür setzt man ihn ein?

**Gute Antwort:** Lose Kopplung = Bausteine kennen voneinander wenig und sind austauschbar; hohe Kohäsion = ein Baustein macht eine Sache, die aber ganz. Single Responsibility = ein Modul hat genau einen Änderungsgrund. Zyklische Abhängigkeit = zwei Module brauchen einander gegenseitig; das erschwert Verständnis, Test und Wiederverwendung und kann beim Laden technisch scheitern — Auflösung durch klare Richtung oder ein drittes Modul. Refactoring verbessert die innere Struktur bei unverändertem Verhalten, weshalb Tests die Voraussetzung sind. Ins Klassendiagramm gehören Klassen mit Attributen, Methoden, Sichtbarkeiten und Beziehungen — **nicht** einzelne Funktionen als Klassen. Ein Decorator umhüllt eine Funktion und ergänzt Verhalten davor/danach (z. B. Rückfrage, Protokoll, Rechteprüfung).

---

## 8. Testen und Qualitätssicherung ★

**Anknüpfung:** Die Bausteine wurden manuell getestet, ergänzt um einen Stresstest und eine Feedbackrunde mit echten Nutzern; automatisierte Unit-Tests gibt es nicht.

**Fragen:**
- Welche Teststufen gibt es, und was prüft jede?
- Worin unterscheiden sich Blackbox- und Whitebox-Test?
- Was gehört zu einem gut beschriebenen Testfall?
- Warum testet man auch die Fälle, die fehlschlagen **sollen** (Negativtests)?
- Was ist ein Mock, und wozu braucht man ihn?
- Was sagt eine Testabdeckung aus — und was sagt sie nicht?
- Warum sind automatisierte Tests gerade vor einem Refactoring wertvoll?

**Gute Antwort:** Ordnet Unit-, Integrations-, System- und Abnahmetest zu (vom kleinsten Baustein bis zur Abnahme durch den Auftraggeber). Blackbox prüft ohne Kenntnis des Innenlebens, Whitebox mit. Ein guter Testfall nennt Vorbedingung, Eingabe, **erwartetes** und tatsächliches Ergebnis sowie Status — sonst ist er nicht wiederholbar. Negativtests belegen, dass Unerlaubtes wirklich abgewiesen wird. Ein Mock ersetzt eine echte Abhängigkeit (z. B. den Mailserver) durch einen kontrollierten Platzhalter, damit Tests schnell und wiederholbar laufen. Wichtig: Hohe Abdeckung sagt nur, wie viel Code durchlaufen wurde, nicht ob sinnvoll geprüft wurde. Vor dem Refactoring sind Tests das Netz, das absichert, dass sich das Verhalten nicht ungewollt ändert.

---

## 9. IT-Sicherheit und Geheimnisverwaltung ★

**Anknüpfung:** Die Sicherheit der Bestätigungslinks hängt an einem geheimen Zusatzwert, der in einer Konfigurationsdatei auf dem Server liegt; das Mail-Passwort wird interaktiv abgefragt.

**Fragen:**
- Was sind die Schutzziele Vertraulichkeit, Integrität und Verfügbarkeit?
- Worin unterscheiden sich Authentifizierung und Autorisierung?
- Wie und wo bewahrt man Geheimnisse (Schlüssel, Passwörter) richtig auf?
- Warum gehören Zugangsdaten nicht in den Quellcode oder ins Repository?
- Was bedeutet „dem Nutzer niemals vertrauen", und warum muss jede Prüfung serverseitig erfolgen?
- Was ist ein Man-in-the-Middle-Angriff, und was hilft dagegen?

**Gute Antwort:** Nennt die drei Schutzziele und ordnet sie ein. Authentifizierung klärt „wer bist du", Autorisierung „was darfst du". Geheimnisse gehören in Umgebungsvariablen, gesonderte Konfiguration mit engen Dateirechten oder einen Schlüsseltresor — nicht in den Code und nicht ins Repository, weil sie sonst in der Versionshistorie und für alle Leseberechtigten sichtbar sind. Dass das Mail-Passwort per Eingabeaufforderung geholt und nicht gespeichert wird, ist positiv. „Nicht vertrauen" heißt: Prüfungen im Browser sind Komfort, verbindlich ist die serverseitige Prüfung. Gegen Man-in-the-Middle hilft Transportverschlüsselung (TLS/HTTPS).

---

## 10. Datenschutz und Recht

**Anknüpfung:** Es wird das Antwortverhalten von über 1.000 Beschäftigten erfasst und gespeichert; die E-Mail-Adresse erscheint im Link nur als gesalzener Hash, was in der Doku als Datenschutzmaßnahme beschrieben wird.

**Fragen:**
- Was sind personenbezogene Daten nach DSGVO?
- Was ist der Unterschied zwischen Anonymisierung und Pseudonymisierung? Ist ein gehashter Wert anonym?
- Welche Grundsätze der DSGVO kennen Sie (z. B. Zweckbindung, Datenminimierung, Speicherbegrenzung)?
- Was ist bei der Auswertung von Beschäftigtendaten zusätzlich zu beachten (Stichwort Mitbestimmung)?
- Worauf achtet man bei Open-Source-Lizenzen im betrieblichen Einsatz?

**Gute Antwort:** Personenbezogen = alles, was einer bestimmbaren Person zuzuordnen ist. Entscheidend die Abgrenzung: **anonym** = Bezug unumkehrbar entfernt (DSGVO greift nicht mehr), **pseudonym** = Bezug mit Zusatzwissen herstellbar (DSGVO gilt weiter). Ein gehashter Wert einer bekannten, aufzählbaren Größe ist bei bekanntem Salt rückführbar, also pseudonym — das Hashing ist eine gute Schutzmaßnahme, aber keine Anonymisierung. Nennt Zweckbindung, Datenminimierung, Speicherbegrenzung und Transparenz. Bei Auswertung von Verhaltensdaten der Belegschaft sollten Rechtsgrundlage, Löschfristen und ggf. die Mitbestimmung des Betriebsrats fallen. Bei Lizenzen: Unterschied zwischen freizügigen (MIT, Apache) und rückwirkenden (GPL).

---

## 11. Wirtschaftlichkeit und Projektmanagement

**Anknüpfung:** Die Arbeit enthält eine Kosten-Nutzen-Analyse mit Stundensätzen, eine Amortisationsrechnung, einen Phasenplan und einen Soll-/Ist-Vergleich mit ausgewiesenen Abweichungen.

**Fragen:**
- Was bedeutet Amortisation, und wie berechnet man sie?
- Was ist der Unterschied zwischen einmaliger Investition und laufenden Betriebskosten?
- Was versteht man unter Gesamtbetriebskosten (Total Cost of Ownership)?
- Was steckt hinter einer Make-or-Buy-Entscheidung?
- Wie schätzt man Aufwände, und wie geht man mit Abweichungen im Soll-/Ist-Vergleich um?
- Was unterscheidet einen prognostizierten von einem realisierten Nutzen?

**Gute Antwort:** Amortisation = Zeitraum, bis die Einsparungen die Anschaffungskosten decken. Trennt einmalige Investition sauber von laufenden Kosten (Betrieb, Pflege, Wiederholung des Prozesses). TCO = alle Kosten über die Lebensdauer, nicht nur die Anschaffung. Bei Make-or-Buy werden Kosten, Zeit, Anbieterabhängigkeit und eigenes Know-how abgewogen. Beim Soll-/Ist-Vergleich sollte klar werden, dass Abweichungen nicht nur ausgewiesen, sondern **begründet** gehören. Wichtig die Einsicht, dass ein Nutzen, der erst nach Projektende eintritt, prognostiziert und nicht realisiert ist — und die Amortisationsrechnung sich entsprechend verschiebt.

---

## 12. Anforderungen, Zieldefinition und Abnahme

**Anknüpfung:** Titel und Fazit sprechen von einem „Deaktivierungssystem" und einer „vollständig funktionsfähigen Lösung"; ein einzelner Schritt (die eigentliche Deaktivierung) steht noch aus, und die Erfüllung wird pauschal erklärt.

**Fragen:**
- Was ist der Unterschied zwischen funktionalen und nicht-funktionalen Anforderungen?
- Was macht eine Anforderung SMART?
- Warum sollten Abnahmekriterien **vor** der Umsetzung feststehen?
- Worin unterscheiden sich Lastenheft und Pflichtenheft?
- Wie grenzt man den Projektumfang sinnvoll ab (Stichwort „in scope / out of scope")?
- Was gehört zu einem belastbaren Soll-/Ist-Abgleich am Projektende?

**Gute Antwort:** Funktional = was das System können muss; nicht-funktional = wie gut (Geschwindigkeit, Sicherheit, Bedienbarkeit). SMART = spezifisch, messbar, erreichbar, relevant, terminiert — mit dem Kerngedanken, dass ohne Messgröße niemand die Zielerreichung objektiv belegen kann. Abnahmekriterien vorab, weil sich sonst am Ende der Erfolg nur behaupten lässt. Lastenheft = Auftraggeber (was), Pflichtenheft = Auftragnehmer (wie). Eine klare Umfangsabgrenzung nennt ausdrücklich, was **nicht** Teil des Projekts ist — genau das trennt „Erfassung gebaut" von „Deaktivierung versprochen". Ein belastbarer Abgleich stellt einzelne Anforderungen mit Soll, Ist und Nachweis gegenüber, statt pauschal „alles erfüllt" zu erklären.

---

## 13. Betrieb, Wartung und Zuverlässigkeit

**Anknüpfung:** Das System soll als wiederkehrender Quartalsprozess laufen; es gibt eine eigene Protokollierung, aber keine erwähnte Überwachung, und der Massenversand fand einmalig unter Aufsicht statt.

**Fragen:**
- Warum protokolliert man (Logging)? Was gehört ausdrücklich **nicht** ins Protokoll?
- Was ist Monitoring, und wie bemerkt man einen Ausfall rechtzeitig?
- Was versteht man unter Idempotenz und Wiederaufnahme bei einem abgebrochenen Vorgang?
- Warum trennt man Test- und Produktivumgebung, und was ist ein Rollback-Plan?
- Wie geht man beim Debuggen systematisch vor?
- Was gehört zu einer guten Betriebs-/Benutzerdokumentation für einen wiederkehrenden Prozess?

**Gute Antwort:** Logging dient Fehlersuche und Nachvollziehbarkeit; Passwörter, Schlüssel und personenbezogene Inhalte gehören nicht hinein. Monitoring meldet aktiv, wenn etwas klemmt — ohne das bleibt ein stiller Ausfall (z. B. eine liegengebliebene Sperre) unbemerkt. Idempotenz/Wiederaufnahme heißt: Ein abgebrochener Lauf lässt sich fortsetzen oder wiederholen, ohne Schaden (kein Doppelversand). Getrennte Umgebungen verhindern, dass Tests echte Daten oder echte Empfänger treffen; ein Rollback-Plan beschreibt, wie man einen fehlerhaften Stand rückgängig macht. Beim Debuggen sollte ein Vorgehen erkennbar sein: reproduzieren, eingrenzen, Hypothese, prüfen — nicht „ausprobieren". Gute Betriebsdoku klärt, wer den Prozess startet, überwacht und im Fehlerfall eingreift, samt Vertretung.

---

## Auswahlhilfe für den Ausschuss

| Wenn geprüft werden soll … | passender Block |
|---|---|
| technisches Grundverständnis Web | 1, 2, 6 |
| Kryptografie und Sicherheit | 3, 9 |
| Nebenläufigkeit (stärkstes Thema der Arbeit) | 4 |
| Daten und Datenhaltung | 5 |
| Denken in Architektur und Qualität | 7, 8 |
| Verantwortungsbewusstsein (Recht, Datenschutz) | 9, 10 |
| kaufmännisches und Projektverständnis | 11, 12 |
| Praxis im Betrieb und Team | 13 |

**Verzahnung mit den projektspezifischen Fragen:** Ein Block eignet sich gut als **Einstieg über das Allgemeine** mit anschließendem Wechsel ins Konkrete. Beispiel: erst Block 3 („Was ist der Unterschied zwischen Hashing und Verschlüsselung?"), dann die projektspezifische Frage 2.1 („Sie schreiben, die Kennung werde entschlüsselt — erklären Sie den Ablauf"). So zeigt sich, ob das allgemeine Wissen auch in der eigenen Umsetzung trägt.
