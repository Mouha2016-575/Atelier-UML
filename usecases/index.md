# Synthèse de l'Atelier UML — Gestion de trois hôtels

## 1. Acteurs

| Acteur | Nature | Attente ou interaction |
| --- | --- | --- |
| **Client** | Humain | Consulter les disponibilités, réserver, verser des arrhes, annuler sa réservation et obtenir le remboursement. |
| **Agent de voyage partenaire** | Humain | Consulter les disponibilités et réserver pour le compte d’un client. |
| **Réceptionniste** | Humain | Enregistrer l’arrivée, les consommations, consulter les arrivées et facturer le départ. |
| **Personnel de restauration / bar** | Humain | Enregistrer les consommations (restaurant et bar). |
| **Gérant** | Humain | Administrer le système (hôtels, chambres, tarifs) et consulter les taux d’occupation. |
| **Service de paiement** | Système externe | Traiter les encaissements et les remboursements. |
| **Temps / horloge** | Temps | Déclencher l’annulation à J-8 et l’édition des arrivées. |

---

## 2. Diagramme de cas d'utilisation simplifié 
```mermaid
graph TD
    subgraph Acteurs
        Client[Client]
        Agent[Agent de voyage]
        Reception[Réceptionniste]
        Personnel[Personnel de restauration]
        Gerant[Gérant]
        Paiement[Service de paiement]
        Temps[Temps / horloge]
    end

    subgraph "Gestion des Hôtels"
        Disponibilites[Consulter les disponibilités]
        Reserver[Réserver une chambre]
        Arrhes[Verser les arrhes]
        Encaisser[Encaisser un paiement]
        Annuler[Annuler sa réservation]
        Rembourser[Rembourser le client]
        Arriver[Enregistrer l'arrivée]
        Consommer[Enregistrer les consommations]
        Facturer[Facturer le départ]
        Admin["Administrer (hôtels, tarifs)"]
        Occupation[Consulter l'occupation]
        Arrivees[Éditer les arrivées du matin]
        Expirer[Annuler à J-8]
    end

    %% Connexions principales simplifiées
    Client --> Disponibilites
    Client --> Reserver
    Client --> Arrhes
    Client --> Annuler
    Client --> Rembourser

    Agent --> Disponibilites
    Agent --> Reserver
    Agent --> Arrhes

    Reception --> Arriver
    Reception --> Consommer
    Reception --> Facturer
    Reception --> Arrivees

    Personnel --> Consommer

    Gerant --> Admin
    Gerant --> Occupation

    Paiement --> Encaisser
    Paiement --> Rembourser

    Temps --> Expirer
    Temps --> Arrivees
