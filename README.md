# 90-Tage Challenge – Sorin Ratiu

Ein strukturierter 90-Tage-Plan zur Entwicklung deines Lebens als Coach und Unternehmer.

## Features

✅ **Tagesbasierte Aufgaben** – Strukturierte Tasks für alle 90 Tage
✅ **Phaseneinteilung** – 3 Phasen à 30 Tage mit klarem Fokus
✅ **Schrritt-für-Schritt Anleitung** – Jede Aufgabe mit detaillierten Steps
✅ **Cloud-Sync** – Daten automatisch mit Supabase synchronisieren
✅ **Offline-ready** – Lokal in localStorage gespeichert bis Supabase konfiguriert

## Schnellstart

### 1. Repository klonen oder Fork erstellen

```bash
git clone https://github.com/infoWeHerzIT/90-tage-challenge.git
cd 90-tage-challenge
```

### 2. Lokal öffnen

Öffne einfach `index.html` in deinem Browser. Die App läuft sofort ohne Setup.

**Hinweis:** Daten werden lokal im Browser gespeichert. Um Daten über mehrere Geräte zu synchronisieren, folge den Schritten zur Supabase-Integration.

---

## Supabase Cloud-Sync aktivieren

### Warum Cloud-Sync?

- 📱 **Multi-Device Sync** – Fortschritt auf allen Geräten verfügbar
- ☁️ **Datensicherheit** – Daten sind nicht an einen Browser gebunden
- 🔄 **Automatisches Backup** – Keine Daten verloren bei Browser-Cache-Löschen

### Setup-Anleitung

#### Schritt 1: Supabase Account erstellen

1. Gehe auf [supabase.com](https://supabase.com)
2. Klicke auf **"Start your project"**
3. Melde dich mit GitHub an (einfachste Methode)
4. Erstelle ein **neues Projekt**
   - Projektname: z.B. `90-tage-challenge`
   - Region: Am nächsten zu dir
   - Passwort: Sicher speichern!

#### Schritt 2: Datenbank-Tabelle erstellen

1. Gehe zu **SQL Editor** in deinem Supabase Dashboard
2. Klicke auf **"New Query"**
3. Kopiere und führe folgendes SQL aus:

```sql
-- Tabelle für Benutzerdaten erstellen
CREATE TABLE sorin_state (
  id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  user_id UUID NOT NULL UNIQUE,
  state_json JSONB NOT NULL,
  updated_at TIMESTAMP DEFAULT NOW(),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Row Level Security aktivieren
ALTER TABLE sorin_state ENABLE ROW LEVEL SECURITY;

-- Sicherheitsrichtlinien (Policies) erstellen
-- Nutzer können nur ihre eigenen Daten lesen
CREATE POLICY "Users can read own state"
  ON sorin_state FOR SELECT
  USING (auth.uid() = user_id);

-- Nutzer können ihre eigenen Daten einfügen
CREATE POLICY "Users can insert own state"
  ON sorin_state FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- Nutzer können ihre eigenen Daten aktualisieren
CREATE POLICY "Users can update own state"
  ON sorin_state FOR UPDATE
  USING (auth.uid() = user_id);
```

4. Klicke **"RUN"** oder drücke `Ctrl+Enter`

#### Schritt 3: Authentication aktivieren

1. Gehe zu **Authentication → Providers** im Supabase Dashboard
2. Stelle sicher, dass der **Anonymous** Provider aktiviert ist
3. Klicke auf den Anonymous-Provider und aktiviere ihn (falls noch nicht geschehen)

#### Schritt 4: API-Schlüssel kopieren

1. Gehe zu **Project Settings → API**
2. Kopiere:
   - **Project URL** (z.B. `https://xxxxx.supabase.co`)
   - **anon public** Key (unter "Project API keys")

#### Schritt 5: index.html aktualisieren

Öffne `index.html` und finde diesen Bereich (ca. Zeile 288):

```javascript
var SUPABASE_URL = 'https://fkxqdrbdynnswxqrpbks.supabase.co';
var SUPABASE_ANON_KEY = 'keysb_publishable_b2FGMpROzhSBPy7XuwmaEw_mfdeGggK';
```

Ersetze mit deinen Werten:

```javascript
var SUPABASE_URL = 'https://dein-projekt.supabase.co';  // Deine Project URL
var SUPABASE_ANON_KEY = 'dein-anon-key-hier';           // Dein anon public key
```

#### Schritt 6: Testen

1. Aktualisiere die Seite (`F5` oder `Cmd+R`)
2. Das Setup-Banner sollte **verschwinden**
3. Erledige eine Aufgabe und aktualisiere die Seite
4. Der Fortschritt sollte erhalten bleiben ✅

---

## Datensicherheit

### Öffentliche Schlüssel?

Die `SUPABASE_ANON_KEY` ist bewusst öffentlich im Code. Das ist sicher, weil:

- ✅ Row Level Security (RLS) ist aktiviert
- ✅ Nutzer können nur ihre eigenen Daten zugreifen
- ✅ Die Tabelle ist streng eingeschränkt

### CORS konfigurieren (optional)

Falls du die App auf einem eigenen Server hostest:

1. Gehe zu **Project Settings → API**
2. Scrolle zu **Allowed origins**
3. Füge deine Domain hinzu (z.B. `https://example.com`)

---

## Struktur der App

### Phasen

| Phase | Tage | Fokus |
|-------|------|-------|
| **Phase 1** | 1–30 | Sichtbarkeit & Fundament |
| **Phase 2** | 31–60 | Gruppen & Academy Aufbau |
| **Phase 3** | 61–90 | Skalierung & Academy Launch |

### Datenstruktur in Supabase

```json
{
  "task_1_0": true,          // Task erledigt (Tag 1, Task 0)
  "step_1_0_0": true,        // Step erledigt (Tag 1, Task 0, Step 0)
  "task_2_0": false,
  ...
}
```

---

## Entwicklung & Anpassungen

### Neue Tage hinzufügen

Bearbeite das `DAYS` Array in `index.html` (ab Zeile ~550):

```javascript
{day:3, phase:1, focus:'Neue Aufgabe',
tasks:[
  {time:'06:00-06:30', text:'Task Name', tag:'KATEGORIE',
   why:'Warum ist das wichtig?',
   steps:[
     {t:'Schritt 1', tip:'Hinweis'},
     {t:'Schritt 2', tip:'Hinweis'}
   ]}
]}
```

### Styling anpassen

Farben sind als CSS-Variablen definiert (Zeile 6-10):

```css
:root {
  --sand: #F5EDD8;
  --brown: #7B4F1E;
  --accent: #A07840;
  --accent2: #C8965A;
  ...
}
```

---

## Fehlerbehebung

### Setup-Banner wird nicht ausgeblendet?

1. ✅ Sind `SUPABASE_URL` und `SUPABASE_ANON_KEY` korrekt?
2. ✅ Browser-Console öffnen (`F12`) und auf Fehler prüfen
3. ✅ Tabelle `sorin_state` existiert in Supabase?
4. ✅ Anonymous Auth ist aktiviert?

### Daten werden nicht synchronisiert?

1. Browser-Console öffnen (`F12`)
2. Nach `"State saved to Supabase"` suchen
3. Falls nur `"State saved to localStorage"` – Check Supabase Connection

### Alle Daten löschen?

```javascript
// In Browser-Console ausführen:
localStorage.removeItem('sorin_state_json');
localStorage.removeItem('sorin_uid');
```

---

## Lizenz & Kontakt

Erstellt von **Sorin Ratiu**  
Für den **For You Academy** 90-Tage-Plan

---

## Changelog

### v1.0 (Juni 2026)
- ✅ Initiale Version mit 90 Tagen
- ✅ Supabase Integration
- ✅ Offline-Mode mit localStorage
- ✅ Responsive Design
