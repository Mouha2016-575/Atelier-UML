# Atelier UML — Logiciel de gestion de trois hôtels

## 1. Les acteurs

| Acteur | Nature | Ce qu'il attend du système |
| --- | --- | --- |
| **Client** | Humain | Voir les chambres libres, réserver, payer les arrhes, annuler sa réservation. |
| **Agent de voyage** | Humain | Voir les chambres libres et réserver à la place d'un client. |
| **Réservant** | Acteur abstrait | Regroupe le client et l'agent pour ce qu'ils font en commun (voir plus bas). |
| **Réceptionniste** | Humain | Enregistrer l'arrivée et les consommations, faire la facture au départ, avoir la liste des arrivées du jour. |
| **Personnel du restaurant / bar** | Humain | Enregistrer ce que les clients consomment au restaurant et au bar. |
| **Gérant** | Humain | Gérer les hôtels, les catégories, les chambres et les tarifs ; voir le taux d'occupation. |
| **Service de paiement** | Système externe | Encaisser les paiements et faire les remboursements. |
| **Temps** | Temps | Lancer tout seul l'annulation à J-8 et la liste des arrivées chaque matin. |

### L'agent de voyage : même acteur, acteur distinct ou généralisation ?

Pour nous, c'est un **acteur distinct** : il n'occupe pas la chambre et il ne peut pas annuler « lui-même » comme le client. Mais comme les deux consultent et réservent de la même façon, on a créé un acteur général **Réservant**, dont le client et l'agent héritent.

## 2. Diagramme de cas d'utilisation

```mermaid
usecase-beta
direction LR
actor Reservant("Réservant")
actor Client
actor AgentVoyage("Agent de voyage")
actor Receptionniste("Réceptionniste")
actor Personnel("Personnel du restaurant / bar")
actor Gerant("Gérant")
actor Paiement("Service de paiement")
actor Temps("Temps")
systemBoundary SI["Logiciel de gestion des hôtels"]
  Consulter("Consulter les disponibilités")
  Reserver("Réserver une chambre")
  VerserArrhes("Verser les arrhes")
  Annuler("Annuler sa réservation")
  Rembourser("Rembourser le client")
  AnnulerAuto("Annuler les réservations non confirmées à J-8")
  Arrivee("Enregistrer l'arrivée")
  Conso("Enregistrer une consommation")
  Facturer("Facturer le départ")
  Encaisser("Encaisser un paiement")
  ListeArrivees("Éditer les arrivées du jour")
  Taux("Éditer le taux d'occupation")
  Admin("Administrer hôtels, catégories, chambres et tarifs")
end
Client --|> Reservant
AgentVoyage --|> Reservant
Reservant --> Consulter
Reservant --> Reserver
Client --> Annuler
Reserver ..> : include Consulter
VerserArrhes ..> : extend Reserver
VerserArrhes ..> : include Encaisser
Rembourser ..> : extend Annuler
Temps --> AnnulerAuto
Temps --> ListeArrivees
Receptionniste --> ListeArrivees
Receptionniste --> Arrivee
Receptionniste --> Conso
Receptionniste --> Facturer
Personnel --> Conso
Facturer ..> : include Encaisser
Gerant --> Taux
Gerant --> Admin
Encaisser -- Paiement
Rembourser -- Paiement
```

**Pourquoi ces relations :**

- *Réserver* **inclut** *Consulter les disponibilités* : on ne peut pas réserver sans vérifier qu'une chambre est libre.
- *Verser les arrhes* **étend** *Réserver* : les arrhes ne sont demandées que si l'arrivée est dans plus de 8 jours.
- *Rembourser* **étend** *Annuler* : le remboursement dépend du délai, il n'a pas toujours lieu.
- *Verser les arrhes* et *Facturer le départ* **incluent** *Encaisser un paiement*, qui passe par le service de paiement.
- La remise des clés et le relevé du compteur téléphonique ne sont pas des cas à part : ce sont des étapes de *Enregistrer l'arrivée*.
