# Gala Héritage — JPV Squad

Copie du gabarit de [monarque-evenements.com](https://monarque-evenements.com) avec le contenu
de la JPV Squad. **Structure, classes, polices, espacements, animations : inchangés.**
Seuls le texte, les images, les liens et la marque ont été remplacés.

## Lancer

Fichiers statiques, rien à compiler.

```bash
python3 -m http.server 4173
```

Puis `http://localhost:4173/gala-heritage/`. Pour mettre en ligne : glisser le dossier sur Vercel ou Netlify.

## Arborescence

```
index.html          la page (90 ko)
css/site.css        le thème compilé (Tailwind v4) — récupéré tel quel
js/a.js             le JS du thème : GSAP, Lenis, SplitText, ScrollTrigger, le slider, le formulaire
fonts/              Columbia (titres) + Overused Grotesk (textes)
img/                photos JPV + cadres de remplacement
icons/              chevron du select, coche, pictogramme info
```

## Le héros — la galaxie de photos

Le héros d'origine (fond bordeaux, photos posées à plat en parallaxe) a été remplacé par une
**galaxie qui orbite en 3D autour du titre**, sur fond noir. Certaines cartes passent devant
« BÂTIR NOTRE HÉRITAGE », d'autres derrière : le titre vit dans le même espace 3D que les cartes,
c'est le navigateur qui trie la profondeur.

**Deux essaims** tournent en sens inverse, à 58 s et 84 s le tour. Comme les durées ne sont pas
multiples l'une de l'autre, le motif ne se répète pas à l'œil. Chaque carte a son angle, sa
distance, sa hauteur, sa taille, son format et son inclinaison — tirés au sort une fois à la
construction, puis figés dans le HTML.

Tout est en **CSS pur** : deux `@keyframes`, zéro JavaScript. Le compositeur du navigateur fait le
travail, le fil principal reste libre pour GSAP et le scroll. L'assombrissement des cartes qui
passent derrière est une seconde animation, calée en phase par un `animation-delay` négatif propre
à chaque carte.

### Régler la galaxie

Tout se passe dans le `<style>` en haut de `index.html` :

| Variable | Où | Effet |
|---|---|---|
| `--rbase` | `.hero3d__card` | rayon de l'orbite — plus grand, le titre respire davantage |
| `--wbase` | `.hero3d__card` | taille de référence des cartes |
| `perspective` | `[data-hero-images-wrapper]` | 1900px : plus bas, la déformation s'accentue |
| `border-radius` | `.hero3d__card` | l'arrondi des coins |
| opacités de `hero3d-dim` | `@keyframes` | contraste entre l'avant et l'arrière-plan |
| `--dur` | sur chaque `.hero3d__ring` | vitesse de chaque essaim |

Au survol du héros, la galaxie se fige — on peut regarder une photo.
`prefers-reduced-motion` l'arrête complètement.

### Le titre du héros

Deux mots superposés, comme sur la maquette Canva :

- **HÉRITAGE** en **Praise** (Typebae), crème — classe `.praise`,
  `font-size: clamp(50px, 15.5vw, 200px)`, bornée pour ne jamais déborder du téléphone au grand écran.
  Praise n'est pas une écriture manuscrite : c'est un serif display dont les minuscules sont
  dessinées en petites capitales, donc « Héritage » s'affiche **HÉRITAGE** quoi qu'on tape.
- **Gala** en anglaise rouge (`#e01f1f`), posé sur le bas-droit de HÉRITAGE — classe `.gala`,
  positionnée en absolu dans `.titre` (`right: -.01em; bottom: -.30em`).

**La police de « Gala » n'est pas celle de ta maquette.** Tu demandais ITC Edwardian Script :
c'est une police Monotype payante (~50 €), elle n'est pas sur ta machine et je ne peux pas la
récupérer. J'ai mis à la place la plus proche en licence libre. Trois candidates sont déjà
téléchargées dans `fonts/script/`, et `_anglaises.html` (à la racine du dossier) les affiche côte
à côte sur le vrai titre :

| Fichier | Caractère | État |
|---|---|---|
| `mrs-saint-delafield.woff2` | la plus proche d'Edwardian | **en place** |
| `herr-von-muellerhoff.woff2` | plus fine, plus discrète | dispo |
| `italianno.woff2` | plus ronde, plus large | dispo |

Pour changer : dans le `@font-face` de `Anglaise`, remplace le nom de fichier. Aucune des trois n'a
le grand G à boucle d'Edwardian — si ce détail compte, il faut acheter la vraie chez Monotype,
la déposer dans `fonts/script/` et pointer le `@font-face` dessus.

Praise n'est pas une écriture manuscrite : c'est un serif display dont les minuscules sont
dessinées en petites capitales. « Héritage » s'affiche donc **HÉRITAGE**, quel que soit ce qu'on
tape. C'est le dessin de la police, pas un réglage.

Le fichier est servi en `.otf`, sans conversion en woff2 — la licence de la fonderie interdit
explicitement de changer le format.

### Mettre tes photos

Les 11 emplacements sont des `<figure class="hero3d__card">` dans `index.html`. Trois portent de
vraies photos JPV, les huit autres des cadres `img/ring-*.svg`. Remplace le `src` et l'`alt` :

```html
<figure class="hero3d__card" style="--a:41.3deg;--rm:0.93;--wm:1.12;--y:-18vh;--tilt:6.4deg;--ar:0.75;--d:-51.95s">
  <img src="img/ma-photo.jpg" alt="Description">
</figure>
```

**Ne touche pas au `style`** : `--a` (angle) et `--d` (phase) vont ensemble, et `--ar` doit rester
le format de ta photo (0.75 = portrait 3/4, 1 = carré, 1.5 = paysage 3/2, 1.777 = 16/9). Les photos
sont recadrées en `object-fit: cover`, donc vise le sujet au centre. Compresse à moins de 300 ko :
elles se chargent toutes les onze au premier écran.

## Ce qu'il reste à remplacer

| Quoi | Où | État |
|---|---|---|
| Heure et lieu exacts | `index.html`, cherche `14 novembre 2026` | la date est confirmée, l'heure (18h00) et « Gembloux (5030) » restent à valider |
| Lien WhatsApp | pied de page + menu, pointe vers Instagram | ⚠️ |
| Photos | tous les `img/*.svg` sont des cadres étiquetés | ⚠️ |
| Portrait de Dady | `img/portrait-dady.svg` | ⚠️ |

Les trois seules vraies photos viennent du site JPV existant : `jpv-eveil.jpg` (extrait de
*L'Éveil*), `jpv-ciel.jpg`, `jpv-reunion.jpg`.

### La section « à propos » : fond vidéo, courbe et polaroid

**Le fond** est le reel Instagram de la JPV (`DCPDlZ-MkF9`), récupéré avec `yt-dlp` — posté par
« Jeunesse de la Pierre Vivante de Gembloux », donc c'est votre contenu. Original : 720×1280, 80 s,
13,5 Mo. Ré-encodé sans piste audio en 480p CRF 35 → **2,1 Mo** (`img/jpv-fond.mp4`), plus une
image d'attente (`img/jpv-fond-poster.jpg`).

Les 2 Mo ne partent **qu'à l'approche de la section** (`IntersectionObserver`, marge de 300 px), et
la lecture s'arrête dès qu'elle quitte l'écran. `prefers-reduced-motion` laisse l'image fixe.

**Le raccord avec le héros est une courbe**, pas une arête : le cadre vidéo est découpé en dôme
(`border-top-*-radius: 50% clamp(46px, 8vw, 130px)`) et un aplat noir placé derrière (`.apropos-noir`)
prolonge le héros dans ce qui dépasse du dôme. Par-dessus la vidéo, un voile noir à 55 %.

Deux pièges rencontrés, notés pour plus tard :
- `overflow: hidden` sur le conteneur **désactive le `position: sticky`** de la vidéo. Il faut
  `overflow: clip`, qui découpe sans créer de contexte de défilement.
- La section ne fait `300vh` qu'à partir de 1024 px. Le `sticky` n'a donc de sens que là ; en
  dessous la vidéo prend simplement `height: 100%`.

**Le polaroid** (`.polaroid`) est posé en bas à gauche, dans l'esprit de faithibiza.com : cadre
blanc, marge plus large en bas, dévers de 5,5°, ombre portée. La légende « 2025 » est en **Caveat**
(`fonts/script/caveat.woff2`, licence libre) — surtout pas en Praise, dont la version démo remplace
les chiffres par un filigrane. Caveat est une police variable : son `@font-face` doit déclarer
`font-weight: 400 700`, sinon le navigateur retombe sur la police système.


### La section « rendez-vous » : bandeau défilant

La grille des quatre rendez-vous a été remplacée par un bandeau dans l'esprit de faithibiza.com :
un mot manuscrit qui mord sur une ligne de capitales défilantes, séparées par des étoiles.

- **« Rendez-vous pour un gala »** en anglaise rouge (`.bandeau__manuscrit`), avec
  `margin-bottom: -.08em` : il mord juste sur la ligne du dessous. Un chevauchement plus fort
  (`-.24em` au premier essai) noyait les deux lignes l'une dans l'autre — Mrs Saint Delafield a
  de très longues descendantes.
- **Les adjectifs** (`.bandeau__mot`) en **Praise**, capitales, séparés par l'étoile à quatre
  branches : Inoubliable · Grandiose · Mémorable · Généreux · Solennel · Unique. La phrase se lit
  d'un trait, quel que soit le mot qui passe.
- **Le défilement** est une animation CSS de 38 s. Chez faithibiza il suit le scroll ; ici une
  boucle continue, plus simple et prise en charge par le compositeur. La séquence est écrite deux
  fois et la piste se translate de `-50 %` : la boucle est sans couture. Elle se fige au survol.
- **Le CTA** pointe vers Instagram (« Suivre nos rendez-vous ») plutôt que vers la réservation :
  c'est là que la JPV annonce Été JPV, JPV Music et la Réunion. Il ne concurrence donc pas le gros
  bouton rouge du gala. À changer dans la section si tu préfères l'inverse.

L'`id="services"` de la section est **conservé** : le thème s'en sert (`#services .offsetTop`) pour
décider quand l'en-tête passe en mode réduit. Le supprimer casserait le comportement de l'en-tête.

### Les mots mis en relief

Dans les grands titres en capitales Columbia, trois mots passent en **minuscules italiques** :
« images » (titre de la galerie), « moment » et « survit » (la phrase de la section à propos).
Classe `.mot-sans`, en **Playfair Display italique** — une police qui a un vrai italique dessiné,
là où Overused n'en a pas et se fait simplement pencher par le navigateur.

Les `<span>` survivent au découpage de GSAP SplitText, vérifié.

### Le pied de page

Il paraissait vide sur trois causes cumulées, toutes corrigées :

1. **Le logo était invisible.** Généré en crème `#DED9D7`… sur un fond crème. Repassé en bordeaux
   `#4C181A`. Attention si tu le régénères : la couleur doit contraster avec le fond du pied de page.
2. **240 px de padding réservé** (`lg:pb-[240px]`) sous un bloc bien plus garni chez Monarque.
   Ramené à 110 px.
3. **Une colonne écrasée et une colonne manquante.** La première avait `lg:pr-[50%]`, qui la
   réduisait au quart gauche. La grille est passée à trois colonnes, avec une colonne « Le gala »
   (date, horaire, lieu, cause) qui remplit l'espace en servant à quelque chose.

### La section partenaire

**Thirty Two Future Belgium** (`@thirtytwofuturebelgium`), en grande section, sur le modèle du bloc
« dream team » de faithibiza.com : polaroid incliné à gauche, logo et texte à droite.

Le fond prend **le bleu de marque de TTFB** (`#020CBC`, échantillonné dans leur logo), comme
faithibiza prend le sien. C'est leur bloc, il porte leur couleur — et ça le détache nettement du
reste du site, qui va du noir au crème.

Le logo fourni était blanc sur un carré bleu. Il a été **détouré** (`ffmpeg colorkey`) pour donner
`img/ttfb-logo.png`, blanc sur transparent : indispensable pour le poser sur le fond bleu sans
carré visible. 520 px, 123 Ko.

**Deux choses à compléter :**

| Quoi | Où |
|---|---|
| La photo de l'équipe TTFB | `img/ttfb-equipe.svg` — cadre à remplacer par une vraie photo carrée |
| La description | `.ttfb-sec__aecrire` — le paragraphe entre crochets, à écrire avec eux |

Le premier paragraphe est volontairement factuel et neutre (ils sont partenaires du gala) : je n'ai
aucune information sur ce qu'est TTFB, et je préfère un texte vrai et court à une présentation
inventée. Le second est un emplacement marqué, dans la même convention que les cadres photo.

### Le bouton de réservation

Rectangle rouge (`#e01f1f`) aux **bords ondulés**, texte en **Praise** — classe `a.btn-gala`.
Les quatre rayons sont donnés en **pixels, pas en pourcentage** : en pourcentage la forme dégénère
en ellipse au lieu de rester un rectangle. L'ondulation est un `@keyframes` de 11 s qui fait
tourner les huit valeurs de `border-radius`.

Il existe en deux tailles : `btn-gala--hero` (héros, `clamp(30px, 4.3vw, 58px)`) et la taille
standard pour l'appel final (`clamp(26px, 3.4vw, 46px)`). Les deux liens secondaires — section Gala
et pied de page — gardent le style discret du thème : ce sont des liens dans une colonne de texte,
un gros pavé rouge y serait déplacé.

Le centrage se fait par `display:flex; width:fit-content; margin-inline:auto`. En `inline-flex`,
les marges automatiques ne centrent rien — c'est ce qui décalait le bouton vers la gauche.


## La billetterie

Le bouton **« Réserver ma place »** ouvre le panneau plein écran du thème, qui contient désormais
la billetterie **Billetweb** de l'événement — le formulaire hérité de Monarque a été retiré. Un
seul chemin de réservation, pas deux qui se concurrencent.

`https://www.billetweb.fr/gala-jpv-heritage` — vérifié : la page existe, et Billetweb n'envoie
aucun en-tête `X-Frame-Options` ni `CSP frame-ancestors`, donc l'intégration passe.

**L'iframe ne se charge qu'à l'ouverture du panneau.** Un `MutationObserver` guette la classe
`opened` sur `[data-form-modal]` et pose alors le `src`. Les visiteurs qui ne réservent pas
n'envoient aucune requête à Billetweb.

### Ce qui est brandé, et ce qui ne peut pas l'être

Le contenu de l'iframe vient d'un autre domaine : **impossible de le styler depuis le site**, c'est
une règle du navigateur, pas une limite de mise en œuvre. Donc :

| | Où ça se règle |
|---|---|
| Fond bordeaux, titre en Overused, intro, ombre, coins arrondis, fondu au chargement, message d'attente, lien de repli | ici, dans `.billetterie*` |
| **Couleurs, police et logo à l'intérieur de la billetterie** | **dans Billetweb** → Options → Apparence |

Pour que l'intérieur s'accorde : bordeaux `#4c181a`, crème `#DED9D7`, rosé `#b99184`.

### La hauteur du cadre

L'iframe est à **1250 px** (1550 px sous 768 px de large), volontairement généreux : il ne doit pas
y avoir de second défilement *à l'intérieur* du cadre — c'est le point que soulevait ta page de
test. Le panneau, lui, défile normalement.

Un écouteur `postMessage` accepte en plus une hauteur annoncée par Billetweb, si leur script en
envoie une. Si tu récupères le **code d'intégration officiel** dans ton back-office Billetweb
(il embarque leur script de redimensionnement automatique), remplace l'`<iframe>` par le leur :
ce sera plus fiable qu'une hauteur fixe.

## Écarts assumés par rapport au gabarit

- **La marque.** Le papillon-blason et le mot « Monarque » sont leur logo déposé. Remplacés par un
  blason JPV (bouclier + étoile Starlight) et par « JPV SQUAD » tracé en Columbia, un `<path>` par
  lettre — l'animation d'entrée lettre par lettre fonctionne donc à l'identique.
- **WordPress retiré.** Google Tag Manager, bannière cookies Complianz, Turnstile, nonces, lazyload
  LiteSpeed, `speculationrules`. Le site ne fait plus un seul appel externe.
- **Ordre des polices.** Leur CSS chargeait le `.ttf` (135 ko) avant le `.woff2` (38 ko). Inversé.
  Aucun changement visuel, ~250 ko économisés sur mobile.
- **Un bug corrigé.** Leur JS cherchait `input[name="…voûtes…"]` en minuscule alors que leur HTML
  écrit `Voûtes` : le lien profond ne cochait jamais la case. Chez nous il marche.

## Licences — à régler avant de publier

- **Praise (Typebae) — le point le plus urgent.** Le fichier fourni est une **version DEMO,
  usage personnel uniquement, usage commercial interdit**. Le fichier `LICENCE-praise.txt` annonce
  une pénalité de 5000 $ en cas d'usage commercial. Un gala qui vend des places sortira du cadre
  « personnel » pour la plupart des fonderies. Deux conséquences concrètes :
  - **Les chiffres sont verrouillés.** Taper `0-9` en Praise affiche un filigrane
    « Personal Use Only — typebae.com » à la place du chiffre. Ne jamais utiliser Praise pour une
    date, un prix ou un nombre. Ici elle ne sert qu'au mot « Héritage », donc c'est sans effet.
  - **Licence complète** : typebae.com/product/praise. Sinon il faut changer de police.

- **Columbia Serial** (la police des titres) est une police commerciale. Le fichier vient de leur
  serveur ; pour une mise en ligne il faut une licence web, ou la remplacer.
- **Overused Grotesk** est libre (RandomMaerks), aucun problème.
- Le CSS et le JS sont le thème sur mesure fait pour Monarque par Solstice. Copie d'un gabarit tiers :
  bon pour une maquette, à réécrire si le site doit vivre publiquement sous le nom de la JPV.
