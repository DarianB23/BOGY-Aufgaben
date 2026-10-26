# Musterlösung – 🟡 Snackautomat

[← Zur Aufgabe](../Zusatzaufgaben/Snackautomat.md)

> ℹ️ **Hinweis für Betreuer\*innen:** Das ist **eine** mögliche Lösung. Lösungen der Praktikant\*innen dürfen anders aussehen. Wichtig ist, dass Geld als **Cent in `int`** gespeichert wird, das Programm nicht abstürzt und das Wechselgeld stimmt.

**Benötigte Kapitel:** 2–10 (besonders Klassen, Listen, Methoden, Fehler abfangen)

---

## Komplettlösung (alle Pflichtstufen)

Enthält: Klasse `Produkt` mit `private set`, Produktliste als Tabelle, Münzen einwerfen, Abbrechen mit Rückgabe, Wechselgeld mit `/` und `%`, ausverkaufte Produkte, geheimer Admin-Modus mit PIN.

Das Projekt besteht aus **zwei Dateien**.

> ❗ **Wichtig:** Steht in `Produkt.cs` ein `namespace` (z. B. `namespace Snackautomat;`), muss oben in `Program.cs` `using Snackautomat;` stehen. Sonst findet `Program.cs` die Klasse nicht.

### `Produkt.cs`

```csharp
public class Produkt
{
    // Von außen lesbar, aber nur über die Methoden unten änderbar
    public string Name { get; private set; }
    public int PreisInCent { get; private set; }
    public int Bestand { get; private set; }

    public Produkt(string name, int preisInCent, int bestand)
    {
        Name = name;
        PreisInCent = preisInCent;
        Bestand = bestand;
    }

    public bool IstVerfuegbar()
    {
        return Bestand > 0;
    }

    public void Verkaufen()
    {
        if (Bestand > 0)
        {
            Bestand--;
        }
    }

    public void Auffuellen(int menge)
    {
        if (menge > 0)
        {
            Bestand += menge;
        }
    }

    // Gibt false zurück, wenn der Preis ungültig ist
    public bool PreisAendern(int neuerPreisInCent)
    {
        if (neuerPreisInCent <= 0 || neuerPreisInCent % 10 != 0)
        {
            return false;
        }
        PreisInCent = neuerPreisInCent;
        return true;
    }
}
```

> 💡 **Tipp:** Der Preis muss durch 10 teilbar sein, weil die kleinste Münze 10 Cent ist. Sonst könnte der Automat kein passendes Wechselgeld geben.

### `Program.cs`

```csharp
// ===== Snackautomat – Musterlösung =====

List<Produkt> produkte = new List<Produkt>
{
    new Produkt("Schokoriegel", 120, 5),
    new Produkt("Chips", 150, 3),
    new Produkt("Wasser", 100, 8),
    new Produkt("Apfelschorle", 180, 0),   // absichtlich ausverkauft
    new Produkt("Gummibärchen", 90, 4)
};

// Erlaubte Münzen in Cent – absteigend sortiert (wichtig für das Wechselgeld!)
List<int> muenzen = new List<int> { 200, 100, 50, 20, 10 };

string adminPin = "4711";
int einnahmen = 0;
bool laeuft = true;

while (laeuft)
{
    Console.WriteLine();
    Console.WriteLine("========= SNACKAUTOMAT =========");
    ZeigeProdukte(produkte);
    Console.WriteLine("0  Beenden");
    Console.Write("Deine Wahl: ");
    string eingabe = (Console.ReadLine() ?? "").Trim();

    if (eingabe == adminPin)
    {
        AdminModus(produkte, einnahmen);
    }
    else if (eingabe == "0")
    {
        laeuft = false;
    }
    else if (int.TryParse(eingabe, out int nummer) && nummer >= 1 && nummer <= produkte.Count)
    {
        Produkt gewaehlt = produkte[nummer - 1];

        if (!gewaehlt.IstVerfuegbar())
        {
            Console.WriteLine($"{gewaehlt.Name} ist leider ausverkauft.");
        }
        else
        {
            einnahmen += Kaufen(gewaehlt, muenzen);
        }
    }
    else
    {
        Console.WriteLine("Ungültige Auswahl.");
    }
}

Console.WriteLine("Auf Wiedersehen!");


// ===== Methoden =====

// Zeigt alle Produkte als Tabelle mit festen Spaltenbreiten
static void ZeigeProdukte(List<Produkt> produkte)
{
    for (int i = 0; i < produkte.Count; i++)
    {
        Produkt p = produkte[i];
        string status;
        if (p.IstVerfuegbar())
        {
            status = $"{p.Bestand} Stück";
        }
        else
        {
            status = "AUSVERKAUFT";
        }

        // {x,-14} = linksbündig, 14 Zeichen breit; {x,8} = rechtsbündig, 8 Zeichen breit
        Console.WriteLine($"{i + 1}  {p.Name,-14}{Euro(p.PreisInCent),8}   {status}");
    }
}

// Münzen einwerfen, verkaufen, Wechselgeld geben.
// Gibt die Einnahme in Cent zurück (0 bei Abbruch).
static int Kaufen(Produkt produkt, List<int> muenzen)
{
    int eingeworfen = 0;

    while (eingeworfen < produkt.PreisInCent)
    {
        int offen = produkt.PreisInCent - eingeworfen;
        Console.WriteLine($"Noch zu zahlen: {Euro(offen)}");
        int muenze = LiesZahl("Münze in Cent (10, 20, 50, 100, 200) oder 0 = Abbrechen: ");

        if (muenze == 0)
        {
            Console.WriteLine("Kauf abgebrochen.");
            GibWechselgeld(eingeworfen, muenzen);
            return 0;
        }

        if (muenzen.Contains(muenze))
        {
            eingeworfen += muenze;
        }
        else
        {
            Console.WriteLine("Diese Münze nimmt der Automat nicht an.");
        }
    }

    produkt.Verkaufen();
    Console.WriteLine($"Bitte entnimm dein Produkt: {produkt.Name}");
    GibWechselgeld(eingeworfen - produkt.PreisInCent, muenzen);
    return produkt.PreisInCent;
}

// Zerlegt den Betrag in möglichst wenige Münzen
static void GibWechselgeld(int betrag, List<int> muenzen)
{
    if (betrag <= 0)
    {
        return;
    }

    Console.WriteLine($"Rückgeld: {Euro(betrag)}");
    foreach (int muenze in muenzen)
    {
        int anzahl = betrag / muenze;   // Wie oft passt die Münze rein?
        betrag = betrag % muenze;       // Was bleibt übrig?

        if (anzahl > 0)
        {
            Console.WriteLine($"  {anzahl} x {Euro(muenze)}");
        }
    }
}

// Geheimer Bereich für das Personal
static void AdminModus(List<Produkt> produkte, int einnahmen)
{
    bool admin = true;
    while (admin)
    {
        Console.WriteLine();
        Console.WriteLine("========= ADMIN-MODUS =========");
        Console.WriteLine($"Einnahmen bisher: {Euro(einnahmen)}");
        ZeigeProdukte(produkte);
        Console.WriteLine("1 = Auffüllen, 2 = Preis ändern, 0 = Zurück");
        int wahl = LiesZahl("Auswahl: ");

        switch (wahl)
        {
            case 1:
            {
                Produkt p = WaehleProdukt(produkte);
                int menge = LiesZahl("Wie viele Stück? ");
                p.Auffuellen(menge);
                Console.WriteLine($"{p.Name}: jetzt {p.Bestand} Stück.");
                break;
            }
            case 2:
            {
                Produkt p = WaehleProdukt(produkte);
                int preis = LiesZahl("Neuer Preis in Cent (durch 10 teilbar): ");
                if (p.PreisAendern(preis))
                {
                    Console.WriteLine($"{p.Name} kostet jetzt {Euro(p.PreisInCent)}.");
                }
                else
                {
                    Console.WriteLine("Ungültiger Preis.");
                }
                break;
            }
            case 0:
                admin = false;
                break;
            default:
                Console.WriteLine("Ungültige Auswahl.");
                break;
        }
    }
}

static Produkt WaehleProdukt(List<Produkt> produkte)
{
    while (true)
    {
        int nummer = LiesZahl($"Produktnummer (1-{produkte.Count}): ");
        if (nummer >= 1 && nummer <= produkte.Count)
        {
            return produkte[nummer - 1];
        }
        Console.WriteLine("Diese Nummer gibt es nicht.");
    }
}

// 120 -> "1,20 €"
static string Euro(int cent)
{
    return $"{cent / 100},{cent % 100:D2} €";
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
```

### Beispielausgabe

```
========= SNACKAUTOMAT =========
1  Schokoriegel     1,20 €   5 Stück
2  Chips            1,50 €   3 Stück
3  Wasser           1,00 €   8 Stück
4  Apfelschorle     1,80 €   AUSVERKAUFT
5  Gummibärchen     0,90 €   4 Stück
0  Beenden
Deine Wahl: 2
Noch zu zahlen: 1,50 €
Münze in Cent (10, 20, 50, 100, 200) oder 0 = Abbrechen: 200
Bitte entnimm dein Produkt: Chips
Rückgeld: 0,50 €
  1 x 0,50 €
```

---

## ⭐ Lösungen zu den Extras

### Extra 1: Neues Produkt im Admin-Modus anlegen

Im `switch` von `AdminModus` einen neuen Fall ergänzen und das Menü um `3 = Neues Produkt` erweitern:

```csharp
case 3:
{
    Console.Write("Name: ");
    string name = (Console.ReadLine() ?? "").Trim();
    int preis = LiesZahl("Preis in Cent (durch 10 teilbar): ");
    int bestand = LiesZahl("Anfangsbestand: ");

    if (name != "" && preis > 0 && preis % 10 == 0 && bestand >= 0)
    {
        produkte.Add(new Produkt(name, preis, bestand));
        Console.WriteLine($"{name} wurde hinzugefügt.");
    }
    else
    {
        Console.WriteLine("Ungültige Angaben.");
    }
    break;
}
```

### Extra 2: Verkaufsstatistik

In der Klasse `Produkt` eine weitere Eigenschaft ergänzen und in `Verkaufen()` hochzählen:

```csharp
public int Verkauft { get; private set; }

public void Verkaufen()
{
    if (Bestand > 0)
    {
        Bestand--;
        Verkauft++;
    }
}
```

Im Admin-Modus anzeigen:

```csharp
foreach (Produkt p in produkte)
{
    Console.WriteLine($"{p.Name,-14} {p.Verkauft,3}x verkauft   {Euro(p.Verkauft * p.PreisInCent),9}");
}
```

> 💡 **Tipp:** Wurde der Preis zwischendurch geändert, stimmt `Verkauft * PreisInCent` nicht mehr genau. Wer das bemerkt, hat gut mitgedacht. Lösung: zusätzlich `UmsatzInCent` in `Verkaufen()` mitzählen.

---

## 🔍 Worauf ihr achten solltet

| Häufiger Fehler | Folge | Lösung |
|---|---|---|
| Preise als `double` (`1.2`) | Rundungsfehler, z. B. `0,30 - 0,10 - 0,10 - 0,10` ist nicht genau `0` | Alles in Cent als `int` speichern |
| Münzliste nicht absteigend sortiert | Wechselgeld in vielen kleinen Münzen (z. B. 5 × 10 Cent statt 50 Cent) | Größte Münze zuerst |
| `Bestand` hat ein öffentliches `set` | `p.Bestand = -5;` ist von außen möglich | `private set` und Änderungen nur über Methoden |
| `produkte[nummer]` statt `produkte[nummer - 1]` | Falsches Produkt bzw. Absturz bei der letzten Nummer | Anzeige beginnt bei 1, Index bei 0 |
| Beim Abbrechen kein Geld zurück | Eingeworfenes Geld „verschwindet“ | `GibWechselgeld(eingeworfen, ...)` beim Abbruch |
| `{cent % 100}` ohne `:D2` | `1,5 €` statt `1,50 €` und `1,0 €` statt `1,00 €` | Format `:D2` (immer zwei Stellen) |

### Gute Testfälle

- [ ] Passend bezahlen (z. B. Wasser mit 100) → kein Rückgeld
- [ ] Überzahlen (Gummibärchen 90 Cent mit 200) → `1 x 1,00 €` und `1 x 0,10 €`
- [ ] Ungültige Münze (`5`, `30`) und Text (`abc`) → Hinweis, kein Absturz
- [ ] Nach dem Einwerfen abbrechen → genau das eingeworfene Geld kommt zurück
- [ ] Ausverkauftes Produkt wählen → Hinweis, kein Münzeinwurf
- [ ] Produkt leer kaufen → danach `AUSVERKAUFT`
- [ ] PIN `4711` → Admin-Modus, Einnahmen stimmen
- [ ] Preis `125` im Admin-Modus → wird abgelehnt
