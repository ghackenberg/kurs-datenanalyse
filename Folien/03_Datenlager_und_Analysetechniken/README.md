---
marp: true
theme: fhooe
header: Datenanalyse - Kapitel 3: Datenlager und Analysetechniken
footer: Dr. Georg Hackenberg, Professor für Informatik und Industriesysteme
paginate: true
math: mathjax
---

![bg right](./Titelbild.png)

# Kapitel 3: Datenlager und Analysetechniken

Dieses dritte Kapitel umfasst die folgenden Abschnitte:

1. Datenbank-Schemata
1. Extract-Transform-Load
1. Online Analytical Processing

---

## Datenbank-Schemata

Es gibt drei typische Datenbank-Schemata für Datenanalysesysteme:

<div class="columns top">
<div>

**1. Sternschema**

![](./Sternschema.png)

</div>
<div>

**2. Schneeflockenschema**

![](./Schneeflockenschema.png)

</div>
<div>

**3. Galaxieschema**

![](./Galaxieschema.png)

</div>
</div>

---

<div class="columns">
<div>

### Sternschema

Das Sternschema ist wie folgt definiert:

- Mehrere **Dimensionstabellen** für die Dimensionenen, nach denen die Daten analysiert werden sollen
- Eine zentrale **Faktetabelle** mit den eigentlichen Kennwerten, die analysiert werden sollen
- Jeder Eintrag der Faktentabelle verweist auf die entsprechenden Einträge der Dimensionstabellen

</div>
<div>

![](./Diagramme/Sternschema.svg)

</div>
</div>

---

<div class="columns">
<div>

### Sternschema (Beispiel)

Das Beispiel auf der rechten Seite zeigt die Umsetzung des Sternschemas für die Analyse von Vertriebsdaten:

- Dimensionen sind verkauften **Produkte**, **Ort** des Umsatzes, und **Zeitpunkt**
- Fakten sind die verkaufte **Menge** und der erzielte **Umsatz** pro Produkt, Ort und Zeitpunkt

*(Beispieldaten auf der folgenden Folie!)*

</div>
<div>

![](./Diagramme/Sternschema_Beispiel.svg)

</div>
</div>

---

<div class="columns top">
<div>

**Tabelle *Fakt***

| <ins class="pfk">fk_1</ins> | <ins class="pfk">fk_2</ins> | <ins class="pfk">fk_3</ins> | Umsatz | Menge |
|-|-|-|-|-|
| 1 | 1 | 1 | 500€ | 5 |
| 1 | 1 | 2 | 450€ | 3 |
| 1 | 2 | 1 | 90€ | 1 |
| 1 | 2 | 2 | 140€ | 2 |
| 2 | 1 | 1 | ... | ... |
| 2 | 1 | 2 | ... | ... |
| 2 | 2 | 1 | ... | ... |
| 2 | 2 | 2 | ... | ... |

</div>
<div>

**Tabelle *Produkt***

| <ins>pk_1</ins> | Kategorie | Name |
|-|-|-|
| 1 | Thriller | Buch A |

**Tabelle *Ort***

| <ins>pk_2</ins> | Land | Bundesland | Stadt |
|-|-|-|-|
| 1 | AT | OÖ | Wels |

**Tabelle *Zeit***

| <ins>pk_3</ins> | Jahr | Quartal | Monat | Tag |
|-|-|-|-|-|
| 1 | 2025 | 1 | 2 | 14 |

</div>
</div>

---

<div class="columns">
<div>

### Dimensionshierarchie

Die Attribute der einzelnen Dimensionen modellieren in der Regel eine Hierarchie:

- **Dimension *Produkt*** - Kategorie und Name
- **Dimension *Ort*** - Land, Bundesland und Stadt
- **Dimension *Zeit*** - Jahr, Quartal, Monat und Tag

*Die Hierarchie ist für die Aggregation der Daten bei der Analyse wichtig!*

</div>
<div>

**Allgemein**

![](./Diagramme/Sternschema_Dimensionshierarchie_1.svg)

**Spezifisch**

![](./Diagramme/Sternschema_Dimensionshierarchie_2.svg)

</div>
</div>

---

### Dimensionsänderungen

Wenn eine Analysedatenbank über einen langen Zeitraum betrieben wird, kann es vorkommen, dass Änderungen an den Dimensionen durchgeführt werden sollen. Ralph Kimball hat 1996 in seinem Buch mehrere Arten beschrieben, damit umzugehen:

<div class="columns top">
<div>

**Typ 1 (Überschreiben)**

![](./Diagramme/Slowly_Changing_Dimensions_Type_1.svg)

</div>
<div>

**Typ 2 (Historisieren)**

![](./Diagramme/Slowly_Changing_Dimensions_Type_2.svg)

</div>
<div>

**Typ 3 (Erweitern)**

![](./Diagramme/Slowly_Changing_Dimensions_Type_3.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Dimensionsänderungen (Typ 1)

Im einfachsten Fall werden einfach die Einträge der Dimensionstabellen **überschrieben**.

Im Beispiel auf der Rechten Seite wird das **Land** und der **Name** eines **Lieferanten** nachträglich geändert.

**Beachte, dass dadurch ältere Fakten nachträglich einem neuen Land zugeordnet werden.**

Die Neuzuordnung kann in diesem Fall durchaus ein Problem darstellen, das man vermeiden möchte.

</div>
<div>

**Dimension *Lieferant***

*Vorher*

| <ins>PK</ins> | Land | Name |
|-|-|-|
| 1 | AT | Lieferant A |

*Nachher*

| <ins>PK</ins> | Land | Name |
|-|-|-|
| 1 | DE | Lieferant A' |

</div>
</div>

---

<div class="columns">
<div class="two">

### Dimensionsänderungen (Typ 2)

Um die Neuzuordnung von alten Fakten zu **vermeiden**, können die Einträge in der Dimensionstabelle **versioniert** werden.

Die Versionierung kann durch eine **zusätzliche Primärschlüsselspalte** für die Versionsnummer erreicht werden.

**Fakten** können (bzw. müssen) nun auf spezifische Versionen des Eintrags in der Dimensionstabelle verweisen.

*$\Rightarrow$ Evtl. Probleme bei Aggregation!*

</div>
<div>

**Dimension *Lieferant***

*Vorher*

| <ins>ID</ins> | <ins>Version</ins> | Land | Name |
|-|-|-|-|
| 1 | 0 | AT | Lieferant A |

*Nachher*

| <ins>ID</ins> | <ins>Version</ins> | Land | Name |
|-|-|-|-|
| 1 | 0 | AT | Lieferant A |
| 1 | 1 | DE | Lieferant A' |

</div>
</div>

---

<div class="columns">
<div class="two">

### Dimensionsänderungen (Typ 3)

Damit die Probleme bei der Aggre-gation zu umgehen, besteht auch die Möglichkeit, die Dimensions-tabelle zu **erweitern**.

Damit verweisen nun alle Fakten **auf denselben Eintrag** in der Dimen-sionstabelle, und der Eintrag bietet alle notwendigen Informationen.

*Die Tabelle kann jedoch sehr schnell sehr groß und ineffizient werden!*

</div>
<div>

**Dimension *Lieferant***

*Vorher*

| <ins>PK</ins> | Land | Name |
|-|-|-|
| 1 | AT | Lieferant |

*Nachher*

| <ins>PK</ins> | Land | LandNeu | Name | NameNeu |
|-|-|-|-|-|
| 1 | AT | Lieferant A | DE | Lieferant A' |

</div>
</div>

---

### Stern-Join

Die Abfrage von Daten aus Datenbanken, die nach dem Sternschema aufgebaut sind, folgt typischerweise einem einfachen Muster:

```sql
select Dimension_1.Attribut_1_1, ..., sum(Fakt.Kennwert_1), ...
    from Fakt
        inner join Dimension_1 on Fakt.fk_1 = Dimension_1.pk_1
        inner join Dimension_2 on Fakt.fk_2 = Dimension_2.pk_2
        inner join Dimension_3 on Fakt.fk_3 = Dimension_3.pk_3
        ...
    where <Bedingung>
    group by Dimension_1.Attribut_1_1, ...
    order by Dimension_1.Attribut_1_1, ..., sum(Fakt.Kennwert_1), ...
```

Die Faktentabelle wird zunächst mit den gewünschten Dimensionstabellen **gekreuzt**, bevor Fakten **selektiert** sowie die Fakten **gruppiert** und **sortiert** werden.

---

<div class="columns">
<div class="two">

### Dimensionsredundanz

Das Sternschema speichert die Werte der Attribute von Dimensionen (z.B. *Kategorie*) potenziell redundant ab:

- Die redundante Speicherung benötigt grundsätzlich relativ **viel Speicherplatz**
- Jedoch ist die redundante Speicherung bei Abfragen auch relativ **performant**

*Es gibt eine Alternative zum Sternschema, das die Redundanz auflöst $\Rightarrow$ das Schneeflockenschema!*

</div>
<div>

**Dimensionstabelle *Produkt***

| <ins>PK</ins> | Kategorie | Name |
|-|-|-|
| 1 | Thriller | Buchtitel A |
| 2 | Thriller | Buchtitel B |
| 3 | Science Fiction | Buchtitel C |
| 4 | Science Fiction | Buchtitel D |
| 5 | Sachbuch | Buchtitel E |
| 6 | Sachbuch | Buchtitel F |

</div>
</div>

---

<div class="columns">
<div>

### Schneeflockenschema

Beim Schneeflockenschema werden die Dimensionstabellen des Sternschemas potenziell in mehrere Tabellen zerlegt:

- Die Faktentabelle bleibt grund-sätzlich bestehen
- Die Dimensionstabellen könnten hingegen Untertabellen haben
- Die Hierarchie der Tabellen spiegelt die Dimensionshierarchie wieder

</div>
<div >

![](./Diagramme/Schneeflockenschema.svg)

</div>
</div>

---

<div class="columns">
<div>

### Schneeflockenschema (Beispiel)

Das Beispiel auf der rechten Seite zeigt die Umsetzung in der Praxis:

- Die Faktentabelle enthält wieder **Umsatz-** und **Stückzahlen**
- Die Fakten beziehen sich jeweils auf ein **Produkt** und einen **Kalendertag**
- Für das *Produkt* ist zusätzlich die **Kategorie** und die **Farbe** definiert
- Der *Kalendertag* ist verweißt hingegen auf **Monat** und **Jahr**

</div>
<div>

![](./Diagramme/Schneeflockenschema_Beispiel.svg)

</div>
</div>

---

<div class="columns">
<div>

**Dimension *Produkt***

| <ins>PK_1</ins> | <ins class="fk">FK_1_1</ins> | <ins class="fk">FK_1_2</ins> | ProduktName |
|-|-|-|-|
| 1 | 1 | 1 | Buch A

**Dimension *Kategorie***

| <ins>PK_1_1</ins> | KategorieName |
|-|-|
| 1 | Thriller |

**Dimension *Farbe***

| <ins>PK_1_2</ins> | FarbeName |
|-|-|
| 1 | Grün |

</div>
<div>

**Dimension *Tag***

| <ins>PK_2</ins> | <ins class="fk">FK_2_1</ins> | TagNummer |
|-|-|-|
| 1 | 1 | 15 |

**Dimension *Monat***

| <ins>PK_2_1</ins> | <ins class="fk">FK_2_1_1</ins> | MonatNummer |
|-|-|-|
| 1 | 1 | 8 |

**Dimension *Jahr***

| <ins>PK_2_1_1</ins> | JahrNummer |
|-|-|
| 1 | 2025 |

</div>
</div>

---

![bg right:40%](./Dimensionshierarchie.png)

### Dimensionshierarchie

Wie erwähnt, wird die Hierarchie der Dimen-sionen durch die Tabellenstuktur abgebildet:

- **Fakten** sind direkt mit der untersten Ebene der Hierarchie verbunden
- Jede **Ebene** der Hierarchie kann weitere Überebenen definieren und verweisen
- Mehrere **Überebenen** führen zu einer unabhängigen Verzweigung
- Die **obersten Ebenen** definieren die maximale Aggregationsstufe

---

![bg contain right](./Kimball.png)

### Dimensionsänderungen

Das **Problem** der Änderungen an den Einträgen der Dimensionstabellen bleibt grundsätzlich bestehen.

Genauso können die bereits eingeführten **Lösungsansätze** (Typ 1, 2 und 3) angewendet werden.

Änderungen beziehen sich dabei auf die entsprechenden **Ebenen** der Hierarchie (z.B. *Kategorie*).

---

### Schneeflocken-Join

Auch für das Schneeflockenschema gibt es wieder ein typisches SQL-Anfragemuster, welches deutlich mehr Join-Operationen benötigt als beim Sternschema:

```sql
select Dimension_1.Attribut_1_1, ..., sum(Fakt.Kennwert_1), ...
    from Fakt
        inner join Dimension_1 on Fakt.fk_1 = Dimension_1.pk_1
            inner join Dimension_1_1 on Dimension_1.fk_1_1 = Dimension_1_1.pk_1_1
                ...
            inner join Dimension_1_2 on Dimension_1.fk_1_2 = Dimension_1_2.pk_1_2
                ...
            ...
        inner join Dimension_2 on Fakt.fk_2 = Dimension_2.pk_2
            ...
    where <Bedingung>
    group by Dimension_1.Attribut_1_1, ...
    order by Dimension_1.Attribut_1_1, ..., sum(Fakt.Kennwert_1), ...
```

---

<div class="columns">
<div>

### Galaxieschema

TODO

</div>
<div>

![](./Diagramme/Galaxieschema.svg)

</div>
</div>

---

<div class="columns">
<div>

### Galaxieschema (Beispiel)

</div>
<div>

</div>
</div>

---

## Extract-Transform-Load

---

### Online Analytical Processing