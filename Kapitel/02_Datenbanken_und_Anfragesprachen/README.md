---
marp: true
theme: fhooe
header: Datenanalyse - Kapitel 2: Datenbanken und Anfragesprachen
footer: Dr. Georg Hackenberg, Professor für Informatik und Industriesysteme
paginate: true
math: mathjax
---

![bg right](./Titelbild.png)

# Kapitel 2: Datenbanken und Anfragesprachen

Dieses zweite Kapitel umfasst die folgenden Abschnitte:

1. Entity-Relationship-Modelle
1. Relationale Datenbanksysteme
1. Structured Query Language

---

![bg right](./Entity-Relationship-Modell.png)

## Entity-Relationship-Modelle

Grafische Beschreibung der relevanten Datenstrukturen einer Anwendungsdomäne:

- Hochschulen mit Studierenden, Lektoren und Kursen
- Betriebe mit Mitarbeitern, Projekten und Arbeitspaketen
- Banken mit Kunden, Konten und Transaktionen

---

### Konzepte

Entity-Relationship-Modelle umfassen die folgenden drei Konzepte für die Beschreibung der Datenstrukturen:

- **Entitäten** - Eindeutig identifizierbare Objekte einer Anwendungsdomäne wie der Kunde oder der Kurs
- **Attribute** - Eigenschaften der Objekte wie der der Name eines Kunden oder die Dauer eines Projekts
- **Beziehungen** - Die Verknüpfungen zwischen den Objekten wie die Zuweisung von Lektoren zu Kursen
- **Kardinalitäten** - Die Häufigkeit von Verknüpfungen (ein Kurs hat genau einen Lektor, ein Projekt hat mehrere Mitarbeiter)

---

![bg right](./Primärschlüssel-Eindeutigkeit.png)

### Attribute und Bedingungen

Des Weiteren können Bedingungen an Attribute geknüpft werden:

- **Primärschlüssel** - Das Attribut identifiziert die zugehörige Entität eindeutig (z.B. Matrikel-nummer eines/einer Student*in)
- **Eindeutigkeit** - Jede Entität hat für das Attribut einen anderen Wert (z.B. E-Mail-Adresse eines/ einer Student*in)

---

![bg right:45%](./Zusammengesetzter-Primärschlüssel.png)

### Zusammengesetzte Primärschlüssel

Auch eine Kombination mehrerer Attribute kann eine Entität eindeutig identifizieren:

- Längen- und Breitengrad identifiziert einen Ort auf dem Globus eindeutig
- Land, Ortschaft, Straße, Hausnummer identifiziert eine Postanschrift eindeutig
- Fluglinie, Quell- und Zielflughafen, sowie Abflugdatum und -uhrzeit identifizieren einen Flug eindeutig

---

### Beziehungen und Kardinalitäten

Des Weiteren kann man anhand der Kardinalitäten (`1` oder `m` bzw. `n`) mehrere Arten von Beziehungen zwischen zwei Entitäten `A` und `B` unterscheiden:

- **`1`-zu-`1` Beziehung** - Jede Entität `A` ist mit genau einer Entität `B` verknüpft und umgekehrt
  - z.B. *Studen\*innen* und seine/ihre *Prüfungsergebnisse*
- **`1`-zu-`n` Beziehung** - Jede Entität `A` ist potenziell mit mehreren Entitäten `B` verknüpft, jede Entität `B` aber nur mit genau einer Entität `A`
  - z.B. *Lektor\*innen* und seine/ihre *Lehrveranstaltungen*
- **`m`-zu-`n` Beziehung** - Jede Entität `A` ist potenziell mit mehreren Entitäten `B` verknüpft und umgekert
  - z.B. *Studiengänge* und ihr *Lehrverstaltungen*

---

<div class="columns">
<div class="two">

### Grundlegende grafische Notation

Das Beispiel auf der rechten Seite zeigt die grafische Notation von Entity-Relationship-Modellen:

- Die **Kästen** repräsentieren die Entitäten der Anwendungsdomäne und deren Attribute
  - Die Attribute sind in den Kästen in **Tabellenform** dargestellt
- Die **Verbindungslinien** repräsentieren die Relationen und deren Kardinalitäten
  - Die Kardinalitäten sind durch Symbole an den **Linienenden** dargestellt

</div>
<div>

![](./Diagramme/ER-Modell.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Grafische Notation der verschiedenen Kardinalitäten einer Beziehung

Die Beziehungen im Diagramm auf der rechten Seite sind wie folgt zu lesen:

- Jedes `A` hat genau ein `B`
- Jedes `B` hat maximal ein `A`
- Jedes `C` hat mindestens ein `D`
- Jedes `D` hat beliebig viele `C`

Die Darstellung definiert somit immer eine untere (`0` oder `1`) und eine obere (`1` oder `n`) Grenze.

</div>
<div>

![](./Diagramme/ER-Modell-Kardinalität.svg)

</div>
</div>

---

<div class="columns">
<div class="three">

### Darstellung von Bedingungen für Attribute

In der erweiterten grafischen Notation sind auch die Bedingungen für die Attribute enthalten:

- `PK` steht für eine Primärschlüssel-Bedingung (Englisch *primary key*)
  - z.B.Name des Studiengangs und Matrikelnummer des Teilnehmers
- `UK` steht für Eindeutigkeits-Bedingung (Englisch *uniqueness key*)
  - z.B. E-Mail-Adresse Teilnehmers

</div>
<div>

![](./Diagramme/ER-Modell-Schlüssel.svg)

</div>
</div>

---

<div class="columns">
<div>

### Darstellung von Kommentaren

Schließlich kann die Darstellung von Entity-Relationship-Modellen noch um Kommentare erweitert werden:

- Kommentare werden als zusätzliche Spalte in der Attributtabelle einer Entität dargestellt
- Kommentare sind somit für die detailliertere Beschreibung von Attributen gedacht

</div>
<div>

![](./Diagramme/ER-Modell-Kommentar.svg)

</div>
</div>

---

<div class="columns">
<div>

### Vererbung in der Programmierung

Aus der objektorientierten Programmierung kennen wir bereits das Prinzip der Vererbung:

- Elternklasse mit Gemeinsamkeiten und Kindklassen mit Spezifika
- Zum Beispiel haben sowohl Lektoren als auch Kursteilnehmer*innen einen Vornamen und einen Nachnamen
- Jedoch haben nur Lektoren eine Personalnummer

</div>
<div>

```csharp
class Person {
  string Vorname;
  string Nachname;
  string Email;
}

class Lektor : Person {
  string Personalnummer;
}

class Teilnehmer : Person {
  string Materialnummer;
}
```

</div>
</div>

---

<div class="columns">
<div>

### Abbildung von Vererbung in Entity-Relationship-Modellen

Prinzipien der Abbildung:

- **Elternentität** mit ID- und Typattribut sowie weiteren gemeinsamen Attributen, **Kindentitäten** mit ID-Attribut und spezifischen Attributen
- **`ist ein`-Beziehungen** (Kardinalität der *Elternentität* ist `genau 1`, Kardinalität der *Kindentität* ist `maximal 1`)


</div>
<div>

![](./Diagramme/ER-Modell-Generalisierung.svg)

</div>
</div>

---

![bg right](../Selbststudium.png)

### Aufgaben für das **Selbststudium**

So kannst du dein Verständnis noch weiter vertiefen:

- Erstelle ein Entity-Relationship-Modell für einen produzierenden Betrieb
- Erstelle ein Entity-Relationship-Modell für eine Social Media Plattform

---

![bg right](./Relationale-Datenbank.png)

## Relationale Datenbanksysteme

Speicherung von Daten in Form von Tabellen und Zugriff über lesende und schreibende Transaktionen:

1. Theorie: Relationale Algebra (Codd 1970)
1. Prototypische Implementierung: System R (IBM 1974)
1. Kommerzialisierung: Oracle (Relational Software 1979)

---

### Relationale Algebra (Edgar Codd, 1970)

Relationale Datenbanksysteme betrachten Datensätze als Tupel einer Relation, welche wiederum eine Teilmenge des kartesischen Produktes der Attributdomänen ist: 

- Domänen $D_i$ mit $n \in \mathbb{N}$ und $i \in \mathbb{N}$ entsprechen den Datentypen der Attribute einer Entität (z.B. `boolean`, `int` oder `string`)
- Relationen $R \subseteq D_1 \times ... \times D_n = \bigtimes_{i \leq n} D_i$ mit $n \in \mathbb{N}$ entsprechen den gesammelten Datensätzen (z.B. E-Mail-Kontakte)
- Tupel $t = (t_1, ..., t_n) \in R$ mit $n \in \mathbb{N}$ entsprechen einzelnen Datensätzen, die in einer Relation enthalten sind (z.B. E-Mail-Kontakt `X`)
- Variablen $t_i \in D_i$ entsprechen den Attributwerten der Tupel $(t_1, ..., t_n) \subseteq R$ mit $i \leq n$ und $i,n \in \mathbb{N}$ sowie Domänen $D_i$

---

![bg contain right:40%](./Diagramme/RDBMS-Idee.svg)

### Analogie mit Excel

Hier ist eine einfache Analogie für die Konzepte der relationalen Algebra:

- **Relationen** entsprichen zweidimensionalen Tabellen wie z.B. aus Excel bekannt
- **Domänen** entsprechen den Spalten der Tabelle und deren Datentypen
- **Tupel** entsprechen den einzelnen Zeilen der Tabellenstruktur
- **Variablen** entsprechen den Zellen der Tabellenstruktur

---

![bg contain right:40%](./Abbildung-ERM-RDBMS.png)

### Ableitung aus Entity-Relationship-Modell

Entity-Relationship-Modelle können einfach in Relationen überführt werden:

- *Entitäten* des Entity-Relationship-Modells werden zu *Relationen*
- *Attribute* des Entity-Relationship-Modells werden zu *Domänen*
- *Beziehungen* des Entity-Relationship-Modells werden abhängig von den Kardinalitäten zu Domänen für Fremdschlüssel und Hilfsrelationen

---

### Abbildung von Entitäten und Attributen

Das folgende Beispiel zeigt, wie Entitäten und deren Attribute auf Relationen und Domänen abgebildet werden:

<div class="columns top">
<div>

**Entität *Person***

![](./Diagramme/ER-Modell-Entität.svg)

</div>
<div class="three">

**Relation *Person* ($P \subseteq D_1 \times D_2 \times D_3 \times D_4$)**

| <ins>ID</ins> ($D_1$) | Vorname ($D_2$) | Nachname ($D_3$) | Email ($D_4$) |
|-|-|-|-|
| 1 | Roman | Froschauer | ...@fh-wels.at |
| 2 | Martin | Jordan | ...@fh-wels.at |
| 3 | Georg | Hackenberg | ...@fh-wels.at |
| ... | ... | ... | ... |

</div>
</div>

---

### Abbildung von `1`-zu-`1` Beziehungen

Zusätzliche *Domänen* für Fremdschlüssel in **beiden** Relationen (oder optional nur einer der beiden Relationen auch möglich):

<div class="columns top">
<div>

**ER-Modell**

![](./Diagramme/ER-Modell-Beziehung-1-zu-1.svg)

</div>
<div>

**Relation *Person***

| <ins>ID</ins> | <ins class="fk">Adresse_ID</ins> | Nachname | ... |
|-|-|-|-|
| 1 | 1 | Müller | ... |
| 2 | 2 | Muster | ... |
| 3 | 3 | Maier | ... |
| ... | ... | ... | ... |

</div>
<div>

**Relation *Adresse***

| <ins>ID</ins> | <ins class="fk">Person_ID</ins> | Ort |... |
|-|-|-|-|
| 1 | 1 | Wels | ... |
| 2 | 2 | Linz | ... |
| 3 | 3 | Steyr | ... |
| ... | ... | ... | ... |

</div>
</div>

---

### Abbildung von `1`-zu-`n` Beziehungen

Zusätzliche *Domänen* für Fremdschlüssel in **nur einer** Relation (welche mit der Kardinalität `n` auftritt):

<div class="columns top">
<div>

**ER-Modell**

![](./Diagramme/ER-Modell-Beziehung-1-zu-n.svg)

</div>
<div>

**Relation *Projekt***

| <ins>ID</ins> | Name | ... |
|-|-|-|
| 1 | Projekt A | ... |
| 2 | Projekt B | ... |
| 3 | Projekt C | ... |
| ... | ... | ... |

</div>
<div>

**Relation *Arbeitspaket***

| <ins>ID</ins> | <ins class="fk">Projekt_ID</ins> | Bezeichung | ... |
|-|-|-|-|
| 1 | 1 | Anforderungsanalyse | ... |
| 2 | 1 | Systementwurf | ... |
| 3 | 1 | Umsetzung | ... |
| ... | ... | ... | ... |

</div>
</div>

---

### Abbildung von `m`-zu-`n` Beziehungen

Zusätzliche *Relation* mit *Domänen* für Fremdschlüssel, die einen zusammengesetzten Primärschlüssel ergeben (plus optional weitere Beziehungsattribute):

<div class="columns top">
<div>

**ER-Modell**

![](./Diagramme/ER-Modell-Beziehung-m-zu-n.svg)

</div>
<div>

**Relation *Projekt***

| <ins>ID</ins> | Name | ... |
|-|-|-|
| 1 | Projekt A | ... |
| 1 | Projekt B | ... |
| 1 | Projekt C | ... |
| ... | ... | ... |

</div>
<div>

**Relation *ProPer***

| <ins class="pfk">Pro_ID</ins> | <ins class="pfk">Per_ID</ins> |
|-|-|
| 1 | 1 |
| 1 | 2 |
| 2 | 3 |
|-|-|


</div>
<div>

**Relation *Person***

| <ins>ID</ins> | Nachname | ... |
|-|-|-|
| 1 | Müller | ... |
| 1 | Muster | ... |
| 1 | Maier | ... |
| ... | ... | ... |

</div>
</div>

---

![bg right](./Relationale-Algebra.png)

### Relationale Operationen

Für die Auswertung der Daten gibt es folgende Operationen:

- **Unäre Operationen**: Projektion, Selektion und Gruppierung
- **Binäre Operationen**: Vereinigung, Schnittmenge, Differenz, Kartesisches Produkt, Join, Equi-Join, Natural Join, Left/Right/Full Outer Join, Division

---

### Unäre relationale Operationen

Unäre Operatonen arbeiten auf einer Relation $R \subseteq D_1 \times ... \times D_n$ mit $n \in \mathbb{N}$:

<div class="columns top">
<div>

**1. Projektion $\pi_\Delta(R)$**

![](https://upload.wikimedia.org/wikipedia/commons/3/31/Relational_Algebra_Single_Table_Projection.svg)

</div>
<div>

**2. Selektion $\sigma_P(R)$**

![](https://upload.wikimedia.org/wikipedia/commons/2/28/Relational_Algebra_Single_Table_Selection.svg)

</div>
<div>

**3. Gruppierung $\gamma_{\Delta,F}(R)$**

![](https://upload.wikimedia.org/wikipedia/commons/8/84/Relational_Algebra_Aggregation.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Projektion

Gegeben sei eine Relation $R \subseteq D_1 \times ... \times D_n$ sowie eine Teilmenge $\Delta \subseteq \{D_1, ..., D_n\}$ mit $n \in \mathbb{N}$. Wir definieren die Projektion $\pi_\Delta(R)$ wie folgt:

$\pi_\Delta(R) = \{ t_\Delta : t \in R \}$

Hierbei repräsentiert $t$ die Tupel der ursprünglichen Relation $R$ über $\{ D_1, ..., D_n \}$, während $t_\Delta$ das zugehörige Tupel über der Teilmenge $\Delta$ beschreibt.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/3/31/Relational_Algebra_Single_Table_Projection.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Selektion

Gegeben sei eine Relation $R \subseteq D_1 \times ... \times D_n$ sowie ein Prädikat $P: D_1 \times ... \times D_n \rightarrow \mathbb{B}$ mit $n \in \mathbb{N}$. Wir definieren die Selektion $\sigma_P(R)$ wie folgt:

<div class="box">

$\sigma_P(R) = \{ t \in R : P(t) = 1 \}$

</div>

Die Selektion $\pi_P(R)$ liefert also die Teilmenge derjenigen Tupel $t \in R$, welche das Prädikat $P$ erfüllen, d.h. $P(t) = 1$.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/2/28/Relational_Algebra_Single_Table_Selection.svg)

</div>
</div>

---

<div class="columns">
<div>

### Gruppierung

Gegeben sei eine Relation $R \subseteq A_1 \times ... \times A_k \times B_1 \times ... \times B_m$ mit einer Menge von Gruppierungsattributen $\Delta = \{A_1, ..., A_k\}$ und $m,k \in \mathbb{N}$.

Gegeben sei des Weiteren eine Liste $F = f_1,...,f_n$ von Aggregatfunktionen $f_i: \mathcal{P}(A_1 \times ... \times A_m \times B_1 \times ... \times B_k) \rightarrow C_i$ mit Aggregatdomänen $C_i$ und $n,i \in \mathbb{N}$.

$\mathcal{P}(\cdot)$ repräsentiert hier eine beliebe Teilmenge von Tupeln der Relation $R$.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/8/84/Relational_Algebra_Aggregation.svg)

</div>
</div>

---

![bg right:35%](./Gruppierung.png)

### Gruppierung (cont'd)

$m,n,k,i \in \mathbb{N}$

$R \subseteq A_1 \times ... \times A_m \times B_1 \times ... \times B_m$ (= Relation)
$\Delta = \{A_1, ..., A_k\}$ (= Gruppierungsattribute)
$F = f_1,...,f_n$ (= Aggregatfunktionen)

Wir definieren die Gruppierung $\gamma_{\Delta,F}(R)$ wie folgt:

<div class="box">

$\gamma_{\Delta,F}(R) = \{ r_{\{B_1, ..., B_m\}} \cup (f_1(x), ..., f_n(x)) :$
$r \in R \wedge x = \{ s \in R : s_{B_1,...,B_m} = r_{B_1,...,B_m} \} \}$

</div>

Beachte die Anwendung der Aggregatfunktionen auf die Teilmengen der Tupel $x \subseteq R$.

---

### Binäre relationale Operationen

Binäre relationale Operationen arbeiten auf zwei Relationen $R$ und $S$:

<div class="columns top">
<div>

**1. Identisches Schema**

<div class="box">

$n \in \mathbb{N}$

$R \subseteq D_1 \times ... \times D_n$
$S \subseteq D_1 \times ... \times D_n$

</div>

- Vereinigung
- Schnittmenge
- Differenz

</div>
<div>

**2. Beliebiges Schema**

<div class="box">

$m,n \in \mathbb{N}$

$R \subseteq A_1 \times ... \times A_m$
$S \subseteq B_1 \times ... \times B_n$

</div>

- Kartesisches Produkt
- Join
- Equi-Join

</div>
<div>

**3. Überschneidendes Sch.**

<div class="box">

$m,n,k,i \in \mathbb{N}$

$R \subseteq A_1 \times ... \times B_k$
$S \subseteq B_1 \times ... \times C_n$

</div>

- Natural Join
- Outer Join
- Division

</div>
</div>

---

<div class="columns">
<div class="two">

### Vereingung

Gegeben seien zwei Relationen $R,S \subseteq D_1 \times ... \times D_n$ mit $n \in \mathbb{N}$. Wir definieren die Vereinigung beider Relationen $R \cup S$ wie folgt:

<div class="box">

$R \cup S = \{ t : t \in R \vee t \in S \}$

</div>

Somit beinhaltet die Vereingung der beiden Relationen sowohl alle Tupel aus $R$, als auch alle Tupel aus $S$ (Duplikate existieren **nicht** doppelt).

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/1/17/Relational_Algebra_Set_Theory_Union.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Schnittmenge

Gegeben seien zwei Relationen $R,S \subseteq D_1 \times ... \times D_n$ mit $n \in \mathbb{N}$. Wir definieren die Schnittmenge beider Relationen $R \cap S$ wie folgt:

<div class="box">

$R \cap S = \{ t : t \in R \wedge t \in S \}$

</div>

Somit beinhaltet die Schnittmenge der beiden Relationen alle Tupel aus $R$, die gleichzeitig **auch** in $S$ enthalten sind.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/c/c0/Relational_Algebra_Set_Theory_Intersection.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Differenz

Gegeben seien zwei Relationen $R,S \subseteq D_1 \times ... \times D_n$ mit $n \in \mathbb{N}$. Wir definieren die Differenz beider Relationen $R \setminus S$ wie folgt:

<div class="box">

$R \setminus S = \{ t : t \in R \wedge t \notin S \}$

</div>

Somit beinhaltet die Differenz der beiden Relationen alle Tupel aus $R$, die **nicht** gleichzeitig auch in $S$ enthalten sind.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/e/e2/Relational_Algebra_Set_Theory_Difference.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Symmetrische Differenz

Gegeben seien zwei Relationen $R,S \subseteq D_1 \times ... \times D_n$ mit $n \in \mathbb{N}$. Wir definieren die symmetrische Differenz beider Relationen $R - S$ wie folgt:

<div class="box">

$R - S = (R \setminus S) \cup (S \setminus R$)

</div>

Somit beinhaltet die symmetrische Differenz der beiden Relationen alle Tupel aus $R$, die **nicht** gleichzeitig auch in $S$ enthalten sind, sowie alle Tupel aus $S$, die **nicht** gleichzeitig auch in $R$ enthalten sind.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/9/94/Relational_Algebra_Set_Theory_Symmetric_Difference.svg)

</div>
</div>

---

<div class="columns">
<div>

### Kartesisches Produkt

Gegeben seien zwei Relationen $R \subseteq A_1 \times ... \times A_m$ mit $m \in \mathbb{N}$ und $S \subseteq B_1 \times ... \times B_n$ mit $n \in \mathbb{N}$.

Wir definieren das kartesische Produkt beider Relationen $R \times S$ wie folgt:

<div class="box">

$R \times S : \{r \cup s : r \in R \wedge s \in S \}$

</div>

Somit wird jedes Tupel aus $R$ mit jedem Tupel aus $S$ kombiniert. Die Domänen der beiden Relationen können unterschiedlich sein.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/d/d6/Relational_Algebra_Cartesian_Product.svg)

</div>
</div>

---

<div class="columns">
<div>

### Join (*Allgemeines Prädikat $P$*)

Gegeben seien zwei Relationen $R \subseteq D_1 \times ... \times D_m$ mit $m \in \mathbb{N}$ und $S \subseteq D_{m+1} \times ... \times D_{m + n}$ mit $n \in \mathbb{N}$.

Gegeben sei des Weiteren ein Prädikat $P: D_1 \times ... \times D_{m+n} \rightarrow \mathbb{B}$ als Funktion über den kombinierten Domänen.

Wir definieren den Join beider Relationen mit Prädikat $R \bowtie_P S$ wie folgt:

<div class="box">

$R \bowtie_P S = \pi_P(R \times S)$

</div>

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/b/be/Relational_Algebra_Join.svg)

</div>
</div>

---

<div class="columns">
<div>

### Equi-Join (*Spezielles Prädikat $P$*)

Gegeben seien zwei Relationen $R \subseteq D_1 \times ... \times D_m$ mit $m \in \mathbb{N}$ und $S \subseteq D_{m+1} \times ... \times D_{m + n}$ mit $n \in \mathbb{N}$.

Gegeben seien des Weiteren $i,j \in \mathbb{N}$ mit $i \leq m$ und $m < j \leq m + n$.

Wir definieren den Equi-Join $R \bowtie_{i = j} S$ mit Prädikat $P(t) = (t_{\{D_i\}} = t_{\{D_j\}})$ wie folgt:

<div class="box">

$R \bowtie_{i = j} S = \pi_P(R \times S)$

</div>

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/6/65/Relational_Algebra_Join_Equijoin.svg)

</div>
</div>

---

<div class="columns">
<div>

### Natural Join (Prädikat + Projektion)

Gegeben seien zwei Relationen $R,S$ mit $R \subseteq A_1 \times ... \times A_m \times B_1 \times ... \times B_k$ und $S \subseteq B_1 \times ... \times B_k \times C_1 \times ... \times C_n$ sowie $m,n,k \in \mathbb{N}$.

Wir definieren den Natural Join $R \bowtie S$ der beiden Relationen wie folgt:

<div class="box">

$R \bowtie S = \{ r \cup s_{\{C_1,...,C_n\}} : r \in R \wedge$
$s \in S \wedge r_{\{B_1,...,B_k\}} = s_{\{B_1,...,B_k\}} \}$

</div>

Beachte, dass Duplikate der Domänen $B_1,...,B_k$ ausgeblendet werden.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/8/82/Relational_Algebra_Join_Natural_Join.svg)

</div>
</div>

---

<div class="columns">
<div>

### Left Outer Join

Gegeben seien zwei Relationen $R,S$ mit $R \subseteq A_1 \times ... \times A_m \times B_1 \times ... \times B_k$ und $S \subseteq B_1 \times ... \times B_k \times C_1 \times ... \times C_n$ sowie $m,n,k \in \mathbb{N}$.

Wir definieren den Left Outer Join $R \triangleright S$ der beiden Relationen wie folgt:

<div class="box">

$R \triangleright S = (R \bowtie S) \cup \{r \cup (\bot,...,\bot) :$
$r \in R \wedge \nexists s \in S : r_{\{B_1,...,B_k\}} = s_{\{B_1,...,B_k\}}\}$

</div>

Tupel aus $R$ **ohne** *Join-Partner* in $S$ sind trotzdem enthalten.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/0/01/Relational_Algebra_Outer_Join_Left_Outer_Join.svg)

</div>
</div>

---

<div class="columns">
<div>

### Right Outer Join

Gegeben seien zwei Relationen $R,S$ mit $R \subseteq A_1 \times ... \times A_m \times B_1 \times ... \times B_k$ und $S \subseteq B_1 \times ... \times B_k \times C_1 \times ... \times C_n$ sowie $m,n,k \in \mathbb{N}$.

Wir definieren den Right Outer Join $R \triangleleft S$ der beiden Relationen wie folgt:

<div class="box">

$R \triangleleft S = (R \bowtie S) \cup \{(\bot,...,\bot) \cup s :$
$s \in S \wedge \nexists r \in R : s_{\{B_1,...,B_k\}} = r_{\{B_1,...,B_k\}}\}$

</div>

Tupel aus $S$ **ohne** *Join-Partner* in $R$ sind trotzdem enthalten.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/6/62/Relational_Algebra_Outer_Join_Right_Outer_Join.svg)

</div>
</div>

---

<div class="columns">
<div>

### Full Outer Join

Gegeben seien zwei Relationen $R,S$ mit $R \subseteq A_1 \times ... \times A_m \times B_1 \times ... \times B_k$ und $S \subseteq B_1 \times ... \times B_k \times C_1 \times ... \times C_n$ sowie $m,n,k \in \mathbb{N}$.

Wir definieren den Full Outer Join $R \otimes S$ der beiden Relationen wie folgt:

<div class="box">

$R \otimes S = (R \triangleright S) \cup (R \triangleleft S)$

</div>

Es sind also Tupel aus $R$ **und** $S$ ohne *Join-Partner* enthalten.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/7/7d/Relational_Algebra_Outer_Join_Full_Outer_Join.svg)

</div>
</div>

---

<div class="columns">
<div>

### (Kartesische) Division

Gegeben seien zwei Relationen $R,S$ mit $R \subseteq A_1 \times ... \times A_m \times B_1 \times ... \times B_n$ und $S \subseteq B_1 \times ... \times B_n$ sowie $m,n \in \mathbb{N}$.

Wir definieren die Division $R \div S$ der beiden Relationen wie folgt:

<div class="box">

$R \div S = \{ r_{\{A_1,...,A_m\}} : r \in R \wedge$
$\exists s \in S : r_{\{B_1,...,B_n\}} = s\}$

</div>

Es sind **nur** Tupel aus $R$ mit *Join-Partner* in $S$ enthalten.

</div>
<div>

![](https://upload.wikimedia.org/wikipedia/commons/e/e0/Relational_Algebra_Cartesian_Division.svg)

</div>
</div>

---

![bg right](../Selbststudium.png)

### Aufgaben für das **Selbststudium**

So kannst du dein Verständnis noch weiter vertiefen:

- Überführen Sie die Entity-Relationship-Modelle aus dem vorigen Abschnitt in relationale Schemas
- Beschreiben Sie sinnvolle Abfragen auf den relationalen Schemas mittels den relationalen Operationen

---

![bg right](./Structured-Query-Language.png)

## Structured Query Language

TODO

---

![bg right](../Selbststudium.png)

### Aufgaben für das **Selbststudium**

So kannst du dein Verständnis noch weiter vertiefen:

- TODO