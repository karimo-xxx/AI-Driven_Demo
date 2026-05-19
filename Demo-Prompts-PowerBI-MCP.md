# Power BI Modeling MCP — Demo Prompts

> Sammlung von Prompts für eine Live-Demo des Power BI Modeling MCP Servers.
> Jeder Prompt kann direkt in GitHub Copilot Chat ausgeführt werden.

---

## 1. Modell erkunden

### Alle Tabellen anzeigen

```
Zeige mir alle Tabellen im Semantic Model mit ihrer Anzahl an Spalten und Measures.
```

### Beziehungen analysieren

```
Liste alle Relationships im Modell auf und prüfe, ob das Star-Schema korrekt eingehalten wird.
```

### Einzelne Tabelle inspizieren

```
Zeige mir alle Spalten und Measures der Tabelle "Sales" mit ihren Datentypen und Formeln.
```

---

## 2. Measures erstellen

### Einfaches Measure

```
Erstelle ein Measure "Total Sales Amount" in der Tabelle Sales, das die Summe von [Sales Amount] berechnet. Formatierung: "#,0.00 €"
```

### Time Intelligence Measure

```
Erstelle ein Measure "YTD Sales" in der Tabelle Sales, das den Year-to-Date Umsatz basierend auf der Spalte Sales[Sales Amount] und der Date-Tabelle berechnet. Formatierung: "#,0.00 €"
```

### Vorjahresvergleich

```
Erstelle zwei Measures in der Tabelle Sales, basierend auf der Spalte Sales[Sales Amount] und der Date-Tabelle:
1. "PY Sales" — Umsatz des Vorjahres, Format "#,0.00 €"
2. "Sales YoY %" — Prozentuale Veränderung zum Vorjahr, Format "0.00%"
```

### Komplexes Measure mit VAR/RETURN

```
Erstelle ein Measure "Sales Above Average" in der Tabelle Sales, das die Anzahl der Produkte zählt, deren Umsatz (Sales[Sales Amount]) über dem Durchschnitt aller Produkte liegt. Nutze VAR/RETURN.
```

---

## 3. DAX Queries ausführen

### Ad-hoc Analyse

```
Führe eine DAX-Query aus, die den Gesamtumsatz (Sales[Sales Amount]) pro Fiscal Year anzeigt.
```

### Top N Analyse

```
Schreibe und führe eine DAX-Query aus: Top 10 Kunden nach Sales Amount mit Kundenname und Betrag.
```

### Measure validieren

```
Validiere folgende DAX-Formel:
CALCULATE(SUM(Sales[Sales Amount]), DATESYTD(Date[Date]))
```

---

## 4. Beziehungen & Modellierung

### Neue Beziehung erstellen

```
Erstelle eine Beziehung zwischen Sales[CurrencyKey] und Currency[CurrencyKey] — Many-to-One, Single Direction.
```

### Beziehungen auditieren

```
Prüfe alle Beziehungen im Modell auf bidirektionale Cross-Filter und liste diese auf. Gibt es welche, die nicht notwendig sind?
```

---

## 5. Calculation Groups

### Calculation Group erstellen

```
Erstelle eine Calculation Group "Time Calculations" mit folgenden Items:
- "Current" (keine Modifikation)
- "YTD" (Year-to-Date)
- "PY" (Previous Year)
- "YoY %" (Year-over-Year prozentual)
```

---

## 6. Spalten & Formatierung

### Spalte ausblenden

```
Blende alle Key-Spalten in der Tabelle Sales aus (isHidden = true), die nur für Beziehungen verwendet werden.
```

### Berechnete Spalte erstellen

```
Erstelle eine berechnete Spalte "Customer Location" in der Tabelle Customer, die City und Country-Region kombiniert (z.B. "Berlin, Germany").
```

---

## 7. Übersetzungen & Perspektiven

### Übersetzung hinzufügen

```
Füge deutsche Übersetzungen (de-DE) für alle Tabellennamen im Modell hinzu:
- Sales → Verkäufe
- Customer → Kunden
- Product → Produkte
- Date → Datum
```

### Perspektive erstellen

```
Erstelle eine Perspektive "Sales Analysis", die nur die Tabellen Sales, Customer, Date und Product enthält.
```

---

## 8. Audit & Best Practices

### Modell-Audit

```
Führe ein Audit des Semantic Models durch:
- Gibt es Measures ohne Format-String?
- Gibt es Spalten ohne Description?
- Werden DIVIDE-Best-Practices eingehalten?
```

### Performance-Check

```
Prüfe alle Measures auf potenzielle Performance-Probleme. Gibt es Measures, die CALCULATE ohne expliziten Filter verwenden oder verschachtelte Iteratoren nutzen?
```

---

## 9. Security Roles

### RLS-Rolle erstellen

```
Erstelle eine Security Role "Regional Sales Manager", die den Zugriff auf Sales Territory[Region] = "Europe" einschränkt.
```

---

## 10. End-to-End Szenario

### Kompletter Workflow

```
Ich möchte eine Profitabilitäts-Analyse aufbauen. Erstelle folgende Measures in der Tabelle Sales:
1. "Gross Profit Margin YTD" — Year-to-Date Bruttomarge basierend auf Sales[Sales Amount] und Sales[Total Product Cost], Format "0.00%"
2. "Cost Ratio %" — Anteil der Produktkosten am Umsatz, Format "0.00%"
Führe danach eine DAX-Query aus, die die Bruttomarge pro Fiscal Year zeigt.
```

---

## Tipps für die Demo

| Tipp | Beschreibung |
|------|-------------|
| **Schrittweise** | Beginne mit einfachen Abfragen (Tabellen anzeigen), dann komplexere Szenarien |
| **Live-Feedback** | Zeige, wie Copilot die MCP-Tools aufruft und Ergebnisse zurückgibt |
| **Fehler provozieren** | Gib absichtlich eine falsche DAX-Formel ein und zeige die Validierung |
| **Iterativ arbeiten** | Erstelle ein Measure, validiere es, passe es an — alles im Chat |
| **Kontext nutzen** | Zeige, dass Copilot das Modell kennt und kontextbezogen antwortet |
