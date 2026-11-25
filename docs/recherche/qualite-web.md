14. Quelles sont les balises HTML sémantiques principales (header, nav, main, article, section, footer…) et comment hiérarchiser les titres h1 à h6 ?
<header> / <footer> : en-tête ou pied d'une page ou section.
<nav> : navigation principale.
<main> : contenu central unique de la page.
<article> : contenu autonome (billet, fiche).
<section> : regroupement thématique.

Titres h1–h6 : suivre l'ordre hiérarchique sans sauter de niveau ; un <h1> principal par page ; styler avec le CSS, pas en changeant de balise.

<main><article><h1>Titre</h1><h2>Sous-titre</h2></article></main>
Source ; https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents

15. Quels formats d'image utiliser sur le web (WebP, AVIF, JPEG, PNG, SVG) ? Comment nommer, dimensionner et charger une image (lazy loading) ?

Formats : AVIF / WebP (photos et visuels modernes légers), JPEG (photo classique), PNG (transparence/sans perte), SVG (icônes/logos vectoriels).
Nommage : minuscules, tirets, sans accents (photo-jura.avif).
Dimensions : renseigner width et height + srcset pour éviter les décalages et adapter la taille aux écrans.
Lazy loading : loading="lazy" sur les images hors écran uniquement (pas sur l'image visible au chargement).

<img src="photo-jura.avif" width="800" height="500" loading="lazy" alt="Paysage du Jura" />

source : https://developer.mozilla.org/fr/docs/Web/Media/Guides/Formats/Image_types

16. Qu'est-ce qu'un slug ? Pourquoi générer les pages depuis des données plutôt que créer une page par contenu ?

Slug : portion d'URL lisible en minuscules avec tirets (mon-article), identifiant une ressource.

Données plutôt qu'une page par contenu : centraliser les contenus (JSON, Markdown, BDD) dans un modèle unique facilite la maintenance, garantit la cohérence et permet d'ajouter des pages sans dupliquer le HTML.

{ "slug": "mon-article", "titre": "Mon article" }
https://developer.mozilla.org/en-US/docs/Glossary/SSG