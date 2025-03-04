Tout ce qui se vend sur ce site est un *produit*, que nous pourrions aussi appeler *marchandise*.

Chaque produit a une fiche sur laquelle les clients pourront trouver :

- le type du produit
- le nom du produit (optionnel)
- la marque du produit
- le prix du produit
- la disponibilité du produit
- les caractéristiques du produit (dépend du type)
- une ou plusieurs photos du produit
- les avis laissés par les acheteurs (note + commentaire)

***

Avant d'aller plus loin, je tiens à signaler que 

- j'ai décidé de nommer toutes les "propriétés" des produits en anglais
- j'utilise le JavaScript pour faire du pseudo code car c'est un langage simple et bien connu des développeurs web. Cela ne veut pas dire que c'est ce langage que nous utiliserons en interne.

***

Ce qui nous donne au format JSON :

```json
{
    "type": "kettle",
    "availability": {
        "in_stock": true,
        "remaining_alert": 0,
        "restocking_date": ""
    },
    "price": {
        "value": 19.90,
        "currency_symbol": "€"
    },
    "caracteristics": {
        "container": {
            "capacity": 1.8,
            "unit": "L"
        },
        "electric": {
            "power": {
                "value": 1500,
                "unit": "W"
            },
            "voltage": {
                "value": 240,
                "unit": "V"
            }
        },
        "look": {
            "color": "copper",
            "finish_type": "polished"
        },
        "object": {
            "dimensions": {
                "height": {
                    "value": 22,
                    "unit": "cm"
                },
                "lenght": {
                    "value": 25, 
                    "unit": "cm"
                },
                "width": {
                    "value": 19,
                    "unit": "cm"
                }
            },
            "weight": {
                "value": 830,
                "unit": "g"
            }
        },
        "product": {
            "brand": "Hagen",
            "manufacturer": "Hagen",
            "model_number": "HA5525-COP",
        }
    }
}
```

***

Avant de décrire les différentes propriétés, j'aimerai parler des raisons qui m'ont amenées à découpler les nombres des unités.

Il aurait été possible de réunir les deux en une seule propriété. Prenons l'exemple de la propriété "price", si j'avais décidé de réunir la valeur et la devise nous aurions obtenu : 

```json
"price": "19.90 €"
```

À première vue ça ne pose pas de problème, mais mon expérience me dit qu'il y en aura. Voici quelques exemples de problèmes que ce "couplage" risque de provoquer :

- la modification se retrouve compliqué car il faudra extraire le nombre, le modifier et le réintroduire dans la chaîne de caractères
- si l'on souhaite offrir aux clients la possibilité de trier les produits par prix, il faudra extraire le nombre à chaque fois
- j'ai mis un expace entre le nombre et le symbôle monéteire mais il est tout à fait possible que cela ne corresponde pas au design du site ce qui rend la tâche de l'équipe en charge d'intégrer le site plus complexe : extraire le nombre et le symbôle monétaire pour les placer aux endroits convenus par la maquette et ce à chaque affichage !
- etc.

C'est donc pour toutes ces raisons que je pense que le découplage entre les valeurs numériques et leur unité est une bonne chose.

***

Voyons à présent, point par point, les différentes "propriétés"...

# type [required]

Type de l'objet.

Par exemple ici : *kettle* qui veut dire *bouilloire* en français.

Le type de l'objet est important car il permet de définir les propriétés communes attendues pour ce type d'objet.

# price [required]

Prix du produit.

Note : j'ai déjà expliqué dans l'aparté du dessus les raisons du découplage entre la valeur numérique et l'unité.

# availability [required]

Disponibilté du produit.

## Propriétés

### in_stock 

Un booléen indiquant si le produit est en stock.

### remaining_alert

Un nombre qui permet d'alerter les clients si un produit est proche de l'épuisement.

Attention : le chiffre indiqué est à mettre en relation avec la propriété `in_stock`. C'est-à-dire que `0` ne veut pas dire que le produit est en rupture de stock mais plutôt qu'il y a suffisamment de produit disponible pour ne pas avoir à alerter les clients !

### restocking_date

Une date (approximative) de réapprovisionnement si le produit n'est plus en stock.

Une chaîne de caractère vide indique qu'aucune date de réapprovisionnement n'a été fixée. Ce qui est sans doute le signe que le produit est épuisé !

## Question : Pourquoi ne pas avoir directement ajouté le stock disponible ?

J'ai principalement pris cette décision pour des raisons de sécurité. Je considère, en effet, que la quantité de produit en stock est une donnée sensible. On peut aisément imaginer que des acteurs malveillants serait plus enclin à commettre des cambriolages si ils venaient à apprendre qu'il nous reste 1000 télés à 1000€ pièce que s'ils ne le savaient pas.

Si l'on décidait de faire du rendu côté serveur, cette API ne serait donc pas accessible aux clients finaux. Mais je ne peux pas garantir à ce stade que ce sera le cas (on pourrait aussi faire du rendu côté client) et quand bien même ce le serait cela ne nous protège pas contre d'éventuels acteurs malveillants en interne. (TODO : à reformuler)

## Question : Par quel intermédiaire pourra t-on connaître le stock disponible ?

En toute honnêteté, je ne sais pas encore vraiment. Une chose est sûre : cette information ne sera pas disponible sur le(s) serveurs en charge de servir les pages aux clients c'est-à-dire *le(s) serveur(s) web*. 

Appelons le serveur sur laquelle cette donnée est disponible "gestionnaire de stock" et le serveur en charge de valider les commandes "gestionnaire de commande".

Lors d'un achat, le gestionnaire de commandes va tout d'abord vérifier que les articles en commande sont bien disponibles, si ils le sont, il les réserve le temps que le paiement ait lieu, une fois le paiement réalisé il ordonne la commande, le gestionnaire de stock met alors à jour l'état de son stock.

Ayant dit tout cela je pense donc que **le gestionnaire de stock devra fournir une API que les serveurs webs utiliseront pour récupérer les informations sur la disponilité du produit**.

Une autre chose qui me semble sûre est que ce n'est pas au niveau des serveurs web que sera décidé à partir de quel niveau de stock une alerte devra être signalée car cela permettrait à des acteurs qui n'ont pas à savoir l'état réel du stock (les développeurs web, etc.) de modifier cette limite afin d'en connaître l'état de manière indirecte.

## Algorithme

Ce groupe de propriétés est assez particulier et pour bien le comprendre je pense qu'il serait bien de définir l'algorithme :

```js
pa = stockManager.GetProductAvailability(productId)
if(!pa.in_stock) {
    if(!pa.restocking_date) {
        return "produit épuisé"
    }
    if(pa.restocking_date <= currentDate) {
        // bug ou retard de livraison...
        stockManager.alert("la date de réapprovisionnement est dépassée")
    }
    if(pa.remaining_alert > 0) {
        // bug !
        stockManager.alert("il est indiqué qu'il reste des produits alors que le stock est indiqué comme épuisé")
    }
    return "le produit est en cours de réapprovisionnement"
}
if(pa.remaining_alert > 0) {
    return `il ne reste plus que ${remaining_alert} exemplaires disponibles`
}
```

# caracteristics

Il y a de nombreux types d'articles disponibles, allant du livre à la tondeuse à gazon. Comme ils sont tous très différents, ils ont donc tous des caractéristiques très différentes.

## product

# brand [required]

Marque de l'objet.

C'est un champs requis car c'est ce qui permet de différencier deux bouilloires ayant l'air similaires.

# manufacturer

Fabricant de l'objet.

C'est un champs optionnel.

# model_number [required]

Numéro de modèle.

C'est un numéro défini par le fabricant.

On pourrait être tenté de l'utiliser comme clé primaire mais je ne suis pas sûr que ce soit un numéro destiné à être unique sur le marché.