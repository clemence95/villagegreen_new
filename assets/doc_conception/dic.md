# Dictionnaire de données - VillageGreen

## Notes générales
- **Calcul du prix de vente** : Le prix de vente est calculé à partir du prix d'achat en appliquant un coefficient spécifique au client. Ce coefficient est défini lors de la création du client et peut être ajusté par le service commercial.
- **Gestion des réductions** : Les clients professionnels peuvent bénéficier de réductions supplémentaires négociées par le service commercial. Ces réductions sont appliquées sur le total HT de la commande.

## Entité : Utilisateur/Admin
| Nom            | Type       | Taille | Règles                          | Signification                             |
|----------------|------------|--------|---------------------------------|-------------------------------------------|
| id_utilisateur | INT        | -      | PK, Auto-incrément              | Identifiant unique de l'utilisateur       |
| nom            | VARCHAR    | 100    | Non nul                         | Nom de l'utilisateur                      |
| prenom         | VARCHAR    | 100    | Non nul                         | Prénom de l'utilisateur                   |
| email          | VARCHAR    | 255    | Unique, Non nul                 | Adresse email de l'utilisateur            |
| mot_de_passe   | VARCHAR    | 255    | Non nul                         | Mot de passe hashé                        |
| role           | ENUM       | -      | Valeurs : "admin", "client"     | Rôle de l'utilisateur                     |
| coefficient    | DECIMAL    | 5,2    | Non nul                         | Coefficient pour le calcul du prix        |
| id_commercial  | INT        | -      | FK                              | Référence au commercial attribué          |
| date_creation  | DATETIME   | -      | Non nul                         | Date de création du compte                |
| type_client    | ENUM       | -      | Valeurs : "particulier", "pro"  | Type de client                            |

### Exemple de données
| id_utilisateur | nom     | prenom | email                  | mot_de_passe | role   | coefficient | id_commercial | date_creation       |
|----------------|---------|--------|------------------------|--------------|--------|-------------|---------------|---------------------|
| 1              | Dupont  | Jean   | jean.dupont@mail.com   | ************ | client | 1.20        | 2             | 2023-01-15 10:00:00 |
| 2              | Martin  | Sophie | sophie.martin@mail.com | **********   | admin  | -           | -             | 2023-01-10 09:00:00 |

## Entité : Catégorie/Sous-catégorie
| Nom                | Type       | Taille | Règles                          | Signification                              |
|--------------------|------------|--------|---------------------------------|--------------------------------------------|
| id_categorie       | INT        | -      | PK, Auto-incrément              | Identifiant unique de la catégorie         |
| nom_categorie      | VARCHAR    | 100    | Non nul                         | Nom de la catégorie                        |
| id_sous_categorie  | INT        | -      | PK, Auto-incrément              | Identifiant unique de la sous-catégorie    |
| nom_sous_categorie | VARCHAR    | 100    | Non nul                         | Nom de la sous-catégorie                   |
| id_categorie_parent| INT        | -      | FK                              | Référence à la catégorie parente           |

## Entité : Produits
| Nom                  | Type       | Taille | Règles                          | Signification                              |
|----------------------|------------|--------|---------------------------------|--------------------------------------------|
| id_produit           | INT        | -      | PK, Auto-incrément              | Identifiant unique du produit              |
| nom_produit          | VARCHAR    | 100    | Non nul                         | Nom du produit                             |
| description_courte   | VARCHAR    | 255    | Non nul                         | Libellé court du produit                   |
| description_longue   | TEXT       | -      | -                               | Libellé long (description détaillée)       |
| reference_fournisseur| VARCHAR    | 100    | Non nul                         | Référence fournisseur du produit           |
| prix_achat           | DECIMAL    | 10,2   | Non nul                         | Prix d'achat du produit (HT)               |
| prix_vente           | DECIMAL    | 10,2   | Calculé                         | Prix de vente calculé (HT)                 |
| photo                | VARCHAR    | 255    | -                               | URL ou chemin de la photo du produit       |
| stock                | INT        | -      | Non nul                         | Quantité en stock                          |
| id_categorie         | INT        | -      | FK                              | Référence à la catégorie du produit        |
| id_sous_categorie    | INT        | -      | FK                              | Référence à la sous-catégorie du produit   |
| id_fournisseur       | INT        | -      | FK                              | Référence au fournisseur exclusif          |
| statut_publication   | ENUM       | -      | Valeurs : "actif", "désactivé"  | Statut de publication du produit           |

### Exemple de données
| id_produit | nom_produit        | description_courte | prix_achat | prix_vente | stock | id_categorie | id_sous_categorie | id_fournisseur | statut_publication |
|------------|--------------------|--------------------|------------|------------|-------|--------------|-------------------|----------------|--------------------|
| 1          | Guitare acoustique | Guitare 6 cordes   | 150.00     | 180.00     | 25    | 1            | 2                 | 1              | actif              |
| 2          | Piano numérique    | Piano 88 touches   | 500.00     | 600.00     | 10    | 3            | 4                 | 2              | actif              |

## Entité : Fournisseurs
| Nom               | Type       | Taille | Règles                          | Signification                              |
|-------------------|------------|--------|---------------------------------|--------------------------------------------|
| id_fournisseur    | INT        | -      | PK, Auto-incrément              | Identifiant unique du fournisseur          |
| nom_fournisseur   | VARCHAR    | 100    | Non nul                         | Nom du fournisseur                         |
| contact           | TEXT       | -      | -                               | Informations de contact du fournisseur     |

## Entité : Commande
| Nom                | Type       | Taille | Règles                          | Signification                              |
|--------------------|------------|--------|----------------------------------|-------------------------------------------|
| id_commande        | INT        | -      | PK, Auto-incrément              | Identifiant unique de la commande          |
| id_utilisateur     | INT        | -      | FK                              | Référence à l'utilisateur ayant commandé   |
| date_commande      | DATETIME   | -      | Non nul                         | Date de la commande                        |
| statut             | ENUM       | -      | Valeurs : "en cours", "expédiée"| Statut de la commande                      |
| total_ht           | DECIMAL    | 10,2   | Non nul                         | Montant total HT de la commande            |
| reduction          | DECIMAL    | 5,2    | -                               | Réduction appliquée sur le total           |
| adresse_livraison  | TEXT       | -      | Non nul                         | Adresse de livraison                       |
| adresse_facturation| TEXT       | -      | Non nul                         | Adresse de facturation                     |
| mode_paiement      | ENUM       | -      | Valeurs : "carte", "virement"   | Mode de paiement                           |
| reglement          | ENUM       | -      | Valeurs : "payé", "en attente"  | Informations sur le règlement              |

### Exemple de données
| id_commande | id_utilisateur | date_commande       | statut     | total_ht | reduction | adresse_livraison       | adresse_facturation      | mode_paiement |reglement |
|-------------|----------------|---------------------|------------|----------|-----------|-------------------------|--------------------------|---------------|----------|
| 1           | 1              | 2023-02-01 14:30:00| en cours    | 780.00   | 50.00     | 12 rue des Lilas, Paris | 12 rue des Lilas, Paris   | carte        | payé      |
| 2           | 2              | 2023-02-02 10:00:00| expédiée    | 1200.00  | 0.00      | 45 avenue des Champs    | 45 avenue des Champs     | virement      |
en attente|

## Entité : Ligne de commande
| Nom               | Type       | Taille | Règles                          | Signification                              |
|-------------------|------------|--------|---------------------------------|--------------------------------------------|
| id_ligne_commande | INT        | -      | PK, Auto-incrément              | Identifiant unique de la ligne de commande |
| id_commande       | INT        | -      | FK                              | Référence à la commande                    |
| id_produit        | INT        | -      | FK                              | Référence au produit                       |
| quantite_commande | INT        | -      | Non nul                         | Quantité commandée par le client           |
| quantite_livree   | INT        | -      | Par défaut : 0                  | Quantité livrée au client                  |
| prix_unitaire     | DECIMAL    | 10,2   | Non nul                         | Prix unitaire au moment de la commande     |

### Exemple de données
| id_ligne_commande | id_commande | id_produit | quantite_commande | quantite_livree | prix_unitaire |
|-------------------|-------------|------------|-------------------|-----------------|---------------|
| 1                 | 1           | 1          | 5                 | 3               | 180.00        |
| 2                 | 1           | 2          | 2                 | 2               | 600.00        |
| 3                 | 2           | 1          | 10                | 0               | 180.00        |

## Entité : Documents associés
| Nom               | Type       | Taille | Règles                          | Signification                              |
|-------------------|------------|--------|---------------------------------|--------------------------------------------|
| id_document       | INT        | -      | PK, Auto-incrément              | Identifiant unique du document             |
| id_commande       | INT        | -      | FK                              | Référence à la commande associée           |
| type_document     | ENUM       | -      | Valeurs : "facture", "bon"      | Type de document                           |
| date_creation     | DATETIME   | -      | Non nul                         | Date de création du document               |
| contenu           | TEXT       | -      | -                               | Contenu ou chemin vers le fichier          |

## Vérification des contraintes
- **Unicité** : Les champs `email` (Utilisateur/Admin) et `reference_fournisseur` (Produits) doivent être uniques.
- **Relations FK** : Vérifiez que toutes les relations FK (ex. `id_commercial`, `id_categorie`) sont correctement définies dans la base de données.
- **Non nul** : Les champs marqués comme "Non nul" doivent être obligatoires dans la base de données.
- **Quantité livrée** : La valeur de `quantite_livree` ne peut pas dépasser `quantite_commande`.

## Notes supplémentaires

### Validation métier
- **Contrainte `quantite_livree <= quantite_commande`** : Cette règle doit être implémentée au niveau de la base de données ou dans la logique applicative pour garantir que la quantité livrée ne dépasse jamais la quantité commandée.
- **Statuts de commande** : Les transitions entre les statuts (ex. "en cours" → "expédiée") doivent être contrôlées pour éviter des incohérences. Par exemple, une commande ne peut pas être marquée comme "expédiée" si toutes les lignes de commande n'ont pas été entièrement livrées.

### Documentation supplémentaire
- **Gestion des réductions** : 
  - Les réductions sont appliquées uniquement aux clients professionnels.
  - La réduction est négociée par le service commercial et enregistrée dans le champ `reduction` de l'entité **Commande**.
  - Exemple : Une commande avec un total HT de 1000 € et une réduction de 10 % aura un champ `reduction` de 100.00 et un total final de 900.00 €.

- **Statuts de commande** :
  - **en cours** : La commande est enregistrée mais pas encore expédiée.
  - **expédiée** : La commande a été entièrement ou partiellement expédiée.
  - **livrée** : La commande a été entièrement livrée.

### Optimisation des types
- **DECIMAL(10,2)** : Vérifiez que la précision est suffisante pour les montants financiers. Si les montants sont très élevés, envisagez d'augmenter la taille (ex. `DECIMAL(12,2)`).
- **VARCHAR** : Les tailles actuelles (ex. `VARCHAR(100)` pour les noms) semblent adaptées, mais elles peuvent être ajustées en fonction des besoins réels.
- **TEXT** : Utilisé pour les descriptions longues et les contenus de documents. Si ces champs contiennent des données volumineuses, envisagez d'utiliser des types comme `LONGTEXT` ou de stocker les fichiers en dehors de la base de données.

### Exemple de validation métier
Pour l'entité **Ligne de commande** :
- Si `quantite_commande = 5` et `quantite_livree = 3`, il reste 2 unités à livrer.
- Une tentative de mise à jour avec `quantite_livree = 6` doit être rejetée.

Pour l'entité **Commande** :
- Une commande avec `statut = "expédiée"` doit avoir au moins une ligne de commande avec une quantité livrée > 0.

### Calcul du prix de vente TTC
Pour calculer le prix de vente TTC, une méthode peut être ajoutée dans le code applicatif. Voici un exemple en PHP :

```php
public function calculerPrixVenteTTC(float $tauxTVA = 0.2): string
{
    // Obtient le coefficient du client
    $coefficient = $this->getClientCoefficient();

    // Calcul du prix hors taxe
    $prixHT = bcmul($this->prixAchat, $coefficient, 2);

    // Calcul du prix avec TVA
    return bcmul($prixHT, (string)(1 + $tauxTVA), 2);
}
```

### Explications :
1. **Coefficient client** : La méthode `getClientCoefficient()` récupère le coefficient spécifique au client.
2. **Prix HT** : Le prix HT est calculé en multipliant le prix d'achat par le coefficient.
3. **Prix TTC** : Le prix TTC est obtenu en appliquant le taux de TVA au prix HT.

Cette méthode garantit un calcul précis grâce à l'utilisation de la fonction `bcmul` pour les opérations arithmétiques avec précision décimale.

## Suggestions d'amélioration

### 1. Gestion des statuts
Ajout d'un diagramme d'état pour clarifier les transitions possibles entre les statuts de commande :

| Statut actuel | Transition possible vers | Validation requise                          |
|---------------|--------------------------|---------------------------------------------|
| en cours      | expédiée                 | Au moins une ligne de commande livrée       |
| expédiée      | livrée                   | Toutes les lignes de commande livrées       |

#### Diagramme d'état (texte descriptif) :
- **en cours** → **expédiée** : Une commande passe à "expédiée" lorsqu'au moins une ligne de commande a été livrée.
- **expédiée** → **livrée** : Une commande passe à "livrée" lorsque toutes les lignes de commande ont été entièrement livrées.

### 2. Optimisation des types
Pour les champs volumineux comme `description_longue` ou `contact`, envisagez de stocker ces informations dans des fichiers externes et de ne conserver que les chemins dans la base de données. Exemple :

| Nom               | Type       | Taille | Règles                          | Signification                              |
|-------------------|------------|--------|---------------------------------|--------------------------------------------|
| chemin_description| VARCHAR    | 255    | -                               | Chemin vers un fichier contenant la description détaillée |

### 3. Gestion des réductions
Ajout d'une colonne `type_client` dans l'entité **Utilisateur/Admin** pour différencier les clients professionnels des particuliers :

| Nom            | Type       | Taille | Règles                          | Signification                             |
|----------------|------------|--------|---------------------------------|-------------------------------------------|
| type_client    | ENUM       | -      | Valeurs : "particulier", "pro"  | Type de client                            |

### 4. Tests automatisés
Ajout d'exemples de tests unitaires pour valider les statuts et le calcul des prix :

#### Exemple en PHP pour valider les statuts
```php
public function testTransitionStatut()
{
    $commande = new Commande();
    $commande->statut = "en cours";

    // Transition vers "expédiée"
    $commande->statut = "expédiée";
    $this->assertEquals("expédiée", $commande->statut);

    // Transition vers "livrée"
    $commande->statut = "livrée";
    $this->assertEquals("livrée", $commande->statut);
}
```

#### Exemple en PHP pour valider le calcul des prix
```php
public function testCalculPrixVenteTTC()
{
    $produit = new Produit();
    $produit->prixAchat = 100.00;
    $produit->coefficient = 1.20;

    $prixTTC = $produit->calculerPrixVenteTTC(0.2);
    $this->assertEquals("144.00", $prixTTC);
}
```

### 5. Diagrammes visuels
Inclure un diagramme UML ou Merise pour représenter les relations entre les entités. Voici une description textuelle pour guider la création du diagramme :

@startuml
' Définition des entités et de leurs relations

class Utilisateur {
    +id_utilisateur : INT
    +nom : VARCHAR(100)
    +prenom : VARCHAR(100)
    +email : VARCHAR(255)
    +mot_de_passe : VARCHAR(255)
    +role : ENUM("admin", "client")
    +coefficient : DECIMAL(5,2)
    +id_commercial : INT
    +date_creation : DATETIME
    +type_client : ENUM("particulier", "pro")
}

class Commande {
    +id_commande : INT
    +id_utilisateur : INT
    +date_commande : DATETIME
    +statut : ENUM("en cours", "expédiée", "livrée")
    +total_ht : DECIMAL(10,2)
    +reduction : DECIMAL(5,2)
    +adresse_livraison : TEXT
    +adresse_facturation : TEXT
    +mode_paiement : ENUM("carte", "virement")
    +reglement : ENUM("payé", "en attente")
}

class LigneCommande {
    +id_ligne_commande : INT
    +id_commande : INT
    +id_produit : INT
    +quantite_commande : INT
    +quantite_livree : INT
    +prix_unitaire : DECIMAL(10,2)
}

class Produit {
    +id_produit : INT
    +nom_produit : VARCHAR(100)
    +description_courte : VARCHAR(255)
    +description_longue : TEXT
    +reference_fournisseur : VARCHAR(100)
    +prix_achat : DECIMAL(10,2)
    +prix_vente : DECIMAL(10,2)
    +photo : VARCHAR(255)
    +stock : INT
    +id_categorie : INT
    +id_sous_categorie : INT
    +id_fournisseur : INT
    +statut_publication : ENUM("actif", "désactivé")
}

class Fournisseur {
    +id_fournisseur : INT
    +nom_fournisseur : VARCHAR(100)
    +contact : TEXT
}

class Categorie {
    +id_categorie : INT
    +nom_categorie : VARCHAR(100)
    +id_categorie_parent : INT
}

class SousCategorie {
    +id_sous_categorie : INT
    +nom_sous_categorie : VARCHAR(100)
    +id_categorie_parent : INT
}

' Relations entre les entités
Utilisateur "1" --> "0..*" Commande : id_utilisateur
Commande "1" --> "1..*" LigneCommande : id_commande
LigneCommande "1" --> "1" Produit : id_produit
Produit "1" --> "1" Fournisseur : id_fournisseur
Produit "1" --> "1" Categorie : id_categorie
Produit "1" --> "1" SousCategorie : id_sous_categorie
@enduml

### 6. Gestion des erreurs
Dans les exemples PHP, ajouter des logs ou des messages d'erreur détaillés pour faciliter le débogage. Exemple :

```php
try {
    $ligneCommande->save();
} catch (Exception $e) {
    error_log("Erreur lors de l'enregistrement de la ligne de commande : " . $e->getMessage());
    throw $e;
}
```

