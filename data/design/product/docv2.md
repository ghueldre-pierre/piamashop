***

En lisant l'article wikipédia consacré à Amazon, j'ai appris que c'était initiaiement une ~~librairie en ligne~~ un revendeur de livres. J'ai aussi appris que **le premier livre vendu** s'intitulait : [Fluid Concepts and Creative Analogies](https://en.wikipedia.org/wiki/Fluid_Concepts_and_Creative_Analogies) de Douglas Hofstadter. Je me dit que ça pourrait être sympa de l'introduire parmi les livres à vendre sur notre plateforme factice et de pourquoi pas le mettre en avant (choix amazon ou meilleur vente) afin de créer une sorte de clin d'oeil.

J'ai aussi une idée entre temps qui pourrait résoudre un problème auquel je vais faire face et qui est celui de la génération de produits factices. L'idée est que nous pourrions utiliser des produits anciens, qui ne sont donc plus vendus et idéalement provenant de marques disparues. On pourrait aussi s'inspirer de jeux vidéos (fallout, etc.)

***

# Terminologie

## Les caractéristiques communes à tous les produits

La plateforme permettra aux clients de trouver toutes sortes de produits, allant du livre à la tondeuse à gazon. Comme chaque produit est très différent, ils n'ont que peu de caractéristiques communes. Mais on peut néanmoins dire que chaque produit aura au minimum :

- un nom
- un prix
- une disponibilité

***

MAJ : 

En plus des trois caractéristiques susmentionnées il faudra ajouter un identifiant de produit :

> La plupart du temps, les produits doivent disposer d'un numéro GTIN (Global Trade Item Number), comme un UPC, un ISBN ou un EAN. Amazon utilise ces identifiants de produits pour identifier l'article exact que vous vendez.

src : 

voir cette courte vidéo : [Identifiants de produit et vente avec Amazon](https://www.youtube.com/watch?v=ujhCuoHWZkw)

***

### le nom du produit

Sur un site très connu de commerce en ligne, **certains noms de produits semble être des aggrégats des différentes caractéristiques de celui-ci. Ce qui me fait penser que nous devrions parler de *titre* plutôt que de *nom***. Voici par exemple le *titre* d'une bouilloire électrique  (issu de ce site très connu) :

- Hagen, Bouilloire Electrique Compacte Inox, Design Elegant, Base 360°, Capacité 1,8L, Puissance 1500W, Base 360 °, Filtre amovible et lavable, HA5525-Cuivre

Si l'on regarde la fiche du produit, on peut constater que le titre est composé dans l'ordre :
- de la marque du produit (*Hagen*)
- du type du produit (*Bouilloire Electrique*)
- du matériau duquel il est composé (*Inox*)
- de sa capacité (*Capacité 1,8L*)
- de sa puissance électrique (*Puissance 1500W*)
- du *numéro de modèle* que le fabricant lui a assigné (*HA5525*)
- de la couleur du produit (*Cuivre*)

On peut remarquer que je n'ai pas cité tous les mots du titre, il y en a en effet certains qui semble avoir été ajouté *à la main* comme :
- Compacte
- Design Elegant
- Base 360° (que l'on retrouve d'ailleurs deux fois)
- Filtre amovible et lavable

Premier constat, **le titre des produits ne semble pas être généré de manière totalement automatique, car l'on peut voir que certains mot-clés ont été ajoutés**. Après coup, Il est assez simple de comprendre que cela fait partie "d'une stratégie en lien avec le SEO".

### le prix

Il n'y a pas grand chose à en dire, si ce n'est que un prix est composé de deux éléments : la valeur numérique et la devise. Cela a de l'importance mais nous y reviendrons lors de la modélisation des données.

### la disponibilité

Cette propriété indique si le produit est disponible immédiatement ou non voire si il est épuisé.

## Les caractéristiques propres

J'appelle caractéristiques propres, **les caractéristiques qui sont en lien direct avec le type du produit** : un livre aura des caractéristiques différentes de celles d'une tondeuse à gazon. 

Ce n'est néanmoins pas si simple que cela, car il peut exister des caractéristiques communes entre des types d'objets totalement différents : une bouilloire électrique et une tondeuse à gazon électrique ont toutes deux des caractéristiques communes découlant du fait qu'elles sont électriques (une puissance, une tension, un indice de protection, etc.)

Ce constat peut sembler anodin mais nous verrons lors de la modélisation des données, qu'il ne l'est absolument pas car c'est à partir de celui-ci que j'ai pris des décisions en ce qui concerne le type de bases de données que j'ai décidé d'utiliser.

# Les interfaces

Avant de concevoir le modèle des données, il me semble important voire primordial de passer en revue les différents acteurs qui vont interagir sur ces données et plus précisément les interfaces par lesquelles ils devront passer pour intéragir avec celles-ci.

## L'interface d'enregistrement de produit

L'acteur qui intéragira avec cette interface sera le vendeur du produit.

Il devra tout d'abord choisir le type de produit qu'il souhaite ajouter parmi une liste de types prédéfinis. Une fois le type de produit choisi, l'interface sera mis à jour afin qu'il puisse définir les caractéristiques du produit.

### Exemple : un livre

Il choisit le type "livre".

L'interface se met à jour 

# Modélisation des données

*notion de sous-type : électrique, décoratif, voire de sous-sous type électrique portatif, etc.*

Pour bien modéliser les données il faut, selon moi, savoir quelles sont les "interfaces" qui y auront accès.

## Les interfaces

