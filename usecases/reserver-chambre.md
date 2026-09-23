# UC01 — Réserver une chambre

## Acteurs

**Acteur principal :** client ou agent de voyage partenaire agissant pour un client identifié.

**Acteur secondaire :** service de paiement pour les arrhes.

**Objectif :** obtenir une réservation d'une chambre adaptée au nombre d'occupants et disponible sur toute la période.

## Préconditions

- Les hôtels, catégories, chambres, capacités et tarifs sont renseignés.
- Le demandeur peut fournir les coordonnées du client bénéficiaire.
- Les règles d'arrhes sont disponibles. Aucune chambre n'est supposée disponible avant le contrôle effectué dans le scénario.

## Scénario nominal

Le scénario décrit une réservation réalisée plus de 8 jours avant l'arrivée, avec paiement réussi des arrhes.

1. Le demandeur indique l'hôtel souhaité, les dates d'arrivée et de départ, le nombre d'occupants et éventuellement une catégorie.
2. Le système contrôle les dates et le nombre d'occupants, puis présente les chambres de capacité suffisante disponibles sur toutes les nuits, avec le montant d'hébergement calculé selon le tarif et les occupants.
3. Le demandeur choisit une chambre et renseigne ou confirme l'identité et les coordonnées du client ; l'agent indique le client pour lequel il agit.
4. Le système récapitule chambre, période, occupants, prix, conditions d'annulation et arrhes exigées d'au moins 10 % du montant d'hébergement, selon l'hypothèse indiquée dans l'index.
5. Le demandeur accepte. Le système revérifie la disponibilité et enregistre atomiquement une réservation « en attente d'arrhes », qui bloque les nuits concernées, avec sa référence et son échéance à J-8.
6. Le demandeur effectue le versement requis via le service de paiement ; le système reçoit et vérifie la confirmation de l'encaissement.
7. Le système rattache le paiement à la réservation, vérifie que le montant atteint les arrhes exigées et passe la réservation à « confirmée ».
8. Le système présente la référence, le récapitulatif et la confirmation au demandeur.

## Alternatives et exceptions

- **2a — Dates ou occupants invalides :** le système explique l'erreur (départ non postérieur à l'arrivée, nombre non positif, capacité insuffisante). Le demandeur corrige à l'étape 1 ; aucune réservation n'est créée.
- **2b — Aucune chambre disponible :** le système indique l'absence de résultat. Le demandeur change ses critères à l'étape 1 ou abandonne sans réservation.
- **3a — Informations client incomplètes :** le système demande les données manquantes et reste à l'étape 3.
- **4a — Arrivée dans 8 jours ou moins :** les arrhes ne sont pas exigées par l'énoncé. Après acceptation, le contrôle et l'enregistrement atomiques de l'étape 5 créent directement une réservation confirmée ; les étapes 6 et 7 sont omises, puis reprise à l'étape 8.
- **5a — Chambre réservée entre-temps :** l'enregistrement est refusé sans créer de doublon ni déclencher de paiement. Le système actualise les disponibilités et revient à l'étape 2.
- **6a — Paiement refusé ou abandonné :** la réservation reste en attente. Le demandeur peut réessayer avant J-8. Sans encaissement suffisant à cette échéance, le cas automatique annule la réservation et libère les nuits.
- **6b — Résultat du paiement inconnu :** le système conserve un état à rapprocher et vérifie la transaction auprès du service avant toute nouvelle demande de paiement, afin d'éviter un double encaissement.
- **7a — Montant encaissé insuffisant :** la réservation n'est pas confirmée ; le système indique le complément requis. Elle reste soumise à l'échéance de J-8.
- **7b — Annulation à J-8 déjà effectuée :** un retour de paiement tardif ne réactive pas automatiquement la réservation. Le système signale le paiement à régulariser, notamment par remboursement, et conserve les nuits déjà réattribuées.

## Postconditions

**Succès :** une réservation confirmée et référencée associe exactement une chambre, une période et un nombre d'occupants compatible avec la capacité. Aucun chevauchement de nuits n'existe. Pour une réservation anticipée, les arrhes exigées ont été encaissées et rattachées une seule fois.

**Attente :** une réservation anticipée sans arrhes suffisantes conserve son statut et son échéance ; elle ne doit pas être présentée comme confirmée.

**Échec avant enregistrement :** aucune réservation ni aucun paiement n'est créé.
