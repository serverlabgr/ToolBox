# ToolBox — Πώς ανεβάζεις updates (GitHub)

Repo: https://github.com/serverlabgr/ToolBox

Οι εγκατεστημένοι clients διαβάζουν αυτό το αρχείο:
https://raw.githubusercontent.com/serverlabgr/ToolBox/main/toolbox-update.json

Αν η `version` εκεί είναι μεγαλύτερη από την τοπική, εμφανίζεται το chip **Update** πάνω δεξιά.

## Τι βλέπει ο χρήστης στο Setup.exe

Οδηγός εγκατάστασης (NSIS), όχι απλό «επόμενο-τέλος»:
1. **Welcome** — εισαγωγή
2. **Άδεια χρήσης**
3. **Φάκελος εγκατάστασης** — επιλογή path
4. **Εκκίνηση με τα Windows** — checkbox
5. Εγκατάσταση + shortcuts (Desktop / Start Menu)
6. **Finish** — επιλογή να ανοίξει το ToolBox

## Κάθε νέο update (checklist)

1. Άλλαξε το version στο `package.json` (π.χ. `0.7.0` → `0.7.1`).
2. Τρέξε build:
   ```powershell
   npm run build:win
   ```
   Το installer βγαίνει στο `release\ToolBox-Setup-0.7.1.exe`.
3. Στο GitHub → **Releases** → **Create a new release**:
   - Tag: `v0.7.1`
   - Title: `ToolBox 0.7.1`
   - Upload το `ToolBox-Setup-0.7.1.exe` ως asset
   - Publish release
4. Ενημέρωσε το `toolbox-update.json` στο repo (Edit file στο GitHub ή τοπικά + push):
   ```json
   {
     "version": "0.7.1",
     "downloadUrl": "https://github.com/serverlabgr/ToolBox/releases/download/v0.7.1/ToolBox-Setup-0.7.1.exe",
     "notes": "Τι άλλαξε σε αυτή την έκδοση"
   }
   ```
5. Commit / Save. Σε λίγα λεπτά τα clients θα δουν το Update chip.

Βοηθητικό script μετά το build:

```powershell
powershell -ExecutionPolicy Bypass -File scripts/prepare-release.ps1
```

Εκτυπώνει το JSON που πρέπει να βάλεις στο GitHub.

## Σημαντικά

- Το repo πρέπει να μένει **Public**.
- Το `update-check.json` μέσα στο app δείχνει ήδη στο raw URL — μην το αλλάξεις εκτός αν μετακινήσεις το repo.
- Οι clients πατάνε Update → κατεβάζουν το Setup.exe και το τρέχουν μόνοι τους (εγκατάσταση πάνω από την παλιά).
