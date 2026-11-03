---
title: 07 - Vertiefung: Arrays und Listen
parent: Kapitel
nav_order: 107
---
# Kapitel 7 – Vertiefung: Arrays, Listen und Indizes

[← Zurück zu Kapitel 7](07-Collections.md) | [Übersicht](../README.md)

Warum fängt der Computer bei 0 an zu zählen? Warum stürzt das Programm ab, wenn man auf Platz 4 eines 4-elementigen Arrays zugreift? Und warum heißen die Eigenschaften einmal `.Length` und einmal `.Count`? Hier erfährst du die Hintergründe.

---

## 1. Warum fängt der Index bei 0 an?

Fast jeder Programmieranfänger wundert sich: *"Warum ist das erste Element nicht Nummer 1?"*

Der Grund liegt tief in der Geschichte des Computer-Speichers:
- Ein Array liegt als zusammenhängender Block im Arbeitsspeicher (RAM).
- Die Variable speichert eigentlich nur die **Start-Adresse** dieses Blocks.
- Der **Index** ist der **Abstand (Offset)** vom Startpunkt!

```
Array im Speicher:
[ Erstes Element ] [ Zweites Element ] [ Drittes Element ]
      ▲                     ▲                   ▲
      │                     │                   │
Start + 0 Schritte    Start + 1 Schritt   Start + 2 Schritte
  (Index 0)             (Index 1)           (Index 2)
```

- Um zum 1. Element zu kommen, musst du **0 Schritte** vom Start weglaufen -> `[0]`.
- Um zum 2. Element zu kommen, machst du **1 Schritt** -> `[1]`.
- Daher hat das letzte Element bei 4 Einträgen den Index **3**!

---

## 2. Array vs. Liste: Die Analogie

| Eigenschaft | Array (`int[]`) | Liste (`List<int>`) |
|---|---|---|
| **Analogie** | Ein **Eierkarton** mit 6 Mulden | Ein **Einkaufszettel** aus Papier |
| **Größe** | Einmal hergestellt, nie mehr änderbar. | Kann wachsen (`.Add()`) und schrumpfen (`.Remove()`). |
| **Größen-Eigenschaft** | `.Length` (feste Länge) | `.Count` (aktuelle Zählung) |
| **Wann nutzen?** | Wenn die Anzahl von vornherein unveränderlich feststeht (z. B. 12 Monate, 7 Wochentage). | In 90 % der Fälle im Alltag: Wenn Daten hinzukommen oder gelöscht werden. |

---

## 3. Die berüchtigte `IndexOutOfRangeException`

```csharp
string[] tiere = { "Hund", "Katze", "Maus" }; // Größe 3: Indizes 0, 1, 2

Console.WriteLine(tiere[3]); // BUMM! IndexOutOfRangeException
```

**Was bedeutet das?**
Du hast dem Computer befohlen: *"Geh an die Startadresse von `tiere` und laufe 3 Schritte weiter."* Dort liegt aber kein Tier mehr, sondern fremder Arbeitsspeicher! Um dich und das System zu schützen, zieht C# sofort die Notbremse.

**Die goldene Regel zur Vermeidung:**
Der gültige Index liegt immer zwischen `0` und `Anzahl - 1`.
Prüfe im Zweifel vorher:
```csharp
int wunschIndex = 3;
if (wunschIndex >= 0 && wunschIndex < tiere.Length)
{
    Console.WriteLine(tiere[wunschIndex]);
}
else
{
    Console.WriteLine("Diesen Platz gibt es nicht!");
}
```

---

## 4. Die Kardinalsünde: Die Liste in `foreach` verändern

Probiere das **niemals** so aus:

```csharp
List<string> namen = new List<string> { "Anna", "Ben", "Clara" };

foreach (string n in namen)
{
    if (n == "Ben")
    {
        namen.Remove(n); // FEHLER: InvalidOperationException!
    }
}
```

**Warum ist das verboten?**
`foreach` verlässt sich darauf, dass die Liste während des Durchlaufs absolut unverändert bleibt. Wenn du mittendrin ein Element entfernst, gerät der interne Zähler völlig aus dem Takt.

**Wie macht man es richtig?**
1. **Möglichkeit A:** Mit einer Rückwärts-`for`-Schleife:
   ```csharp
   for (int i = namen.Count - 1; i >= 0; i--)
   {
       if (namen[i] == "Ben")
       {
           namen.RemoveAt(i);
       }
   }
   ```
2. **Möglichkeit B:** Die elegante C#-Methode:
   ```csharp
   namen.RemoveAll(n => n == "Ben");
   ```

---

## 5. Vertiefungsaufgaben

### Vertiefungsaufgabe 7.1 – Array rückwärts ausgeben
Lege ein Array mit 5 Wörtern an. Gib alle Wörter in **umgekehrter Reihenfolge** (vom letzten zum ersten) mit einer `for`-Schleife aus:

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
string[] worte = { "Eins", "Zwei", "Drei", "Vier", "Fünf" };

for (int i = worte.Length - 1; i >= 0; i--)
{
    Console.WriteLine(worte[i]);
}
```
*Beachte:* Wir starten bei `worte.Length - 1` (Index 4) und laufen, solange `i >= 0` ist, mit `i--`.
</details>

### Vertiefungsaufgabe 7.2 – Das Zufalls-Los
Schreibe ein Programm, das eine Liste von 5 Teilnehmern hat. Ziehe mit `Random.Shared.Next(0, teilnehmer.Count)` einen zufälligen Gewinner und gib ihn aus:

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
List<string> teilnehmer = new List<string> { "Tom", "Sara", "Mia", "Leo", "Jonas" };

int gewinnerIndex = Random.Shared.Next(0, teilnehmer.Count);
Console.WriteLine($"Der Gewinner ist: {teilnehmer[gewinnerIndex]}!");
```
</details>
