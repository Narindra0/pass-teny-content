# pass-teny-content

Le contenu canon de Pass'Teny — **lyrics + annotations validées**, versionnés.

> Ce dossier est un **repo GitHub public dédié** (`Narindra0/pass-teny-content`),
> séparé du code de l'application. Les contributions de la communauté arrivent
> par Pull Request (ouvertes automatiquement par l'app après validation des votes).

## Arborescence

```
content/
├── index.json                  # index généré (artistes + titres)
└── <artiste-slug>/
    └── <titre-slug>/
        ├── meta.json           # métadonnées (artiste, album, date, source…)
        ├── lyrics.lrc          # paroles synchronisées (format LRC)
        ├── lyrics.txt          # paroles brutes (dérivé du .lrc)
        └── annotations.json    # annotations validées (format ci-dessous)
```

## Slugification

- **Artiste** : slug Pass'io si présent, sinon dérivé du nom (minuscules,
  sans accents, `-` pour les espaces).
- **Titre** : dérivé du titre de la piste, désambiguïsé (`-2`, `-3`…)
  si collision au sein d'un même artiste.

## `meta.json`

```jsonc
{
  "id": "ailleurs",                // slug du titre
  "title": "Ailleurs",
  "artist": "Toulyo R.",
  "artists": ["Toulyo R."],
  "album": "Neuf Huit Part.2",
  "albumSlug": "neuf-huit-part-2-98dd",
  "releaseDate": "2026-05-08",
  "coverUrl": "https://…",
  "language": [],                  // ex. ["mg", "fr"]
  "tags": [],
  "source": {                      // attribution automatique (seed Pass'io)
    "platform": "passio",
    "albumId": "…",
    "trackId": "…",
    "note": "Source : catalogue Pass'io"
  },
  "addedAt": "2026-08-13"
}
```

## `annotations.json`

Chaque annotation référence un passage par **offsets caractères** dans
`lyrics.txt` (début inclus, fin exclusive) et en copie la citation exacte.

```jsonc
{
  "language": "mg-fr",
  "annotations": [
    {
      "id": "a_8f3k2",
      "start": 142,
      "end": 178,
      "quote": "Tia anao aho, ry malala",
      "body": "« Tia » — racine de l'amour… Explication du sens, du contexte et de la référence culturelle.",
      "tags": ["amour", "ohabolana"],
      "author": "github_handle",
      "createdAt": "2026-08-13T10:00:00.000Z",
      "updatedAt": "2026-08-13T10:00:00.000Z"
    }
  ]
}
```

## Règles de contribution

1. **Les offsets doivent rester stables** : ne jamais modifier `lyrics.txt`
   sans réviser simultanément les annotations qui pointent dedans.
2. `lyrics.txt` est **dérivé** de `lyrics.lrc` (génération automatique).
3. L'app valide chaque annotation (`quote === lyrics.txt.slice(start, end)`).
4. Le contenu est versionné : chaque merge de PR = un commit, une étape
   de l'historique (réputation dérivable du Git).

## Génération du seed initial

```bash
cd PassTeny
node scripts/seed-from-passio.mjs            # depuis le dump Pass'io (API)
node scripts/seed-from-passio.mjs --dry-run  # aperçu
```

> ⚠ Les paroles proviennent du catalogue Pass'io (attribution dans
> `meta.json.source`). Vérifier les accords de diffusion avant publication
> publique du repo.
