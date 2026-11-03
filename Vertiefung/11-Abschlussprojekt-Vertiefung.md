---
title: 11 - Vertiefung: Abschlussprojekt
parent: Kapitel
nav_order: 111
---
# Kapitel 11 – Vertiefung: Projektarchitektur und Game-Loop

[← Zurück zu Kapitel 11](11-Abschlussprojekt.md) | [Übersicht](../README.md)

Ein größeres Projekt von Grund auf zu entwickeln, kann einschüchternd wirken. *"Wo fange ich an? Wie baue ich den Code auf, ohne den Überblick zu verlieren?"*
In dieser Vertiefung lernst du die Denkweise von Software-Entwicklern kennen: Wie man ein Projekt in kleine, beherrschbare Häppchen zerlegt und mit dem **Game Loop**-Muster arbeitet.

---

## 1. Das goldene Entwickler-Prinzip: Schicht für Schicht

Der größte Fehler von Anfängern ist: Sie schreiben 150 Zeilen Code auf einmal, drücken auf Start – und werden von 12 Fehlern erschlagen.

**Die Profi-Regel:**
1. Schreibe den kleinstmöglichen Teil, der funktioniert (z. B. nur die Zufallszahl generieren und mit `Console.WriteLine` anzeigen).
2. Teste ihn.
3. Füge die nächste Funktion hinzu (z. B. eine einmalige Raterunde).
4. Teste wieder.
5. Erst wenn das klappt, baue die Schleife für mehrere Runden ein!

---

## 2. Der "Game Loop" (Die Spielschleife)

Fast jedes Spiel der Welt – von *Zahlenraten* bis *Super Mario* – folgt demselben Muster:

```
┌────────────────────────────────────────────────────────┐
│ 1. INIT: Spielstand einrichten (Zufallszahl, Zähler=0) │
└──────────────────────────┬─────────────────────────────┘
                           │
                           ▼
              ┌───────────────────────────┐
       ┌────► │ 2. INPUT: Eingabe lesen   │
       │      └────────────┬──────────────┘
       │                   │
       │                   ▼
       │      ┌───────────────────────────┐
       │      │ 3. UPDATE: Logik prüfen   │
       │      │    (Größer? Kleiner? ++)  │
       │      └────────────┬──────────────┘
       │                   │
       │                   ▼
       │      ┌───────────────────────────┐
       │      │ 4. RENDER: Feedback geben │
       │      │    ("Zu hoch!", "Treffer")│
       │      └────────────┬──────────────┘
       │                   │
       └────── Ist Zahl noch nicht erraten? ─────┘
                           │ Ja, erraten!
                           ▼
┌────────────────────────────────────────────────────────┐
│ 5. ENDE: Statistik zeigen & "Nochmal spielen? (j/n)"   │
└────────────────────────────────────────────────────────┘
```

---

## 3. Die Tücke mit `Random.Shared.Next`

```csharp
int zahl = Random.Shared.Next(1, 100); // VORSICHT!
```
- In C# ist die Untergrenze **inklusive** (1 kann vorkommen).
- Die Obergrenze ist jedoch **exklusiv** (100 kommt **niemals** vor!). Die höchste Zahl wäre 99!
- Wenn du eine Zahl von 1 bis 100 haben willst, musst du immer schreiben:
  ```csharp
  int geheim = Random.Shared.Next(1, 101); // 1 bis 100
  ```

---

## 4. Debugging-Tipps in Visual Studio

Wenn sich dein Spiel seltsam verhält:

1. **"Schummel-Modus" beim Entwickeln:**
   Gib die geheime Zahl am Anfang der Runde zur Kontrolle aus:
   ```csharp
   // Debug-Hilfe (später wieder löschen):
   Console.WriteLine($"[DEBUG] Geheime Zahl ist: {geheimeZahl}");
   ```
   So musst du beim Testen nicht 10-mal blind raten.

2. **Haltepunkte (Breakpoints):**
   Klicke in Visual Studio links neben die Zeilennummer (in den grauen Rand). Es erscheint ein roter Punkt.
   - Wenn du jetzt mit **F5** startest, hält das Programm exakt an dieser Zeile an.
   - Du kannst mit der Maus über Variablen fahren und sehen, welcher Wert gerade darin steht!
   - Mit **F10** gehst du Schritt für Schritt eine Zeile weiter.

---

## 5. Vollständige Muster-Architektur als Orientierungshilfe

Falls du bei der Umsetzung von Stufe 1 und 2 feststeckst, findest du hier einen sauber strukturierten Leitfaden:

```csharp
bool weiterspielen = true;

while (weiterspielen)
{
    SpieleEineRunde();

    Console.Write("\nMöchtest du noch eine Runde spielen? (j/n): ");
    string antwort = Console.ReadLine()?.ToLower();
    if (antwort != "j" && antwort != "ja")
    {
        weiterspielen = false;
    }
}

Console.WriteLine("Vielen Dank fürs Spielen! Bis zum nächsten Mal.");

// --- Methoden ---

void SpieleEineRunde()
{
    Console.Clear();
    Console.WriteLine("=== ZAHLENRATEN ===");
    Console.WriteLine("Ich habe mir eine Zahl zwischen 1 und 100 ausgedacht.\n");

    int geheim = Random.Shared.Next(1, 101);
    int versuche = 0;
    int tipp = 0;

    do
    {
        tipp = LiesGueltigenTipp(1, 100);
        versuche++;

        if (tipp < geheim)
        {
            Console.WriteLine("-> Größer!");
        }
        else if (tipp > geheim)
        {
            Console.WriteLine("-> Kleiner!");
        }
        else
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine($"\nTreffer! Du hast die Zahl {geheim} in {versuche} Versuchen erraten!");
            Console.ResetColor();
        }

    } while (tipp != geheim);
}

int LiesGueltigenTipp(int min, int max)
{
    int zahl;
    while (true)
    {
        Console.Write($"Dein Tipp ({min}-{max}): ");
        if (int.TryParse(Console.ReadLine(), out zahl) && zahl >= min && zahl <= max)
        {
            return zahl;
        }
        Console.WriteLine($"Ungültig! Bitte gib eine Zahl zwischen {min} und {max} ein.");
    }
}
```

---

## 6. Du bist jetzt bereit für eigene Projekte!

Herzlichen Glückwunsch! Du hast nun nicht nur gelernt, wie C#-Code geschrieben wird, sondern auch:
- wie Software intern funktioniert (Speicher, Typen, Scopes),
- wie du Fehler abfängst, bevor Benutzer frustriert sind,
- und wie du größere Probleme systematisch in einfache Bausteine zerlegst.
