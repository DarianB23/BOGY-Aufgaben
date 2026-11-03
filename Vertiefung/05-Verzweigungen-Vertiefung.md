---
title: 05 - Vertiefung: Verzweigungen
parent: Kapitel
nav_order: 105
---
# Kapitel 5 – Vertiefung: Verzweigungen und Entscheidungslogik

[← Zurück zu Kapitel 5](05-Verzweigungen.md) | [Übersicht](../README.md)

Computer wirken intelligent, weil sie Entscheidungen treffen können. Doch wie genau hangelt sich C# durch Bedingungen? Warum ist der Unterschied zwischen mehreren `if`s und einer Kette aus `else if` so entscheidend?

---

## 1. Die große Falle: `if` vs. `else if`

Stell dir eine Ampel vor: Sie kann rot, gelb oder grün sein – aber immer nur **eines** davon.

Schau dir diesen Fehler an:

```csharp
int punkte = 95;

// FALSCH: Lauter einzelne ifs
if (punkte >= 50)
{
    Console.WriteLine("Bestanden!");
}
if (punkte >= 90)
{
    Console.WriteLine("Auszeichnung erhalten!");
}
```
**Ausgabe:**
```
Bestanden!
Auszeichnung erhalten!
```
Beide Blöcke werden ausgeführt, weil C# jedes `if` als **völlig neue, unabhängige Frage** betrachtet!

### Die Lösung: Die `else if`-Kette

Bei einer `if - else if - else`-Kette gilt: **Sobald eine Tür aufgeht, sind alle anderen Türen für immer verschlossen!**

```csharp
if (punkte >= 90)
{
    Console.WriteLine("Note: Sehr Gut");
}
else if (punkte >= 75)
{
    Console.WriteLine("Note: Gut");
}
else if (punkte >= 50)
{
    Console.WriteLine("Note: Bestanden");
}
else
{
    Console.WriteLine("Nicht bestanden");
}
```
*Beachte auch die Reihenfolge:* Wir prüfen von **oben nach unten**, beginnend mit dem höchsten Wert. Wenn du mit `punkte >= 50` angefangen hättest, wäre jeder mit 95 Punkten sofort bei "Bestanden" gelandet und hätte nie das "Sehr Gut" erreicht!

---

## 2. Der unsichtbare Killer: Das Semikolon nach dem `if`

Einer der am schwersten zu findenden Tippfehler ist dieser:

```csharp
int alter = 12;

if (alter >= 18); // <-- ACHTUNG: Hier steht ein Semikolon!
{
    Console.WriteLine("Du darfst Auto fahren!");
}
```
**Was passiert hier?**
Das Programm gibt fröhlich *"Du darfst Auto fahren!"* aus, obwohl `alter` nur 12 ist!

**Warum?**
Ein Semikolon beendet eine Anweisung. C# liest das so:
`if (alter >= 18)` -> *Mache gar nichts! (leere Anweisung)*
Danach kommt ein ganz gewöhnlicher Code-Block in `{ }`, der **in jedem Fall** ausgeführt wird!

**Merke:** Hinter die runde Klammer einer Bedingung `if (...)` kommt **niemals** direkt ein Semikolon `;`!

---

## 3. `switch` vs. `if`: Wann nimmt man was?

| Kriterium | `if` / `else if` | `switch` |
|---|---|---|
| Wertebereiche | Perfekt: `alter >= 18 && alter < 67` | Umständlich / nicht dafür gedacht |
| Feste Einzelwerte | Möglich, aber viel Schreibarbeit | Perfekt: Menüs (`"1"`, `"2"`, `"3"`), Wochentage |
| Logische Verknüpfungen | Beliebig mit `&&` und `\|\|` kombinierbar | Nur ein einziger Wert wird verglichen |

### Der Trick mit `switch` und gemeinsamen Fällen

Wusstest du, dass mehrere `case`-Blöcke denselben Code ausführen können, wenn du das `break` weglässt?

```csharp
Console.Write("Gib einen Buchstaben ein: ");
char c = char.ToLower(Console.ReadKey().KeyChar);
Console.WriteLine();

switch (c)
{
    case 'a':
    case 'e':
    case 'i':
    case 'o':
    case 'u':
        Console.WriteLine("Das ist ein Vokal!");
        break;
    default:
        Console.WriteLine("Das ist ein Konsonant oder Sonderzeichen.");
        break;
}
```

---

## 4. Vertiefungsaufgaben

### Vertiefungsaufgabe 5.1 – Logik aufräumen
Vereinfache folgenden verschachtelten Code in ein einziges, übersichtliches `if - else`:

```csharp
if (istWochenende == true)
{
    if (scheintSonne == true)
    {
        Console.WriteLine("Ab an den See!");
    }
    else
    {
        Console.WriteLine("Zuhause Serien schauen.");
    }
}
else
{
    Console.WriteLine("Arbeiten oder lernen.");
}
```

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
if (istWochenende && scheintSonne)
{
    Console.WriteLine("Ab an den See!");
}
else if (istWochenende && !scheintSonne)
{
    Console.WriteLine("Zuhause Serien schauen.");
}
else
{
    Console.WriteLine("Arbeiten oder lernen.");
}
```
*Tipp:* `== true` lässt man in C# einfach weg. `if (istWochenende)` ist viel eleganter und liest sich wie gesprochenes Deutsch!
</details>

### Vertiefungsaufgabe 5.2 – Das Text-Abenteuerspiel
Erstelle ein winziges textbasiertes Verzweigungs-Abenteuer:
Der Spieler steht vor zwei Türen ("links" oder "rechts").
- Links: Dahinter ist ein schlafender Drache. Der Spieler kann "schleichen" oder "rennen".
- Rechts: Dahinter ist eine Schatztruhe. Der Spieler kann "oeffnen" oder "umkehren".

Baue die Entscheidungen mit `if` oder `switch` und fang Fehleingaben mit einem `else` / `default` ab.
