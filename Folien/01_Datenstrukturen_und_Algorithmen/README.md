---
marp: true
theme: fhooe
header: Datenanalyse - Kapitel 1: Datenstrukturen und Algorithmen
footer: Dr. Georg Hackenberg, Professor für Informatik und Industriesysteme
paginate: true
math: mathjax
---

![bg right](./Titelbild.png)

# Kapitel 1: Daten-strukturen und Algorithmen
Dieses erste Kapitel umfasst die folgenden drei Abschnitte:

1. Listen
1. Graphen
1. Bäume

---

![bg right](./Liste.png)

## Listen

Geordnete (d.h. nummerierte) Sammlung von Elementen eines gegebenen Datentyps:

- Nachrichten im Eingang des eigenen Postfachs
- Adressen von Kontakten in einer Kontaktdatenbank
- Posten mit Stückzahlen und Preisen einer Rechnung

---

### Konzeptualisierung

Im Folgenden benötigen wir einige Begriffe, um über Listen sprechen zu können. Unsere Konzeptualisierung umfasst die folgenden Begriffe:

- *Listenelement* - Eintrag in einer Liste mit seinen zugehörigen Informationen (z.B. Name des Kontakts oder Preis des Rechnungspostens)
- *Listenindex* - Eindeutige Nummer des Eintrags einer Liste, über die auf den Eintrag zugegriffen werden kann
- *Listenkopf* - Erster Eintrag einer Liste (d.h. der Eintrag mit der Nummer 1 bzw. 0 in Rechnersystemen)
- *Listenlänge* - Anzahl der Einträge in einer Liste (hast den Wert 0, wenn die Liste keine Einträge hat!)

---

### Formalisierung

Listen können basiertend auf der Menge der Datenelemente $D$ und der Menge der natürlichen Zahlen $\mathbb{N}$ (ohne Null) als **Folgen** oder **Sequenzen** formalisiert werden:

- Die Menge der **endlichen Sequenzen definierter Länge** $n \in \mathbb{N}$ über $D$ ist definiert als $D^n = \{ \{i \in \mathbb{N}: i \leq n\} \rightarrow D \}$
- Die Menge der **endlichen Sequenzen beliebiger Länge** über $D$ ist definiert als $D^\# = \bigcup_{n \in \mathbb{N}} D^n$
- Die Menge der **endlichen Listen beliebiger Länge** über $D$ ist definiert als $l(D) = \{(n, S) \in \mathbb{N} \times E^* | S \in E^n\}$
  - *Listen merken sich in diesem Formalismus explizit ihre Länge, während Sequenzen diese Information nicht mit sich tragen*

---

<div class="columns">
<div class="three">

### Programmierschnittstelle

Und so sieht die Programmierschnittstelle in C# aus:

```csharp
class List<T> {
    private Element<T> head = null;
    public int length() { ... }
    public T get(int index) { ... }
    public T remove(int index) { ... }
    public void append(T element) { ... }
    public void prepend(T element) { ... }
    public void replace(int index, T element) { ... }
    public boolean contains(T element) { ... }
}

class Element<T> {
    public T value;
    public Element<T> next;
}
```

</div>
<div>

![](./Diagramme/Liste.svg)

</div>
</div>

---

### Anwendungsbeispiel

```csharp
// Erzeuge eine neue Listenstruktur
List<string> list = new List();

// Füge der Listenstruktur Elemente hinzu
list.append("A");
list.append("B");

// Berechne die Länge der Liste und gib den Wert auf der Konsole aus
Console.WriteLine(list.length());

// Lösche das erste Element aus der Liste
list.remove(0);

// Ersetze das erste Element der Liste
list.replace(0, "C");
```

---

<div class="columns">
<div class="four">

### Implementierung der Methode `length()`

```csharp
public int length() {
    // Iterator mit dem Listenkopf initialisieren
    Element<T> iterator = head;
    // Zähler mit dem Wert Null initialisieren
    int counter = 0;
    // Solange iterieren, bis Listenende erreicht ist
    while (iterator != null) {
        // Iterator auf das nächste Listenelement setzen
        iterator = iterator.next;
        // Zähler um den Wert Eins erhöhen
        counter++;
    }
    // Wert des Zählers zurückgeben
    return counter;
}
```

</div>
<div>

![](./Diagramme/Liste_get.svg)

</div>
</div>

---

### Implementierung der Methode `get(...)`

```csharp
public T get(int index) {
    // Iterator mit dem Listenkopf initialisieren
    Element<T> iterator = head;
    // Solange iterieren, bis gewünschtes Listenelement oder Listenende erreicht ist
    while (index > 0 && iterator != null) {
        // Iterator auf das nächste Listenelement setzen
        iterator = iterator.next;
        // Index um den Wert eins reduzieren
        index--;
    }
    // Wenn Index nicht gültig war, eine Ausnahme werfen
    if (iterator == null) {
        throw new Exception("Index out of bounds.");
    }
    // Wert des aktuellen Listenelements zurückgeben
    return iterator.value;
}
```

---

### Implementierung der Methode `remove(...)`

```csharp
public T remove(int index) {
    // Prüfe, ob Listenkopf entfernt werden soll oder nicht
    if (index == 0) {
        // Wenn Liste leer ist, werfe eine Ausnahme
        if (head == null) {
            throw new Exception("Index out of bounds.")
        }
        // Merke den aktuellen Listenkopf
        Element<T> temp = head;
        // Setze den Listenkopf auf das zweite Listenelement
        head = head.next;
        // Gebe den Wert des ursprünglichen Listenkopfs zurück
        return temp.value;
    } else {
        // (siehe nächste Folie!)
    }
}
```

---

### Implementierung der Methode `remove(...)` (cont'd)

```csharp
public T remove(int index) {
    // Prüfe, ob Listenkopf entfernt werden soll oder nicht
    if (index == 0) {
        // (siehe vorige Folie!)
    } else {
        // Bestimme das vorangehende Listenelement
        Element<T> prev = get(index - 1);
        // Bestimme das zu entfernende Listenelement
        Element<T> temp = prev.next;
        // Aktualisiere den Nachfolger des Vorgängers
        prev.next = temp.next;
        // Gib den Wert des zu entfernenden Listenelements zurück
        return temp.value;
    }
}
```

---

### Implementierung der Methode `contains(...)`

```csharp
public boolean contains(T element) {
    // Iterator mit dem Listenkopf initialisieren
    Element<T> iterator = head;
    // Prüfen, ob Listenende erreicht wurde
    while (iterator != null) {
        // Prüfen, ob der aktuelle Listeniterator den gesuchten Wert hat
        if (iterator.value == element) {
            // Falls ja, wahr zurückgeben
            return true
        }
        // Den Iterator auf das nächste Listenelement setzen
        iterator = iterator.next;
    }
    // Der gewünschte Wert wurde nicht gefunden, also falsch zurückgeben
    return false;
}
```

---

![bg contain right:50%](./Diagramme/Liste_von_Listen.svg)

### Geschachtelte Listen

Die Elemente einer Liste können wiederum Listen sein, sodass eine Schachtelung entsteht:

- Eine einfache Schaltelung ergibt eine zweidimensionale Datenstruktur
- Eine zweifache Schachtelung ergibt eine dreidimensionale Datenstruktur

---

### Anwendungsbeispiel

```csharp
// Erzeuge eine Liste von Listen
List<List<string>> list = new List();

// Füge der Liste zwei Unterlisten hinzu
list.append(new List<string>());
list.append(new List<string>());

// Füge der ersten Unterliste zwei Element hinzu
list.get(0).append("A");
list.get(0).append("B");

// Gib das erste und zweite Element der ersten Unterliste auf der Konsole aus
Console.WriteLine(list.get(0).get(0));
Console.WriteLine(list.get(0).get(1));

// Berechne die Länge der zweiten Unterliste und gib den Wert auf der Konsole aus
Console.WriteLine(list.get(1).length());
```

---

![bg right](../Selbststudium.png)

### Aufgaben für das **Selbststudium**

Um Ihr Verständnis zu vertiefen, widmen Sie sich den folgenden Aufgaben im Selbststudium:

- Implementierung der Methode `void append(T element)`
- Implementierung der Methode `void prepend(T Element)`
- Implementierung der Methode `void replace(int index, T element)`

---

![bg right](./Graphen.png)

## Graphen

Strukturen bestehend aus Knoten (die Elemente) und Kanten (die Beziehungen zwischen Elementen):

- Soziale Netzwerke wie LinkedIn oder Instragram
- Verkehrsnetze für Fahrzeuge, Flugzeuge, Schiffe, usw.
- Kommunikationsnetze für Telefon und Internet

---

### Formalisierung

Im Folgenden benötigen wir einen Formalismus, um Graphen und ihre Eigenschaften beschreiben zu können. Unser Formalismus umfasst die folgenden Elemente:

- $V$ ist die Menge der Knoten
  - $v \in V$ ist ein spezieller Knoten
- $E \subseteq V \times V$ ist die Menge der Kanten als Teilmenge des karthesischen Produkts
  - $(u, v) = e \in E$ ist eine spezielle Kante von Knoten $u$ nach Knoten $v$
  - Kanten haben in diesem Formalismum **eine Richtung** und **kein Gewicht**
  - *Es gibt auch Varianten ohne Richtung und mit Gewichtung!*
- $G = (V, E)$ ist der Graph selbst bestehend aus Knoten- und Kantenmenge

---

![bg contain right:50%](./Diagramme/Adjazenzmatrix.svg)

### Exkurs: Adjazenzmatrix

Die Kantenmenge $E \subseteq V \times V$ kann als sogenannte Adjazenzmatrix $A$ mit Einträgen $A_{i,j}$ dargestellt werden:

- Die **Spalten** $i$ und **Zeilen** $j$ dieser Matrix repräsentieren die Knoten des Graphen
- Die **Zellen** $A_{i,j}$ der Matrix mit den Einträgen $1$ repräsentieren die Kanten des Graphen

---

![bg contain right:40%](./Diagramme/Graph_Hülle.svg)

### Transitive Hülle

Um bestimmen zu können, ob ein Pfad von Knoten $u \in V$ zu Knoten $v \in V$ führt, hilft das Konzept der transitiven Hülle $t(E) \subseteq V \times V$ mit $(u, v) \in t(E)$ genau dann, wenn

- **Fall 1 (direkte Verbindung)** - Es existiert eine Kante $(u, v) \in E$ oder
- **Fall 2 (indirekte Verbindung)** - Es existiert ein Zwischenknoten $w \in V$ mit $(u, w) \in t(E)$ und $(w, v) \in t(E)$

---

<div class="columns">
<div class="two">

### Zyklische und azyklische Graphen

Für manche Berechnungen ist es wichtig zwischen zyklischen und azyklischen Graphen $G = (V, E)$ zu unterscheiden:

- In **azyklischen Graphen** gibt es *keinen* Pfad von einem Knoten zu sich selbst, das heißt es gilt $\forall (u, v) \in t(E) : u \neq v$
- Für **zyklische Graphen** gibt es mindestens einen Pfad von einem Knoten zu sich selbst, das heißt es gilt $\exists (u, v) \in t(E) : u = v$

</div>
<div>

![](./Diagramme/Graph_Zyklisch_Azyklisch.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Programmierschnittstelle

Und so sieht eine mögliche Umsetzung in C# aus:

```csharp
class Graph<T> {
    private List<Node<T>> nodes;

    public Node<T> addNode(T value) { ... }
    public void removeNode(Node<T> node) { ... }

    public boolean hasEdge(Node<T> source, Node<T> target) { ... }
    public void addEdge(Node<T> source, Node<T> target) { ... }
    public void removeEdge(Node<T> source, Node<T> target) { ... }

    public boolean hasPath(Node<T> source, Node<T> target) { ... }
}

class Node<T> {
    public T value;
    public List<Node<T>> neighbors;
}
```

</div>
<div>

![](./Diagramme/Graph.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Anwendungsbeispiel

```csharp
// Erstelle eine neue Graphenstruktur
Graph<string> graph = new Graph();

// Füge dem Graphen seine Knoten hinzu
var a = graph.addNode("A");
var b = graph.addNode("B");
var c = graph.addNode("C");

// Füge dem Graphen seine Kanten hinzu
graph.addEdge(a, b);
graph.addEdge(b, c);

// Prüfe, ob eine Kante zwischen zweo Knoten existiert
Console.WriteLine(graph.hasEdge(a, c));

// Prüfe, ob ein Pfad zwischen zwei Knoten existiert
Console.WriteLine(graph.hasPath(a, c));
```

</div>
<div>

![](./Diagramme/Graph_Beispiel.svg)

</div>
</div>

---

### Implementierung der Methode `addNode(...)`

Und so könnte die Methode aussehen, die dafür verantwortlich ist, neue Knoten in die Graphenstruktur einzufügen:

```csharp
public Node<T> addNode(T value) {

    // Neues Knotenobjekt erzeugen
    Node<T> node = new Node();
    // Wert des Knoteobjektes initialisieren
    node.value = value;
    // Knotenobjekt in die Knotenliste aufnehmen
    nodes.append(node);
    // Knotenobjekt zurückgeben
    return node;

}
````

---

<div class="columns">
<div class="two">

### Implementierung der Methode `hasEdge(...)`

Bevor wir eine Verbindung zwischen zwei Knoten hinzufügen, sollten wir prüfen, ob die Kante bereits exisiert:

```csharp
public void hasEdge(Node<T> source, Node<T> target) {
    // Durchlaufe alle Nachbarknoten
    foreach (Node<T> neighbor in source.neighbors) {
        // Prüfe, ob der Nachbarknoten der Zielknoten ist
        if (neighbor == target) {
            // Wenn ja, geben wahr zurück
            return true;
        }
    }
    // Wenn der Zielknoten nicht gefunden wurde, gebe falsch zurück
    return false;
}
```

</div>
<div>

![](./Diagramme/Graph_hasEdge.svg)

</div>
</div>

---

### Implementierung der Methode `addEdge(...)`

Nun können wir auf Basis der vorigen Methoden die Methode für das Hinzufügen von neuen Kanten definieren:

```csharp
public void addEdge(Node<T> source, Node<T> target) {
    // Prüfen, ob die Kante bereits erstellt wurde
    if (hasEdge(source, target)) {
        // Falls ja, eine Ausnahme werfen
        throw new Exception("Edge already exists!");
    }
    // Ansonsten den Zielknoten in die Liste der Nachbarknoten eintragen
    source.neighbors.append(target);
}
```

---

<div class="columns">
<div class="two">

### Implementierung der Methode `hasPath(...)` für **azyklische Graphen**

Für azyklische Graph könnte die Implementierung der Methode `hasPath(...)` als einfache rekursive Funktion erfolgen:

```csharp
public boolean hasPath(Node<T> source, Node<T> target) {
    // Prüfe, ob eine direkte Verbindung (d.h. eine Kante) existert
    if (hasEdge(source, target)) {
        // Wenn ja, gib wahr zurück
        return true
    }
    // Prüfe, ob einer der Nachbarknoten eine Pfad zum Zielknoten hat
    foreach (Node<T> neighbor in source.neighbors) {
        // Führe die Pfadsuche rekursiv durch (Vorsicht bei zyklischen Graphen!)
        if (hasPath(neighbor, target)) {
            // Falls ein Pfad gefunden wurde, gib wahr zurück
            return true
        }
    }
    // Fall weder direkter noch indirekter Pfad gefunden wurde, gib falsch zurück
    return false
}
```

</div>
<div>

![](./Diagramme/Graph_hasPath_azyklisch.svg)

</div>
</div>

---

<div class="columns">
<div class="six">

### Problem bei **zyklischen Graphen**

Im rechten Beispiel würde der Aufruf der naiven Implementierung von `hasPath(A, D)` zu folgender Endlosrekursion führen:

- **`hasPath(A, D)`** führt zu:
    - `hasPath(B, D)` führt zu:
      - **`hasPath(A, D)`** führt zu:
        - `hasPath(B, D)` führt zu:
          - ...
      - *`hasPath(C, D)` wird nie erreicht!*

</div>
<div>

![width:100](./Diagramme/Graph_Zyklisch.svg)

</div>
</div>

---

### Implementierung der Methode `hasPath(...)` für **zyklische Graphen**

```csharp
public boolean hasPath(Node<T> source, Node<T> target, List<Node<T>> visited = new List<T>()) {
    // Prüfe, ob eine direkte Verbindung (d.h. eine Kante) existert
    if (hasEdge(source, target)) {
        // Wenn ja, gib wahr zurück
        return true
    }
    // Markiere den Quellknoten als besucht (damit er in einem rekursiven Aufruf nicht noch einmal besucht wird)
    visited.append(source);
    // Prüfe, ob einer der Nachbarknoten eine Pfad zum Zielknoten hat
    foreach (Node<T> neighbor in source.neighbors) {
        // Prüfe, ob der Nachbarknoten bereits in einem vorigen Rekursionsschritt besucht wurde
        if (!visited.contains(source)) {
            // Führe die Pfadsuche rekursiv durch und übergebe die Liste der besuchten Knoten
            if (hasPath(neighbor, target, visited)) {
                // Falls ein Pfad gefunden wurde, gib wahr zurück
                return true
            }
        }
    }
    // Fall weder ein direkter noch ein indirekter Pfad gefunden wurde, gib falsch zurück
    return false
}
```

---

<div class="columns">
<div class="six">

### Illustration der Lösung

Hier wieder der Rekursionsbaum für den Aufruf der Methode `hasPath(A, B)`:

- `hasPath(A, D, [])` führt zu:
  - `hasPath(B, D, [A])` führt zu:
    - *`hasPath(A, D, [A, B])` wird **nicht** ausgeführt, da Knoten `A` bereits besucht!*
    - `hasPath(C, D, [A, B])` wird hingegen als nächstes ausgeführt und liefert den Wert `true`, da eine direkte Verbindung zwischen Knoten `C` und `D` besteht

</div>
<div>

![width:100](./Diagramme/Graph_Zyklisch.svg)

</div>
</div>

---

![bg right](../Selbststudium.png)

### Aufgaben für das **Selbststudium**

Um Ihr Verständnis zu vertiefen, widmen Sie sich den folgenden Aufgaben im Selbststudium:

- Implementierung der Methode `void removeNode(Node<T> node)`
- Implementierung der Methode `void removeEdge(Node<T> source, Node<T> target)`

---

![bg right](./Bäume.png)

## Bäume

Hierarchische Strukturen bestehend aus Knoten, die in einer Eltern-/Kindbeziehung stehen:

- Organisationstrukturen in einem Unternehmen
- Dateisystemstrukturen auf einer Festplatte
- Klassische Seitenstrukturen im World-Wide-Web

---

<div class="columns">
<div class="two">

### Knotenarten

Bei Bäumen kann man grundsätzlich zwischen den folgenden drei Arten von Knoten unterscheiden:

- Jeder Baum hat genau einen **Wurzelknoten (blau)**, der keinen Elternknoten besitzt
- **Innere Knoten (grün)** haben sowohl Eltern- als auch Kindknoten
- **Blattknoten (orange)** haben einen Eltern-, aber keinen Kindknoten

</div>
<div>

![](./Diagramme/Baum_Knotenarten.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Knotenebenen

Des Weiteren ist jeder Knoten auf einer definierten Ebene, die als die Länge des Pfades zwischen dem Knoten und dem Wurzelknoten definiert ist:

- Der Wurzelknoten ist somit selbst per Definition auf der Ebene 0
- Für jeden anderen Knoten ergibt sich die Ebene aus der Ebene des Elternknoten plus 1
- Somit kann die Ebene vom Wurzelknoten ausgehend nach unten rekursiv berechnet werden

</div>
<div>

![](./Diagramme/Baum_Knotenebenen.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Baumbreite und -tiefe

Des Weiteren können für Bäume zwei wichtige Eigenschaften definiert werden, die Baumbreite und die Baumtiefe:

- Die **Baumbreite** ist definiert als die Länge des längsten Pfades vom Wurzelknoten zu einem der Blattknoten (= 2 im Beispiel auf der rechten Seite)
- Die **Baumbreite** ist definiert als die größte Anzahl an Knoten auf einer beliebigen Knotenebene (= 3 im Beispiel auf der rechten Seite)

</div>
<div>

![](./Diagramme/Baum_Breite_Tiefe.svg)

</div>
</div>

---

### Formalisierung

Wir versuchen wieder eine Formalisierung der Datenstruktur als Grundlage für die weitere Betrachtung:

- Menge der Knoten $V$
- Elternabbildung $E: V \rightarrow V \cup \{\bot\}$
  - $E(u) = v$ besagt, dass $v$ der Elternknoten von $u$ ist
  - $E(v) = \bot$ besagt, dass $v$ der Wurzelknoten ist
  - Es gibt einen Wurzelknoten: es existiert $v \in V$ mit $E(v) = \bot$
  - Es gibt genau einen Wurzelknoten: $E(u) = \bot$ und $E(v) = \bot$ impliziert $u = v$
- Baum $B = (V, E)$

---

<div class="columns">
<div class="three">

### Transitive Hülle

Momentan lässt die Elternabbildung noch einge Fälle zu, die nicht erwünscht sind (z.B. Zyklen). Um dies zu verhindern benötigen wir wieder die transitive Hülle $t(E) \subseteq V \times V$ mit $(u, v) \in t(E)$ genau dann wenn:

- **Fall 1 (direkte Verbindung)** - Knoten $v$ ist Elternknoten von Knoten $u$, das heißt es gilt also $E(u) = v$
- **Fall 2 (indirekte Verbindung)** - Knoten $v$ ist Vorfahre von $u$, es existiert also ein Zwischenknoten $w$ mit $(u, w) \in t(E)$ und $(w, v) \in t(E)$

</div>
<div>

![](./Diagramme/Baum_Hülle.svg)

</div>
</div>

---

### Azyklische Elternbeziehung

Nun wollen wir mithilfe der transitiven Hülle $t(E)$ über der Elternbeziehung $E$ sicherstellen, dass die Elternbeziehung frei von Zyklen ist. Dies erreichen wir dadurch, dass wir folgende Eigenschaft fordern:

- Ein Knoten darf sich über die Elternbeziehung nicht selbst erreichen können
- Wir formalisieren diese Eigenschaft als $(u, v) \in t(E) \Rightarrow u \neq v$
- Somit kann ein Tupel $(v, v)$ nicht in der transitiven Hülle $t(E)$ enthalten sein
- Dies wiederum verhindert, das ein Pfad von einem beliebigen Knoten $v \in V$ über die Elternbeziehungen $E$ sich sich selbst führt
- Somit haben wir eine azyklische Datenstruktur mit genau einem Wurzelknoten

---

<div class="columns">
<div class="two">

### Programmierschnittstelle

Und so könnte die die API in C# aussehen:

```csharp
class Tree<T> {

    public T value;

    public Tree<T> parent;

    public List<Tree<T>> children

    public Tree<T> append(T value);

    public int level() { ... }
    public int depth() { ... }
    public int width(List<int> counts = new List<int>()) { ... }
}
```

</div>
<div>

![](./Diagramme/Baum.svg)

</div>
</div>

---

<div class="columns">
<div class="two">

### Anwendungsbeispiel

Das könnte eine mögliche Anwendung sein:

```csharp
// Ebene 0
Tree<string> root = new Tree<string>()

// Ebene 1
Tree<string> a = root.append("A");
Tree<string> b = root.append("B");

// Ebene 2
Tree<string> c = a.append("C")
Tree<string> d = a.append("D")

// Ebene 3
Tree<string> e = c.append("E")

// Ebene eines Knotens berechnen und auf der Konsole ausgeben
Console.WriteLine(e.level())
```

</div>
<div>

![](./Diagramme/Baum_Beispiel.svg)

</div>
</div>

---

### Implementierung der Methode `level(...)`

Die Ebene eines Knotens kann durch eine einfache rekursive Methode berechnet und muss daher nicht gespeichert werden:

```csharp
public int level() {

    // Prüfe, ob es sich um den Wurzelknoten handlet
    if (parent == null) {
        // Falls ja, sind wir auf der 0-ten Ebene
        return 0;
    }

    // Falls nein, ergibt sich die Ebene aus der Ebene des Elternknotens plus 1
    return parent.level() + 1;

}
```

---

<div class="columns">
<div class="two">

### Illustration der Lösung

Im Folgenden sehen wir, wie der Aufruf der Methode `level()` auf dem Knoten `e` abgearbeitet wird:

- `e.level() = c.level() + 1`
  - `c.level() = a.level() + 1`
    - `a.level() = root.level() + 1`
      - `root.level() = 0`
    - `= 1`
  - `= 2`
- `= 3`

</div>
<div>

![](./Diagramme/Baum_Beispiel.svg)

</div>
</div>

---

### Implementierung der Methode `depth(...)`

Die Tiefe eines Baums kann ebenfalls über eine einfache rekursive Methode ausgerechnet und muss nicht gespeichert werden:

```csharp
public void depth() {
    // Initialisieren des Ergebnisses mit dem Wert 0
    int result = 0;

    // Durchlaufen aller Kindknoten des aktuellen Knotens
    foreach (Tree<T> child in children) {
        // Überschreiben des Ergebnisses, wenn erforderlich
        result = Math.Max(result, child.depth() + 1)
    }

    // Rückgabe des berechneten Ergebnisses
    return result;
}
```

---

<div class="columns">
<div class="two">

### Illustration der Lösung

Im Folgenden sehen wir, wie der Aufruf der Methode `depth()` auf dem Knoten `root` abgearbeitet wird:

- `root.depth()` führt zu:
  - `a.depth()` führt zu:
    - `c.depth()` führt zu:
      - `d.depth() = 0`
    - `=> c.depth() = d.depth() + 1`
  - `=> a.depth() = c.depth() + 1`
  - `b.depth() = 0`
- `=> root.depth() = a.depth() + 1`

</div>
<div>

![](./Diagramme/Baum_Beispiel.svg)

</div>
</div>

---

![bg right](../Selbststudium.png)

### Aufgaben für das **Selbststudium**

Um Ihr Verständnis zu vertiefen, widmen Sie sich den folgenden Aufgaben im Selbststudium:

- Implementierung der Methode `void append(T element)`
- Implementierung der Methode `void width(List<int> counts)`<br/>*Hinweis: Jeder Baumknoten muss sich in die Liste `counts` an der richtigen Stelle eintragen!*