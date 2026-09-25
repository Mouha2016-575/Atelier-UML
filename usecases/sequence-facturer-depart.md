```mermaid
sequenceDiagram
    actor R as Réceptionniste
    participant S as Système
    participant P as Service de paiement

    R->>S: Rechercher le séjour (nom ou numéro de chambre)
    alt Séjour introuvable
        S-->>R: Aucun séjour trouvé, vérifier la recherche
    else Séjour trouvé
        S-->>R: Chambre, dates, personnes et consommations
        R->>S: Saisir le relevé final du compteur téléphonique
        S->>S: Calculer chambre, consommations, téléphone et taxe de séjour
        S->>S: Retirer les arrhes déjà payées
        S-->>R: Facture et reste à payer
        opt Le client conteste la facture
            R->>S: Corriger la donnée fausse
            S-->>R: Facture recalculée
        end
        R->>S: Valider la facture
        opt Reste à payer supérieur à 0
            S->>P: Demander l'encaissement du reste
            P-->>S: Résultat du paiement
            break Paiement refusé
                S-->>R: Facture non payée, proposer un autre moyen de paiement
            end
        end
        S->>S: Marquer la facture payée et enregistrer le départ
        S-->>R: Facture acquittée à remettre au client
    end
