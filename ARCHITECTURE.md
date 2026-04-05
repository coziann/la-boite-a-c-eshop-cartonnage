# Architecture technique — Marketplace Cartonnage
**Version** : 1.0  
**Date** : Avril 2026  
**Statut** : En cours

---

## 1. Stack retenue

| Couche | Technologie | Version cible |
|---|---|---|
| Framework fullstack | Next.js (App Router) | 15.x |
| CMS & interface admin | Payload CMS | 3.x |
| Base de données | PostgreSQL (Neon) | 16.x |
| Stockage fichiers PDF | Cloudflare R2 | — |
| Paiement carte | Stripe | API v2 |
| Paiement PayPal | PayPal REST SDK | v2 |
| Emails transactionnels | Resend + React Email | — |
| Déploiement | Vercel | — |
| Authentification | Payload Auth (intégré) | — |
| Langage | TypeScript | 5.x |

---

## 2. Architecture générale

```
┌──────────────────────────────────────────────────────────┐
│                         Vercel                           │
│                                                          │
│  ┌─────────────────────┐   ┌──────────────────────────┐  │
│  │   Next.js (vitrine) │   │   Payload CMS (/gestion) │  │
│  │   - Catalogue       │   │   - Gestion tutoriels    │  │
│  │   - Fiches produits │   │   - Gestion commandes    │  │
│  │   - Blog            │   │   - Blog admin           │  │
│  │   - Espace client   │   │   - Dashboard ventes     │  │
│  │   - Tunnel d'achat  │   │   - File d'attente       │  │
│  └──────────┬──────────┘   └─────────────┬────────────┘  │
│             │                            │               │
│             └────────────┬───────────────┘               │
│                          │ API Routes Next.js            │
└──────────────────────────┼───────────────────────────────┘
                           │
           ┌───────────────┼──────────────────┐
           │               │                  │
      PostgreSQL       Cloudflare R2       Resend
      (Neon)           (PDFs privés)       (emails)
           │
    Stripe Webhooks
    PayPal Webhooks
```

### Principe clé : monorepo Next.js + Payload

Payload CMS 3 s'intègre directement dans une application Next.js. Le projet est un seul dépôt, un seul déploiement Vercel. L'interface admin Payload est exposée sur une route non-standard (ex : `/gestion`).

---

## 3. Structure du projet

```
laboiteac/
├── src/
│   ├── app/
│   │   ├── (vitrine)/          # Routes publiques (catalogue, blog, compte)
│   │   │   ├── page.tsx        # Page d'accueil
│   │   │   ├── tutoriels/      # Catalogue + fiches produits
│   │   │   ├── blog/           # Articles de blog
│   │   │   ├── compte/         # Espace client (mes achats)
│   │   │   └── commande/       # Tunnel d'achat + confirmation
│   │   ├── (payload)/          # Routes Payload CMS (admin)
│   │   │   └── gestion/        # Interface admin (URL non-standard)
│   │   └── api/
│   │       ├── stripe/         # Webhooks Stripe
│   │       ├── paypal/         # Webhooks PayPal
│   │       └── download/       # Génération des liens signés R2
│   ├── collections/            # Schémas de données Payload
│   │   ├── Tutoriels.ts
│   │   ├── Commandes.ts
│   │   ├── Clientes.ts
│   │   ├── Articles.ts
│   │   └── Categories.ts
│   ├── hooks/                  # Hooks Payload (logique métier)
│   │   ├── onCommandeValidee.ts
│   │   └── onPaiementConfirme.ts
│   ├── emails/                 # Templates React Email
│   │   ├── LivraisonPDF.tsx
│   │   ├── ConfirmationCommande.tsx
│   │   └── ConfirmationCheque.tsx
│   └── lib/
│       ├── stripe.ts
│       ├── paypal.ts
│       ├── r2.ts               # Client Cloudflare R2 + génération liens signés
│       └── resend.ts
├── payload.config.ts
├── next.config.ts
└── .env.local
```

---

## 4. Modèle de données

### Collection `Tutoriels`

| Champ | Type | Notes |
|---|---|---|
| titre | Text | Requis |
| slug | Text | Auto-généré, unique |
| description | RichText | Lexical WYSIWYG |
| prix | Number | En centimes (ex : 1200 = 12,00 €) |
| niveau | Select | Débutant / Intermédiaire / Avancé |
| categorie | Relationship | → Categories |
| photos | Array of Upload | Images du résultat final |
| contenuPDF | Text | Description du contenu du PDF |
| materiaux | Text | Matériaux nécessaires |
| fichierPDF | Upload | Stocké dans R2 (privé) |
| publie | Checkbox | Dépublication sans suppression |
| createdAt | Date | Auto |

### Collection `Commandes`

| Champ | Type | Notes |
|---|---|---|
| numero | Text | Généré (ex : CMD-2026-0001) |
| cliente | Relationship | → Clientes |
| tutoriels | Array of Relationship | → Tutoriels |
| montantTotal | Number | En centimes |
| modePaiement | Select | carte / paypal / cheque |
| statut | Select | Voir machine à états ci-dessous |
| stripePaymentIntentId | Text | Si carte |
| paypalOrderId | Text | Si PayPal |
| notesInternes | Textarea | Date réception chèque, numéro chèque… |
| emailLivraisonEnvoye | Checkbox | Traçabilité |
| createdAt | Date | Auto |

**Machine à états — statut commande :**

```
carte/paypal :
  en_attente_paiement → paiement_confirme → pdf_envoye

cheque :
  en_attente_reception_cheque
    → cheque_recu_en_cours_validation
      → commande_validee (déclenche envoi PDF)
```

### Collection `Clientes`

| Champ | Type | Notes |
|---|---|---|
| email | Email | Unique, identifiant de connexion |
| prenom | Text | |
| motDePasse | Password | Hashé bcrypt par Payload |
| commandes | Relationship | → Commandes (relation inverse) |
| createdAt | Date | Auto |

### Collection `Articles` (blog)

| Champ | Type | Notes |
|---|---|---|
| titre | Text | |
| slug | Text | Auto-généré |
| contenu | RichText | Lexical WYSIWYG |
| photos | Array of Upload | |
| tutorielLie | Relationship | → Tutoriels (optionnel) |
| publie | Checkbox | |
| createdAt | Date | Auto |

### Collection `Categories`

| Champ | Type | Notes |
|---|---|---|
| nom | Text | Ex : Boîtes, Cadres, Albums |
| slug | Text | |

---

## 5. Sécurité des fichiers PDF

Les PDFs ne sont **jamais** exposés publiquement.

**Flux de téléchargement sécurisé :**

```
Cliente clique "Télécharger"
        │
        ▼
GET /api/download?commandeId=xxx&tutorielId=yyy
        │
        ▼
Vérification session cliente (Payload Auth)
        │
        ▼
Vérification que la commande appartient à la cliente
ET que le statut = "paiement_confirme" ou "commande_validee"
        │
        ▼
Génération d'un lien signé Cloudflare R2 (TTL : 1 heure)
        │
        ▼
Redirection 302 vers le lien signé → téléchargement direct
```

- Les fichiers PDF sont uploadés dans un bucket R2 **privé** (accès public bloqué)
- Les liens signés expirent après 1 heure mais sont **re-générables** depuis "Mes achats" sans limite de durée
- L'URL `/api/download` vérifie à chaque appel les droits en base — impossible de contourner par partage de lien

---

## 6. Flux de paiement

### Carte bancaire (Stripe)

```
1. Cliente valide le panier
2. Création d'un PaymentIntent Stripe côté serveur
3. Affichage du formulaire Stripe Elements (PCI-DSS délégué)
4. Paiement confirmé par Stripe
5. Webhook Stripe → POST /api/stripe
6. Vérification signature webhook (secret Stripe)
7. Mise à jour commande : statut → "paiement_confirme"
8. Hook Payload → envoi email Resend avec lien téléchargement
9. Redirection vers page de confirmation
```

### PayPal

```
1. Création d'un Order PayPal côté serveur
2. Redirection vers PayPal (ou PayPal JS SDK)
3. Retour sur le site après approbation
4. Capture du paiement côté serveur
5. Webhook PayPal → POST /api/paypal (+ vérification)
6. Mise à jour commande : statut → "paiement_confirme"
7. Hook → envoi email PDF
```

### Chèque

```
1. Cliente valide la commande "paiement par chèque"
2. Commande créée : statut → "en_attente_reception_cheque"
3. Email automatique avec instructions (libellé, adresse, n° commande)
4. Page de confirmation avec les 3 étapes illustrées

--- côté admin ---

5. Admin reçoit le chèque → passe statut à "cheque_recu_en_cours_validation"
6. Admin valide en 1 clic → statut → "commande_validee"
7. Hook Payload → envoi automatique email avec lien PDF
```

---

## 7. Emails transactionnels

Tous les emails sont construits avec **React Email** et envoyés via **Resend**.

| Email | Déclencheur | Contenu |
|---|---|---|
| Confirmation commande (carte/PayPal) | Webhook paiement confirmé | Récapitulatif, lien vers "Mes achats" |
| Livraison PDF (carte/PayPal) | Même déclencheur | Lien signé + image fléchée "où cliquer" |
| Confirmation commande chèque | Création commande | Instructions d'envoi du chèque |
| Livraison PDF chèque | Admin valide manuellement | Lien signé + accès "Mes achats" |
| Renvoi manuel PDF | Admin clique "Renvoyer" | Même template que livraison |

Les templates incluent le logo de la créatrice et sont personnalisables depuis l'admin (message d'introduction).

---

## 8. Interface admin — points clés

L'admin est accessible sur `/gestion` (URL non-standard, non-devinable).

**Vues spécifiques créées dans Payload :**

- **File d'attente chèques** : vue filtrée sur commandes statut `cheque_recu_en_cours_validation` et `en_attente_reception_cheque`, avec compteur d'alerte dans la navigation
- **Dashboard ventes** : vue custom Payload avec CA jour/semaine/mois, tutoriels populaires, répartition paiements (requêtes PostgreSQL directes)
- **Base clients** : liste clientes avec commandes associées, email cliquable

**Libellés en français** dans toute l'interface admin (Payload permet de surcharger tous les labels).

---

## 9. Déploiement & infrastructure

### Environnements

| Environnement | URL | Usage |
|---|---|---|
| Production | laboiteac.fr | Clients réels |
| Preview | *.vercel.app | Validation avant mise en prod |
| Local | localhost:3000 | Développement |

### Variables d'environnement requises

```bash
# Base de données
DATABASE_URI=postgresql://...

# Payload
PAYLOAD_SECRET=...

# Cloudflare R2
R2_ACCOUNT_ID=...
R2_ACCESS_KEY_ID=...
R2_SECRET_ACCESS_KEY=...
R2_BUCKET_NAME=...

# Stripe
STRIPE_SECRET_KEY=...
STRIPE_WEBHOOK_SECRET=...
STRIPE_PUBLISHABLE_KEY=...

# PayPal
PAYPAL_CLIENT_ID=...
PAYPAL_CLIENT_SECRET=...
PAYPAL_WEBHOOK_ID=...

# Resend
RESEND_API_KEY=...
RESEND_FROM_EMAIL=contact@laboiteac.fr

# App
NEXT_PUBLIC_APP_URL=https://laboiteac.fr
```

### Sauvegardes

- **Base de données** : sauvegardes automatiques quotidiennes via Neon (point-in-time recovery 7 jours)
- **PDFs** : Cloudflare R2 réplication automatique (durabilité 99,999999999%)
- **Export manuel** : l'admin peut exporter la liste des commandes et clientes en CSV depuis Payload

---

## 10. Conformité RGPD & légal

- Données collectées : email, prénom, historique commandes uniquement
- Stockage en Europe (région UE sur Vercel + Neon EU)
- Droit à la suppression : action dans l'admin Payload → anonymisation de la fiche cliente
- Bandeau cookies : intégré côté Next.js (aucun cookie tiers hormis Stripe et PayPal sur leurs pages)
- Mentions légales, CGV, politique de confidentialité : pages statiques Next.js

---

## 11. Roadmap technique

### MVP
- [ ] Setup Next.js 15 + Payload CMS 3 + PostgreSQL (Neon)
- [ ] Collections Tutoriels, Commandes, Clientes, Categories
- [ ] Catalogue public + fiches produits
- [ ] Tunnel d'achat Stripe (carte uniquement)
- [ ] Webhooks Stripe + envoi email PDF (Resend)
- [ ] Espace client "Mes achats" + liens signés R2
- [ ] Admin : gestion tutoriels + liste commandes
- [ ] Page confirmation post-achat avec 3 étapes
- [ ] HTTPS, sécurité PDF, auth Payload

### V1
- [ ] Paiement PayPal
- [ ] Workflow chèque complet + file d'attente admin
- [ ] Blog avec Lexical WYSIWYG + lien tutoriel
- [ ] Partage réseaux sociaux (Facebook, Pinterest)
- [ ] Dashboard de ventes admin
- [ ] Gestion des catégories
- [ ] Email personnalisable (logo, message)
- [ ] Mentions légales, CGV, bandeau RGPD

### V2
- [ ] Newsletter
- [ ] Filtres et recherche catalogue
- [ ] Connexion Google (OAuth)
- [ ] Commentaires articles
- [ ] Galerie réalisations élèves
- [ ] Avis sur tutoriels
- [ ] Codes promo
