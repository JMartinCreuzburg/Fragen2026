**Klärungsfragen:**

1. In ihrem Umsetzungskonzept haben sie in ihrem Klassendiagramm die Klasse ContainerSortingLevel mit public Attributes designed. Erklären sie warum sie sich dafür entschieden haben. Gegen welches Designprinzip wird damit verstoßen?
2. Weiter haben die Methoden von AdvancedSortingPopUp den Zugriffsmodifizierer #. Was bedeutet das und welche Auswirkungen hat das auf ihr Design?
3. SIew haben sich in ihrer Arbeit mit Sortieralgorithmen auseinandergesetzt. Nennen Sie zwei Sortierverfahren und erklären Sie deren Grundprinzip in eigenen Worten.
4. Was bedeutet es, wenn ein Sortierverfahren „stabil" ist?
5. Wie wird die Komplexität von Sortieralgorithmen gemessen?  O-Notation
6. Was ist eine Schnittstelle (API) und wofür braucht man sie?
7. Welche API Konzepte kennen Sie?
8. Nach welchen Prinzipien folgt REST? 
9. Was bedeutet Zustandslosigkeit bei REST und was ist der Vorteil?


1. *Kap. 4.4 (S. 18–20):* „Beschreiben Sie, wie die Middleware mehrere Sortierstufen nacheinander anwendet. Wie behandeln Sie leere Werte oder unterschiedliche Datentypen in einer Spalte?" — Gute Antwort erklärt die Vergleichslogik (z. B. Verkettung von Vergleichern) und Sonderfälle.


**Vertiefungsfragen:**
1. *Abb. 12 (S. 17):* „Erklären Sie, was diese Datenverarbeitungskette tut und warum Sie den letzten Vergleichsschritt so gewählt haben.":
```
    prev?.parentElementPath === curr?.parentElementPath
    && prev?.definitionPath === curr?.definitionPath
    && prev?.sortingLevels === curr?.sortingLevels
```
 Kann der Vergleich fehlschlagen auch wenn der Inhalt von sortingLevels gleich ist?

 Prüfpunkt: Der Vergleich der Sortierstufen erfolgt dort über die Objektreferenz, nicht über den Inhalt; sortingLevels ist aber vermutlich ein Array oder Objekt. Bei Objekten/Arrays vergleicht === nur die Referenz, nicht den Inhalt. Zwei inhaltlich identische Arrays mit unterschiedlicher Referenz gelten als "verschieden".
