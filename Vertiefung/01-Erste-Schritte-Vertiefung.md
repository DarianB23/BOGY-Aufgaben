---
title: 01 - Erste Schritte Vertiefung
parent: Kapitel
nav_order: 101
---
# Kapitel 1 – Vertiefung: Erste Schritte & Fehlersuche

[← Zurück zu Kapitel 1](01-Erste-Schritte.md) | [Übersicht](../README.md)

Dieses Vertiefungskapitel ist für dich gedacht, wenn du beim ersten Durchgang noch Fragezeichen hattest. Hier schauen wir hinter die Kulissen: Warum meckert C# so schnell, was passiert im Terminal und wie liest du Fehlermeldungen ohne Frust?

---

## 1. Das mentale Modell: Wie der Computer deinen Code liest

Stell dir den C#-Compiler (den Übersetzer deines Codes in Maschinensprache) wie einen extrem genauen, aber sturen Roboter vor:
- Er kann nicht "erahnen", was du gemeint hast.
- Er versteht nur exakt definierte Symbole.
- Für ihn ist `Console` etwas völlig anderes als `console` (genauso wie du einen "Kater" nicht mit einem "Vater" verwechselst, auch wenn nur ein Buchstabe anders ist).

### Die Anatomie einer Befehlszeile

```csharp
Console.WriteLine("Hello, World!");
```

Schauen wir uns jedes Puzzleteil an:

```
  Console  .  WriteLine  (  "Hello, World!"  )  ;
    │      │      │      │         │         │  │
    │      │      │      └── Klammern auf/zu ───┘  └── Jede Anweisung endet mit ;
    │      │      │          Hier gehört das
    │      │      │          Argument hinein
    │      │      │
    │      │      └── Der Methoden-Name (die Aktion: "Schreib eine Zeile")
    │      │
    │      └── Der Zugriffsoperator (bedeutet: "Nimm aus 'Console' die Funktion...")
    │
    └── Die Klasse (das Werkzeugkasten-Objekt für Bildschirmausgaben)
```

---

## 2. Häufige Stolpersteine & "Aha!"-Erklärungen

### `Write` vs. `WriteLine` bildlich verstehen

Stell dir eine alte Schreibmaschine vor:
- **`Console.Write(...)`**: Tippt die Buchstaben ein. Der Schreibkopf bleibt **genau hinter dem letzten Buchstaben** stehen. Der nächste Text wird direkt daran geklatscht!
- **`Console.WriteLine(...)`**: Tippt die Buchstaben ein und drückt danach automatisch den Hebel für den **Zeilenvorschub (Enter)**. Der Schreibkopf springt an den Anfang der nächsten Zeile.

**Experiment:**
```csharp
Console.Write("A");
Console.Write("B");
Console.WriteLine("C");
Console.Write("D");
```
**Ausgabe:**
```
ABC
D
```
*Erklärung:* A, B und C stehen in einer Zeile. Erst `WriteLine("C")` sorgt dafür, dass das nachfolgende D in die nächste Zeile rutscht!

---

### Warum meckert der Backslash `\`? (Escape-Sequenzen)

Wenn du im Text `"Hallo \ Welt"` schreibst, wundert sich der Compiler. Der Backslash `\` ist in C# ein **Fluchtzeichen (Escape-Zeichen)**. Er kündigt an: *"Achtung, jetzt kommt ein Steuerbefehl!"*

| Steuerzeichen | Bedeutung | Beispiel | Ausgabe |
|---|---|---|---|
| `\n` | Neue Zeile (New Line) | `Console.Write("A\nB");` | A und B untereinander |
| `\t` | Tabulator (Abstand) | `Console.Write("Name:\tLea");` | Name:   Lea |
| `\"` | Anführungszeichen im Text | `Console.Write("Er sagte: \"Hi\"");` | Er sagte: "Hi" |
| `\\` | Ein echter Backslash | `Console.Write("C:\\Programme");` | C:\Programme |

{% capture tipp %}
Wenn du viele Backslashes hast (z. B. für ASCII-Art oder Dateipfade), setze ein `@` vor die Anführungszeichen. Das nennt man **Verbatim-String**:
```csharp
Console.WriteLine(@"  /\_/\  
 ( o.o ) 
  > ^ <  ");
```
Mit `@` zählt jeder Zeilenumbruch und jeder Backslash genau so, wie du ihn tippst!
{% endcapture %}
{% include tipp.html inhalt=tipp %}

---

## 3. Fehlermeldungen lesen lernen (Visual Studio)

Wenn dein Programm nicht startet, gerate nicht in Panik. Fehlermeldungen sind keine Kritik, sondern nützliche Wegweiser.

Ein typischer Fehler in der Fehlerliste:
```
Fehler CS1002 ; erwartet. Program.cs Zeile 5 Spalte 32
```

Wie du das liest:
1. **Fehlercode (CS1002):** Zeigt den genauen Fehlertyp an. (Kannst du googeln, falls die Erklärung unklar ist).
2. **Beschreibung (`; erwartet`):** Der Compiler sagt dir direkt, was ihm fehlt!
3. **Ort (`Zeile 5`):** Schaue in Zeile 5 deines Codes nach. Manchmal fehlt das Semikolon auch ganz am Ende von Zeile 4!

---

## 4. Vertiefungsaufgaben

### Vertiefungsaufgabe 1.1 – Der Kassenbon
Erstelle ein Programm, das einen kleinen Kassenbon ausgibt. Nutze dazu geschickt `Console.Write`, `Console.WriteLine` und Leerzeichen oder Tabulatoren `\t`:

```
=========================
        KASSENBON        
=========================
1x Kaffee         2.50 EUR
2x Brezel         3.20 EUR
-------------------------
Gesamt            5.70 EUR
=========================
```

### Vertiefungsaufgabe 1.2 – Fehlersuchspiel
Finde die 3 Fehler in diesem kurzen Code-Schnipsel, **ohne** ihn vorher in Visual Studio einzufügen:

```csharp
console.WriteLine("Willkommen beim Quiz!")
Console.WriteLine("Er sagte: "Hallo Welt"");
Console.Write("Ende des Programms")
```

<details>
<summary>👉 Klicke hier für die Lösung</summary>

1. **Zeile 1:** `console` muss großgeschrieben werden (`Console`), und am Zeilenende fehlt das Semikolon `;`.
2. **Zeile 2:** Die inneren Anführungszeichen verwirren C#. Richtig ist `\"Hallo Welt\"`.
3. **Zeile 3:** Am Zeilenende fehlt das Semikolon `;`.
</details>
