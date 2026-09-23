# UC02 — Facturer le départ

## Acteurs

**Acteur principal :** réceptionniste.

**Acteur secondaire :** service de paiement, si un solde positif doit être encaissé.

**Bénéficiaire :** client qui reçoit la facture de son séjour.

**Objectif :** établir une facture complète, tenir compte des arrhes déjà versées et solder le séjour.

## Préconditions

- Un séjour existe et l'arrivée a été enregistrée avec la chambre, les occupants et le relevé téléphonique initial.
- Les tarifs applicables à l'hébergement et aux prestations, ainsi que la règle de taxe de séjour, sont accessibles.
- Les consommations et les paiements déjà enregistrés sont rattachés au séjour.
- Le séjour n'est pas déjà clôturé et soldé.

## Scénario nominal

Le scénario décrit un départ avec un solde positif et un paiement réussi.

1. La réceptionniste retrouve le séjour du client et vérifie sa chambre, la période réellement facturée et le nombre d'occupants.
2. Elle vérifie les consommations de restaurant et de bar, complète les éventuels éléments manquants et saisit le relevé téléphonique final.
3. Le système contrôle les données et calcule le détail : hébergement selon la période, le tarif et le nombre d'occupants ; prestations consommées, dont le téléphone selon l'écart des relevés et le tarif ; taxe de séjour selon la règle applicable.
4. Le système présente le détail et le solde : **total du séjour - arrhes et autres paiements déjà encaissés**. La réceptionniste vérifie le récapitulatif avec le client.
5. La réceptionniste valide ; le système établit une facture identifiée et mémorise son état « à régler ».
6. La réceptionniste déclenche l'encaissement du solde par le service de paiement externe ; le système reçoit et vérifie son résultat.
7. Après confirmation du paiement, le système rattache la transaction une seule fois, marque la facture comme payée et clôture le séjour en enregistrant le départ.
8. La réceptionniste remet au client la facture acquittée produite par le système.

## Alternatives et exceptions

- **1a — Séjour introuvable :** la réceptionniste corrige la référence ; aucune facture n'est créée tant que le séjour n'est pas identifié.
- **1b — Séjour déjà soldé :** le système propose de consulter ou rééditer la facture existante ; aucun nouvel encaissement n'est lancé.
- **2a — Consommation manquante ou contestée :** la réceptionniste vérifie auprès du service concerné et corrige les données avant de reprendre l'étape 3.
- **3a — Relevé final inférieur au relevé initial, tarif ou règle de taxe absent :** le calcul est bloqué ; les données sont vérifiées ou complétées, puis l'étape 3 est reprise. Le système n'invente ni tarif ni taxe.
- **4a — Solde nul :** après validation de l'étape 5, aucun paiement externe n'est demandé. Le système marque la facture payée à partir des versements existants et reprend à l'étape 7 pour clôturer le séjour, puis à l'étape 8.
- **4b — Solde négatif :** un trop-perçu est détecté. L'énoncé ne fixe pas sa procédure de remboursement au départ : la réceptionniste sollicite sa régularisation selon la procédure du gérant, et le système ne présente pas le dossier comme soldé avant résolution.
- **4c — Erreur dans le récapitulatif :** retour à l'étape 1 ou 2 selon la donnée à corriger, puis nouveau calcul avant émission.
- **6a — Paiement refusé :** la facture reste impayée et le séjour non soldé. La réceptionniste propose une nouvelle tentative via le service externe ; reprise à l'étape 6 si le client accepte.
- **6b — Service indisponible ou réponse incertaine :** la facture reste en attente. Le système rapproche d'abord la transaction avec le prestataire avant de relancer, pour éviter un double débit ; reprise à l'étape 7 si le paiement est confirmé.

## Postconditions

**Succès :** une facture détaillée est conservée, les arrhes sont déduites une seule fois, le paiement éventuel est confirmé et tracé, le solde est nul et le séjour est clôturé.

**Paiement en échec ou incertain :** la facture conserve son état impayé ou en attente ; aucun acquittement ni clôture pour solde réglé n'est produit.

**Erreur avant émission :** les données restent à corriger ; aucune facture définitive ni demande de paiement n'est créée.

## Hypothèses à valider

Les règles de taxe de séjour, le tarif téléphonique, les changements d'occupants, les départs anticipés et la régularisation des trop-perçus ne sont pas chiffrés dans le sujet. Les scénarios utilisent les paramètres métier fournis, sans leur attribuer de valeurs arbitraires.
