# Allgemeine Fachthemen für das Fachgespräch
## Anknüpfend an die Projektdokumentation „Entwicklung eines interaktiven KI-Chatbots mit Sprachvisualisierung für eine VR-Anwendung"

**Fachrichtung:** Fachinformatiker Anwendungsentwicklung · **Prüfungsjahr:** 2026

**Zweck:** Ergänzung zu den projektbezogenen Fragen. Diese Themen kommen im Projekt vor, lassen sich aber **allgemein** abfragen — also unabhängig davon, was der Prüfling konkret gebaut hat. Damit lässt sich prüfen, ob hinter der Umsetzung breites Fachwissen steht.

**Hinweis zur Nutzung:** Pro Themenblock steht zuerst der Anknüpfungspunkt im Projekt (als Einstieg ins Gespräch), dann mögliche Fragen, dann eine kurze Orientierung, was eine gute Antwort enthalten sollte. Die Orientierung ist bewusst knapp und allgemeinverständlich gehalten, damit auch fachfremde Ausschussmitglieder die Antwort einordnen können.

**Empfehlung:** Zwei bis drei Themenblöcke genügen für ein Fachgespräch. Besonders naheliegend sind hier die Blöcke 2, 5, 7, 9 und 15.

---

## 1. Objektorientierung und Programmiergrundlagen

**Anknüpfung:** Die Anwendung besteht aus drei C#-Klassen mit eigenen Feldern und Methoden; im Anhang liegt ein Klassendiagramm.

**Fragen:**
- Was bedeuten Kapselung, Vererbung und Polymorphie? Bitte je ein Beispiel.
- Worin unterscheiden sich Klasse und Objekt?
- Wann verwendet man eine Schnittstelle (Interface) statt einer Basisklasse?
- Was ist der Unterschied zwischen einer statischen Methode und einer Instanzmethode?

**Gute Antwort:** Erklärt Kapselung als „Daten nach außen schützen und nur über Methoden zugänglich machen", nennt Vererbung als Weitergabe von Eigenschaften an spezialisierte Klassen und Polymorphie als „gleicher Aufruf, unterschiedliches Verhalten". Beim Interface fällt das Stichwort „Vertrag ohne Implementierung", oft verbunden mit Austauschbarkeit und Testbarkeit.

---

## 2. Nebenläufigkeit und asynchrone Programmierung ★

**Anknüpfung:** Alle Netzwerkaufrufe laufen über Coroutinen, also über eine Technik, die Arbeit über mehrere Einzelbilder verteilt, statt das Programm anzuhalten.

**Fragen:**
- Worin unterscheiden sich synchron, asynchron und parallel?
- Was ist ein Thread, was ein Prozess?
- Warum darf man den Haupt- bzw. Oberflächen-Thread nicht blockieren?
- Was ist eine Race Condition, und wie verhindert man sie?
- Was versteht man unter einem Deadlock?

**Gute Antwort:** Trennt sauber „asynchron = nicht warten, aber nicht zwingend gleichzeitig" von „parallel = tatsächlich gleichzeitig auf mehreren Kernen". Erklärt am Beispiel, dass eine blockierte Oberfläche einfriert und der Nutzer keine Rückmeldung mehr bekommt. Nennt bei der Race Condition den unkontrollierten gleichzeitigen Zugriff auf dieselbe Ressource und als Gegenmittel Sperren (Locks), Warteschlangen oder Transaktionen.

---

## 3. Softwarearchitektur und Entwurfsprinzipien

**Anknüpfung:** Die Anwendung ist in drei Komponenten aufgeteilt, die sich gegenseitig direkt aufrufen; der Prüfling schreibt selbst in den Lessons Learned, dass Schnittstellen stärker gekapselt werden sollten.

**Fragen:**
- Was bedeuten enge und lose Kopplung? Was ist Kohäsion?
- Nennen Sie ein Entwurfsmuster und wofür man es einsetzt.
- Was besagt das Prinzip der einzigen Verantwortlichkeit (Single Responsibility)?
- Welche Vorteile hat eine Schichtenarchitektur?

**Gute Antwort:** Beschreibt lose Kopplung als „Bausteine kennen voneinander möglichst wenig, dadurch austauschbar" und hohe Kohäsion als „ein Baustein macht eine Sache, die aber ganz". Bei den Mustern reichen Beobachter (Observer), Fassade oder Singleton mit einem Satz Einsatzzweck. Bei der Schichtenarchitektur sollten Trennung von Oberfläche, Logik und Datenzugriff sowie leichtere Wartung und Testbarkeit fallen.

---

## 4. UML und Modellierung

**Anknüpfung:** Die Arbeit enthält ein Zustandsdiagramm der Aufnahmesteuerung, ein Klassendiagramm und ein Diagramm des Datenflusses.

**Fragen:**
- Welche UML-Diagrammarten kennen Sie, und wofür ist welche gedacht?
- Worin unterscheidet sich ein Aktivitäts- von einem Sequenzdiagramm?
- Was gehört in ein Klassendiagramm?
- Wozu dient ein Anwendungsfalldiagramm (Use Case)?

**Gute Antwort:** Unterscheidet Struktur- von Verhaltensdiagrammen. Erklärt, dass ein Aktivitätsdiagramm den Ablauf einer Tätigkeit zeigt, ein Sequenzdiagramm dagegen den zeitlichen Nachrichtenaustausch zwischen Beteiligten. Beim Klassendiagramm sollten Attribute, Methoden, Sichtbarkeiten (öffentlich/privat) und Beziehungen mit Kardinalitäten genannt werden.

---

## 5. Netzwerk, HTTP und Schnittstellen ★

**Anknüpfung:** Das Programm spricht drei externe Dienste über HTTP an und verarbeitet deren Antworten; die Antwortzeit war das wichtigste Qualitätsziel.

**Fragen:**
- Welche HTTP-Methoden kennen Sie und wofür stehen sie? Was heißt idempotent?
- Was sagen die Statuscode-Gruppen 2xx, 4xx und 5xx aus?
- Was macht eine Schnittstelle zu einer REST-Schnittstelle?
- Erklären Sie das Client-Server-Prinzip.
- Was ist der Unterschied zwischen Latenz und Bandbreite?
- Was bringt HTTPS gegenüber HTTP?

**Gute Antwort:** Ordnet GET dem Lesen und POST dem Erzeugen bzw. Senden zu und erklärt idempotent als „mehrfach ausgeführt ändert sich nichts zusätzlich". Nennt 4xx als Fehler beim Aufrufer, 5xx als Fehler beim Server. Bei REST fallen Ressourcen, Zustandslosigkeit und die Nutzung der HTTP-Methoden. Latenz ist die Verzögerung, Bandbreite die Datenmenge pro Zeit — beides ist unabhängig voneinander.

---

## 6. Datenformate und Serialisierung

**Anknüpfung:** Die Dienste liefern ihre Antworten als JSON; die Audiodaten werden in ein Binärformat mit definiertem Dateikopf umgewandelt.

**Fragen:**
- Wofür steht JSON, wie ist es aufgebaut, welche Vor- und Nachteile hat es gegenüber XML?
- Was ist der Unterschied zwischen einem Text- und einem Binärformat?
- Warum braucht man Zeichenkodierungen wie UTF-8?
- Was versteht man unter Serialisierung?

**Gute Antwort:** Beschreibt JSON als schlankes, gut lesbares Format aus Schlüssel-Wert-Paaren, das kompakter ist als XML, aber weniger Prüfmöglichkeiten (Schema) mitbringt. Erklärt UTF-8 als Standard, der Sonderzeichen und Umlaute über Systemgrenzen hinweg korrekt darstellt. Serialisierung: Objekte im Speicher in eine übertragbare oder speicherbare Form bringen.

---

## 7. IT-Sicherheit ★

**Anknüpfung:** Die Zugangsschlüssel für die drei kostenpflichtigen Dienste werden mit der Anwendung auf das Endgerät ausgeliefert.

**Fragen:**
- Was ist der Unterschied zwischen Verschlüsselung und Hashing?
- Wann nutzt man symmetrische, wann asymmetrische Verschlüsselung?
- Worin unterscheiden sich Authentifizierung und Autorisierung?
- Wie bewahrt man Zugangsdaten und Schlüssel richtig auf?
- Was ist ein Man-in-the-Middle-Angriff?

**Gute Antwort:** Stellt klar, dass Verschlüsselung umkehrbar ist und Hashing nicht — Hashes dienen dem Vergleich, nicht der Wiederherstellung. Symmetrisch heißt ein gemeinsamer Schlüssel (schnell), asymmetrisch ein Schlüsselpaar (Austausch über unsichere Wege). Authentifizierung klärt „wer bist du", Autorisierung „was darfst du". Bei Schlüsseln sollten Umgebungsvariablen, Schlüsseltresore (Vaults) oder ein serverseitiger Zwischendienst genannt werden — niemals im ausgelieferten Programm.

---

## 8. Datenschutz und Recht

**Anknüpfung:** Sprachaufnahmen von Messebesuchern werden an drei externe Cloud-Dienste übertragen.

**Fragen:**
- Was sind personenbezogene Daten nach DSGVO? Ist eine Stimmaufnahme dazu zu zählen?
- Welche Grundsätze der DSGVO kennen Sie?
- Was ist eine Auftragsverarbeitung, und was gilt bei Anbietern außerhalb der EU?
- Worauf achtet man bei Open-Source-Lizenzen in kommerziellen Produkten?
- Was ist bei der Nutzung von KI-Werkzeugen im Unternehmen zu beachten?

**Gute Antwort:** Erkennt Sprachaufnahmen als personenbezogen (die Stimme ist ein biometrisches Merkmal, zudem sind Inhalte zuordenbar). Nennt Zweckbindung, Datenminimierung, Speicherbegrenzung und Transparenz. Weiß, dass für Dienstleister ein Auftragsverarbeitungsvertrag nötig ist und dass bei Anbietern außerhalb der EU zusätzliche Grundlagen erforderlich sind. Bei Lizenzen: Unterschied zwischen freizügigen Lizenzen (MIT, Apache) und solchen mit Rückwirkung auf den eigenen Code (GPL).

---

## 9. Testen und Qualitätssicherung ★

**Anknüpfung:** Die Arbeit enthält neun automatisierte Testfälle, einen durchgehenden Test der Verarbeitungskette und manuelle Prüfungen.

**Fragen:**
- Welche Teststufen gibt es, und was prüft jede?
- Worin unterscheiden sich Blackbox- und Whitebox-Test?
- Was ist ein Mock, und wozu braucht man ihn?
- Was sagt eine Testabdeckung aus — und was sagt sie nicht?
- Was ist ein Regressionstest, und warum ist er wichtig?
- Was versteht man unter testgetriebener Entwicklung?

**Gute Antwort:** Ordnet Unit-, Integrations-, System- und Abnahmetest richtig zu (vom kleinsten Baustein bis zur Abnahme durch den Auftraggeber). Blackbox prüft ohne Kenntnis des Innenlebens, Whitebox mit. Ein Mock ersetzt eine echte Abhängigkeit — etwa einen kostenpflichtigen Onlinedienst — durch einen kontrollierten Platzhalter, damit Tests schnell, kostenlos und wiederholbar laufen. Wichtig: Eine hohe Testabdeckung sagt nur, wie viel Code durchlaufen wurde, nicht ob richtig geprüft wurde.

---

## 10. Qualitätsmerkmale von Software

**Anknüpfung:** Die Arbeit enthält eine Tabelle mit Qualitätsmerkmalen wie Zuverlässigkeit, Effizienz, Wartbarkeit, Testbarkeit und Erweiterbarkeit.

**Fragen:**
- Was ist der Unterschied zwischen funktionalen und nicht-funktionalen Anforderungen?
- Was macht eine Anforderung SMART?
- Woran misst man Wartbarkeit ganz konkret?
- Warum sollten Abnahmekriterien vor der Umsetzung feststehen?

**Gute Antwort:** Funktional = was das System können muss; nicht-funktional = wie gut es das tut (Geschwindigkeit, Verfügbarkeit, Bedienbarkeit). SMART wird korrekt aufgelöst: spezifisch, messbar, erreichbar, relevant, terminiert — mit dem Kerngedanken, dass ohne Messgröße niemand die Zielerreichung objektiv belegen kann.

---

## 11. Vorgehensmodelle und Projektmanagement

**Anknüpfung:** Das Projekt folgt einem iterativ-inkrementellen Vorgehen mit Phasenplan und abschließendem Soll-/Ist-Vergleich.

**Fragen:**
- Wasserfallmodell gegenüber agilem Vorgehen — Vor- und Nachteile?
- Was bedeutet iterativ-inkrementell?
- Welche Rollen und Artefakte kennen Sie aus Scrum?
- Worin unterscheiden sich Lastenheft und Pflichtenheft?
- Wie schätzt man Aufwände, und wie geht man mit Abweichungen um?
- Was gehört zu einem Risikomanagement?

**Gute Antwort:** Beschreibt das Wasserfallmodell als klar planbar, aber unflexibel bei Änderungen, agile Verfahren als anpassungsfähig, aber mit höherem Abstimmungsaufwand. Iterativ = in Durchläufen verbessern, inkrementell = in funktionsfähigen Teilstücken liefern. Lastenheft kommt vom Auftraggeber (was), Pflichtenheft vom Auftragnehmer (wie). Beim Risikomanagement sollten Erkennen, Bewerten nach Eintrittswahrscheinlichkeit und Schadenshöhe sowie Gegenmaßnahmen genannt werden.

---

## 12. Wirtschaftlichkeit

**Anknüpfung:** Die Arbeit enthält eine Kostenaufstellung, eine gewichtete Nutzwertanalyse zur Auswahl der Dienste und eine Kosten-Nutzen-Betrachtung.

**Fragen:**
- Wie funktioniert eine Nutzwertanalyse, und wo liegen ihre Schwächen?
- Was bedeutet Amortisation, und wie berechnet man sie?
- Was steckt hinter einer Make-or-Buy-Entscheidung?
- Was ist der Unterschied zwischen einmaliger Investition und laufenden Betriebskosten?
- Wie schätzt man Nutzen, der sich nicht direkt in Euro ausdrücken lässt?

**Gute Antwort:** Erklärt die Nutzwertanalyse als Bewertung mehrerer Alternativen anhand gewichteter Kriterien und erkennt die Schwäche: Gewichtung und Punktvergabe sind subjektiv, das Ergebnis wirkt objektiver als es ist. Amortisation = Zeitraum, bis die Einsparungen die Anschaffungskosten decken. Bei Make-or-Buy sollten Kosten, Zeit, Abhängigkeit vom Anbieter und eigenes Know-how abgewogen werden.

---

## 13. Versionsverwaltung und Auslieferung

**Anknüpfung:** Der Quellcode wird mit Git verwaltet; die Anwendung wird gebaut und auf das Zielgerät übertragen.

**Fragen:**
- Wozu dient eine Versionsverwaltung? Was ist ein Branch, was ein Merge?
- Was passiert bei einem Merge-Konflikt, und wie löst man ihn?
- Was bedeuten CI und CD?
- Warum trennt man Entwicklungs-, Test- und Produktivumgebung?
- Wie sieht ein Rollback-Plan aus?

**Gute Antwort:** Nennt Nachvollziehbarkeit, Zusammenarbeit und die Möglichkeit, zu einem früheren Stand zurückzukehren. Branch = paralleler Entwicklungsstrang, Merge = Zusammenführen. Beim Konflikt: gleiche Stelle wurde doppelt geändert, die Auflösung erfolgt manuell. CI/CD = automatisiertes Bauen, Testen und Ausliefern.

---

## 14. Betrieb, Wartung und Support

**Anknüpfung:** Die Arbeit enthält eine Konfigurationsanleitung, ein Benutzerhandbuch und eine Liste typischer Fehlerszenarien.

**Fragen:**
- Warum protokolliert man (Logging)? Was gehört ausdrücklich **nicht** ins Protokoll?
- Was ist Monitoring, und wie merkt man einen Ausfall rechtzeitig?
- Wie gehen Sie beim Debuggen systematisch vor?
- Was gehört in eine gute Benutzerdokumentation?
- Was ist ein Service Level Agreement?

**Gute Antwort:** Logging dient der Fehlersuche und Nachvollziehbarkeit; Passwörter, Schlüssel und personenbezogene Daten gehören nicht hinein. Beim Debuggen sollte ein Vorgehen erkennbar sein: Fehler reproduzieren, eingrenzen, Hypothese bilden, prüfen — nicht „ausprobieren".

---

## 15. KI-Grundlagen ★

**Anknüpfung:** Das Projekt nutzt ein großes Sprachmodell, eine Spracherkennung und eine Sprachsynthese; die Antwortqualität wird über die Gestaltung der Anweisungen (Prompts) gesteuert.

**Fragen:**
- Was ist ein Token, was ein Kontextfenster?
- Was bedeutet Halluzination bei Sprachmodellen, und wie begegnet man ihr?
- Was ist der Unterschied zwischen Training und Inferenz?
- Was versteht man unter Prompt Engineering, und wo sind seine Grenzen?
- Welche Risiken entstehen, wenn Firmen- oder Kundendaten in Cloud-KI-Dienste fließen?
- Wann ist ein Sprachmodell die falsche Lösung für ein Problem?

**Gute Antwort:** Token als kleinste Verarbeitungseinheit (Wortteil), Kontextfenster als begrenzter „Arbeitsspeicher" des Modells. Halluzination = das Modell erfindet plausibel klingende, falsche Aussagen; Gegenmittel sind klare Anweisungen, das Mitgeben geprüfter Fakten und die Beschränkung auf belegte Quellen. Training erzeugt das Modell einmalig, Inferenz ist die spätere Nutzung. Gute Antworten erwähnen, dass ein Sprachmodell bei exakten, regelbasierten Aufgaben (Rechnen, verbindliche Auskünfte) die falsche Wahl ist.

---

## 16. Usability und Ergonomie

**Anknüpfung:** Lesbarkeit und Gestaltung der Textanzeige wurden als Entwurfskriterien festgelegt und manuell getestet.

**Fragen:**
- Was macht eine Benutzeroberfläche benutzerfreundlich?
- Wie führt man einen Usability-Test durch?
- Was versteht man unter Barrierefreiheit in der Softwareentwicklung?
- Warum ist Rückmeldung an den Nutzer (Feedback) bei längeren Wartezeiten wichtig?

**Gute Antwort:** Nennt Verständlichkeit, Konsistenz, Fehlertoleranz und Rückmeldung. Beim Usability-Test: echte Nutzer, realistische Aufgaben, beobachten statt erklären. Bei Barrierefreiheit sollten Kontraste, Schriftgrößen und alternative Bedienwege fallen.

---

## Auswahlhilfe für den Ausschuss

| Wenn geprüft werden soll … | passender Block |
|---|---|
| technisches Grundverständnis | 1, 2, 5, 6 |
| Denken in Architektur und Qualität | 3, 4, 9, 10 |
| Verantwortungsbewusstsein (Sicherheit, Recht) | 7, 8 |
| kaufmännisches und Projektverständnis | 11, 12 |
| Praxis im Betrieb und Team | 13, 14 |
| Aktualität und Einordnung neuer Technik | 15 |
| Blick für den Anwender | 16 |
