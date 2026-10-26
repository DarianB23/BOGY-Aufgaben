# Musterlösung – 🟢 Galgenraten

[← Zur Aufgabe](../Zusatzaufgaben/Galgenraten.md)

> ℹ️ **Hinweis für Betreuer\*innen:** Das ist **eine** mögliche Lösung. Lösungen der Praktikant\*innen dürfen anders aussehen. Wichtig ist, dass das Spiel funktioniert, nicht abstürzt und der Code lesbar ist.

**Benötigte Kapitel:** 2–8 und 10 (Variablen, Eingaben, Verzweigungen, Schleifen, Listen, Methoden, Fehler abfangen)

---

## Komplettlösung (alle Pflichtstufen)

Enthält: zufälliges Wort, Anzeige mit `_`, bereits geratene Buchstaben in einer `List<char>`, Fehlerzähler, absturzsichere Eingabe, mehrere Runden und Statistik.

**`Program.cs`**

```csharp
// ===== Galgenraten – Musterlösung =====

string[] woerter =
{
    "PROGRAMM", "COMPUTER", "SCHLEIFE", "VARIABLE", "METHODE",
    "TASTATUR", "BILDSCHIRM", "PRAKTIKUM", "KONSOLE", "ENTWICKLER"
};
int maxFehler = 8;

int gewonnen = 0;
int verloren = 0;
bool nochmal = true;

Console.WriteLine("=== GALGENRATEN ===");

while (nochmal)
{
    // --- Neue Runde vorbereiten ---
    string wort = woerter[Random.Shared.Next(woerter.Length)];
    List<char> geraten = new List<char>();
    int fehler = 0;
    bool erraten = false;

    // --- Eine Runde spielen ---
    while (fehler < maxFehler && !erraten)
    {
        Console.WriteLine();
        Console.WriteLine("Wort:    " + ZeigeWort(wort, geraten));
        Console.WriteLine($"Fehler:  {fehler} von {maxFehler}");
        if (geraten.Count > 0)
        {
            Console.WriteLine("Geraten: " + string.Join(" ", geraten));
        }

        char buchstabe = LiesBuchstabe();

        if (geraten.Contains(buchstabe))
        {
            Console.WriteLine($"'{buchstabe}' hattest du schon. Das zählt nicht.");
        }
        else
        {
            geraten.Add(buchstabe);

            if (wort.Contains(buchstabe))
            {
                Console.WriteLine("Treffer!");
            }
            else
            {
                fehler++;
                Console.WriteLine($"'{buchstabe}' kommt nicht vor.");
            }

            erraten = IstErraten(wort, geraten);
        }
    }

    // --- Auswertung ---
    Console.WriteLine();
    if (erraten)
    {
        gewonnen++;
        Console.WriteLine($"Gewonnen! Das Wort war {wort}. Du hattest {fehler} Fehler.");
    }
    else
    {
        verloren++;
        Console.WriteLine($"Verloren! Das Wort war {wort}.");
    }

    Console.WriteLine($"Statistik: {gewonnen} gewonnen, {verloren} verloren");
    nochmal = LiesJaNein("Nochmal spielen? (j/n): ");
}

Console.WriteLine("Danke fürs Spielen!");


// ===== Methoden =====

// Baut die Anzeige, z. B. "P R O _ R _ M M"
static string ZeigeWort(string wort, List<char> geraten)
{
    string anzeige = "";
    foreach (char c in wort)
    {
        if (geraten.Contains(c))
        {
            anzeige += c + " ";
        }
        else
        {
            anzeige += "_ ";
        }
    }
    return anzeige;
}

// true, wenn jeder Buchstabe des Wortes schon geraten wurde
static bool IstErraten(string wort, List<char> geraten)
{
    foreach (char c in wort)
    {
        if (!geraten.Contains(c))
        {
            return false;
        }
    }
    return true;
}

// Liest genau EINEN Buchstaben ein und gibt ihn als Großbuchstaben zurück
static char LiesBuchstabe()
{
    while (true)
    {
        Console.Write("Dein Buchstabe: ");
        string eingabe = (Console.ReadLine() ?? "").Trim().ToUpper();

        if (eingabe.Length == 1 && char.IsLetter(eingabe[0]))
        {
            return eingabe[0];
        }

        Console.WriteLine("Bitte genau einen Buchstaben eingeben.");
    }
}

// Fragt so lange, bis j oder n eingegeben wird
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
=== GALGENRATEN ===

Wort:    _ _ _ _ _ _ _
Fehler:  0 von 8
Dein Buchstabe: e
Treffer!

Wort:    _ E _ _ _ _ E
Fehler:  0 von 8
Geraten: E
Dein Buchstabe: xy
Bitte genau einen Buchstaben eingeben.
Dein Buchstabe: e
'E' hattest du schon. Das zählt nicht.
...
Gewonnen! Das Wort war METHODE. Du hattest 2 Fehler.
Statistik: 1 gewonnen, 0 verloren
Nochmal spielen? (j/n):
```

---

## ⭐ Lösungen zu den Extras

### Extra 1: Das ganze Wort auf einmal raten

Die Eingabe wird als `string` gelesen. Ist sie länger als ein Zeichen, gilt sie als Lösungsversuch. Ein falscher Lösungsversuch kostet **2** Fehler.

Ersetze in der Rundenschleife die Zeile `char buchstabe = LiesBuchstabe();` und den folgenden Block durch:

```csharp
Console.Write("Buchstabe oder ganzes Wort: ");
string eingabe = (Console.ReadLine() ?? "").Trim().ToUpper();

if (eingabe.Length > 1)
{
    // Lösungsversuch
    if (eingabe == wort)
    {
        erraten = true;
    }
    else
    {
        fehler += 2;
        Console.WriteLine("Leider falsch! Das kostet 2 Fehler.");
    }
}
else if (eingabe.Length == 1 && char.IsLetter(eingabe[0]))
{
    char buchstabe = eingabe[0];
    // ... ab hier wie in der Komplettlösung (Contains, Add, Treffer/Fehler)
}
else
{
    Console.WriteLine("Bitte einen Buchstaben oder ein Wort eingeben.");
}
```

### Extra 2: Zwei-Spieler-Modus

Spieler 1 gibt das Wort ein, danach wird die Konsole gelöscht, damit Spieler 2 es nicht sieht.

```csharp
string wort;
Console.Write("Spieler 1, gib ein geheimes Wort ein: ");
wort = (Console.ReadLine() ?? "").Trim().ToUpper();

while (wort.Length < 3)
{
    Console.Write("Das Wort muss mindestens 3 Buchstaben haben: ");
    wort = (Console.ReadLine() ?? "").Trim().ToUpper();
}

Console.Clear();
Console.WriteLine("Spieler 2 ist dran!");
```

---

## 🔍 Worauf ihr achten solltet

| Häufiger Fehler | Folge | Lösung |
|---|---|---|
| Eingabe nicht in Großbuchstaben umgewandelt | `e` wird nicht gefunden, weil das Wort `METHODE` heißt | `.ToUpper()` bei der Eingabe |
| Doppelte Buchstaben zählen als Fehler | Spieler verliert unfair | Vor dem Hinzufügen `geraten.Contains(...)` prüfen |
| `eingabe[0]` ohne Längenprüfung | Absturz bei leerer Eingabe (`IndexOutOfRangeException`) | Erst `eingabe.Length == 1` prüfen |
| Liste nicht pro Runde neu angelegt | In Runde 2 sind schon Buchstaben aufgedeckt | `new List<char>()` **innerhalb** der äußeren Schleife |
| `Random.Shared.Next(woerter.Length - 1)` | Das letzte Wort kommt nie dran | Obergrenze ist exklusiv, also `woerter.Length` |

### Gute Testfälle

- [ ] Leere Eingabe und Eingabe `123` → Hinweis, kein Absturz, kein Fehlerpunkt
- [ ] Kleinbuchstaben werden erkannt
- [ ] Gleichen Buchstaben zweimal eingeben → zählt nicht als Fehler
- [ ] Buchstabe, der mehrfach vorkommt (z. B. `M` in `PROGRAMM`) → alle Stellen werden aufgedeckt
- [ ] 8 Fehler → Spiel verloren, Wort wird angezeigt
- [ ] Zweite Runde startet mit neuem Wort und leerer Liste, Statistik zählt weiter
