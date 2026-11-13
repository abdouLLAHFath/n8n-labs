# Lab 01 – Agent Nova : du chatbot au devis par e-mail

## Contexte
**Agent Nova** est une agence marocaine qui organise des mariages et des conférences. Elle veut automatiser ses demandes de devis : un client discute avec un chatbot, puis il reçoit son devis PDF par e-mail.

### Les tarifs
| Produit | Règle | Exemple |
|---------|-------|---------|
| Mariage | 5 000 MAD par table de 10 personnes (on arrondit à la table supérieure) | 13 personnes = 2 tables = **10 000 MAD** |
| Conférence | À partir de 10 personnes : 1 000 MAD par personne et par jour | 20 personnes × 2 jours = **40 000 MAD** |

## Objectifs
- Créer un chatbot avec un agent IA et une mémoire
- Enchaîner deux agents : le premier transmet ses informations au second
- Générer un document PDF
- Envoyer un e-mail automatique avec une pièce jointe

## Architecture
```
Chat client → Agent 1 (chatbot) → Agent 2 (devis PDF) → Envoi de l'e-mail
```

On construit le workflow **en 3 étapes**, avec un prompt par étape dans l'**AI Workflow Builder** de n8n. Teste chaque étape avant de passer à la suivante.

---

## Étape 1 – Le chatbot (Agent 1)

Crée un nouveau workflow, ouvre l'AI Workflow Builder et colle ce prompt :

```
Crée-moi un workflow pour "Agent Nova", une agence qui organise des mariages et des conférences au Maroc.

Nos prix :
- Mariage : 5 000 dirhams par table de 10 personnes. Si le nombre ne tombe pas juste, on ajoute une table (13 personnes = 2 tables = 10 000 dirhams).
- Conférence : à partir de 10 personnes, 1 000 dirhams par personne et par jour (2 jours = prix multiplié par 2).

Pour commencer, crée un chatbot avec un agent IA qui a une mémoire de la conversation.
Il discute avec le client de façon simple et sympathique pour comprendre son besoin. Il lui demande son nom, son téléphone, son e-mail, le type d'événement (mariage ou conférence), le nombre de personnes, le nombre de jours, la date et le lieu.
Quand il a toutes les informations, il fait un petit résumé, demande au client de confirmer et lui dit que son devis arrive par e-mail.
```

**✅ Test :** ouvre le chat et joue le client : « Bonjour, je prépare mon mariage pour 25 personnes. »
Le chatbot doit poser ses questions, faire le résumé et demander ta confirmation.

---

## Étape 2 – Le devis PDF (Agent 2)

Dans le même workflow, colle ce prompt :

```
Ajoute à ce workflow un deuxième agent IA qui crée le devis.
Quand le client a confirmé, le deuxième agent reçoit les informations du premier agent, calcule le prix avec nos tarifs et crée un joli devis en PDF au nom d'Agent Nova (numéro de devis, date, informations du client, détail de l'événement, prix et total).
Il prépare aussi un petit message de remerciement pour le client.
```

**✅ Test :** termine une conversation et vérifie le PDF qui est généré.
- Mariage pour 25 personnes : 3 tables = **15 000 MAD**
- Conférence pour 15 personnes sur 2 jours : **30 000 MAD**

---

## Étape 3 – L'envoi de l'e-mail

Colle ce dernier prompt :

```
Ajoute une dernière étape à ce workflow : une fois le devis prêt, le workflow envoie automatiquement un e-mail au client avec Gmail, avec le message de remerciement et le devis PDF en pièce jointe.
```

**✅ Test :** fais une conversation complète avec ta propre adresse e-mail et vérifie que tu reçois le devis.

---

## Résultat attendu
Un workflow complet : le client discute avec le chatbot, confirme, puis reçoit son devis PDF par e-mail en quelques secondes.

## Pour aller plus loin
- Enregistrer chaque demande dans un Google Sheet
- Ajouter la TVA (20 %) sur le devis
- Prévenir l'équipe commerciale sur WhatsApp ou Slack à chaque nouveau devis
