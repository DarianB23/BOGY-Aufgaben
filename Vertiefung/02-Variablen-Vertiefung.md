---
title: 02 - Vertiefung Variablen und Datentypen
parent: Vertiefung
nav_order: 102
---
# Kapitel 2 – Vertiefung: Variablen und Datentypen

[← Zurück zu Kapitel 2](02-Variablen.md) | [Übersicht](../README.md)

Variablen wirken am Anfang simpel, führen aber oft zu Verwirrung: *Warum muss ich den Typ angeben? Warum darf ich später kein `int` mehr davor schreiben? Was bedeutet das `$` bei Strings?* Hier klären wir diese Fragen im Detail.

---

## 1. Was passiert wirklich im Computer-Speicher?

Stell dir den Arbeitsspeicher (RAM) deines PCs wie ein riesiges Regal mit nummerierten Fächern vor:

1. Wenn du schreibst:
   ```csharp
   int alter = 25;
   ```
   reserviert der Computer ein Fach im Regal, das groß genug für eine ganze Zahl ist (bei `int` sind das genau 4 Bytes / 32 Bits), gibt diesem Fach das Namensschild **`alter`** und legt die Zahl **`25`** hinein.

2. **Deklaration vs. Zuweisung:**
   - **Deklaration:** `int alter;` -> "Bitte reserviere mir eine Box für eine ganze Zahl namens 'alter'."
   - **Initialisierung:** `alter = 25;` -> "Lege als Startwert 25 in die Box."
   - **Zuweisung:** `alter = 26;` -> "Nimm den alten Inhalt raus und lege 26 rein."

### Warum steht später kein `int` mehr davor?

Das ist der **häufigste Anfängerfehler**:
```csharp
int punkte = 10;
int punkte = 20; // FEHLER: Eine lokale Variable namens "punkte" ist bereits definiert!
```
Wenn du nochmals `int punkte` schreibst, sagst du dem Computer: *"Erstelle bitte eine NEUE Box namens punkte."* Der Computer antwortet: *"Halt! Ich habe schon eine Box mit diesem Namen!"*

Möchtest du den Wert nur ändern, nennst du **nur den Namen**:
```csharp
punkte = 20; // Richtig: Greift auf die existierende Box zu und überschreibt den Wert.
```

---

## 2. Die Typen-Familie genauer betrachtet

| Typ | Größe im Speicher | Wertebereich / Besonderheit | Typische Eselsbrücke |
|---|---|---|---|
| `int` | 4 Bytes | ca. -2,1 Milliarden bis +2,1 Milliarden | Ganze Zählbare Dinge (Äpfel, Alter, Punktestand) |
| `double` | 8 Bytes | Sehr große/kleine Kommazahlen (ca. 15-17 Stellen genau) | Messwerte, Gewichte, Preise (`19.99`) |
| `bool` | 1 Byte | Nur `true` (wahr) oder `false` (falsch) | Lichtschalter: an oder aus |
| `char` | 2 Bytes | Genau **ein** Zeichen in einfachen Hochkommas: `'A'` | Einzelner Buchstabe auf einer Scrabble-Tafel |
| `string` | dynamisch | Ganzer Text in doppelten Anführungszeichen: `"Hallo"` | Perlenkette aus vielen `char`-Zeichen |

### `char` vs. `string` – Der Unterschied zwischen `'A'` und `"A"`

- `'A'` (`char`): Ist ein einzelnes Zeichen. C# speichert intern einfach die Ziffer 65 (den Unicode-Code für 'A').
- `"A"` (`string`): Ist ein Text-Objekt, das zufällig nur aus einem Zeichen besteht.
- **Merkhilfe:** Ein einzelnes Zeichen bekommt einfache Striche `' '`. Text bekommt doppelte Striche `" "`.

---

## 3. String-Interpolation: Das magische `$`

Vergleiche diese beiden Zeilen:
```csharp
string name = "Sam";
Console.WriteLine("Hallo {name}!");   // Ausgabe: Hallo {name}!
Console.WriteLine($"Hallo {name}!");  // Ausgabe: Hallo Sam!
```

Das **`$` vor dem Anführungszeichen** schaltet den "Dolmetscher-Modus" ein. Es sagt C#: *"Achtung! Wenn geschweifte Klammern `{ }` im Text vorkommen, gib sie nicht wörtlich aus, sondern schau nach, was in der Variablen drinsteht!"*

Du kannst in `{ }` sogar direkt rechnen:
```csharp
int preis = 5;
Console.WriteLine($"Zwei Stück kosten {preis * 2} Euro."); // Ausgabe: Zwei Stück kosten 10 Euro.
```

---

## 4. Vertiefungsaufgaben

### Vertiefungsaufgabe 2.1 – Der Gläsertausch (Variablen tauschen)
Stell dir vor, du hast zwei Variablen:
```csharp
int glasWasser = 1;  // Glas 1 enthält Wasser
int glasSaft = 2;    // Glas 2 enthält Saft
```
Ziel: Tausche die Werte so, dass `glasWasser` am Ende `2` und `glasSaft` am Ende `1` hat.
Wenn du einfach `glasWasser = glasSaft;` schreibst, ist das Wasser für immer verloren!

Wie schaffst du den Tausch mithilfe einer dritten Hilfsvariablen `int hilfsGlas`?

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
int glasWasser = 1;
int glasSaft = 2;

// Tausch über eine temporäre Hilfsvariable:
int hilfsGlas = glasWasser; // Wasser zwischenparken
glasWasser = glasSaft;      // Saft ins Wasserglas kippen
glasSaft = hilfsGlas;       // Zwischengeparktes Wasser ins Saftglas kippen

Console.WriteLine($"Wasserglas: {glasWasser}, Saftglas: {glasSaft}");
```
</details>

### Vertiefungsaufgabe 2.2 – Welcher Datentyp passt?
Wähle für folgende Beispiele den perfekten Datentyp (`string`, `int`, `double`, `bool`, `char`):
1. Die Anzahl der Leben in einem Spiel (z. B. 3)
2. Der Preis eines Liters Benzin (z. B. 1.749)
3. Eine Postleitzahl (z. B. "01067" – Achtung, Fangfrage!)
4. Ob ein Benutzer eingeloggt ist (ja/nein)
5. Die Schulnote als Buchstabe (z. B. 'A')

<details>
<summary>👉 Klicke hier für die Erklärung</summary>

1. `int` (Ganze Zahl für Leben)
2. `double` (Kommazahl für Benzinpreis)
3. `string`! Warum nicht `int`? Weil Postleitzahlen mit einer `0` beginnen können (z. B. Dresden `01067`). Ein `int` würde die führende Null einfach weglöschen (`1067`)! Außerdem rechnet man nie mit Postleitzahlen.
4. `bool` (Nur zwei Zustände möglich: eingeloggt oder nicht)
5. `char` (Genau ein Zeichen in einfachen Anführungszeichen)
</details>
