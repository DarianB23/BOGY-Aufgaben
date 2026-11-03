---
title: 04 - Vertiefung Benutzereingaben
parent: Vertiefung
nav_order: 4
---
# Kapitel 4 – Vertiefung: Benutzereingaben und Typumwandlung

[← Zurück zu Kapitel 4](04-Eingaben.md) | [Übersicht](../README.md)

Bis jetzt war dein Programm stumm und starr. Mit `Console.ReadLine()` wird es interaktiv. Doch warum liefert `ReadLine()` immer nur Text? Und warum stürzt das Programm ab, wenn man sich vertippt? Das klären wir hier.

---

## 1. Was macht `Console.ReadLine()` eigentlich?

Wenn der Computer an die Zeile `string eingabe = Console.ReadLine();` gelangt, passiert Folgendes:

1. **Der Computer hält an:** Er wartet geduldig, bis der Benutzer Tasten drückt.
2. **Der Eingabepuffer:** Jeder Tastendruck wird in einem temporären Zwischenspeicher gesammelt.
3. **Die Enter-Taste:** Erst wenn du **Enter** drückst, liest `ReadLine()` den gesamten Text bis zum Zeilenumbruch aus und übergibt ihn an dein Programm.
4. **Wichtig:** Für die Tastatur ist jedes Zeichen zunächst ein Textzeichen (`char`). Selbst wenn du `42` tippst, liest der Computer das Zeichen `'4'` und das Zeichen `'2'` – also den Text `"42"`.

---

## 2. Text vs. Zahl: Das `+` Problem

Weil `ReadLine()` immer einen `string` zurückgibt, passiert ohne Umwandlung Folgendes:

```csharp
Console.Write("Gib eine Zahl ein: ");
string a = Console.ReadLine(); // Benutzer tippt: 10

Console.Write("Gib noch eine Zahl ein: ");
string b = Console.ReadLine(); // Benutzer tippt: 20

Console.WriteLine(a + b); // Ausgabe: 1020 (Text aneinandergehängt!)
```

Das Plus-Zeichen `+` hat in C# eine doppelte Bedeutung:
- Bei zwei Zahlen: **Mathematische Addition** (`10 + 20 = 30`).
- Bei Texten: **Aneinanderhängen (Konkatenation)** (`"10" + "20" = "1020"`).

---

## 3. Was bedeutet "Parsen" (z. B. `int.Parse`)?

Das englische Wort *to parse* bedeutet *grammatikalisch zerlegen und verstehen*.

Wenn du `int.Parse("123")` aufrufst, geschieht intern Folgendes:
1. C# prüft jedes Zeichen: Ist `'1'` eine Ziffer? Ja (Wert 1).
2. Ist `'2'` eine Ziffer? Ja. Aus `1` und `2` wird der Wert `12`.
3. Ist `'3'` eine Ziffer? Ja. Der Gesamtwert wird als die Ganzzahl `123` im Speicher abgelegt.
4. Trifft C# dabei auf einen Buchstaben wie `"12a3"`, weiß es nicht mehr weiter und bricht mit einer Fehlermeldung (**`FormatException`**) ab!

---

## 4. Die Sache mit Punkt und Komma

Warum stürzt `double.Parse("1.5")` auf vielen deutschen PCs ab?

- In Deutschland schreiben wir Kommazahlen mit einem **Komma**: `1,5 Euro`.
- In den USA schreibt man Kommazahlen mit einem **Punkt**: `1.5 Dollars`.
- C# orientiert sich bei `double.Parse(...)` an der **Spracheinstellung deines Betriebssystems**.
  - Deutsches Windows: Erwartet bei der Eingabe im Terminal `1,5`.
  - Englischer Code: Im C#-Quellcode schreibst du immer den Punkt `1.5`.

{% capture tipp %}
Wenn du möchtest, dass dein Programm immer den Punkt akzeptiert (egal auf welchem PC), kannst du später `CultureInfo.InvariantCulture` kennenlernen. Für den Moment gilt: **Im Code der Punkt, bei der Konsolen-Eingabe auf deutschen Tastaturen das Komma!**
{% endcapture %}
{% include tipp.html inhalt=tipp %}

---

## 5. Die grüne Wellenlinie bei `ReadLine()`

Vielleicht ist dir in Visual Studio eine kleine gelbe oder grüne Welle unter `Console.ReadLine()` aufgefallen mit der Meldung:
`CS8600: Das NULL-Literal oder ein möglicher NULL-Wert wird in einen Non-Nullable-Typ konvertiert.`

**Was bedeutet das?**
Der Benutzer könnte rein theoretisch die Eingabe abbrechen (z. B. mit `Strg + Z` im Terminal). Dann würde `ReadLine()` den Wert `null` ("gar nichts / kein Objekt") zurückgeben.
C# möchte dich nur freundlich warnen: *"Hey, bist du sicher, dass da Text ankommt?"*

Für Einsteiger gilt: Du kannst diese Warnung vorerst getrost ignorieren, oder du schreibst `string? eingabe = Console.ReadLine();` (mit einem Fragezeichen hinter `string`), um C# zu beruhigen.

---

## 6. Vertiefungsaufgaben

### Vertiefungsaufgabe 4.1 – Die Kassen-Abrechnung
Schreibe ein Programm für einen Kiosk:
1. Frage nach dem Preis eines Artikels (als `double`).
2. Frage nach der Stückzahl (als `int`).
3. Frage, wie viel Bargeld der Kunde gibt (als `double`).
4. Berechne den Gesamtpreis und das Wechselgeld und gib beides übersichtlich aus.

<details>
<summary>👉 Klicke hier für eine Beispiellösung</summary>

```csharp
Console.Write("Preis pro Stück (z. B. 1,50): ");
double einzelpreis = double.Parse(Console.ReadLine());

Console.Write("Anzahl: ");
int anzahl = int.Parse(Console.ReadLine());

double gesamtpreis = einzelpreis * anzahl;
Console.WriteLine($"Das macht zusammen: {gesamtpreis:0.00} €");

Console.Write("Gegebener Betrag: ");
double gegeben = double.Parse(Console.ReadLine());

double wechselgeld = gegeben - gesamtpreis;
Console.WriteLine($"Wechselgeld: {wechselgeld:0.00} €");
```
</details>

### Vertiefungsaufgabe 4.2 – Denkaufgabe: Was passiert hier?
Schau dir folgenden Code an:
```csharp
Console.Write("Alter: ");
int alter = int.Parse(Console.ReadLine());
Console.WriteLine("In 5 Jahren bist du: " + alter + 5);
```
Warum gibt das Programm bei Eingabe von `20` am Ende `In 5 Jahren bist du: 205` aus?
Wie reparierst du den Fehler?

<details>
<summary>👉 Klicke hier für die Erklärung</summary>

C# arbeitet von links nach rechts. Sobald ein `string` ("In 5 Jahren bist du: ") mit einer Zahl (`alter = 20`) mit `+` verbunden wird, wird das Gesamte zu Text ("In 5 Jahren bist du: 20"). Das nächste `+ 5` hängt dann einfach wieder die `5` hinten als Text an!

**Lösung 1 (Klammern):**
`Console.WriteLine("In 5 Jahren bist du: " + (alter + 5));`

**Lösung 2 (viel schöner mit `$`):**
`Console.WriteLine($"In 5 Jahren bist du: {alter + 5}");`
</details>
