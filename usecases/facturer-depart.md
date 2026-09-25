# Fiche — Facturer le départ

**Acteur principal :** le réceptionniste.

**Acteur secondaire :** le service de paiement.

## Précondition

Le client est arrivé à l'hôtel : son arrivée a été enregistrée, avec le relevé du compteur téléphonique au début du séjour.

## Scénario nominal

1. Le réceptionniste recherche le séjour du client (nom ou numéro de chambre).
2. Le système affiche le séjour : chambre, dates, nombre de personnes et consommations déjà enregistrées (restaurant, bar).
3. Le réceptionniste saisit le relevé du compteur téléphonique à la fin du séjour.
4. Le système calcule la facture : prix de la chambre selon le nombre de personnes, consommations, téléphone, et taxe de séjour.
5. Le système retire les arrhes déjà payées et affiche le reste à payer.
6. Le réceptionniste vérifie la facture avec le client et la valide.
7. Le client paie le reste par le service de paiement (cas *Encaisser un paiement*).
8. Le système marque la facture comme payée et enregistre le départ du client.
9. Le réceptionniste remet la facture au client.

## Alternatives

- **1a — Séjour introuvable :** le réceptionniste vérifie le nom ou le numéro de chambre et recommence la recherche.
- **2a — Une consommation manque ou est fausse :** le réceptionniste la corrige avant de passer à l'étape 3.
- **5a — Rien à payer (tout est déjà couvert par les arrhes) :** on passe directement à l'étape 8.
- **6a — Le client conteste la facture :** le réceptionniste corrige l'erreur, puis le système recalcule (retour à l'étape 4).
- **7a — Le paiement est refusé :** la facture reste « non payée ». Le client essaie un autre moyen de paiement (retour à l'étape 7).

## Postcondition

La facture est enregistrée et payée, et le départ du client est enregistré. La chambre redevient libre.
