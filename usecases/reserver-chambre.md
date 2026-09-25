# Fiche — Réserver une chambre

**Acteur principal :** le réservant (le client lui-même, ou un agent de voyage qui réserve pour lui).

**Acteur secondaire :** le service de paiement (pour les arrhes).

## Précondition

Les hôtels, les chambres et les tarifs sont déjà enregistrés dans le logiciel.

## Scénario nominal

Cas d'une réservation faite plus de 8 jours avant l'arrivée.

1. Le réservant choisit un hôtel, ses dates d'arrivée et de départ, et le nombre de personnes.
2. Le système affiche les chambres libres sur toutes ces nuits, assez grandes pour le nombre de personnes, avec leur prix.
3. Le réservant choisit une chambre.
4. Le réservant donne le nom et les coordonnées du client.
5. Le système affiche un résumé : chambre, dates, nombre de personnes, prix, et montant des arrhes (10 % minimum du prix).
6. Le réservant valide. Le système enregistre la réservation « en attente d'arrhes » et bloque la chambre pour ces nuits.
7. Le réservant paie les arrhes par le service de paiement (cas *Verser les arrhes*).
8. Le système passe la réservation en « confirmée » et donne un numéro de réservation.

## Alternatives

- **2a — Aucune chambre libre :** le système prévient le réservant, qui change ses dates ou son hôtel (retour à l'étape 1) ou abandonne.
- **2b — Trop de personnes pour les chambres de l'hôtel :** le système affiche un message d'erreur, retour à l'étape 1.
- **5a — Arrivée dans 8 jours ou moins :** pas d'arrhes à payer. À l'étape 6, la réservation est directement confirmée, puis on passe à l'étape 8.
- **6a — La chambre a été prise entre-temps par quelqu'un d'autre :** le système refuse l'enregistrement (une chambre ne peut pas être réservée deux fois la même nuit) et revient à l'étape 2.
- **7a — Le paiement est refusé :** la réservation reste « en attente d'arrhes ». Le réservant peut réessayer plus tard. Si rien n'est payé à J-8, la réservation est annulée automatiquement.

## Postcondition

Une réservation est enregistrée pour une chambre, une période et un nombre de personnes compatible avec la capacité. Elle est « confirmée » si les arrhes sont payées (ou pas exigées), sinon « en attente d'arrhes ».
