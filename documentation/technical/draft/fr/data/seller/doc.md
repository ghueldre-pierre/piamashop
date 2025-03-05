# Données vendeur

## Données profil

- adresse électronique
- mot de passe (crypté)
- nom d'utilisateur

*Les données profil utilisateur seront très similaire à celles des vendeurs...*

## Données produits

C'est la liste des produits que le vendeur propose sur Piamashop.

Pour chaque produit il y a :

- la clé primaire produit (je pense que ce sera le GTIN)
- le prix auquel le vendeur propose de vendre le produit

### Le prix

Problème : le prix peut varier en fonction de la "zone commerciale", on vendra à un client résidant dans la zone euro en euro et aux états-unis en dollar.