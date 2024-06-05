**Die Namen der Regatten und Boote, die sie gewonnen haben**

SELECT Teilnehmer.name
FROM Teilnehmer
JOIN main.Platzierung P on Teilnehmer.SegelNr = P.SEGELNR
WHERE platz = 1;

---



**Die Bootsfarbe, die am häufigsten vertreten ist und die SegelNr der Boote, die in dieser Farbe gestrichen oder lackiert sind**

WITH COUNT AS (SELECT Teilnehmer.Farbe, count(Teilnehmer.Farbe) as Anzahl
from Teilnehmer
group by Farbe)

MAX as (SELECT MAX(Anzahl) from COUNT)

Farbe as (SELECT farbe from teilnehmer
group by farbe
having count(farbe) == (select * from MAX))

SELECT f.farbe, t.segelnr
from farbe f join teilnehmer t on f.farbe = t.farbe

---



**Die Namen der Pokale, an denen keine Holzboote teilgenommen haben. Dabei soll jeder Name nur genau einmal vorkommen**

SELECT name
FROM Wettfahrt
WHERE FahrtNr NOT IN (
SELECT w.FahrtNr
FROM Bootsklasse b
JOIN Teilnehmer t ON b.Klasse = t.Bootsklasse
JOIN Platzierung p ON t.SegelNr = p.SEGELNR
JOIN Wettfahrt w ON p.WETTFAHRT = w.FahrtNr
WHERE b.Bauart = 'Holz'
);

---



**Finde alle Teilnehmer (SegelNr und Name), die an mindestens zwei verschiedenen Wettfahrten teilgenommen haben**

SELECT Teilnehmer.SegelNr, Teilnehmer.Name FROM Teilnehmer
JOIN Platzierung on Platzierung.SegelNr = Teilnehmer.Segelnr
GROUP BY Teilnehmer.SegelNr, Teilnehmer.Name
HAVING COUNT(Teilnehmer.SegelNr) >=2
ORDER BY Teilnehmer.SegelNr;

---



**Finde alle Teilnehmer (SegelNr und Eigner) welche keine Platzierung erlangt haben**

SELECT Teilnehmer.SEGELNR, Teilnehmer.Eigner FROM Teilnehmer
WHERE Teilnehmer.SegelNr NOT IN (SELECT Teilnehmer.SegelNr FROM Teilnehmer JOIN Platzierung on Platzierung.SEGELNR = Teilnehmer.SegelNr);
