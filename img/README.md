# Images des personnages

Dépose ici **7 fichiers JPG**, un par personnage, avec ces noms EXACTS
(en minuscules, sans accent) :

| Personnage | Fichier attendu     |
|------------|---------------------|
| Sangoku    | `goku.jpg`          |
| Végéta     | `vegeta.jpg`        |
| Sangohan   | `gohan.jpg`         |
| Piccolo    | `piccolo.jpg`       |
| Bulma      | `bulma.jpg`         |
| Krilin     | `krillin.jpg`       |
| Freezer    | `freezer.jpg`       |

## Recommandations

- **Format** : JPG. (Pas de transparence : l'image remplit entièrement le cercle,
  pense donc à une photo/illustration au cadrage propre autour du visage.)
- **Forme** : image **carrée** (ex. 800×800). Elle est recadrée en cercle, en mode
  « cover » (centrée, remplit le cercle) — garde donc le visage bien centré.
- **Résolution** : au moins 600×600 px (la carte partagée fait 1080×1920, le portrait
  y occupe ~460 px). Plus grand = plus net.
- **Poids** : compresse si possible (< 300 Ko / image) pour un chargement rapide.

## Important

- Les images sont chargées depuis **le même domaine** que la page (GitHub Pages),
  ce qui permet d'exporter la carte en PNG et de la partager. N'utilise pas d'URL
  externe : cela « tainterait » le canvas et casserait le partage.
- Tant qu'un fichier est absent, le quiz fonctionne et affiche l'**emoji** du
  personnage à la place. Ajoute les images quand tu veux, puis pousse-les.

Une fois les fichiers déposés ici :

```bash
git add img/*.jpg
git commit -m "Ajoute les portraits des personnages"
git push
```
