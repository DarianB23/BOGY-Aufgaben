---
title: 10 - Vertiefung Fehler abfangen
parent: Kapitel
nav_order: 110
---
# Kapitel 10 – Vertiefung: Fehlerbehandlung und TryParse

[← Zurück zu Kapitel 10](10-Fehler-abfangen.md) | [Übersicht](../README.md)

Ein Programm, das abstürzt, frustriert Benutzer. Professionelle Software zeichnet sich dadurch aus, dass sie stabil bleibt – egal, was der Benutzer eintippt. Hier schauen wir uns an, warum `int.TryParse` so genial ist und wie das Geheimnis des Wortes `out` gelüftet wird.

---

## 1. Was ist eine Exception wirklich?

Eine **Exception** (Ausnahme) ist das Signal von C#: *"Hier ist etwas passiert, mit dem ich unmöglich alleine weitermachen kann!"*

Wenn ein Fehler auftritt und **kein** `catch`-Block da ist, stürzt das Programm ab.
Ein `try / catch` funktioniert wie ein Rettungsnetz:

```csharp
try
{
    // Akrobat auf dem Seil (kann schiefgehen!)
    int ergebnis = 100 / teiler;
}
catch (DivideByZeroException)
{
    // Das Rettungsnetz fängt den Sturz sanft ab
    Console.WriteLine("Teilen durch 0 ist in der Mathematik nicht erlaubt!");
}
```

---

## 2. `try-catch` vs. `TryParse`: Wann nimmt man was?

| Eigenschaft | `try-catch` | `TryParse` |
|---|---|---|
| **Einsatzgebiet** | Echte, unvorhersehbare Notfälle (z. B. Datei auf der Festplatte plötzlich gelöscht, Server nicht erreichbar). | Völlig normale Benutzereingaben, die sich vertippen können. |
| **Geschwindigkeit** | Sehr langsam, wenn ein Fehler auftritt (Computer muss den Notfall protokollieren). | Rasend schnell, weil es nur ein simpler `true/false`-Check ist. |
| **Code-Stil** | Schreibt man für System- und Hardware-Probleme. | Standard für jede Konsolen-Zahleneingabe! |

**Faustregel:** Nutze für Benutzereingaben an der Konsole immer **`int.TryParse`** oder **`double.TryParse`** statt `try-catch`!

---

## 3. Was bedeutet das Zauberwort `out`?

Normalerweise kann eine C#-Methode mit `return` immer nur **einen einzigen Wert** zurückgeben.

Bei einer Zahleneingabe wollen wir aber eigentlich **zwei Dinge auf einmal** wissen:
1. Hat die Umwandlung überhaupt geklappt? (`bool`: `true` oder `false`)
2. Wenn ja, wie lautet die umgewandelte Zahl? (`int`)

Hier kommt der **`out`-Parameter** ins Spiel:

```csharp
bool erfolg = int.TryParse("42", out int zahl);
```

- Das `out` vor der Variablen sagt: *"Liebe Methode `TryParse`, ich gebe dir meine leere Variable `zahl` mit. Bitte fülle sie im Inneren mit dem fertigen Ergebnis!"*
- Der normale Rückgabewert der Methode (`true`/`false`) landet in `erfolg`.
- Der Wert der Zahl landet direkt in der `out`-Variable `zahl`!

---

## 4. Die unzerstörbare Eingabe-Schleife: Schritt für Schritt erklärt

Hier ist das Standard-Muster, das du in jedem professionellen Programm wiederfinden wirst:

```csharp
int zahl;
while (!int.TryParse(Console.ReadLine(), out zahl))
{
    Console.ForegroundColor = ConsoleColor.Red;
    Console.Write("Ungültig! Bitte gib eine ganze Zahl ein: ");
    Console.ResetColor();
}
```

**Wie das Schritt für Schritt tickt:**
1. `Console.ReadLine()` holt den Text ab.
2. `int.TryParse(...)` prüft den Text:
   - Ist es `"123"`? `TryParse` liefert `true`, schreibt 123 in `zahl`. Durch das `!` davor wird daraus `false` -> **Die Schleife bricht ab, weiter geht's!**
   - Ist es `"abc"`? `TryParse` liefert `false`. Durch das `!` davor wird daraus `true` -> **Die Schleife läuft noch einmal** und fragt erneut nach!

---

## 5. Mehrere Fehler gezielt abfangen

Wenn du doch `try-catch` nutzt, kannst du mehrere Fangnetze untereinander spannen:

```csharp
try
{
    Console.Write("Zahl 1: ");
    int a = int.Parse(Console.ReadLine());
    Console.Write("Zahl 2: ");
    int b = int.Parse(Console.ReadLine());
    Console.WriteLine($"Ergebnis: {a / b}");
}
catch (FormatException)
{
    Console.WriteLine("Fehler: Du hast keine Zahl eingegeben!");
}
catch (DivideByZeroException)
{
    Console.WriteLine("Fehler: Du darfst nicht durch 0 teilen!");
}
catch (Exception ex)
{
    // Fängt alle anderen, unbekannten Fehler ab
    Console.WriteLine($"Unerwarteter Fehler: {ex.Message}");
}
```

---

## 6. Vertiefungsaufgaben

### Vertiefungsaufgabe 10.1 – Altersabfrage mit Wertebereich
Schreibe eine Schleife, die das Alter des Benutzers abfragt. Das Programm darf **nur** weitergehen, wenn:
- eine echte Zahl eingegeben wurde, **UND**
- die Zahl zwischen `0` und `120` liegt.

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
int alter;
while (true)
{
    Console.Write("Gib dein Alter ein (0-120): ");
    if (int.TryParse(Console.ReadLine(), out alter) && alter >= 0 && alter <= 120)
    {
        break; // Alles super, wir verlassen die Schleife!
    }
    Console.WriteLine("Ungültige Eingabe! Bitte ein realistisches Alter eingeben.");
}

Console.WriteLine($"Erfasst: Alter {alter} Jahre.");
```
</details>

### Vertiefungsaufgabe 10.2 – Die universelle `LiesGanzeZahl`-Methode
Schreibe eine Methode mit folgendem Kopf:
`int LiesGanzeZahl(string meldung, int min, int max)`
Sie soll die `meldung` ausgeben und so lange nachfragen, bis eine Zahl zwischen `min` und `max` eingegeben wurde.
