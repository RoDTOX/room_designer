# Plan

## Original request (verbatim)

```text
Exista posibilitatea sa adaugam mai multa mobila? Ike? Ar trebui creata. Ce input ai avea nevoie ? De asemenea as dori sa avem cateva sageti 3d control pentru obiecte, lucrul cu valorile in proprietati e cam tricky
```

## Stage tracker

- [x] Răspuns documentat despre adăugarea de mobilier nou / IKEA și inputurile necesare
- [x] Import `TransformControls` din `three/addons/controls/TransformControls.js` în `odaia.html`
- [x] Integrare gizmo 3D pe obiectul selectat (`UI.sel.kind === 'item'`):
  - Săgeți de translație 3D (X, Y pentru elevație, Z)
  - Mod de rotire 3D (inel de rotație Y)
  - Sincronizare bidirecțională cu starea `S.items`, panoul de proprietăți și planul 2D
  - Trecere fluentă între OrbitControls și TransformControls
- [x] Adăugare butoane/comenzi rapide în interfață pentru comutare gizmo (Mută / Rotește)
- [x] Sincronizare fișiere (`odaia.html`, `odaia-local.html`) și repository git

## Verification

- La selectarea unei piese de mobilier în 3D (mod Orbit), apar săgețile 3D de control (TransformControls).
- Tragerea de săgeți mută obiectul pe axele X, Z și Y (elevație), actualizând simultan cotele în Proprietăți și Plan 2D.
- Rotirea camerei OrbitControls continuă să funcționeze când nu se trage de gizmo.
- Verificare consolă browser pentru 0 erori.
