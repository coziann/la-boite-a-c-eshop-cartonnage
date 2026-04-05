# PRD — Marketplace Cartonnage
**Version** : 1.0  
**Date** : Avril 2026  
**Statut** : En cours

---

## 1. Objectif du produit

### 1.1 Vision
Permettre à une créatrice de cartonnage auto-entrepreneur de vendre ses tutoriels PDF en ligne et de partager ses réalisations via un blog, sans avoir besoin de compétences techniques pour gérer la plateforme au quotidien.

### 1.2 Problème résolu
Aujourd'hui, la créatrice n'a pas de canal de vente digitale autonome. Elle dépend de l'envoi manuel de fichiers, de virements, ou de plateformes tierces non adaptées à son activité. Les clientes, peu habituées au digital, ont besoin d'un parcours extrêmement guidé pour acheter et récupérer leurs fichiers en toute confiance.

### 1.3 Objectifs mesurables (à 12 mois)
- Atteindre une cinquantaine de clientes actives
- Proposer un catalogue entre 50 et 200 tutoriels PDF
- 100 % des livraisons de PDF automatisées (zéro envoi manuel pour les paiements carte/PayPal)
- Temps moyen de prise en main de l'admin < 30 minutes pour un profil non technique

### 1.4 Personas

**La cliente** — Femme, 50-70 ans, passionnée de loisirs créatifs, peu à l'aise avec le digital. Elle achète depuis un ordinateur ou un smartphone. Elle a besoin d'être rassurée à chaque étape et de comprendre exactement comment récupérer ce qu'elle a acheté.

**La créatrice / admin** — Auto-entrepreneur, experte en cartonnage, novice en outils digitaux. Elle doit pouvoir ajouter un tutoriel, publier un article de blog et consulter ses ventes sans aide extérieure.

---

## 2. Périmètre du produit

### 2.1 Dans le périmètre

- Marketplace de vente de tutoriels PDF
- Blog de réalisations (créatrice + élèves)
- Interface d'administration intuitive
- Paiement par carte, PayPal et chèque
- Livraison automatique des PDF
- Espace client avec historique d'achats
- Partage sur les réseaux sociaux (marketplace et admin)
- Dashboard de ventes pour l'admin

### 2.2 Hors périmètre

- Application mobile native
- Système d'abonnement mensuel / accès illimité
- Cours en vidéo ou contenu streamé
- Marketplace multi-vendeurs
- Programme d'affiliation
- Traduction multilingue
- Paiement en plusieurs fois

---

## 3. Fonctionnalités

### 3.1 Catalogue & découverte
- Page d'accueil avec mise en avant des tutoriels vedettes et dernières réalisations
- Galerie des tutoriels avec aperçu (image, titre, prix, niveau de difficulté)
- Fiche produit détaillée : description, photos du résultat final, contenu du PDF, matériaux nécessaires, niveau requis
- Filtres et recherche par thème, niveau, prix (V2)
- Mise en avant de tutoriels vedettes gérée par l'admin (V2)

### 3.2 Achat & paiement
- Tunnel d'achat simplifié en le moins d'étapes possible
- Paiement sécurisé par carte bancaire
- Paiement par PayPal
- Paiement par chèque avec instructions claires (libellé, adresse d'envoi, numéro de commande à écrire au dos)
- Livraison automatique du PDF par email après paiement carte/PayPal
- Livraison du PDF par email après validation manuelle de l'admin pour les chèques
- Le PDF n'est jamais accessible avant confirmation de paiement
- Les liens de téléchargement sont personnels, sécurisés et non partageables

### 3.3 Espace client
- Inscription et connexion par email
- Connexion via Google (V2)
- Profil minimal : email, prénom, historique de commandes
- Espace "Mes achats" : tous les PDFs achetés, téléchargeables sans limite de durée
- Statut de commande visible en temps réel

| Statut | Description |
|---|---|
| En attente de réception du chèque | Le chèque n'a pas encore été reçu |
| Chèque reçu, en cours de validation | Le chèque est entre les mains de l'admin |
| Commande validée | Le PDF est disponible au téléchargement |

### 3.4 Blog
- Articles avec photos (réalisations créatrice et élèves)
- Lien entre chaque article et le tutoriel correspondant
- Partage sur les réseaux sociaux depuis la marketplace (Facebook, Pinterest, Instagram)
- Commentaires sur les articles (V2)
- Galerie des réalisations des élèves avec soumission de photos (V2)
- Système d'avis sur les tutoriels (V2)

### 3.5 Interface admin — Gestion des produits
- Ajout d'un tutoriel : formulaire guidé (titre, description, prix, niveau, photos, upload PDF) avec prévisualisation avant publication
- Modification et dépublication sans suppression définitive
- Gestion des catégories (boîtes, cadres, albums…)

### 3.6 Interface admin — Gestion du blog
- Éditeur d'articles visuel WYSIWYG (sans code)
- Upload de photos simplifié avec recadrage intégré
- Partage direct vers Facebook et Pinterest au moment de la publication (comptes connectés une fois pour toutes dans les réglages)
- Programmation de publication à l'avance (V2)

### 3.7 Interface admin — Gestion des commandes
- Liste des commandes avec statut
- File d'attente dédiée aux commandes chèque avec compteur d'alerte
- Validation manuelle en un clic (déclenche envoi email + déblocage téléchargement)
- Champ de note interne (date de réception, numéro de chèque…)
- Renvoi manuel du PDF si besoin
- Base clients : qui a acheté quoi, email de contact

### 3.8 Interface admin — Dashboard de ventes
- Chiffre d'affaires du jour / semaine / mois
- Nombre de commandes par période
- Tutoriels les plus vendus
- Répartition des paiements (carte vs PayPal vs chèque)
- Graphiques simples et lisibles, sans jargon

### 3.9 Communication
- Email automatique de confirmation d'achat personnalisable (logo, message)
- Newsletter simple vers les clientes (V2)
- Gestion des codes promo et réductions (V2)

### 3.10 Onboarding client

**Après achat par carte ou PayPal — page de confirmation :**
1. 📧 Vérifiez vos emails — un message vient de vous être envoyé avec votre PDF
2. 👤 Connectez-vous à votre compte — votre PDF est aussi disponible dans "Mes achats"
3. 📥 Cliquez sur "Télécharger" — et c'est parti pour créer !

**Après achat par chèque — page de confirmation distincte :**
1. ✉️ Envoyez votre chèque à cette adresse, avec votre numéro de commande au dos
2. ⏳ Nous validons dès réception — vous recevrez un email dès que votre PDF est disponible
3. 📧 Guettez votre boîte mail — le lien vous sera envoyé automatiquement

**Dans l'email de livraison :**
- Image fléchée montrant exactement où cliquer pour télécharger
- Lien direct vers "Mes achats"
- Rappel : le fichier est disponible à tout moment dans l'espace personnel

**Dans l'espace "Mes achats" :**
- Bandeau d'aide contextuel à la première visite
- Mini FAQ : "Je ne trouve pas mon email", "Mon téléchargement ne fonctionne pas", "J'ai changé d'appareil"

---

## 4. Exigences fonctionnelles

### 4.1 Performances
- Temps de chargement des pages < 3 secondes sur une connexion standard (4G ou fibre)
- Le catalogue doit supporter jusqu'à 200 tutoriels sans dégradation visible
- La plateforme doit supporter jusqu'à 200 utilisateurs simultanés sans dégradation (marge confortable au-dessus de la cible de 50 clientes actives)
- Les emails de livraison de PDF doivent être envoyés dans les 60 secondes suivant la validation du paiement

### 4.2 Disponibilité
- Disponibilité cible : 99,5 % (moins de 44 heures d'indisponibilité par an)
- Sauvegardes automatiques quotidiennes des données et des fichiers PDF
- Procédure de restauration documentée en cas de panne

### 4.3 Compatibilité navigateurs & appareils
- Compatible mobile et ordinateur à égalité (responsive design obligatoire)
- Navigateurs supportés : Chrome, Firefox, Safari, Edge — dans leurs deux dernières versions majeures
- Taille minimale d'écran supportée : 320px (iPhone SE)
- Le parcours d'achat complet doit être réalisable sur mobile sans zoom ni scroll horizontal

### 4.4 Internationalisation
- Langue unique : français
- Devise unique : euro (€)
- Conformité fiscale française (TVA auto-entrepreneur)

---

## 5. Exigences non-fonctionnelles

### 5.1 Sécurité

**Paiement**
- Aucune donnée de carte bancaire ne transite ou n'est stockée sur les serveurs de l'application — délégation totale à un prestataire certifié PCI-DSS (ex : Stripe)
- Les paiements PayPal sont gérés exclusivement via l'API officielle PayPal

**Accès aux fichiers**
- Les PDF ne sont accessibles qu'après confirmation de paiement validée
- Les liens de téléchargement sont signés, personnels, et ont une durée d'expiration (ex : 24h par lien, mais re-générables depuis "Mes achats")
- Les fichiers PDF sont stockés dans un espace privé, non indexable et non accessible sans authentification

**Authentification**
- Mots de passe stockés sous forme hashée (bcrypt ou équivalent) — jamais en clair
- Protection contre les attaques par force brute (blocage temporaire après plusieurs tentatives échouées)
- Sessions sécurisées avec expiration automatique après inactivité

**Interface admin**
- Accès admin protégé par identifiant + mot de passe fort
- L'interface admin n'est pas accessible depuis une URL devinable (pas de /admin standard)
- Déconnexion automatique après inactivité

**Infrastructure**
- Connexion chiffrée HTTPS obligatoire sur toutes les pages
- Certificat SSL valide et renouvelé automatiquement
- En-têtes de sécurité HTTP configurés (Content Security Policy, X-Frame-Options, etc.)

### 5.2 Confidentialité & RGPD

- Collecte minimale des données personnelles (email, prénom, historique d'achats uniquement)
- Bandeau de consentement aux cookies conforme RGPD dès la première visite
- Politique de confidentialité accessible depuis toutes les pages
- Droit à la suppression du compte et des données personnelles sur demande
- Aucune donnée personnelle partagée avec des tiers sauf prestataires de paiement (Stripe, PayPal) dans le cadre strict du traitement des commandes
- Les PDFs achetés ne contiennent aucune donnée personnelle de la cliente

### 5.3 Accessibilité

- Contraste des textes conforme aux recommandations WCAG 2.1 niveau AA (ratio minimum 4,5:1)
- Taille de police de base ≥ 16px pour le contenu principal
- Tous les boutons d'action ont une taille tactile ≥ 44x44px (confort mobile pour un public senior)
- Les images contiennent des textes alternatifs (attribut alt) pour les lecteurs d'écran
- Le parcours d'achat est navigable au clavier
- Pas de contenu uniquement transmis par la couleur (ex : les statuts de commande utilisent aussi un texte explicite)
- Formulaires avec labels explicites et messages d'erreur clairs en français

### 5.4 Ergonomie & UX (public novice)

- Vocabulaire simple, sans jargon technique, dans toute l'interface
- Chaque action importante est confirmée par un message visible (achat, téléchargement, envoi de formulaire)
- Pas plus de 3 clics entre la page d'accueil et l'achat d'un tutoriel
- L'interface admin doit être utilisable sans formation, avec des libellés d'action explicites ("Ajouter un tutoriel", "Valider ce chèque", "Publier l'article")
- Les erreurs sont expliquées en langage clair avec une suggestion de résolution

### 5.5 Légal & conformité (France)

- Mentions légales obligatoires (nom, statut auto-entrepreneur, SIRET, adresse, email de contact)
- Conditions Générales de Vente (CGV) conformes au droit français
- Politique de remboursement explicite : les PDF étant des biens numériques téléchargeables, le droit de rétractation de 14 jours ne s'applique pas — cette exception doit être acceptée explicitement par la cliente avant l'achat
- Facturation : une facture ou un récapitulatif de commande est envoyé automatiquement par email après chaque achat
- Gestion de la TVA adaptée au statut auto-entrepreneur (franchise en base de TVA si applicable)

### 5.6 Maintenabilité

- Le code doit être documenté pour permettre à un développeur tiers de reprendre le projet
- Les mises à jour de sécurité des dépendances doivent pouvoir être appliquées sans refonte
- Les PDFs et les données sont exportables (sauvegarde manuelle possible par l'admin)
- L'ajout d'un nouveau tutoriel ou article de blog ne nécessite aucune intervention technique

---

## 6. Roadmap

### 🟢 MVP — "Ça marche et on peut vendre"
*Objectif : valider que des clientes achètent et reçoivent leur PDF sans friction*

- Page d'accueil simple avec les tutoriels disponibles
- Fiche produit (photo, description, prix, niveau)
- Paiement par carte bancaire uniquement
- Livraison automatique du PDF par email après paiement
- Inscription / connexion par email
- Espace "Mes achats" avec téléchargement des PDFs achetés
- Ajout et modification d'un tutoriel dans l'admin
- Liste des commandes avec statut dans l'admin
- Page de confirmation post-achat avec les 3 étapes illustrées
- Email de livraison clair avec lien de téléchargement
- HTTPS, sécurité des fichiers, hash des mots de passe

### 🔵 V1 — "C'est professionnel et complet"
*Objectif : couvrir tous les modes de paiement et créer la vitrine créative*

**Paiement**
- Ajout du paiement par chèque avec instructions claires
- Ajout du paiement PayPal
- Statut de commande visible côté client
- Validation manuelle par l'admin + déclenchement automatique du PDF
- File d'attente chèques dans le dashboard avec compteur d'alerte

**Blog**
- Éditeur d'articles visuel (sans code)
- Upload de photos simplifié
- Lien entre article et tutoriel correspondant
- Partage sur les réseaux sociaux depuis la marketplace (Facebook, Pinterest, Instagram)
- Partage direct depuis l'interface admin au moment de la publication

**Admin**
- Dashboard de ventes : CA jour/semaine/mois, commandes, tutoriels populaires, répartition paiements
- Gestion des catégories de tutoriels
- Email de confirmation personnalisable (logo, message)

**Légal & conformité**
- Mentions légales et CGV
- Bandeau cookies RGPD
- Politique de remboursement + case à cocher explicite avant achat
- Facturation automatique par email

### 🟣 V2 — "On fidélise et on développe"
*Objectif : créer de la récurrence et de la communauté*

- Newsletter simple vers les clientes
- Filtres et recherche dans le catalogue
- Mise en avant de tutoriels vedettes gérée par l'admin
- Connexion via Google
- Commentaires sur les articles de blog
- Galerie des réalisations des élèves (soumission de photos)
- Système d'avis sur les tutoriels
- Statistiques détaillées (revenus par période, clientes actives)
- Programmation de publication d'articles à l'avance
- Gestion des codes promo et réductions

### ⛔ Hors périmètre
*Fonctionnalités volontairement écartées*

- Application mobile native
- Système d'abonnement mensuel / accès illimité
- Cours en vidéo ou contenu streamé
- Marketplace multi-vendeurs
- Programme d'affiliation
- Traduction multilingue
- Paiement en plusieurs fois

---

## 7. Glossaire

| Terme | Définition |
|---|---|
| PDF tutoriel | Fichier numérique vendu sur la plateforme contenant les instructions de réalisation d'un ouvrage de cartonnage |
| Admin | Interface de gestion réservée à la créatrice, accessible par identifiant et mot de passe |
| Espace "Mes achats" | Page du compte client listant tous les PDFs achetés et permettant leur téléchargement |
| Validation manuelle | Action effectuée par l'admin pour confirmer la réception d'un chèque et débloquer l'accès au PDF |
| WYSIWYG | Éditeur visuel où ce que l'on voit à l'écran correspond exactement au rendu final publié |
| RGPD | Règlement Général sur la Protection des Données — réglementation européenne sur la vie privée |
| PCI-DSS | Standard de sécurité international pour le traitement des données de carte bancaire |
| WCAG 2.1 | Référentiel international d'accessibilité web (Web Content Accessibility Guidelines) |