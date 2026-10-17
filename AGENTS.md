# AGENTS.md — Ghid Tehnic pentru AI (Proiectul Odaia)

Ghid de lucru și referință tehnică pentru asistenți AI (Claude Code, Cursor, Copilot, Codex, Windsurf, Antigravity) care dezvoltă sau modifică aplicația **Odaia**.

## Principii Cheie și Invariante

1. **Aplicație Single-File:** Întregul cod al aplicației se află în `odaia.html` (~2870 linii). Nu adăuga pași de build (webpack, vite, rollup) și nu crea dependențe `npm` decât dacă utilizatorul cere expres acest lucru.
2. **CDN ES Modules:** Three.js și `OrbitControls` se încarcă exclusiv prin `importmap` de pe jsDelivr.
3. **Fără imagini externe de textură:** Toate materialele (lemn, plăci gresie/faianță, tapet, tencuială, decking etc.) sunt generate procedural pe elemente HTML Canvas la runtime. Obiectele 3D create noi trebuie să folosească generatoarele procedurale existente sau să definească un generator nou în secțiunea de texturi.
4. **Unități de măsură:** Toate dimensiunile și coordonatele din scenă (`x`, `z`, `w`, `d`, `h`, `e`) sunt exprimate în **metri (m)**. Rotațiile sunt stocate intern în **radiani** și introduse în UI în **grade**.
5. **Persistență & Undo/Redo:** Orice modificare a stării modelului (`S`) trebuie înregistrată prin `saveState()` și `record()` pentru a menține sincronizate stocarea în `localStorage` și istoricul de Undo/Redo.

---

## Harta Fișierului `odaia.html`

| Interval Linii | Secțiune | Responsabilitate și Componente Principale |
|---|---|---|
| **L1 – L4** | Shell HTML & Fonturi | Titlu, legături Google Fonts (`Inter`, `JetBrains Mono`). |
| **L5 – L214** | Stiluri CSS (`<style>`) | Teme dark/light, layout flex/grid, bara de unelte, vizualizator split 2D/3D, tab-uri laterale, modale, joystick mobil. |
| **L216 – L288** | Structură HTML UI | `#app`, `#planWrap` (panza 2D), `#viewWrap` (viewport Three.js), controale overlay, tab-uri (catalog, proprietăți, poze, proiect), dialoguri modale, container de notificări toast. |
| **L292 – L297** | Import Map | Configurare CDN Three.js r128 și OrbitControls. |
| **L298 – L304** | Inițializare modul JS | Declarații variabile globale de scenă și stare. |
| **L305 – L351** | Utilitare & Matematică | `rad(deg)`, `deg(rad)`, `clamp()`, `lerp()`, `uid()`, `hexToRgb()`, gestionare cache texturi. |
| **L352 – L593** | Texturi procedurale Canvas | Generatoare procedurale pentru: `laminate`, `parquet`, `bigtiles`, `tiles`, `bathBand`, `beigeTiles`, `damask`, `herringbone`, `marble`, `plaster`, `brick`, `dots`, `floral`, `deck`, `wood`, `fabric`, `art`, `collage`. |
| **L594 – L645** | Catalog de mobilier (`CAT`) | Dicționar cu metadatele tuturor obiectelor: dimensiuni implicite (`w`, `d`, `h`), elevație (`e`), culoare (`c`), textură (`t`), categorie (`g`), etichetă (`n`). |
| **L646 – L863** | Stare & Modele de date | `baseProject()`, instanțiere camere `R_()`, deschideri `O_()`, obiecte `I_()`. Proiectele demo: `sampleApartment()`, `sampleEmpty()`, `sampleMine()` (apartamentul curent al utilizatorului). Sistemul de istoric `hist` (Undo/Redo) și salvare `localStorage`. |
| **L864 – L978** | Model Geometric & Pereți | Calcul muchii camere, detectare pereți comuni/exteriori, decupaje pentru goluri de uși și ferestre (`roomEdges`, `hostOf`). |
| **L979 – L1102** | Configurare Three.js | Instanțiere `Scene`, `PerspectiveCamera`, `WebGLRenderer`, lumini (soare, ambient, umbre), controale `OrbitControls`, plasare avatar manechin. |
| **L1103 – L1516** | Buildere Mobilier (`BUILD`) | Funcții generatoare de geometrie 3D pentru fiecare tip de obiect din catalog: `sofa`, `armchair`, `diningTable`, `desk`, `officeChair`, `chair`, `bed`, `wardrobe`, `pax`, `dresser`, `tvUnit`, `kitchen`, `sink`, `bathtub`, `toilet`, `washer`, `fridge`, `curtains`, `lantern`, `mirror`, `picture`, `plant`, `box` etc. Utilizează primitivele compuse `B` (Box), `Cy` (Cilindru), `P` (Plan). |
| **L1517 – L1526** | Instanțiere Obiecte 3D | Funcția `buildItemObj(it)`: creează grupul Three.js, aplică materialele și atașează metadatele `userData.itemId`. |
| **L1527 – L1661** | Arhitectură 3D (`buildArch`) | Construcție pereți cu goluri pentru uși/ferestre, plinte (`skirting`), tocuri uși, foi de ușă (lemn stejar `oak`, lemn închis `dark`, sticlă `glass`, glisante `slide`), ferestre și balustrade de terasă. |
| **L1662 – L1681** | Iluminat & Temă Zi/Noapte | `setLighting()`: comutare lumină de zi cu umbre sau mod noapte cu lămpi punctiforme calde. |
| **L1682 – L1719** | Cameră & Încadrare | Moduri cameră orbit, top-down (`top`), încadrare automată pe camera selectată, cutaway dinamic al pereților (`cut` / `low`). |
| **L1720 – L1896** | Mod Mers (First / Third Person) | Control deplasare WASD / săgeți, orientare mouse cu pointer lock, avatar manechin în TP, coliziuni elipsoidale cu pereții și piesele mari de mobilier (`itemBlocks`). |
| **L1897 – L1953** | Selecție & Editare | Scurtături tastatură: `Delete` (ștergere), `Ctrl+Z` (undo), `Ctrl+Y` (redo), `Ctrl+D` (duplicare), `R` (rotire 90°), taste săgeți (ajustare fină poziție), snapping la perete. |
| **L1954 – L2280** | Plan 2D Canvas | Randare completă plan 2D pe Canvas: scară metrică, pan/zoom, selecție elemente, drag & drop mobilier, cote dimensiuni, arcuri deschidere uși, simboluri grafice pentru obiecte (`drawPlanItem`). |
| **L2281 – L2316** | Verificări & Validare Model | Motor de validare intern: detecție obiecte suprapuse, uși sau ferestre fără perete gazdă, camere izolate fără acces. |
| **L2317 – L2493** | Panou Lateral (Inspector) | Rulare tab-uri: Catalog de obiecte pe categorii, Panou de proprietăți pentru elementul selectat (cote, rotație, culori, mostre texturi). |
| **L2494 – L2752** | Poze & Reconstrucție AI | Etapă de asistență pentru încărcare poze, calibrare puncte de fugă și orientare camere. |
| **L2753 – L2808** | Tab Proiect & Export/Import | Statistici proiect (suprafețe, piese), redenumire, descărcare fișier JSON și încărcare proiect existent. |
| **L2809 – L2846** | Chrome UI & Notificări | Butoane antet, mesaje de stare, notificări animate tip toast. |
| **L2847 – L2872** | Render Loop & Bootstrap | Bucla `requestAnimationFrame`, inițializare evenimente fereastră și încărcare model de pornire (`sampleMine()`). |

---

## Structura Datelor Proiectului (Formatul JSON)

Modelul este serializat conform structurii din `sample-proiect.json`:

```json
{
  "app": "odaia",
  "v": 1,
  "name": "Apartamentul nostru",
  "wallH": 2.6,
  "wallT": 0.15,
  "exterior": "#ECE9E3",
  "eye": 1.62,
  "rooms": [
    {
      "id": "id_1",
      "name": "Living + bucătărie",
      "x": 2.27,
      "z": 0.79,
      "w": 4.9,
      "d": 5.76,
      "floor": { "c": "#ffffff", "t": "laminate" },
      "walls": { "c": "#F3F2EE", "t": "plaster" },
      "wallSides": {},
      "open": {}
    }
  ],
  "openings": [
    {
      "id": "id_11",
      "type": "door",
      "x": 1.0,
      "z": 6.55,
      "w": 0.95,
      "h": 2.1,
      "sill": 0,
      "dir": -1,
      "hinge": 0,
      "finish": "dark"
    }
  ],
  "items": [
    {
      "id": "id_21",
      "type": "sofa",
      "x": 6.595,
      "z": 2.1,
      "rot": 4.71238898038469,
      "w": 2.1,
      "d": 1.0,
      "h": 1.0,
      "e": 0,
      "c": "#1D8C9B",
      "t": "",
      "s": 0
    }
  ]
}
```

---

## Proceduri pentru Extinderea Aplicației

### Adăugarea unui nou obiect de mobilier

1. **Înregistrare în Catalog (`CAT`, L594):**
   Adaugă cheia obiectului cu dimensiunile implicite și eticheta:
   ```javascript
   bookshelf: { n: 'Bibliotecă rafturi', g: 'Depozitare', w: 0.8, d: 0.35, h: 1.8, c: '#E3DFD5' },
   ```
2. **Definire Builder 3D (`BUILD`, L1103):**
   Implementează geometria folosind primitivele rapide `B(g, w, h, d, x, y, z, mat, radius)`:
   ```javascript
   BUILD.bookshelf = function(g, it, m) {
     const { w, d, h } = it;
     B(g, w, h, d, 0, h / 2, 0, m, 0.01);
     // Adaugă rafturi interioare dacă este cazul
   };
   ```
3. **Simbol 2D Plan (`drawPlanItem`, L2211):**
   Dacă este necesară o reprezentare grafică specifică în planul 2D, adaugă un branch în `drawPlanItem`. Implicit, se desenează dreptunghiul cu contur și etichetă.

### Adăugarea unei texturi noi procedurale

1. Adaugă generatorul în secțiunea de texturi (L352) definind o funcție care randează pe un `HTMLCanvasElement` și returnează un `THREE.CanvasTexture` cu `wrapS = wrapT = THREE.RepeatWrapping`.
2. Înregistrează finisajul în lista de mostre din Panoul de Proprietăți (L2317).

---

## Pași de Verificare după Modificări

Orice modificare trebuie validată parcurgând următorii pași:

1. Pornește serverul local:
   ```bash
   python -m http.server 8765
   ```
2. Deschide `http://localhost:8765/odaia.html` în browser.
3. **Consolă DevTools:** Verifică să nu existe erori (`TypeError`, `ReferenceError`) la încărcare sau la comutarea camerelor.
4. **Panoul „Verificări” din UI:** Verifică tab-ul de verificări (dreapta jos); acesta trebuie să arate `0 probleme`.
5. **Moduri de vizualizare:**
   - Testează comutarea între **Orbit**, **Persoana I (FP)** și **Persoana a III-a (TP)**.
   - Testează modul **Noapte** (corpurile de iluminat trebuie să radieze lumină caldă).
6. **Plan 2D & Undo/Redo:**
   - Trage o piesă de mobilier pe planul 2D; verifică actualizarea instantanee în scena 3D.
   - Apasă `Ctrl+Z` și verifică revenirea piesei în poziția inițială.
7. **Export JSON:**
   - Accesează tab-ul Proiect, exportă JSON-ul și verifică integritatea fișierului descărcat.
