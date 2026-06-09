# Images des personnages

Le quiz cherche ici **7 portraits PNG** (transparence préservée), un par personnage :

| Personnage | Fichier         |
|------------|-----------------|
| Sangoku    | `goku.png`      |
| Végéta     | `vegeta.png`    |
| Sangohan   | `gohan.png`     |
| Piccolo    | `piccolo.png`   |
| Bulma      | `bulma.png`     |
| Krilin     | `krillin.png`   |
| Freezer    | `freezer.png`   |

## D'où viennent-elles ?

Elles ont été récupérées depuis l'API publique pour développeurs
**[dragonball-api.com](https://dragonball-api.com)**, puis converties en PNG et
redimensionnées (plus grande dimension ≤ 1200 px).

> ⚠️ Ces images restent des œuvres sous droit d'auteur (Bird Studio / Shueisha / Toei).
> Elles sont gardées **en local** et **exclues du dépôt** (voir `.gitignore`) : elles
> ne sont donc pas publiées sur GitHub. Le quiz fonctionne sans elles (repli sur l'emoji
> du personnage). Pour un usage public, utilise des visuels dont tu as les droits.

## Comment elles s'affichent

- Chargées depuis **le même domaine** que la page → l'export PNG de la carte reste valide.
- Recadrées dans un cercle avec un **cadrage haut** (tête + buste).
- Forme idéale : image au moins ~600 px de large, personnage centré horizontalement.

## Re-télécharger les portraits

```bash
# exemple pour un personnage
curl -L "https://dragonball-api.com/characters/goku_normal.webp" -o /tmp/goku.webp
python3 -c "from PIL import Image; Image.open('/tmp/goku.webp').convert('RGBA').save('img/goku.png')"
```
(Liste complète des URLs : `GET https://dragonball-api.com/api/characters?limit=100`.)
