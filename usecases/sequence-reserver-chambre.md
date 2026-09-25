sequenceDiagram
    actor R as Réservant
    participant S as Système
    participant P as Service de paiement

    R->>S: Choisir hôtel, dates et nombre de personnes
    S->>S: Chercher les chambres libres et assez grandes
    alt Aucune chambre libre
        S-->>R: Aucune chambre disponible, changer les critères
    else Des chambres sont libres
        S-->>R: Liste des chambres avec leur prix
        R->>S: Choisir une chambre et donner les coordonnées du client
        S-->>R: Résumé (chambre, dates, prix, arrhes)
        R->>S: Valider la réservation
        alt La chambre a été prise entre-temps
            S-->>R: Chambre plus disponible, retour à la liste
        else La chambre est toujours libre
            S->>S: Enregistrer la réservation et bloquer les nuits
            opt Arrivée dans plus de 8 jours
                S-->>R: Demander les arrhes (10 % minimum)
                R->>P: Payer les arrhes
                P-->>S: Résultat du paiement
                alt Paiement accepté
                    S->>S: Passer la réservation en « confirmée »
                else Paiement refusé
                    S-->>R: Paiement refusé, réservation en attente jusqu'à J-8
                end
            end
            S-->>R: Numéro et récapitulatif de la réservation
        end
    end
