## Fachgesprächs-Vorlage

**Klärungsfragen**
**Vertiefungsfragen**
1. *4.2 Datenmopdellierung (Klassendiagramm)*. UstvaStammdatenUnternehmer und -berater erbt von abstrakter Klasse UstvaStammdatenAdresse.Begründen Sie ihre fachliche Entscheidung für die Vererbungsbeziehung. Welche Alternativen Designmöglichkeiten hätte es gegeben und was sind die Vor- und Nachteile? Besser Komposition?


2. Sie haben einen neuen REST-Endpunkt angelegt. Warum haben Sie POST gewählt und nicht GET oder PUT?

> *Erwartet:* GET ist lesend und darf keinen Zustand ändern, Daten passen nicht in die URL; PUT ist idempotent und ersetzt eine bekannte Ressource. Hier wird eine Verarbeitung angestoßen und ein Vorgang erzeugt → POST. Begriff Idempotenz sollte fallen.
> 

3. Welche Restprinzipien kennen Sie? Was bedeutet Zustandslosigkeit und welche Vorteile hat das?

4. *RxJS (Abbildung 15):* Die ``next`` und ``error`` Methoden behandeln die Rückgabewerte des Observables. Was verbirgt sich hinter dem Begriff Observables? Skizzieren sie das Observer Pattern. Welche Probleme löst das Observer Pattern? Welche weiteren Entwurfsmuster kennen sie? 

5. Sie haben ein eigenes DTO definiert, statt die vorhandenen Modellklassen direkt entgegenzunehmen. Was ist ein DTO und welchen Vorteil bringt das?

> *Erwartet:* Reines Transportobjekt zwischen Schichten/Systemen. Entkopplung von internem Modell und externer Schnittstelle, kein ungewolltes Offenlegen interner Felder, API bleibt stabil bei Refactoring, gezielte Validierung. 

6. *Konvertierungskette (Abbildung 9/10):* Warum der Umweg DTO → ValueSheetRow → TaxCase statt DTO → TaxCase direkt? — Gute Antwort: Wiederverwendung der bestehenden `createTaxCase`-Logik, mit Bewusstsein für die Redundanz (vgl. Kapitel 6.4).

7. In Ihrem Controller steht praktisch keine Logik – die Verarbeitung liegt im Service. Warum ist diese Trennung sinnvoll?

> *Erwartet:* Schichtenarchitektur, Separation of Concerns; Controller = HTTP-Übersetzung, Service = Fachlogik. Nutzen: Testbarkeit ohne HTTP, Wiederverwendbarkeit der Logik, Austauschbarkeit der Schnittstelle, geringere Kopplung.

8. *6.4 Code Review* Sie schreiben: "*Durch die Reviews wurden mir auch neue Wege klar, durch die ich Redundanzen und unnötige Komplexität der DTOs und Logik in zukünftigen Iterationen vermeiden konnte.*". Erklären Sie warum.

9. *Kapitel 5.5:* dueFormulareEricSerive.validate validiert XML. Was passiert bei ungültigem XML genau, und warum ein früher Ausstieg (HTTP 422) statt einer Exception? — Gute Antwort: bewusster Kontrollfluss und bewusste Statuscode-Wahl.

10. **`createTaxCase`-Überladung (Abbildung 10):** Was bedeutet Überladen von Methoden? Warum haben sie sich für das Überladen entscheiden, statt einen eigenen Use Case für das UstvaObjekt zu erstellen?

*Nachfrage (Sicherheit, falls Zeit):* Sie verarbeiten Steuerdaten mehrerer Mandanten. Welche Schutzmaßnahmen sind hier zwingend?
> *Erwartet:* Authentifizierung/Autorisierung am Endpunkt (Token), Prüfung der Mandantenzugehörigkeit, TLS, Datenminimierung, keine Echtdaten in Testfällen, Aufbewahrungs-/Nachweispflichten.

11. Sie haben Unit-Tests und Integrationstests geschrieben. Worin unterscheiden sich beide, und wozu braucht man beide?

> *Erwartet:* Unit = kleinste Einheit isoliert, Abhängigkeiten gemockt, schnell, präzise Fehlerlokalisierung. Integration = Zusammenspiel mehrerer Komponenten inkl. HTTP/DB, langsamer, findet Fehler an den Schnittstellen. Testpyramide.

12. Ihre Testabdeckung muss eine vorgegebene Mindestgrenze erreichen. Warum ist eine hohe Abdeckung allein noch kein Beleg für gute Qualität?

> *Erwartet:* Coverage misst durchlaufene Zeilen/Zweige, nicht die Sinnhaftigkeit der Assertions; Tests ohne Prüfungen erhöhen die Quote. Entscheidend sind Fehler- und Grenzfälle (Äquivalenzklassen, Grenzwerte, Negativtests), nicht die Kennzahl.