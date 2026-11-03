---
title: 09 - Vertiefung Klassen und Objekte
parent: Vertiefung
nav_order: 9
---
# Kapitel 9 – Vertiefung: Klassen, Objekte und Kapselung

[← Zurück zu Kapitel 9](09-Klassen.md) | [Übersicht](../README.md)

Die objektorientierte Programmierung (OOP) ist die Art und Weise, wie fast alle großen Softwaresysteme der Welt gebaut sind. Am Anfang fällt der Übergang von "einfachen Befehlen" zu "Klassen und Objekten" oft schwer. Hier machen wir die Zusammenhänge kristallklar.

---

## 1. Bauplan vs. Haus (Klasse vs. Objekt)

Der Unterschied lässt sich am besten an einer Baustelle erklären:

- **Die Klasse (`Auto`):** Ist die **technische Zeichnung** auf dem Papier. Auf dem Papier kann man nicht fahren! Sie beschreibt nur: *"Jedes Auto hat 4 Räder, eine Farbe und eine Methode namens Hupen()."*
- **Das Objekt (`meinGolf`, `nachbarBMW`):** Ist das **echte Auto aus Blech**, das nach der Zeichnung in der Fabrik gebaut wurde. Du kannst 100 Autos nach demselben Plan bauen – jedes hat seine eigene Lackierung, seinen eigenen Kilometerstand und fährt unabhängig von den anderen!

```
    KLASSE (Bauplan)                 OBJEKTE (Instanzen)
┌──────────────────────┐         ┌──────────────────────┐
│  Klasse: Auto        │  new    │  meinGolf            │
│  - Farbe             ├────────►│  Farbe: "Blau"       │
│  - PS                │         └──────────────────────┘
│  - Hupen()           │  new    ┌──────────────────────┐
└──────────────────────┘ ────────►│  nachbarBMW          │
                                 │  Farbe: "Schwarz"    │
                                 └──────────────────────┘
```

---

## 2. Was genau macht das Wort `new`?

Wenn du schreibst:
```csharp
Auto meinGolf = new Auto("Blau", 110);
```

Passieren drei Dinge in Bruchteilen einer Sekunde:
1. **Speicher reservieren:** Der Computer sucht im Speicher einen freien Platz für ein neues Auto-Objekt.
2. **Konstruktor anrufen:** Er ruft den Konstruktor auf und übergibt `"Blau"` und `110`.
3. **Adresse speichern:** Die Variable `meinGolf` merkt sich, wo dieses Auto im Speicher steht.

---

## 3. Kapselung: Warum `{ get; private set; }` Gold wert ist

Stell dir vor, du hast ein `BankKonto`:

```csharp
public class UnsicheresKonto
{
    public double Kontostand; // Jeder darf direkt reinschreiben!
}
```

Jeder Programmierer (oder ein Fehler im Code) könnte jetzt schreiben:
```csharp
UnsicheresKonto k = new UnsicheresKonto();
k.Kontostand = -500000; // Autsch!
k.Kontostand = 1000000; // Geld aus dem Nichts gezaubert!
```

### Die Lösung: Sichere Kapselung
```csharp
public class SicheresKonto
{
    // Lesen darf jeder (get), aber ÄNDERN darf nur das Konto selbst (private set)!
    public double Kontostand { get; private set; }

    public void Einzahlen(double betrag)
    {
        if (betrag > 0)
        {
            Kontostand += betrag;
        }
    }
}
```
Jetzt gibt es **Türsteher-Methoden** (`Einzahlen`, `Abheben`). Niemand kann von außen schummeln!

---

## 4. Die Referenzfalle: Zwei Namen für dasselbe Ding

Objekte verhalten sich anders als einfache Zahlen (`int`).
Schau dir das an:

```csharp
Hund a = new Hund("Bello", 3);
Hund b = a; // ACHTUNG: Hier wird KEIN neuer Hund geklont!

b.Name = "Rex";

Console.WriteLine(a.Name); // Was steht hier? Bello oder Rex?
```

**Ergebnis:** Es steht **Rex** da!
**Warum?**
`a` und `b` sind wie zwei Fernbedienungen für denselben Fernseher. Wenn du mit Fernbedienung `b` das Programm umschaltest, zeigt der Fernseher auch für Fernbedienung `a` das neue Programm.
Nur wenn das Wort **`new`** fällt, entsteht ein wirklich eigenständiges neues Objekt!

---

## 5. Vertiefungsaufgaben

### Vertiefungsaufgabe 9.1 – Das Smartphone
Erstelle eine Klasse `Smartphone`:
- **Eigenschaften:** `Modell` (string), `Akkustand` (int, startet bei 100, `private set`)
- **Konstruktor:** Setzt das Modell.
- **Methoden:**
  - `void Benutze(int minuten)`: Verbraucht 1 % Akku pro 2 Minuten. Wenn der Akku 0 erreicht, geht nichts mehr.
  - `void Laden()`: Setzt den Akku wieder auf 100 %.
  - `void Status()`: Gibt Modell und Akkustand aus.

<details>
<summary>👉 Klicke hier für die Lösung</summary>

```csharp
public class Smartphone
{
    public string Modell { get; set; }
    public int Akkustand { get; private set; }

    public Smartphone(string modell)
    {
        Modell = modell;
        Akkustand = 100;
    }

    public void Benutze(int minuten)
    {
        int verbrauch = minuten / 2;
        Akkustand -= verbrauch;
        if (Akkustand < 0) Akkustand = 0;
        
        Console.WriteLine($"{Modell} benutzt. Akku jetzt: {Akkustand}%");
        if (Akkustand == 0) Console.WriteLine("Akku ist leer! Bitte laden.");
    }

    public void Laden()
    {
        Akkustand = 100;
        Console.WriteLine($"{Modell} ist wieder voll aufgeladen (100%).");
    }

    public void Status()
    {
        Console.WriteLine($"[Smartphone] Modell: {Modell}, Akku: {Akkustand}%");
    }
}
```
</details>
