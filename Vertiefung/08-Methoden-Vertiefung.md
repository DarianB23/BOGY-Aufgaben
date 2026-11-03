---
title: 08 - Vertiefung: Methoden
parent: Kapitel
nav_order: 108
---
# Kapitel 8 – Vertiefung: Methoden, Parameter und Scope

[← Zurück zu Kapitel 8](08-Methoden.md) | [Übersicht](../README.md)

Methoden sind der wichtigste Schritt vom "Spaghetti-Code" zu echter, professioneller Software. Warum gibt es sie? Was bedeutet `void` wirklich? Und warum kann eine Methode eine Variable aus einer anderen Methode nicht "sehen"?

---

## 1. Die Kaffeemaschinen-Analogie

Stell dir eine Kaffeemaschine vor:

```
[ Kaffeebohnen & Wasser ] ───► ( Kaffeemaschine ) ───► [ Frischer Kaffee ]
      (Parameter)                   (Methode)              (Rückgabewert)
```

1. **Parameter (Eingabe):** Was die Methode braucht, um zu arbeiten.
2. **Methodenkörper `{ }` (Verarbeitung):** Die Magie im Inneren. Von außen sieht man nur das Ergebnis.
3. **Rückgabewert / `return` (Ausgabe):** Das fertige Produkt, das an den Aufrufer zurückgereicht wird.

### Was bedeutet `void`?
Nicht jede Aktion liefert ein Objekt zurück.
- Wenn du eine Waschmaschine anstellst, spuckt sie dir am Ende kein neues Getränk aus, sondern sie hat einfach eine Arbeit verrichtet.
- In C# bedeutet `void`: *"Diese Methode tut etwas (z. B. Text auf den Bildschirm schreiben oder einen Ton abspielen), gibt mir aber danach keinen Wert zurück, mit dem ich weiterrechnen könnte."*

---

## 2. Der Gültigkeitsbereich (Scope): Warum Variablen nicht überall leben

Schau dir diesen Fehler an:

```csharp
void BerechnePreis()
{
    double rabatt = 0.2;
    double endpreis = 100 * (1 - rabatt);
}

// Im Hauptprogramm:
Console.WriteLine(endpreis); // FEHLER: Der Name 'endpreis' ist im aktuellen Kontext nicht vorhanden!
```

**Die Lebensregel für Variablen:**
Jede Variable, die du innerhalb von geschweiften Klammern `{ ... }` anlegst, **stirbt**, sobald die schließende Klammer `}` erreicht wird!
Sie existiert ausschließlich innerhalb dieses Blocks.

**Die Lösung:**
Wenn das Hauptprogramm das Ergebnis braucht, **gib es mit `return` zurück**:

```csharp
double BerechnePreis()
{
    double rabatt = 0.2;
    double endpreis = 100 * (1 - rabatt);
    return endpreis; // Hier wird der Wert nach draußen gereicht!
}

double gesamt = BerechnePreis();
Console.WriteLine(gesamt);
```

---

## 3. Parameter werden als Kopie übergeben (Call by Value)

Was gibt dieses Programm aus?

```csharp
int meinguthaben = 50;
Verdopple(meinguthaben);
Console.WriteLine(meinguthaben); // Ist es 50 oder 100?

void Verdopple(int betrag)
{
    betrag = betrag * 2;
}
```

**Antwort:** Es wird **50** ausgegeben!
**Warum?**
Wenn du `meinguthaben` an `Verdopple` übergibst, wird der Wert `50` abfotografiert und als **Kopie** in die neue Variable `betrag` gelegt. Die Methode verdoppelt nur ihre eigene Kopie! Das Original `meinguthaben` bleibt völlig unberührt.

Möchtest du das Original ändern, musst du das Ergebnis zurückgeben:
```csharp
meinguthaben = Verdopple(meinguthaben);

int Verdopple(int betrag)
{
    return betrag * 2;
}
```

---

## 4. Guard Clauses: Frühes `return` für sauberen Code

Ein echter Profi-Tipp: Du kannst `return` nutzen, um eine Methode sofort abzubrechen, wenn etwas nicht stimmt.

**Statt unübersichtlicher Verschachtelung:**
```csharp
void ZahleAus(int betrag)
{
    if (betrag > 0)
    {
        if (betrag <= kontostand)
        {
            kontostand -= betrag;
            Console.WriteLine("Ausgezahlt.");
        }
    }
}
```

**Viel eleganter mit Guard Clauses:**
```csharp
void ZahleAus(int betrag)
{
    if (betrag <= 0) return;             // Ungültig? Sofort raus!
    if (betrag > kontostand) return;      // Zu wenig Geld? Sofort raus!

    kontostand -= betrag;
    Console.WriteLine("Ausgezahlt.");
}
```

---

## 5. Vertiefungsaufgaben

### Vertiefungsaufgabe 8.1 – Der Text-Rahmen
Schreibe eine Methode `void ZeichneRahmen(string text)`. Sie soll einen beliebigen Text mit passenden Sternchen umrahmen:

```
*********
* Hallo *
*********
```

{% capture tipp %}
Mit `text.Length` erfährst du, wie viele Zeichen der Text hat.
Mit `new string('*', anzahl)` kannst du ganz leicht eine Reihe Sternchen erzeugen!
{% endcapture %}
{% include tipp.html inhalt=tipp %}

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
void ZeichneRahmen(string text)
{
    string linien = new string('*', text.Length + 4);
    Console.WriteLine(linien);
    Console.WriteLine($"* {text} *");
    Console.WriteLine(linien);
}

// Aufruf:
ZeichneRahmen("Hallo");
ZeichneRahmen("Willkommen zu C#!");
```
</details>

### Vertiefungsaufgabe 8.2 – Der Mini-Rechner als Methoden-Sammlung
Schreibe vier Methoden:
- `double Addiere(double a, double b)`
- `double Subtrahiere(double a, double b)`
- `double Multipliziere(double a, double b)`
- `double Dividiere(double a, double b)` (Achte darauf: Was machst du, wenn `b == 0` ist?)
