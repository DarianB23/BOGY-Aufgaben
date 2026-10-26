# Musterlösung – 🔴 Tic-Tac-Toe

[← Zur Aufgabe](../Zusatzaufgaben/Tic-Tac-Toe.md)

> ℹ️ **Hinweis für Betreuer\*innen:** Das ist **eine** mögliche Lösung. Lösungen der Praktikant\*innen dürfen anders aussehen, z. B. mit einem 3×3-Array statt einem Array mit 9 Feldern. Wichtig ist, dass alle 8 Gewinnlinien erkannt werden, ein Unentschieden erkannt wird und belegte Felder nicht überschrieben werden können.

**Benötigte Kapitel:** 2–10 (besonders Arrays, Methoden, Klassen)

---

## Komplettlösung (alle Pflichtstufen)

Enthält: Klasse `Spielfeld` mit privatem Array, Spielfeld zeichnen, Züge prüfen, Gewinn und Unentschieden erkennen, zwei Spieler oder gegen den Computer, Computer gewinnt/blockiert, mehrere Runden mit Punktestand.

Das Projekt besteht aus **zwei Dateien**.

> ❗ **Wichtig:** Steht in `Spielfeld.cs` ein `namespace` (z. B. `namespace TicTacToe;`), muss oben in `Program.cs` `using TicTacToe;` stehen.

### Aufbau des Spielfelds

Die 9 Felder liegen in einem Array. Der Spieler gibt 1–9 ein, im Array ist das Index 0–8:

```
 1 | 2 | 3          Index:  0 | 1 | 2
---+---+---                ---+---+---
 4 | 5 | 6                  3 | 4 | 5
---+---+---                ---+---+---
 7 | 8 | 9                  6 | 7 | 8
```

### `Spielfeld.cs`

```csharp
public class Spielfeld
{
    // Privat: Von außen kann niemand direkt ein Feld überschreiben
    private char[] felder = new char[9];

    public Spielfeld()
    {
        for (int i = 0; i < felder.Length; i++)
        {
            felder[i] = ' ';
        }
    }

    // Leere Felder zeigen ihre Nummer, belegte Felder X oder O
    public void Zeichnen()
    {
        Console.WriteLine();
        for (int zeile = 0; zeile < 3; zeile++)
        {
            for (int spalte = 0; spalte < 3; spalte++)
            {
                int index = zeile * 3 + spalte;
                string anzeige;
                if (felder[index] == ' ')
                {
                    anzeige = (index + 1).ToString();
                }
                else
                {
                    anzeige = felder[index].ToString();
                }

                Console.Write($" {anzeige} ");
                if (spalte < 2)
                {
                    Console.Write("|");
                }
            }
            Console.WriteLine();
            if (zeile < 2)
            {
                Console.WriteLine("---+---+---");
            }
        }
        Console.WriteLine();
    }

    public bool IstFrei(int index)
    {
        return index >= 0 && index < 9 && felder[index] == ' ';
    }

    // Gibt false zurück, wenn das Feld nicht frei ist
    public bool Setzen(int index, char symbol)
    {
        if (!IstFrei(index))
        {
            return false;
        }
        felder[index] = symbol;
        return true;
    }

    // Wird vom Computer gebraucht, um Züge "auszuprobieren"
    public void Entfernen(int index)
    {
        if (index >= 0 && index < 9)
        {
            felder[index] = ' ';
        }
    }

    // Prüft alle 8 Gewinnlinien
    public bool HatGewonnen(char s)
    {
        for (int i = 0; i < 3; i++)
        {
            // Zeile i: Felder i*3, i*3+1, i*3+2
            if (felder[i * 3] == s && felder[i * 3 + 1] == s && felder[i * 3 + 2] == s)
            {
                return true;
            }
            // Spalte i: Felder i, i+3, i+6
            if (felder[i] == s && felder[i + 3] == s && felder[i + 6] == s)
            {
                return true;
            }
        }

        // Diagonalen
        if (felder[0] == s && felder[4] == s && felder[8] == s)
        {
            return true;
        }
        if (felder[2] == s && felder[4] == s && felder[6] == s)
        {
            return true;
        }

        return false;
    }

    public bool IstVoll()
    {
        foreach (char f in felder)
        {
            if (f == ' ')
            {
                return false;
            }
        }
        return true;
    }
}
```

### `Program.cs`

```csharp
// ===== Tic-Tac-Toe – Musterlösung =====

Console.WriteLine("=== TIC-TAC-TOE ===");
int modus = LiesZahlImBereich("1 = Zwei Spieler, 2 = Gegen den Computer: ", 1, 2);
bool gegenComputer = modus == 2;

int siegeX = 0;
int siegeO = 0;
int unentschieden = 0;
bool nochmal = true;

while (nochmal)
{
    Spielfeld feld = new Spielfeld();
    char aktuell = 'X';
    bool vorbei = false;

    while (!vorbei)
    {
        feld.Zeichnen();

        int index;
        if (gegenComputer && aktuell == 'O')
        {
            index = ComputerZug(feld, 'O', 'X');
            Console.WriteLine($"Der Computer setzt auf Feld {index + 1}.");
        }
        else
        {
            index = LiesFreiesFeld(feld, aktuell);
        }

        feld.Setzen(index, aktuell);

        if (feld.HatGewonnen(aktuell))
        {
            feld.Zeichnen();
            Console.WriteLine($"Spieler {aktuell} hat gewonnen!");
            if (aktuell == 'X')
            {
                siegeX++;
            }
            else
            {
                siegeO++;
            }
            vorbei = true;
        }
        else if (feld.IstVoll())
        {
            feld.Zeichnen();
            Console.WriteLine("Unentschieden!");
            unentschieden++;
            vorbei = true;
        }
        else
        {
            // Spieler wechseln
            if (aktuell == 'X')
            {
                aktuell = 'O';
            }
            else
            {
                aktuell = 'X';
            }
        }
    }

    Console.WriteLine($"Punktestand – X: {siegeX}  O: {siegeO}  Unentschieden: {unentschieden}");
    nochmal = LiesJaNein("Nochmal spielen? (j/n): ");
}

Console.WriteLine("Danke fürs Spielen!");


// ===== Methoden =====

// Fragt so lange, bis ein freies Feld 1-9 gewählt wurde. Gibt den Index 0-8 zurück.
static int LiesFreiesFeld(Spielfeld feld, char spieler)
{
    while (true)
    {
        int nummer = LiesZahl($"Spieler {spieler}, wähle ein Feld (1-9): ");

        if (nummer < 1 || nummer > 9)
        {
            Console.WriteLine("Es gibt nur die Felder 1 bis 9.");
        }
        else if (!feld.IstFrei(nummer - 1))
        {
            Console.WriteLine("Das Feld ist schon belegt.");
        }
        else
        {
            return nummer - 1;
        }
    }
}

// Strategie: 1. selbst gewinnen, 2. Gegner blockieren, 3. Mitte, 4. zufälliges freies Feld
static int ComputerZug(Spielfeld feld, char computer, char gegner)
{
    int zug = FindeGewinnfeld(feld, computer);
    if (zug != -1)
    {
        return zug;
    }

    zug = FindeGewinnfeld(feld, gegner);   // Wo würde der Gegner gewinnen? Dort blockieren!
    if (zug != -1)
    {
        return zug;
    }

    if (feld.IstFrei(4))
    {
        return 4;
    }

    List<int> freieFelder = new List<int>();
    for (int i = 0; i < 9; i++)
    {
        if (feld.IstFrei(i))
        {
            freieFelder.Add(i);
        }
    }
    return freieFelder[Random.Shared.Next(freieFelder.Count)];
}

// Probiert jedes freie Feld aus: Gewinnt "symbol" damit? Gibt den Index zurück oder -1.
static int FindeGewinnfeld(Spielfeld feld, char symbol)
{
    for (int i = 0; i < 9; i++)
    {
        if (feld.IstFrei(i))
        {
            feld.Setzen(i, symbol);
            bool gewinnt = feld.HatGewonnen(symbol);
            feld.Entfernen(i);          // Ausprobieren rückgängig machen!

            if (gewinnt)
            {
                return i;
            }
        }
    }
    return -1;
}

static int LiesZahlImBereich(string frage, int min, int max)
{
    while (true)
    {
        int zahl = LiesZahl(frage);
        if (zahl >= min && zahl <= max)
        {
            return zahl;
        }
        Console.WriteLine($"Bitte eine Zahl von {min} bis {max} eingeben.");
    }
}

// Absturzsichere Zahleneingabe (aus Kapitel 10)
static int LiesZahl(string frage)
{
    while (true)
    {
        Console.Write(frage);
        try
        {
            return int.Parse(Console.ReadLine() ?? "");
        }
        catch (FormatException)
        {
            Console.WriteLine("Das ist keine gültige Zahl!");
        }
        catch (OverflowException)
        {
            Console.WriteLine("Die Zahl ist zu groß!");
        }
    }
}

static bool LiesJaNein(string frage)
{
    while (true)
    {
        Console.Write(frage);
        string antwort = (Console.ReadLine() ?? "").Trim().ToLower();

        if (antwort == "j")
        {
            return true;
        }
        if (antwort == "n")
        {
            return false;
        }

        Console.WriteLine("Bitte nur j oder n eingeben.");
    }
}
```

### Beispielausgabe

```
=== TIC-TAC-TOE ===
1 = Zwei Spieler, 2 = Gegen den Computer: 2

 1 | 2 | 3 
---+---+---
 4 | 5 | 6 
---+---+---
 7 | 8 | 9 

Spieler X, wähle ein Feld (1-9): 1

 X | 2 | 3 
---+---+---
 4 | 5 | 6 
---+---+---
 7 | 8 | 9 

Der Computer setzt auf Feld 5.
...
```

---

## ⭐ Lösungen zu den Extras

### Extra 1: Wer fängt an?

Damit nicht immer X beginnt, wechselt der Startspieler jede Runde. Vor der äußeren Schleife:

```csharp
char startSpieler = 'X';
```

Am Anfang jeder Runde `char aktuell = startSpieler;` statt `'X'`, und am Ende der Runde:

```csharp
if (startSpieler == 'X')
{
    startSpieler = 'O';
}
else
{
    startSpieler = 'X';
}
```

### Extra 2 (für Profis): Unschlagbarer Computer mit Minimax

Minimax probiert **alle** möglichen Spielverläufe bis zum Ende durch. Jedes Ergebnis wird bewertet: +1 = Computer gewinnt, -1 = Mensch gewinnt, 0 = Unentschieden. Der Computer nimmt den Zug mit der besten Bewertung und geht davon aus, dass der Mensch immer den für ihn besten Zug spielt.

Die Methode ruft sich **selbst** auf (Rekursion). Das kam im Tutorial nicht vor und ist der eigentlich schwierige Teil.

```csharp
// Ersetzt ComputerZug
static int BesterZug(Spielfeld feld, char computer, char mensch)
{
    int besterWert = -2;
    int besterIndex = -1;

    for (int i = 0; i < 9; i++)
    {
        if (feld.IstFrei(i))
        {
            feld.Setzen(i, computer);
            int wert = Minimax(feld, false, computer, mensch);
            feld.Entfernen(i);

            if (wert > besterWert)
            {
                besterWert = wert;
                besterIndex = i;
            }
        }
    }
    return besterIndex;
}

static int Minimax(Spielfeld feld, bool computerAmZug, char computer, char mensch)
{
    // Spiel vorbei? Dann bewerten.
    if (feld.HatGewonnen(computer))
    {
        return 1;
    }
    if (feld.HatGewonnen(mensch))
    {
        return -1;
    }
    if (feld.IstVoll())
    {
        return 0;
    }

    int besterWert;
    char symbol;
    if (computerAmZug)
    {
        besterWert = -2;   // Computer sucht das Maximum
        symbol = computer;
    }
    else
    {
        besterWert = 2;    // Mensch sucht das Minimum
        symbol = mensch;
    }

    for (int i = 0; i < 9; i++)
    {
        if (feld.IstFrei(i))
        {
            feld.Setzen(i, symbol);
            int wert = Minimax(feld, !computerAmZug, computer, mensch);
            feld.Entfernen(i);

            if (computerAmZug)
            {
                besterWert = Math.Max(besterWert, wert);
            }
            else
            {
                besterWert = Math.Min(besterWert, wert);
            }
        }
    }
    return besterWert;
}
```

In `Program.cs` dann `index = BesterZug(feld, 'O', 'X');` statt `ComputerZug(...)` aufrufen. Gegen diesen Computer kann man höchstens unentschieden spielen.

---

## 🔍 Worauf ihr achten solltet

| Häufiger Fehler | Folge | Lösung |
|---|---|---|
| Nur Zeilen und Spalten geprüft | Diagonaler Sieg wird nicht erkannt | Alle 8 Linien prüfen (3 Zeilen, 3 Spalten, 2 Diagonalen) |
| Erst auf „voll“, dann auf Gewinn geprüft | Sieg mit dem letzten Zug wird als Unentschieden gewertet | **Zuerst** `HatGewonnen`, dann `IstVoll` |
| Eingabe 1–9 direkt als Index verwendet | Feld 9 → Absturz, alle Züge um eins verschoben | `nummer - 1` |
| Belegtes Feld wird überschrieben | Spieler können Züge „klauen“ | `IstFrei` vor dem Setzen prüfen |
| `Entfernen` beim Ausprobieren vergessen | Der Computer setzt plötzlich mehrere Steine | Nach jedem Test-Zug sofort zurücknehmen |
| Spielfeld nicht pro Runde neu erzeugt | Runde 2 startet mit vollem Feld | `new Spielfeld()` **innerhalb** der äußeren Schleife |
| Array `public` statt `private` | Klasse kann von außen kaputt gemacht werden | `private` und Zugriff nur über Methoden |

### Gute Testfälle

- [ ] Sieg über Zeile (1-2-3), Spalte (2-5-8) und beide Diagonalen (1-5-9, 3-5-7)
- [ ] Unentschieden: z. B. X: 1, 3, 4, 8, 9 / O: 2, 5, 6, 7
- [ ] Sieg genau mit dem 9. Zug → als Sieg gewertet, nicht als Unentschieden
- [ ] Eingaben `0`, `10`, `abc` und ein belegtes Feld → Hinweis, kein Absturz
- [ ] Computer blockiert: X auf 1 und 2 → Computer setzt auf 3 (wenn er nicht selbst gewinnen kann)
- [ ] Computer gewinnt, wenn er kann, statt zu blockieren
- [ ] Punktestand zählt über mehrere Runden richtig
