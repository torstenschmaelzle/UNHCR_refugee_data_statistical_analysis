# Wohlstand und relative Flüchtlingsaufnahme

## Ziel der Analyse
Dieses Notebook untersucht den Zusammenhang zwischen dem durchschnittlichen Wohlstand eines Landes (gemessen als GDP pro Kopf) und seiner durchschnittlichen relativen Flüchtlingsaufnahme (`refugee_share`) im Zeitraum von 2000 bis 2019.  
Die Analyse erfolgt sowohl weltweit als auch getrennt nach Kontinenten.

## Datengrundlage und Methodik
- Zeitraum: 2000–2019  
- Aggregation auf Länderebene (eine Beobachtung pro Land)
- Logarithmierung des GDP pro Kopf zur besseren Vergleichbarkeit
- Grafische Analyse mittels Scatterplots
- Lineare Regression und t-Test zur Überprüfung statistischer Zusammenhänge

## Zentrale Ergebnisse
- Weltweit zeigt sich kein statistisch signifikanter Zusammenhang zwischen dem durchschnittlichen Wohlstand eines Landes und seiner relativen Flüchtlingsaufnahme.
- Die geschätzte Regressionssteigung ist sehr klein und der p-Wert deutlich über gängigen Signifikanzniveaus, sodass die Nullhypothese nicht verworfen werden kann.
- Auch bei einer getrennten Analyse nach Kontinenten ergibt sich kein robuster oder konsistenter Zusammenhang.
- Die Scatterplots zeigen eine starke Streuung mit wenigen Ausreißern, während die Mehrheit der Länder unabhängig vom Wohlstandsniveau eine sehr geringe relative Flüchtlingsaufnahme aufweist.

## Interpretation
Die Ergebnisse deuten darauf hin, dass der durchschnittliche wirtschaftliche Wohlstand eines Landes allein kein guter Prädiktor für dessen relative Flüchtlingsaufnahme ist.  
Andere Faktoren wie geografische Lage, regionale Konflikte, politische Entscheidungen oder institutionelle Rahmenbedingungen scheinen eine deutlich wichtigere Rolle zu spielen.

## Fazit
Die empirische Analyse liefert keine Evidenz für einen systematischen Zusammenhang zwischen Wohlstand und Flüchtlingsaufnahme.  
Damit wird die Nullhypothese sowohl auf globaler als auch auf kontinentaler Ebene gestützt.
