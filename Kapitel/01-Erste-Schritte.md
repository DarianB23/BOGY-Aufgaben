---
title: 01 - Erste Schritte
parent: Kapitel
nav_order: 1
---
# Kapitel 1 – Erste Schritte

[Übersicht](../README.md) | [Weiter: Variablen →](02-Variablen.md)

## Ziel

Nach diesem Kapitel kannst du:

- ein neues Konsolenprojekt in Visual Studio anlegen und starten
- Text im Terminal ausgeben
- Kommentare schreiben
- eine Fehlermeldung lesen und verstehen

---

## Erklärung

### Projekt erstellen

##### Visual Studio öffnen → **Neues Projekt erstellen**
   
![image](Bilder/NewProjectButton.png)
##### Die Vorlage **Konsolen-App** (Sprache: C#) auswählen → **Weiter**

![image](Bilder/ConsoleAppSelection.png)
##### Einen Projektnamen eingeben, z. B. `ErsteSchritte` → **Weiter**

![image](Bilder/ProjectName.png)

{% capture notiz %}
Lege für jedes Kapitel ein **eigenes Projekt** an (z. B. `Kapitel02`, `Kapitel03` …). So bleibt dein alter Code erhalten und du kannst später nachschauen.
{% endcapture %}
{%include notiz.html typ="warning" titel="Bitte beachten" offen=true inhalt=notiz %}

Visual Studio öffnet die Datei `Program.cs` mit diesem Code:

```csharp
Console.WriteLine("Hello, World!");
```

Gestartet wird das Programm mit **F5** oder dem grünen Pfeil ▶. Es öffnet sich ein schwarzes Fenster – das **Terminal** (auch **Konsole** genannt).

### Was bedeutet diese Zeile?

| Teil | Bedeutung |
|---|---|
| `Console` | Eine eingebaute **Klasse**, mit der du mit dem Terminal arbeitest |
| `.` | „Davon möchte ich …“ – damit greifst du auf etwas *in* `Console` zu |
| `WriteLine` | Schreibt Text ins Terminal und springt danach in die **nächste Zeile** |
| `("Hello, World!")` | Der Text, der ausgegeben wird. Text steht immer in **Anführungszeichen** `" "` |
| `;` | Jede Anweisung endet mit einem **Semikolon** |

### Kommentare

Kommentare werden vom Programm **ignoriert**. Du nutzt sie, um deinen Code zu erklären – für andere und für dich selbst.

```csharp
// Alles, was in einer Zeile nach "//" steht, ist ein Kommentar.

/* Mit Schrägstrich und Stern
   kannst du Kommentare über
   mehrere Zeilen schreiben. */

Console.WriteLine("Hallo!"); // Ein Kommentar darf auch hinter Code stehen.
```

---
{% capture notiz %}
| Fehler | Was passiert |
|---|---|
| Semikolon `;` vergessen | Fehler **CS1002: ; erwartet** |
| `console.writeline(...)` statt `Console.WriteLine(...)` | C# unterscheidet **Groß- und Kleinschreibung**. Das Programm kennt `console` nicht. |
| Anführungszeichen vergessen | Das Programm hält deinen Text für Code und versteht ihn nicht. |
{% endcapture %}
{%include notiz.html typ="warning" titel="Typische Fehler" offen=false inhalt=notiz %}

---

## Aufgaben

### Aufgabe 1.1 – Stell dich vor

Schreib ein Programm, das zuerst deinen **Namen** und in der nächsten Zeile dein **Lieblingshobby** ausgibt.

Beispielausgabe:
```
Ich heiße Lea.
Mein Lieblingshobby ist Klettern.
```

### Aufgabe 1.2 – `Write` oder `WriteLine`?

Ersetze in deinem Programm `Console.WriteLine` durch `Console.Write` und starte es erneut.
Erkläre in einem Satz: **Was ist der Unterschied?**

{% capture tipp %}
Achte darauf, **wo** der zweite Text erscheint. Was passiert mit dem Zeilenumbruch?
{% endcapture %}
{% include tipp.html inhalt =tipp%}

### Aufgabe 1.3 – Fehler mit Absicht

Fehlermeldungen zu lesen ist eine der wichtigsten Fähigkeiten beim Programmieren. Wir üben das jetzt:

1. Lösche das Semikolon am Ende einer Zeile und versuche das Programm zu starten.
   - Was steht in der **Fehlerliste** (unten in Visual Studio)?
   - In welcher **Zeile** ist der Fehler?
2. Mach das Semikolon wieder hin und schreibe stattdessen `console` klein. Welche Meldung bekommst du jetzt?

### Bonus-Aufgabe 1.4 – Konsolen-Kunst

Zeichne mit mehreren `Console.WriteLine` ein Bild aus Zeichen, zum Beispiel ein Haus oder ein Tier:

```
   /\
  /  \
 /____\
 |    |
 |_[]_|
```


{% capture notiz %}
Der Backslash `\` hat in C#-Texten eine Sonderbedeutung. Um einen echten Backslash auszugeben, musst du ihn **doppelt** schreiben: `"\\"`.
{% endcapture %}
{%include notiz.html typ="important" titel="Wichtig" offen=true inhalt=notiz %}

---

## Checkliste

- [ ] Ich kann ein neues Konsolenprojekt anlegen und starten.
- [ ] Ich weiß, was `Console.WriteLine` macht und wie es sich von `Console.Write` unterscheidet.
- [ ] Ich weiß, dass jede Anweisung mit `;` endet.
- [ ] Ich kann Kommentare schreiben.
- [ ] Ich finde in der Fehlerliste heraus, in welcher Zeile ein Fehler ist.

[Übersicht](../README.md) | [Weiter: Variablen →](02-Variablen.md)
