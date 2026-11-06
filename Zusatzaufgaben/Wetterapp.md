---
title: "8 - Wetterapp"
parent: Zusatzaufgaben
nav_order: 8
---

# Weatherapp

*Ziel ist es, eine Wetterapp zu erstellen, die Live-Wetterdaten liefert.*

### Code:
Klicke [hier](../Code/WeatherCLIBasis.zip), um das Basisprojekt der Wetterapp herunterzuladen.

## Dokumentation:

- 📁 WeatherCLI
	- 📁 Dto
	- 📄 ConsoleExtensions.cs
	- 📄 Forecast.cs *(Hier schreibst du deinen Code hinein)*
	- 📄 OpenMetroApiClient.cs
	- 📄 Program.cs

Die Klasse `Forecast`{:.language-csharp .highlight} enthält bereits ein konfiguriertes `OpenMetroApiClient`{:.language-csharp .highlight}-Objekt.\
Dieses besitzt die Methode `openMetroApi.GetTemperatureForecast`{:.language-csharp .highlight} mit zwei Übergabeparametern:

|**Parameter**|**Erklärung**|
|`int pastDays`{:.language-csharp .highlight} | Anzahl der Tage in der Vergangenheit (1 => gestern, 2 => vorgestern)|
|`int forecastDays`{:.language-csharp .highlight} | Anzahl der Tage in die Zukunft, inklusive heute (1 => nur heute, 2 => heute und morgen) |

{% capture tipp %}
```csharp
var temperaturdaten = await openMetroApi.GetTemperatureForecast(2, 2);
```
Dieser Aufruf liefert die Temperaturdaten von vorgestern bis morgen.
{% endcapture %}
{% include tipp.html inhalt=tipp %}

### Dto:
```csharp
class TemperatureForecast
{
	//Properties
	public List<DayTemperatureForecast> DayTemperatureForecasts { get; };
	public IEnumerable<DateOnly> Days { get; };

	//Method
	DayTemperatureForecast GetForecastForDay(DateOnly day)
}
```

```csharp
class DayTemperatureForecast
{
	//Properties
	public DateOnly Day { get; }
	public List<(DateTime Time, double Temperature)> TimeTemperature { get; }
}
 
```

## Aufgaben
*Schreibe deinen Code ausschließlich in der Datei `Forecast.cs`!\
Die Methode `StartAsync()` wird beim Start des Programms ausgeführt.*

### 1. Benutzerabfrage
Frage den Benutzer nach dem Zeitraum, für den er Temperaturdaten sehen möchte. Dafür brauchst du zwei Eingaben:

- Wie viele Tage in die **Vergangenheit** sollen angezeigt werden?
- Wie viele Tage in die **Zukunft** sollen angezeigt werden?

Beide Werte dürfen höchstens 14 Tage betragen (also maximal von -14 bis +14 Tage). Ungültige Eingaben sollen abgelehnt werden, und der Benutzer wird erneut gefragt.

### 2. API-Aufruf
Rufe mit den eingegebenen Werten die Methode `openMetroApi.GetTemperatureForecast(pastDays, forecastDays)`{:.language-csharp .highlight} auf, um die Temperaturdaten des gewählten Zeitraums zu erhalten.

### 3. Zusammengefasste Anzeige
Gib nun für jeden Tag des gewählten Zeitraums die höchste und die niedrigste Temperatur aus.

Die Formatierung ist dir überlassen.

{% capture tipp %}
**Mögliche Darstellung:**
```
  06.11.2026: Min: -0,0 °C | Max: 7,6 °C
  07.11.2026: Min: 5,7 °C | Max: 9,1 °C
  08.11.2026: Min: 3,7 °C | Max: 10,4 °C
```
{% endcapture %}
{% include tipp.html inhalt=tipp %}

### 4. Detaillierte Anzeige
Der Benutzer soll nun einen Tag auswählen können, zu dem er genauere Daten sehen möchte.
Anschließend wird die Temperatur für jede Stunde dieses Tages ausgegeben.

Die Formatierung ist dir überlassen.

{% capture tipp %}
**Mögliche Darstellung:**
```
                                                   Temperatur für 05.11.2026
00:00  █████████████████████████████████████████████████████████████████████ 6.6
01:00  ████████████████████████████████████████████████████████ 5.5
02:00  █████████████████████████████████████████████████ 4.8
03:00  ███████████████████████████████████████████ 4.3
04:00  ██████████████████████████████████████████ 4
05:00  ███████████████████████████████████ 3.6
06:00  ██████████████████████████████████ 3.5
07:00  ███████████████████████████████████████ 3.9
08:00  █████████████████████████████████████████████████████ 5.2
09:00  ███████████████████████████████████████████████████████████████████████ 6.8
10:00  █████████████████████████████████████████████████████████████████████████████████ 7.7
11:00  ███████████████████████████████████████████████████████████████████████████████████████████ 8.6
12:00  █████████████████████████████████████████████████████████████████████████████████████████████████ 9.2
13:00  ████████████████████████████████████████████████████████████████████████████████████████████████████████ 9.8
14:00  ████████████████████████████████████████████████████████████████████████████████████████████████████████████ 10.2
15:00  ████████████████████████████████████████████████████████████████████████████████████████████████████████████ 10.2
16:00  ████████████████████████████████████████████████████████████████████████████████████████████████ 9.1
17:00  █████████████████████████████████████████████████████████████████████████████████ 7.7
18:00  ███████████████████████████████████████████████████████████████████████ 6.8
19:00  █████████████████████████████████████████████████████████████ 5.9
20:00  ██████████████████████████████████████████████████████ 5.3
21:00  ████████████████████████████████████████████████████ 5.1
22:00  ██████████████████████████████████████████████ 4.6
23:00  █████████████████████████████████████████ 4.1
```

```
┌───────┬─────────────┐
│ Time  │ Temperature │
├───────┼─────────────┤
│ 00:00 │ 6,6 °C      │
│ 01:00 │ 5,5 °C      │
│ 02:00 │ 4,8 °C      │
│ 03:00 │ 4,3 °C      │
│ 04:00 │ 4,0 °C      │
│ 05:00 │ 3,6 °C      │
│ 06:00 │ 3,5 °C      │
│ 07:00 │ 3,9 °C      │
│ 08:00 │ 5,2 °C      │
│ 09:00 │ 6,8 °C      │
│ 10:00 │ 7,7 °C      │
│ 11:00 │ 8,6 °C      │
│ 12:00 │ 9,2 °C      │
│ 13:00 │ 9,8 °C      │
│ 14:00 │ 10,2 °C     │
│ 15:00 │ 10,2 °C     │
│ 16:00 │ 9,1 °C      │
│ 17:00 │ 7,7 °C      │
│ 18:00 │ 6,8 °C      │
│ 19:00 │ 5,9 °C      │
│ 20:00 │ 5,3 °C      │
│ 21:00 │ 5,1 °C      │
│ 22:00 │ 4,6 °C      │
│ 23:00 │ 4,1 °C      │
└───────┴─────────────┘
```
{% endcapture %}
{% include tipp.html inhalt=tipp %}

### 5. Zurück zur Auswahl
Nachdem sich der Benutzer die Tagesdaten angesehen hat, soll er die Möglichkeit haben, einen anderen Tag aus dem gewählten Zeitraum auszuwählen und sich dessen Daten anzeigen zu lassen.


[← Bankautomat](Bankautomat.md) | [Übersicht](../README.md)
