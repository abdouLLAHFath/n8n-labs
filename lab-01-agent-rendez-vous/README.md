# Lab 01 – Agent IA : questions, prise de rendez-vous et résumé par e-mail

## Contexte
**Agent Nova**, l'agence marocaine d'organisation de mariages et de conférences, veut un assistant IA sur son site. Cet assistant :
- répond aux questions des clients ;
- enregistre chaque client dans Google Sheets ;
- prend un rendez-vous dans Google Calendar ;
- envoie un résumé de l'échange par e-mail au client.

### Les tarifs (pour répondre aux questions)
| Produit | Règle |
|---------|-------|
| Mariage | 5 000 MAD par table de 10 personnes (on arrondit à la table supérieure) |
| Conférence | À partir de 10 personnes : 1 000 MAD par personne et par jour |

## Objectifs
- Créer un agent IA réutilisable dans n8n et l'enrichir étape par étape
- Connecter l'agent à Google Sheets, Google Calendar et Gmail
- Appeler cet agent depuis un workflow
- Publier le workflow et intégrer le chatbot sur un site Replit

## Architecture
```
Site Replit (chatbot) → Workflow n8n → Agent IA Nova
                                         ├─ Google Sheets (fiche client)
                                         ├─ Google Calendar (rendez-vous)
                                         └─ Gmail (résumé au client)
```

## Préparation (5 min)
1. Crée un **Google Sheet** nommé `Clients Agent Nova`, avec ces colonnes sur la première ligne :
   `Date | Nom | Téléphone | E-mail | Besoin | Rendez-vous`
2. Vérifie que tes comptes **Google Sheets**, **Google Calendar** et **Gmail** sont connectés dans n8n (menu *Credentials*).

---

## Partie A – Construire l'agent IA

### Étape 1 – Créer l'agent qui répond aux questions
Dans n8n, crée un nouvel agent IA et donne-lui ce prompt :

```
Tu es Nova, l'assistante d'Agent Nova, une agence marocaine qui organise des mariages et des conférences.
Tu réponds aux questions des clients de façon simple, chaleureuse et courte, dans leur langue.

Nos prix :
- Mariage : 5 000 dirhams par table de 10 personnes. Si le nombre ne tombe pas juste, on ajoute une table (13 personnes = 2 tables = 10 000 dirhams).
- Conférence : à partir de 10 personnes, 1 000 dirhams par personne et par jour.

Si tu ne connais pas la réponse, tu proposes au client un rendez-vous avec notre équipe.
```

**✅ Test :** demande à l'agent « Combien coûte un mariage pour 35 personnes ? ». Réponse attendue : 4 tables = **20 000 MAD**.

### Étape 2 – Demander le nom, l'e-mail et le téléphone au début
Ajoute ce prompt à l'agent :

```
Avant de commencer la discussion, demande poliment au client son nom, son e-mail et son numéro de téléphone.
Tu ne réponds à ses questions qu'une fois que tu as ces trois informations. Vérifie que l'e-mail et le téléphone semblent valides.
```

**✅ Test :** écris « Bonjour, quels sont vos prix ? ». L'agent doit d'abord demander ton nom, ton e-mail et ton téléphone.

### Étape 3 – Enregistrer le client dans Google Sheets
Ajoute ce prompt :

```
Donne à l'agent un outil Google Sheets.
Dès que le client a donné son nom, son e-mail et son téléphone, l'agent ajoute une ligne dans le Google Sheet "Clients Agent Nova" avec la date du jour, le nom, le téléphone, l'e-mail et le besoin du client.
```

**✅ Test :** donne tes informations, puis vérifie qu'une nouvelle ligne apparaît dans le Google Sheet.

### Étape 4 – Prendre un rendez-vous dans Google Calendar
Ajoute ce prompt :

```
Donne à l'agent un outil Google Calendar.
Quand le client a eu ses réponses, l'agent lui propose un rendez-vous de 30 minutes avec notre équipe et lui demande de choisir le jour et l'heure (du lundi au vendredi, de 9h à 18h).
L'agent vérifie que le créneau est libre, crée le rendez-vous dans Google Calendar avec le nom et le téléphone du client, puis met à jour la colonne "Rendez-vous" de sa ligne dans le Google Sheet.
```

**✅ Test :** choisis un créneau et vérifie que le rendez-vous apparaît dans ton Google Calendar.

### Étape 5 – Envoyer le résumé par e-mail
Ajoute ce prompt :

```
Donne à l'agent un outil Gmail.
À la fin de la discussion, l'agent demande au client à quelle adresse e-mail il veut recevoir le résumé (celle qu'il a donnée au début ou une autre).
Il envoie ensuite un e-mail clair et cordial avec le résumé de l'échange : le besoin du client, les réponses principales, la date et l'heure du rendez-vous. L'e-mail est signé "L'équipe Agent Nova".
```

**✅ Test :** termine une discussion et vérifie que tu reçois le résumé à l'adresse choisie.

---

## Partie B – Le workflow et la mise en ligne

### Étape 6 – Créer le workflow qui utilise l'agent
Crée un nouveau workflow et colle ce prompt dans l'AI Workflow Builder :

```
Crée un workflow avec un chatbot pour Agent Nova.
Chaque message du client est envoyé à l'agent IA "Nova" que j'ai déjà créé : utilise cet agent, ne crée pas un nouvel agent.
Le workflow garde en mémoire toute la conversation avec chaque client.
```

**✅ Test :** ouvre le chat du workflow et refais une discussion complète, du nom jusqu'à l'e-mail de résumé.

### Étape 7 – Publier et récupérer le code du chatbot
Colle ce prompt dans l'AI Workflow Builder :

```
Rends le chatbot de ce workflow accessible publiquement pour que je puisse l'intégrer sur mon site web.
Génère-moi le code HTML complet à copier-coller dans mon site pour afficher le chatbot Agent Nova (une bulle de chat en bas à droite, avec le message d'accueil "Bonjour 👋 Je suis Nova. Pour commencer, quel est votre nom ?").
```

Ensuite :
1. Clique sur **Publish** pour mettre le workflow en ligne.
2. **Copie le code HTML** généré par n8n.

**✅ Test :** ouvre l'URL publique du chat et vérifie que le chatbot répond.

### Étape 8 – Ajouter le chatbot sur le site (Replit)
Ouvre ton projet sur **Replit** et colle ce prompt dans l'Agent Replit, en remplaçant la dernière ligne par le code copié depuis n8n :

```
Ajoute à mon site le chatbot d'Agent Nova avec le code HTML ci-dessous, sans le modifier.
Le chatbot doit apparaître sur toutes les pages, en bas à droite.
Ajoute aussi un bouton "Prendre rendez-vous" sur la page d'accueil qui ouvre le chatbot.

[colle ici le code HTML copié depuis n8n]
```

**✅ Test final :** sur ton site Replit, fais un parcours complet et vérifie ces quatre points :
- [ ] le chatbot demande le nom, l'e-mail et le téléphone ;
- [ ] une ligne est ajoutée dans le Google Sheet ;
- [ ] le rendez-vous apparaît dans Google Calendar ;
- [ ] le résumé arrive à l'adresse e-mail choisie.

---

## Pour aller plus loin
- Envoyer un rappel par e-mail au client la veille du rendez-vous
- Prévenir l'équipe sur WhatsApp ou Slack à chaque nouveau rendez-vous
- Enchaîner avec le Lab 02 pour envoyer aussi un devis PDF
