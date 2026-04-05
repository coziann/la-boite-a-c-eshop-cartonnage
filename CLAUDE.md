# CLAUDE.md — La Boite à C

## Aperçu du projet

Marketplace de vente de tutoriels PDF de cartonnage pour une créatrice auto-entrepreneur. L'objectif est de permettre la vente en ligne de tutoriels PDF avec livraison automatique, un blog de réalisations, et une interface d'administration simple utilisable sans compétences techniques.

Cible : une cinquantaine de clientes actives (50-70 ans, peu à l'aise avec le digital), parcours d'achat ultra-guidé, 100 % des livraisons de PDF automatisées.

## Architecture globale

Stack : Next.js 15 (App Router) + Payload CMS 3 + PostgreSQL (Neon) + Cloudflare R2 + Stripe + Resend, déployé sur Vercel. Monorepo unique — Payload s'intègre directement dans Next.js.

Grandes zones :
- `(vitrine)` — routes publiques : catalogue, fiches produits, blog, espace client, tunnel d'achat
- `(payload)/gestion` — interface admin (URL non-standard)
- `api/` — webhooks Stripe/PayPal, génération des liens signés R2
- `collections/` — schémas Payload : Tutoriels, Commandes, Clientes, Articles, Categories
- `emails/` — templates React Email (livraison PDF, confirmation commande, chèque)

Les PDFs sont stockés dans un bucket R2 **privé** et ne sont jamais accessibles directement. Les liens de téléchargement sont signés (TTL 1 heure, re-générables depuis "Mes achats").

## Style visuel

- Interface claire et minimaliste
- Pas de mode sombre
- Utilisation de photos pour les produits

## Contraintes et politiques

- Ne jamais exposer les clés API au client (toutes les clés restent côté serveur)
- Préférer les composants existants plutôt qu'ajouter de nouvelles bibliothèques UI
- À la fin de chaque développement impliquant l'interface graphique : tester avec le skill Playwright — l'interface doit être responsive, fonctionnelle et répondre au besoin développé

## Context7

Utiliser systématiquement les outils MCP context7 (résolution d'identifiant + récupération de documentation) pour tout besoin de génération de code, étapes de configuration/installation, ou documentation de bibliothèque/API — sans attendre une demande explicite.

## Spécifications et documentation

- PRD : [PRD.md](./PRD.md)
- Architecture technique : [ARCHITECTURE.md](./ARCHITECTURE.md)

Toutes les spécifications doivent être rédigées en français, y compris les sections Purpose et Scénarios des specs OpenSpec. Seuls les titres de requirements doivent être en anglais avec les mots-clés SHALL/MUST pour la validation OpenSpec.
