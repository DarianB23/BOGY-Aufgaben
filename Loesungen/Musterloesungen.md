# Musterlösungen (nur für Betreuer*innen)

> [!WARNING]
> Diese Datei ist für Betreuer*innen gedacht. Am besten nicht im Haupt-Branch verlinken oder in einem separaten Branch/Repository ablegen.
> Es gibt fast immer **mehrere richtige Lösungen** – eine andere Lösung ist kein Fehler, solange sie funktioniert und verständlich ist.

[🏠 Übersicht](../README.md)

---

## Kapitel 1 – Erste Schritte

**1.1**
```csharp
Console.WriteLine("Ich heiße Lea.");
Console.WriteLine("Mein Lieblingshobby ist Klettern.");
```

**1.2** `Console.Write` springt nach dem Text **nicht** in die nächste Zeile. Der nächste Text wird direkt dahinter geschrieben.

**1.3** Fehlendes Semikolon → `CS1002: ; erwartet` mit Zeilenangabe. `console` klein → `CS0103: Der Name "console" ist im aktuellen Kontext nicht vorhanden.`

**1.4** Backslashes müssen doppelt geschrieben werden:
```csharp
Console.WriteLine("   /\\");
Console.WriteLine("  /  \\");
Console.WriteLine(" /____\\");
Console.WriteLine(" |    |");
Console.WriteLine(" |_[]_|");
```

---

## Kapitel 2 – Variablen

**2.1**
```csharp
string produktName = "Kopfhörer";
double preis = 19.99;
bool verfuegbar = true;

Console.WriteLine($"Das Produkt {produktName} kostet {preis} € und ist verfügbar: {verfuegbar}");
```
Denkfrage: `19.99` → `double` (oder `decimal`), `42` → `int` würde reichen. Da Preise aber oft Nachkommastellen haben, ist `double` für einen Preis immer sinnvoll.

**2.2** Nach `preis = 14.99;` erneut ausgeben. Eine Änderung an `const` führt zu Fehler **CS0131** (Die linke Seite einer Zuweisung muss eine Variable … sein).

---

## Kapitel 3 – Rechnen und Vergleichen

**3.1** Ausgabe: `13`, `7`, `30`, `3`, `1`

**3.2** `a` und `b` sind `int`. Ganze Zahlen können keine Nachkommastellen speichern, deshalb werden diese einfach **abgeschnitten** (nicht gerundet).
```csharp
// Weg 1: Typ ändern
double a = 10;
double b = 3;
Console.WriteLine(a / b);

// Weg 2: Umwandeln (Cast)
int c = 10;
int d = 3;
Console.WriteLine((double)c / d);
```

**3.3**
```csharp
int sekundenGesamt = 3725;
int stunden = sekundenGesamt / 3600;
int minuten = (sekundenGesamt % 3600) / 60;
int sekunden = sekundenGesamt % 60;

Console.WriteLine($"{sekundenGesamt} Sekunden sind {stunden} Stunde(n), {minuten} Minute(n) und {sekunden} Sekunde(n).");
```

**3.4** `true`, `false`, `true`, `false`, `true`

---

## Kapitel 4 – Benutzereingaben

**4.1**
```csharp
Console.Write("Wie heißt du? ");
string name = Console.ReadLine();

Console.Write("In welchem Jahr bist du geboren? ");
int geburtsjahr = int.Parse(Console.ReadLine());

int jahr = DateTime.Now.Year;
Console.WriteLine($"Hallo {name}, du wirst dieses Jahr {jahr - geburtsjahr} Jahre alt.");
```

**4.2**
```csharp
Console.Write("Temperatur in Celsius: ");
int celsius = int.Parse(Console.ReadLine());
int fahrenheit = celsius * 9 / 5 + 32;
Console.WriteLine($"Die Temperatur {celsius}° Celsius entspricht circa {fahrenheit}° Fahrenheit.");
```
Genaues Ergebnis: `celsius` und `fahrenheit` als `double` und `double.Parse` verwenden → `82,4`.
Falle: `celsius * (9 / 5)` – `9 / 5` ist eine Ganzzahl-Division und ergibt `1`. Dann wird nur noch `celsius + 32` gerechnet.

**4.3**
```csharp
Console.Write("Erste Zahl: ");
int a = int.Parse(Console.ReadLine());
Console.Write("Zweite Zahl: ");
int b = int.Parse(Console.ReadLine());

Console.WriteLine($"{a} + {b} = {a + b}");
Console.WriteLine($"{a} - {b} = {a - b}");
Console.WriteLine($"{a} * {b} = {a * b}");
Console.WriteLine($"{a} / {b} = {a / b}");
Console.WriteLine($"{a} % {b} = {a % b}");
```

**4.4** `abc` → `FormatException`. Auf deutschem Windows ergibt `double.Parse("1.5")` den Wert `15`, weil der Punkt als Tausendertrennzeichen interpretiert wird. `1,5` ergibt `1,5`.

---

## Kapitel 5 – Verzweigungen

**5.1** Bei `alter = 20` werden **beide** Meldungen ausgegeben („erwachsen“ **und** „Teenager“), weil das zweite `if` eine neue, unabhängige Verzweigung ist. Das `else` gehört dann nur noch zum zweiten `if`.

**5.2**
```csharp
Console.Write("Gib eine Zahl ein: ");
int zahl = int.Parse(Console.ReadLine());

if (zahl == 0)
{
    Console.WriteLine("Die Zahl 0 ist weder positiv noch negativ, aber trotzdem gerade.");
}
else
{
    string vorzeichen;
    if (zahl > 0)
    {
        vorzeichen = "positive";
    }
    else
    {
        vorzeichen = "negative";
    }

    string art;
    if (zahl % 2 == 0)
    {
        art = "gerade";
    }
    else
    {
        art = "ungerade";
    }

    Console.WriteLine($"Die Zahl {zahl} ist eine {art} {vorzeichen} Zahl.");
}
```

**5.3**
```csharp
// Teil 1
Console.Write("Note (1-6): ");
int note = int.Parse(Console.ReadLine());

switch (note)
{
    case 1: Console.WriteLine("sehr gut"); break;
    case 2: Console.WriteLine("gut"); break;
    case 3: Console.WriteLine("befriedigend"); break;
    case 4: Console.WriteLine("ausreichend"); break;
    case 5: Console.WriteLine("mangelhaft"); break;
    case 6: Console.WriteLine("ungenügend"); break;
    default: Console.WriteLine("Das ist keine gültige Note."); break;
}

// Teil 2
Console.Write("Punkte (0-100): ");
int punkte = int.Parse(Console.ReadLine());
int ergebnisNote;

if (punkte >= 90) { ergebnisNote = 1; }
else if (punkte >= 80) { ergebnisNote = 2; }
else if (punkte >= 65) { ergebnisNote = 3; }
else if (punkte >= 50) { ergebnisNote = 4; }
else if (punkte >= 30) { ergebnisNote = 5; }
else { ergebnisNote = 6; }

Console.WriteLine($"Mit {punkte} Punkten bekommst du die Note {ergebnisNote}.");
```

**5.5**
```csharp
Console.Write("Alter: ");
int alter = int.Parse(Console.ReadLine());
Console.Write("Wochentag: ");
string tag = Console.ReadLine();

int preis;
if (alter < 12) { preis = 6; }
else if (alter < 18) { preis = 8; }
else { preis = 10; }

if (tag == "Dienstag")
{
    preis -= 2;
}

Console.WriteLine($"Der Eintritt kostet {preis} €.");
```

---

## Kapitel 6 – Schleifen

**6.1**
1. Startet `i` bei 6, ist `i <= 5` sofort falsch → die Schleife läuft **kein einziges Mal**.
2. `i` wird immer größer und nie kleiner als 0 → praktisch eine Endlosschleife. (Nach `2147483647` springt `int` auf den kleinsten negativen Wert um – erst dann endet die Schleife, nach über 2 Milliarden Durchläufen.)
3. Ausgabe: `10`, `7`, `4`, `1`. Danach ist `nummer` `-2`, die Bedingung `nummer > 0` ist falsch.
4. Die Schleife endet erst bei genau `ende`. `Ende` ist ein anderer Text → die Schleife läuft weiter.

**6.2**
```csharp
for (int i = 1; i <= 10; i++) { Console.WriteLine(i); }

for (int i = 2; i <= 20; i += 2) { Console.WriteLine(i); }

for (int i = 1; i <= 20; i++)
{
    if (i % 2 == 0) { Console.WriteLine(i); }
}

for (int i = 10; i >= 1; i--) { Console.WriteLine(i); }
Console.WriteLine("Start!");
```

**6.3**
```csharp
Console.Write("Welche Reihe? ");
int reihe = int.Parse(Console.ReadLine());

for (int i = 1; i <= 10; i++)
{
    Console.WriteLine($"{i} x {reihe} = {i * reihe}");
}
```

**6.4**
```csharp
string passwort = "geheim";
string eingabe;
int versuche = 0;

do
{
    Console.Write("Passwort: ");
    eingabe = Console.ReadLine();
    versuche++;

    if (eingabe != passwort && versuche >= 3)
    {
        Console.WriteLine("Zugang gesperrt!");
        break;
    }
} while (eingabe != passwort);

if (eingabe == passwort)
{
    Console.WriteLine($"Willkommen! Du hast {versuche} Versuch(e) gebraucht.");
}
```

**6.5**
```csharp
for (int i = 1; i <= 100; i++)
{
    if (i % 15 == 0) { Console.WriteLine("FizzBuzz"); }
    else if (i % 3 == 0) { Console.WriteLine("Fizz"); }
    else if (i % 5 == 0) { Console.WriteLine("Buzz"); }
    else { Console.WriteLine(i); }
}
```
Typischer Fehler: `FizzBuzz` wird zuletzt geprüft – dann wird es nie erreicht.

**6.6**
```csharp
Console.Write("Höhe: ");
int hoehe = int.Parse(Console.ReadLine());

for (int zeile = 1; zeile <= hoehe; zeile++)
{
    for (int stern = 1; stern <= zeile; stern++)
    {
        Console.Write("*");
    }
    Console.WriteLine();
}
```

---

## Kapitel 7 – Arrays und Listen

**7.1** Ausgabe: `Apfel`, `Kirsche`

**7.2** `IndexOutOfRangeException`.
```csharp
int[] punkte = { 90, 75, 60, 85, 100 };
for (int i = 0; i < punkte.Length; i++)
{
    Console.WriteLine(punkte[i]);
}
```

**7.3**
```csharp
List<int> zahlen = new List<int>();

for (int i = 1; i <= 5; i++)
{
    Console.Write($"Zahl {i}: ");
    zahlen.Add(int.Parse(Console.ReadLine()));
}

int summe = 0;
int maximum = zahlen[0];

foreach (int zahl in zahlen)
{
    summe += zahl;
    if (zahl > maximum)
    {
        maximum = zahl;
    }
}

double durchschnitt = (double)summe / zahlen.Count;

Console.WriteLine($"Summe: {summe}");
Console.WriteLine($"Durchschnitt: {durchschnitt}");
Console.WriteLine($"Größte Zahl: {maximum}");
```

**7.4**
```csharp
List<string> einkaufsliste = new List<string>();
string artikel;

do
{
    Console.Write("Artikel (oder 'fertig'): ");
    artikel = Console.ReadLine();

    if (artikel == "fertig")
    {
        break;
    }

    if (einkaufsliste.Contains(artikel))
    {
        Console.WriteLine($"{artikel} steht schon auf der Liste.");
    }
    else
    {
        einkaufsliste.Add(artikel);
    }
} while (true);

einkaufsliste.Sort();
for (int i = 0; i < einkaufsliste.Count; i++)
{
    Console.WriteLine($"{i + 1}. {einkaufsliste[i]}");
}
```

---

## Kapitel 8 – Methoden

**8.1**
```csharp
for (int i = 1; i <= 10; i++)
{
    if (IstGerade(i))
    {
        Console.WriteLine($"{i} ist gerade.");
    }
    else
    {
        Console.WriteLine($"{i} ist ungerade.");
    }
}

bool IstGerade(int zahl)
{
    return zahl % 2 == 0;
}
```

**8.2**
```csharp
for (int c = -10; c <= 40; c += 10)
{
    Console.WriteLine($"{c} °C = {InFahrenheit(c)} °F");
}

double InFahrenheit(double celsius)
{
    return celsius * 9 / 5 + 32;
}
```

**8.3**
```csharp
int a = LiesZahl("Erste Zahl: ");
int b = LiesZahl("Zweite Zahl: ");
Console.WriteLine($"{a} + {b} = {a + b}");
// ...

int LiesZahl(string frage)
{
    Console.Write(frage);
    return int.Parse(Console.ReadLine());
}
```

**8.4** Beispiele: Fehler muss nur an **einer** Stelle korrigiert werden · Code wird kürzer und übersichtlicher · der Methodenname erklärt, was passiert · Methoden lassen sich einzeln testen und wiederverwenden.

---

## Kapitel 9 – Klassen und Objekte

**9.1**
```csharp
public void Geburtstag()
{
    Alter++;
    Console.WriteLine($"{Name} hat Geburtstag und ist jetzt {Alter} Jahre alt!");
}
```

**9.2 + 9.3**
```csharp
public class BankKonto
{
    public string Besitzer { get; }
    public double Kontostand { get; private set; }

    public BankKonto(string besitzer)
    {
        Besitzer = besitzer;
        Kontostand = 0;
    }

    public void Einzahlen(double betrag)
    {
        if (betrag <= 0)
        {
            Console.WriteLine("Der Betrag muss größer als 0 sein.");
            return;
        }
        Kontostand += betrag;
    }

    public bool Abheben(double betrag)
    {
        if (betrag <= 0 || betrag > Kontostand)
        {
            return false;
        }
        Kontostand -= betrag;
        return true;
    }

    public bool Ueberweisen(BankKonto ziel, double betrag)
    {
        if (!Abheben(betrag))
        {
            return false;
        }
        ziel.Einzahlen(betrag);
        return true;
    }

    public void Anzeigen()
    {
        Console.WriteLine($"{Besitzer}: {Kontostand:0.00} €");
    }
}
```

---

## Kapitel 10 – Fehler abfangen

**10.2**
```csharp
int LiesZahl(string frage)
{
    Console.Write(frage);
    int zahl;
    while (!int.TryParse(Console.ReadLine(), out zahl))
    {
        Console.Write("Das war keine gültige Zahl. Bitte nochmal: ");
    }
    return zahl;
}

double LiesKommazahl(string frage)
{
    Console.Write(frage);
    double zahl;
    while (!double.TryParse(Console.ReadLine(), out zahl))
    {
        Console.Write("Das war keine gültige Zahl. Bitte nochmal: ");
    }
    return zahl;
}
```

**10.3 (Notenrechner)**
```csharp
Console.Write("Note (1-6): ");
int note;
while (!int.TryParse(Console.ReadLine(), out note) || note < 1 || note > 6)
{
    Console.Write("Bitte eine Note von 1 bis 6 eingeben: ");
}
```

**10.4** `int` geteilt durch `0` → `DivideByZeroException`. Bei `double` gibt es keinen Absturz, das Ergebnis ist `∞` (unendlich).
Die `if`-Prüfung vor der Rechnung ist meist besser: Der Fehler wird gar nicht erst ausgelöst, und der Code ist leichter lesbar. `try`/`catch` ist für Fälle gedacht, die man nicht vorher prüfen kann.

---

## Kapitel 11 – Abschlussprojekt (Stufe 1 + 2)

```csharp
string nochmal;
do
{
    int geheimeZahl = Random.Shared.Next(1, 101);
    int versuche = 0;
    int tipp;

    Console.WriteLine("Ich denke mir eine Zahl zwischen 1 und 100 aus. Rate mal!");

    do
    {
        tipp = LiesZahl("Dein Tipp: ");

        if (tipp < 1 || tipp > 100)
        {
            Console.WriteLine("Bitte nur Zahlen von 1 bis 100!");
            continue;   // springt zur Bedingung, der Versuch wird nicht gezählt
        }

        versuche++;

        if (tipp < geheimeZahl)
        {
            Console.WriteLine("Größer!");
        }
        else if (tipp > geheimeZahl)
        {
            Console.WriteLine("Kleiner!");
        }
    } while (tipp != geheimeZahl);

    Console.WriteLine($"Richtig! Die Zahl war {geheimeZahl}. Du hast {versuche} Versuche gebraucht.");

    Console.Write("Nochmal spielen? (j/n) ");
    nochmal = Console.ReadLine();
} while (nochmal == "j");

int LiesZahl(string frage)
{
    Console.Write(frage);
    int zahl;
    while (!int.TryParse(Console.ReadLine(), out zahl))
    {
        Console.Write("Das war keine gültige Zahl. Bitte nochmal: ");
    }
    return zahl;
}
```

**Stufe 4 – „Tauschen“:** Die beste Strategie ist die **binäre Suche** – der Computer rät immer die Mitte des verbleibenden Bereichs. Damit findet er jede Zahl zwischen 1 und 100 in höchstens **7 Versuchen**.
