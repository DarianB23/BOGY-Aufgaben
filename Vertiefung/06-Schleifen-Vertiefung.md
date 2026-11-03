---
title: 06 - Vertiefung Schleifen
parent: Vertiefung
nav_order: 6
---
# Kapitel 6 – Vertiefung: Schleifen meistern

[← Zurück zu Kapitel 6](06-Schleifen.md) | [Übersicht](../README.md)

Schleifen gehören zu den mächtigsten Werkzeugen eines Programmierers: Was ein Mensch stundenlang von Hand tippen müsste, erledigt der Computer in Millisekunden. Doch gerade bei Schleifen passieren oft Fehler wie Endlosschleifen oder Schleifen, die genau einmal zu oft laufen.

---

## 1. Die `for`-Schleife unter der Lupe

Viele Anfänger stolpern über die drei Teile in den Klammern der `for`-Schleife:

```csharp
for (int i = 0; i < 3; i++)
{
    Console.WriteLine($"Durchgang {i}");
}
```

Schauen wir uns die **exakte Chronologie** an:

```
                  ┌──────────────────────┐
                  │ 1. Start: int i = 0  │ (wird NUR EINMAL ausgeführt)
                  └──────────┬───────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
       ┌───── │ 2. Bedingung: Ist i < 3 ?    │ ◄─────────────────────────┐
       │      └──────────────┬───────────────┘                           │
Nein   │                     │ Ja                                        │
       │                     ▼                                           │
       │      ┌──────────────────────────────┐                           │
       │      │ 3. Schleifenrumpf ausführen: │                           │
       │      │    Console.WriteLine(...)    │                           │
       │      └──────────────┬───────────────┘                           │
       │                     │                                           │
       │                     ▼                                           │
       │      ┌──────────────────────────────┐                           │
       │      │ 4. Schritt: i++ (i um 1 hoch)│ ──────────────────────────┘
       │      └──────────────────────────────┘
       ▼
[Schleife beendet]
```

### Die Schreibtisch-Tabelle (Trace Table)

| Durchlauf | Wert von `i` | Bedingung `i < 3`? | Was passiert? | Neuer Wert nach `i++` |
|---|---|---|---|---|
| 1 | `0` | `true` | Ausgabe: *Durchgang 0* | `1` |
| 2 | `1` | `true` | Ausgabe: *Durchgang 1* | `2` |
| 3 | `2` | `true` | Ausgabe: *Durchgang 2* | `3` |
| Ende | `3` | `false` (3 ist nicht < 3!) | Schleife bricht ab | - |

---

## 2. Der klassische "Off-by-One"-Fehler

Zählst du von 0 oder von 1?
- `for (int i = 0; i < 5; i++)` -> Läuft **5-mal** (mit den Werten `0, 1, 2, 3, 4`). Das ist der Standard in der Programmierung!
- `for (int i = 0; i <= 5; i++)` -> Läuft **6-mal** (mit den Werten `0, 1, 2, 3, 4, 5`).
- `for (int i = 1; i <= 5; i++)` -> Läuft **5-mal** (mit den Werten `1, 2, 3, 4, 5`).

Achte immer ganz genau darauf, ob du `<` oder `<=` benutzt!

---

## 3. `while` vs. `do-while`: Vorher oder nachher prüfen?

Stell dir eine Achterbahn vor:
- **`while` (Kopfgesteuert):** Der Fahrkartenkontrolleur steht **am Eingang**. Wer kein Ticket hat, darf gar nicht erst einsteigen! (Läuft 0 bis unendlich oft).
  ```csharp
  int geld = 0;
  while (geld > 0)
  {
      Console.WriteLine("Kirmes-Fahrt!"); // Läuft 0-mal, weil geld = 0 ist!
  }
  ```
- **`do-while` (Fußgesteuert):** Man darf einsteigen und die Runde fahren, aber **am Ausgang** wird kontrolliert, ob man noch eine Runde drehen darf. (Läuft **garantiert mindestens 1-mal**!).
  ```csharp
  int versuche = 0;
  do
  {
      Console.Write("Gib ein Passwort ein: ");
      versuche++;
  } while (versuche < 3);
  ```

---

## 4. `break` und `continue` bildlich erklärt

- **`break` (Die Notbremse):** Wirft dich sofort und endgültig aus der gesamten Schleife heraus. Der Code springt hinter die schließende geschweifte Klammer `}`.
- **`continue` (Runde überspringen):** Bricht nur den **aktuellen Durchlauf** vorzeitig ab und springt sofort zum nächsten Schritt (`i++`) weiter.

```csharp
for (int i = 1; i <= 5; i++)
{
    if (i == 3)
    {
        continue; // Die 3 lassen wir aus!
    }
    Console.Write(i + " ");
}
// Ausgabe: 1 2 4 5
```

---

## 5. Verschachtelte Schleifen: Die Matrix

Wenn eine Schleife in einer anderen Schleife steckt, stell dir eine Tabelle vor:
- Die **äußere Schleife** wandert von Zeile zu Zeile.
- Die **innere Schleife** wandert von Spalte zu Spalte in der aktuellen Zeile.

```csharp
for (int zeile = 1; zeile <= 3; zeile++)
{
    for (int spalte = 1; spalte <= 3; spalte++)
    {
        Console.Write($"[{zeile},{spalte}] ");
    }
    Console.WriteLine(); // Neue Zeile am Ende jeder Reihe!
}
```
**Ausgabe:**
```
[1,1] [1,2] [1,3] 
[2,1] [2,2] [2,3] 
[3,1] [3,2] [3,3] 
```

---

## 6. Vertiefungsaufgaben

### Vertiefungsaufgabe 6.1 – Das Schachbrettmuster
Zeichne mit zwei verschachtelten Schleifen ein 4x4-Schachbrett aus `#` und `.`:

```
# . # .
. # . #
# . # .
. # . #
```

{% capture tipp %}
Rechne `(zeile + spalte) % 2 == 0`. Ist die Summe gerade, gib `# ` aus, sonst `. `!
{% endcapture %}
{% include tipp.html inhalt=tipp %}

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
for (int y = 0; y < 4; y++)
{
    for (int x = 0; x < 4; x++)
    {
        if ((x + y) % 2 == 0)
        {
            Console.Write("# ");
        }
        else
        {
            Console.Write(". ");
        }
    }
    Console.WriteLine();
}
```
</details>

### Vertiefungsaufgabe 6.2 – Endlosschleifen entlarven
Warum hört dieses Programm niemals auf zu laufen? Finde den Fehler:

```csharp
int counter = 10;
while (counter > 0)
{
    Console.WriteLine(counter);
    // Was fehlt hier?
}
```

<details>
<summary>👉 Klicke hier für die Erklärung</summary>

In der Schleife wird `counter` nie verändert. Da `counter` immer 10 bleibt, ist `counter > 0` für immer wahr!
**Lösung:** Vor dem Ende der Schleife muss `counter--;` stehen.
</details>
