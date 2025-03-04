# Choix de SGBD

# Pour les produits

## Constat (*Rational ?*)

- Plus de lecture que d'écriture
- Une grande variété de produits différents ayant chacune des caractéristiques propres mais aussi des caractéristiques parfois communes (exemple : tondeuse à gazon électrique et machine à laver électrique)

*faire un lien vers le design des données produits*

## Conclusion

Un SGBD relationnel (SQL) nécessiterait de nombreuses tables qu'il faudrait aggréger/joindre (*join*) à chaque requête pour reconstituer les données du produit. Cette vidéo intitulée [MongoDB vs. PostgreSQL : performances et fonctionnalités](https://youtu.be/ZZ2tx8iL3P4&t=275) explique bien cette problématique.

Je pense qu'un SGDB orientée objet serait idéal pour conserver les données à propos des produits car elle ne nécessitera qu'une seule opération de lecture pour récupérer les données à propos d'un produit.

## PostgreSQL ou MongoDB ?

### PostgreSQL

Bien que PostgreSQL soit un SGBD relationnel, il supporte de nombreux types de données dont les données au format JSON.

### MongoDB


## Pour les comptes "utilisateurs"

- Les données pour les comptes utilisateur sont fixes

J'hésite entre un SGBD relationnel (SQL), un SGBD orientée objet voire un autre type de SGBD. TODO : bien se renseigner avant de prendre cette décision sachant que l'on utilise déjà un SGBD orientée documents pour stocker les données des produits.

////
Je pense qu'un SGBD relationnel (SQL) serait idéal pour conserver les données des comptes utilisateurs. Mais un SGBD orientée objet pourrait aussi faire l'affaire. 