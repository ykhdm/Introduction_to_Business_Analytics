# 📊 **One-Pager: Datenaufbereitung mit VS Code Data Wrangler**

**Yilmaz Kavurgaci** | Mai 2026 | Introduction to Business Analytics, HS der Medien Stuttgart

---

## 🎯 **Use-Case in 30 Sekunden**

Identifizierung von **Golden-Cross-Signalen** in 5 Jahren SPY-Aktiendaten mittels gleitender Durchschnitte (50-Tage & 200-Tage). Ein Golden Cross tritt auf, wenn der kurzfristige MA den langfristigen MA von unten nach oben kreuzt – klassisches technisches Analyse-Signal für Bullenmarkt-Einstiege.

**Quelle:** Maven Analytics Challenge „Turning Bullish"

---

## 🛠️ **Tools & Technologien**

| Tool | Rolle |
|---|---|
| **VS Code Data Wrangler** | Grafische Datenaufbereitung ohne Code-Schreiben |
| **Python / pandas** | Gleitende Durchschnitte, Signalberechnung |
| **Jupyter Notebook** | Reproduzierbare Dokumentation & Ausführung |
| **matplotlib** | Visualisierung von Kursverlauf + Signalen |

---

## 📈 **Die Lösung**

### Feature Engineering (pandas)
```python
df['ma_50']  = df['Close'].rolling(window=50).mean()
df['ma_200'] = df['Close'].rolling(window=200).mean()
```

### Golden-Cross-Detektion
```python
df['golden_cross'] = (
    (df['ma_50'] > df['ma_200']) & 
    (df['ma_50'].shift(1) <= df['ma_200'].shift(1))
).astype(int)
```

### Ergebnis
✅ **Alle Golden-Cross-Ereignisse identifiziert**  
✅ **Schlusskurse an Signaldaten extrahiert**  
✅ **Visualisiert mit Annotationen**

---

## 💡 **Key Insights: Data Wrangler vs. Klassisches pandas**

### **Data Wrangler (GUI-Ansatz)**
✅ *Vorteile:*
- Schnelle, intuitive Exploration ohne Code
- Automatische Code-Generierung
- Ideal für prototypisches Arbeiten

❌ *Nachteile:*
- Hardcodierte Dateipfade (nicht portierbar)
- DataFrame wird direkt mutiert (Original geht verloren)
- Keine Fehlerbehandlung oder Validierung
- Generierter Code folgt nicht Best Practices

### **Klassisches pandas (Code-basiert)**
✅ *Vorteile:*
- Volle Kontrolle & Nachvollziehbarkeit
- Original-Daten bleiben erhalten (`df.copy()`)
- Explizite Fehlerprüfung (`isnull()`, `dtypes`)
- Wiederverwendbarer, testbarer Code
- Visualisierung direkt integriert

---

## 🗂️ **Bonus: Sternschema (Data Warehouse Design)**

Das Sternschema ist ein bewährtes relationales Modell für analytische Datenbanken. Es trennt **Kennzahlen** (Fact-Tabelle) von **Kontextinformationen** (Dimensionen) – Basis für skalierbare BI-Systeme.

### **Das erweiterte SPY-Schema:**

**F_SPY_KURSENTWICKLUNG** (Fact-Tabelle)  
→ Eine Zeile pro Handelstag mit Messgrößen:
- SCHLUSSKURS, TÄGLICHE_ÄNDERUNG, 30T_DURCHSCHNITT, ÄNDERUNG_PROZENT
- Fremdschlüssel zu allen Dimensionen

**D_WERTPAPIER**  
→ Stammdaten: TICKER, ASSET_KLASSE, BOERSE  
→ Erlaubt Erweiterung auf mehrere Wertpapiere

**D_ZEIT**  
→ Kalender-Hierarchie: DATUM, MONAT, QUARTAL, JAHR  
→ Basis für zeitliche Aggregationen

**D_HANDELSTAG**  
→ Marktcharakteristiken: IST_HANDELSTAG, LAND  
→ Trennt Kalender- von Marktlogik

**D_MARKTPHASE**  
→ Klassifizierung: PHASE_NAME (Bullenmarkt / Bärenmarkt / Konsolidierung)  
→ Flexibles Tagging ohne Code-Änderung

**Vorteile:** Skalierbar | Wartbar | Performant | BI-ready

---

## 🎓 **Learnings & Best Practices**

| Kategorie | Learnings |
|---|---|
| **Portabilität** | Relative Pfade statt absoluten: `Path(__file__).parent / 'data.csv'` |
| **Clean Code** | Transformation und Filterung in separate Funktionen – Single Responsibility! |
| **Kernel-Setup** | Umgebung als Jupyter-Kernel registrieren: `python -m ipykernel install --user --name=modul_vier` |
| **Versionskontrolle** | Virtual Environments isolieren → keine Paketkonklikte zwischen Projekten |
| **Dokumentation** | 80% Markdown, 20% Code → Wiederverwendbarkeit & Wartbarkeit steigt exponentiell |

---

## 🚀 **Fazit**

**Data Wrangler ist exzellent für Exploration und Prototyping** – mit Vorsicht aber auch für Produktion, wenn man sich bewusst macht, dass nachträgliche Optimierungen nötig sind.

**Empfehlung:**
1. Datenaufbereitung mit **Data Wrangler** erkunden
2. Generierter Code als **Vorlage** verstehen
3. In produktives pandas-Code **refaktorieren** (Pfade, Fehlerhandling, Best Practices)
4. In **Versionskontrolle** (Git) gehen

---

**Dateien dieses Projektes:**
- `Transferaufgabe_yk031.ipynb` — Vollständige Dokumentation & Ausführung
- `SPY_close_price_5Y.csv` — Eingangsdaten
- `ONE_PAGER.md` — Diese Zusammenfassung