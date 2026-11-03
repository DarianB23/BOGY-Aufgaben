---
title: 03 - Vertiefung Rechnen und Vergleichen
parent: Vertiefung
nav_order: 3
---
# Kapitel 3 – Vertiefung: Rechnen, Modulo und Logik

[← Zurück zu Kapitel 3](03-Rechnen-und-Vergleichen.md) | [Übersicht](../README.md)

Rechnen am Computer scheint vertraut, birgt aber zwei der größten Überraschungen für Programmieranfänger: die Ganzzahldivision und der Modulo-Operator `%`. Hier erfährst du, was dahintersteckt.

---

## 1. Das Rätsel der Ganzzahldivision

Warum ergibt `7 / 2` in C# das Ergebnis `3` und nicht `3.5`?

### Die goldene Regel der Typ-Berechnung
C# schaut sich vor dem Rechnen die Typen der beiden Operanden an:
- `int` geteilt durch `int` ergibt **immer einen `int`**!
- Bei einem `int` gibt es schlicht keine Nachkommastellen im Speicher. C# schneidet die Nachkommastellen einfach rigoros ab (es rundet nicht!). Aus `3.99` wird `3`.

```csharp
int a = 7;
int b = 2;
int ergebnis = a / b; // Ergebnis ist 3
```

### Wie erzwingst du Nachkommastellen?
Sobald **mindestens einer** der beiden Werte ein `double` ist, rechnet C# in Kommazahlen weiter!

```csharp
double ergebnis1 = 7.0 / 2;         // 3.5 (weil 7.0 ein double ist)
double ergebnis2 = 7 / 2.0;         // 3.5 (weil 2.0 ein double ist)
double ergebnis3 = (double)a / b;   // 3.5 (Typumwandlung / "Cast" von a in double)
```

{% capture notiz %}
Achtung Falle:
```csharp
double falsch = 7 / 2; // falsch ist 3.0, NICHT 3.5!
```
Warum? Weil C# zuerst rechts rechnet: `7 / 2` (zwei ints) ergibt `3`. Erst danach wird die `3` in das `double` gespeichert und zu `3.0`!
{% endcapture %}
{% include notiz.html typ="warning" titel="Typische Denk-Falle" offen=true inhalt=notiz %}

---

## 2. Modulo `%` bildlich verstehen

Der Modulo-Operator `%` berechnet den **Rest** einer ganzzahligen Division.

### Das Kisten-Beispiel:
Du hast **14 Äpfel** und Kartons, in die genau **4 Äpfel** passen:
- **Ganzzahl-Division (`14 / 4`):** Wie viele Kartons werden randvoll? -> **3 Kartons** (denn 3 * 4 = 12).
- **Modulo (`14 % 4`):** Wie viele Äpfel bleiben lose übrig? -> **2 Äpfel** (14 - 12 = 2).

### Wofür braucht man das ständig?
1. **Gerade oder ungerade Zahl?**
   Jede gerade Zahl lässt sich restlos durch 2 teilen:
   ```csharp
   bool istGerade = (zahl % 2 == 0); // true, wenn Rest 0 ist
   ```
2. **Uhren & Kreisläufe:**
   Wenn es 10 Uhr ist und 5 Stunden vergehen: `(10 + 5) % 12` ergibt `3` Uhr.
3. **Jeden N-ten Durchlauf erkennen:**
   Z. B. in Spielen jeden 10. Frame speichern: `if (runde % 10 == 0)`.

---

## 3. Zuweisung `=` vs. Vergleich `==`

- **`=` (Ein Gleichheitszeichen):** Der **Befehl zum Speichern**. Nimm den Wert rechts und pack ihn in die Variable links.
  `alter = 18;` -> "Mach das Alter zu 18!"
- **`==` (Zwei Gleichheitszeichen):** Die **Frage nach Gleichheit**. Vergleiche links und rechts und antworte mit `true` oder `false`.
  `alter == 18;` -> "Ist das Alter gleich 18?"

---

## 4. Logische Operatoren im Alltag

Logik-Operatoren verbinden Wahrheitswerte:

| Operator | Bedeutung | Alltagsbeispiel |
|---|---|---|
| `&&` (UND) | **Beide** müssen wahr sein | *"Du darfst den Rabatt nutzen, wenn du Schüler bist `&&` einen Schülerausweis dabeihast."* Fehlt eins davon, gibt es keinen Rabatt. |
| `\|\|` (ODER) | **Mindestens eine** Seite muss wahr sein | *"Du darfst kostenlos fahren, wenn du ein Ticket hast `\|\|` heute autofreier Sonntag ist."* Eines reicht völlig aus! |
| `!` (NICHT) | Dreht den Wert um | `!istRegnerisch` -> Es ist **nicht** regnerisch (also schönes Wetter). |

### Das "Kurzschluss"-Prinzip (Short-Circuit)
C# ist clever:
- Bei `false && ...`: Da die erste Bedingung schon `false` ist, kann der gesamte Ausdruck niemals mehr `true` werden. C# schaut die zweite Bedingung gar nicht mehr an!
- Bei `true || ...`: Da die erste Bedingung schon `true` ist, steht fest, dass das Gesamtergebnis `true` ist.

---

## 5. Vertiefungsaufgaben

### Vertiefungsaufgabe 3.1 – Rechenausdrücke vorhersagen
Was kommt bei diesen Rechnungen heraus? Überlege erst im Kopf:
1. `15 / 4`
2. `15 % 4`
3. `15.0 / 4`
4. `(10 + 2) * 3`
5. `10 + 2 * 3`

<details>
<summary>👉 Klicke hier für die Lösung</summary>

1. `3` (15 / 4 passt 3-mal ganz hinein)
2. `3` (Rest: 3 * 4 = 12; 15 - 12 = 3)
3. `3.75` (Kommadivision wegen `15.0`)
4. `36` (Klammern zuerst: 12 * 3)
5. `16` (Punkt vor Strich: 2 * 3 = 6, dann 10 + 6)
</details>

### Vertiefungsaufgabe 3.2 – Der Schaltjahr-Detektiv
Ein Jahr ist ein Schaltjahr, wenn:
- es durch 4 teilbar ist, **UND**
- es **nicht** durch 100 teilbar ist (außer es ist durch 400 teilbar).

Versuche diesen Ausdruck in einer einzigen C#-Bedingung für `int jahr = 2024;` aufzuschreiben:

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
int jahr = 2024;
bool istSchaltjahr = (jahr % 4 == 0 && jahr % 100 != 0) || (jahr % 400 == 0);
Console.WriteLine($"Ist {jahr} ein Schaltjahr? {istSchaltjahr}");
```
</details>
