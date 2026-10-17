# Plan

## Original request (verbatim)

```text
muta continutul asta de aici, in folderul nostru C:/Users/BSeceleanu/OneDrive - Luxoft/Documents/Odaia

Aici e prompt ul raspuns de alt AI, cel care a creat asta:
Proiectul e aproape portabil deja. Tot Odaia este un singur fișier HTML (`odaia.html`, ~230 KB), fără npm și fără pas de build. Three.js se încarcă de pe CDN. Ca să-l poată modifica oricine sau orice AI, ai nevoie de un folder cu sursa, de o notă de arhitectură și de un mod de rulare.

**1. Folderul proiectului** (în `Documents\Odaia`, care există deja):

- `odaia.html`: sursa completă, aceeași cu artifactul.
- `README.md`: cum se rulează și cum se publică.
- `AGENTS.md`: ghidul pentru AI, cu harta fișierului (secțiunile CSS, texturi, catalog, geometrie, builderele de obiecte, plan 2D, verificări), convențiile și pașii de verificare.
- `sample-proiect.json`: exportul apartamentului curent, ca un AI să vadă formatul datelor.
- Opțional, `git init`, ca să vezi ce a schimbat fiecare AI.
```

## Stage tracker

- [x] Localizare surse, artifacte și istoric de generare din spațiul Claude Desktop (`scratch-2026-09-24-fe3f34`)
- [x] Generare și validare `sample-proiect.json` din funcția `sampleMine()` (10 camere, 89 obiecte, 10 deschideri)
- [x] Migrare imagini de referință (13 fișiere foto + hartă lidar) în `reference-photos/`
- [x] Creare `README.md` complet cu arhitectură, comenzi de rulare și ghid de publicare
- [x] Creare `AGENTS.md` detaliat pentru ghidarea oricărui model AI (hartă pe secțiuni/linii, convenții, proceduri de verificare)
- [x] Inițializare `git init` și commit inițial în `C:/Users/BSeceleanu/OneDrive - Luxoft/Documents/Odaia`
- [x] Sincronizare / copiere proiect în workspace-ul curent `c:\Users\BSeceleanu\.github_repos\room_designer`

## Verification

- Fișierele `odaia.html`, `odaia-local.html`, `sample-proiect.json`, `README.md`, `AGENTS.md`, `reference-photos/` există în directorul țintă.
- `sample-proiect.json` este JSON valid și poate fi parsat fără erori.
- Git repository este inițializat și conține istoricul curat.
