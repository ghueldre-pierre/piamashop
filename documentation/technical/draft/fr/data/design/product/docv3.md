# Données produits

## Contexte

Avant toute chose, comme il l'a été mentionné dans [la documentation générale](), *Piamashop* a pour objectif de permettre aux vendeurs de proposer à la vente toutes sortes de marchandises. C'est-à-dire que les vendeurs pourront vendre tout aussi bien des livres que des canapés ou encore des ordinateurs.

Il a ausse été dit dans [la documentation générale]() qu'à chaque type de marchandise correspondra un ensemble de caractéristiques propres que le vendeur devra fournir afin d'aider le client a faire son choix entre plusieurs marchandises du même type. Pour simplifier, cela veut dire qu'un livre aura un nombre de pages, un canapé, un nombre de places, un ordinateur, un certain type de processeur, etc.

## Caractéristiques communes à tous les produits

Bien que chaque type de marchandise ait des caractéristiques propres ils ont aussi des caractéristiques communes.

### Un titre

Tous les produits auront un titre.

***

On remarquera que **j'utilise le mot "titre" et non le mot "nom"** car sur un site très connu de commerce en ligne, certains « noms » de produits semble être une aggrégation des différentes caractéristiques de celui-ci. Voici par exemple le *titre* d'une bouilloire électrique  (issu de ce site très connu) - j'ai modifié la marque :

> Pasco, Bouilloire Electrique Compacte Inox, Design Elegant, Base 360°, Capacité 1,8L, Puissance 1500W, Base 360 °, Filtre amovible et lavable, HA5525-Cuivre

Si l'on regarde la fiche du produit, on peut constater que le titre est composé dans l'ordre :
- de la marque du produit (*Pasco*)
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

***

### Un Code Article International

On le connait plus sous le sigle GTIN (*Global Trade Item Number*). C'est **un identifiant unique et international** délivré par un organisme à but non lucratif appelé le GS1 (Global Standards 1) et dont le siège est à Bruxelles.

***

À noter qu'Amazon a son propre code d'identification de produit nommé ASIN [(*Amazon Standard Identification Number*)](https://en.wikipedia.org/wiki/Amazon_Standard_Identification_Number) et que celui-ci ne fait pas partie "des identifiants GTIN".

***

Il y a divers types de GTIN mais ils ont pour caractéristique commune d'être **uniquement composé de chiffres**. Chaque type est nommé de la même manière à savoir GTIN suivi d'un tiret et du nombre de chiffres qui le compose ainsi le type GTIN-8 est composé de 8 chiffres. À ce jour l'on en dénombre quatre types : le GTIN-8, le GTIN-12, le GTIN-13 et le GTIN-14.

***

Ce code offre de nombreux avantages parmi lesquelles celui de permettre la récupération d'informations à propos d'un produit :

Voir : https://www.gs1.org/services/verified-by-gs1 - https://www.ean-search.org/ - etc.

***

#### Brouillon : Autres formulations

Chaque type de GTIN est défini en fonction du nombre de chiffres qui le compose.

/////////////////
## Quelle sera la clé primaire des produits ?

Dans une base de données relationnelle, une clé primaire est la donnée qui permet d'identifier de manière unique un enregistrement dans une table.

### Titre

On peut tout de suite rejeter le titre car il est susceptible de changer.

### GTIN

GTIN est le sigle de **Global Trade Item Number** que l'on peut traduire en français pas **code article international**.

Comme il sert à iden

Ça semble être le candidat idéal car il est dédié à cela c'est-à-dire à identifier 