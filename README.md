# Odaia — 3D & 2D Interior Room Designer

Odaia este o aplicație web autonomă pentru planificarea și amenajarea 2D/3D a locuinței (camere individuale sau apartament complet). Permite aranjarea mobilierului, configurarea finisajelor de pardoseală și pereți, navigarea la persoana I și a III-a cu coliziuni, comutarea zi/noapte și export/import complet în format JSON.

## Arhitectură și Principii

- **Zero Build Step:** Proiectul este o aplicație single-file HTML (`odaia.html`). Nu necesită `npm`, `node_modules`, `webpack` sau `vite`.
- **Three.js din CDN:** Motorul 3D (Three.js r128 și `OrbitControls`) este încărcat prin `importmap` nativ ES modules de pe jsDelivr.
- **Texturi procedurale:** Toate finisajele (parchet laminat, gresie, faianță baie cu brâu, mozaic, marmură, tencuială, cărămidă, tapet, decking) sunt generate procedural la runtime prin Canvas 2D. Nu există dependențe de imagini externe.
- **Persistență:** Proiectul se salvează automat în `localStorage` (`KEY = 'odaia_project'`) la fiecare modificare și poate fi exportat/importat ca fișier JSON.

## Structura Folderului

- `odaia.html`: Sursa principală completă a aplicației (compatibilă direct cu mediul de artifacte Claude).
- `odaia-local.html`: Versiunea ambalată cu `<!doctype html><html>...` pentru servire optimizată pe browser local.
- `sample-proiect.json`: Exportul JSON al apartamentului reconstruit („Apartamentul nostru”: 10 camere, 89 obiecte de mobilier, uși glisante/stejar/metalice, plinte).
- `AGENTS.md`: Ghidul tehnic detaliat pentru modele AI (harta completă a fișierului pe linii, convenții și pași de testare).
- `reference-photos/`: Cele 13 imagini de referință (fotografii din încăperi și harta Lidar a aspiratorului) utilizate la calibrarea modelului.
- `.agent/plan.md`: Jurnalul de implementare și planul de sarcini conform fluxului de lucru al asistenților.

## Rulare Locală

Nu deschide fișierul prin dublu-click direct din File Explorer (protocoalele `file://` pot bloca unele module ES). Pornește un server HTTP local:

```bash
cd "C:/Users/BS/OneDrive - X/Documents/Odaia"
python -m http.server 8765
```

Apoi deschide în browser:

- `http://localhost:8765/odaia.html` (sau `http://localhost:8765/odaia-local.html`)

## Funcționalități Cheie

### 1. Plan 2D Interactiv

- Unelte dedicate: **Selecție** (`V`), **Cameră** (`C`), **Ușă** (`U`), **Fereastră** (`F`).
- Drag & Drop intuitiv pentru piese de mobilier, cu snapping la pereți și unghiuri.
- Afișare cote pereți în metri, suprafețe încăperi (mp) și simboluri tehnice de deschidere uși.
- Istoric complet de acțiuni: Undo (`Ctrl+Z`), Redo (`Ctrl+Y`), Rotire 90° (`R`), Duplicare (`Ctrl+D`), Ștergere (`Delete` / `Backspace`).

### 2. Vizualizare și Navigare 3D

- **Orbit:** Rotire liberă în jurul modelului, zoom fin și centrare automată pe încăperea selectată.
- **Tăiere pereți:** Vizualizare pereți compleți, pereți tăiați dinamic în funcție de unghiul camerei (`cut`) sau pereți reduși la nivel scăzut (`low`).
- **First-Person (FP):** Navigare la persoana I prin încăperi cu tastele `WASD` / săgeți și orientare cu mouse-ul (crosshair activ). Include sistem de coliziuni cu pereții și piesele masive de mobilier.
- **Third-Person (TP):** Manechin avatar 3D cu orientare dinamică și joystick tactil pe ecrane touch.
- **Iluminat Zi / Noapte:** Comutare instantă între lumină solară naturală cu umbre și ambianță nocturnă cu corpuri de iluminat active (lămpi, aplice, pendule).

### 3. Panou Lateral și Catalog

- **Catalog:** Piese de mobilier împărțite pe categorii (living, bucătărie, dormitor, baie, depozitare, birou, terasă, iluminat, decor).
- **Proprietăți:** Editare cote (lățime, adâncime, înălțime, elevație de la sol), orientare, culoare și textură material pentru obiectul selectat.
- **Verificări:** Motor intern de validare a modelului (detectare suprapuneri mobilier, uși fără gol în perete, elemente în afara camerelor).
- **Proiect:** Redenumire proiect, statistici generale (număr camere, mp totali, număr obiecte), descărcare/încărcare JSON.

## Cum Lucrează un AI pe Acest Proiect

Orice asistent AI (Antigravity, Claude Code, Cursor, Copilot, Codex, Windsurf) trebuie să consulte fișierul `AGENTS.md` înainte de a face modificări.
Aplicația are ~2870 de linii de cod organizate modular. Pentru modificări punctuale (ex: adăugare obiect nou în catalog sau ajustare coliziuni), AI-ul poate edita direct secțiunea indicată în harta fișierului din `AGENTS.md`.
