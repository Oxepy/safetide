---
projet: SafeTide — transfert depuis Nanocorp vers git perso
date: 2026-04-22
---

## Fait

Rebuild complet de l'UI SafeTide en un seul fichier HTML autonome, indépendant de Nanocorp et de Vercel.

Fichier livré : `D:\Apps\Tools\Claude\Cowork\PROJECTS\DeepSearch\Deepsearch\safetide\index.html` (≈ 50 Ko, zéro dépendance serveur, Chart.js chargé via cdnjs).

Fonctionnalités en place :
- 3 métriques sur échelle 0‑10 : Overall (calculé auto), Energy, Sleep
- Formule Overall = (Energy + Sleep) / 2, vérifiée contre les 44 lignes de ton export du 22/04
- 4 onglets : Aujourd'hui / Progression / Analyse / Historique
- Suggestions contextuelles (niveau × tendance × moment × métrique)
- 4 modes d'analyse : derniers jours / évolution semaine / comparaison semaines / plage custom
- Keyword mining sur les notes (7 catégories FR + EN : sommeil, activité, social, bienêtre, nature, stress, travail)
- Rappel quotidien si pas d'entrée aujourd'hui
- Import / export CSV (format identique à ton export Nanocorp, donc réimport direct)
- Persistance localStorage
- PWA meta tags iOS + safe‑area CSS pour usage iPad ajouté à l'écran d'accueil

Tâche planifiée D+30 (rappel Ko‑fi via agent Nanocorp) désactivée — décision de supprimer le compte avant.

## À faire

1. **Créer le repo git perso.** Par exemple `github.com/oxepy/safetide` ou un nom dédié. Dépose `index.html` à la racine.
2. **Activer GitHub Pages** sur ce repo : Settings → Pages → source = branche main, dossier `/` (root). URL type : `https://oxepy.github.io/safetide/` (ou ton handle).
3. **Optionnel — domaine `safetide.app`.** Le domaine était à toi via Nanocorp ? À vérifier avant de supprimer le compte. Si oui : transférer le domaine ou repointer le CNAME vers GitHub Pages avant suppression. Si le domaine appartient à Nanocorp, tu le perds en supprimant → prévoir un nom de remplacement.
4. **Premier chargement :** importer `safetide-export-2026-04-22.csv` via le bouton Import de l'app pour récupérer tes 44 entrées historiques.
5. **Ajouter à l'écran d'accueil iPad :** Safari → Partager → « Sur l'écran d'accueil ». L'app s'ouvre ensuite en plein écran.
6. **Backup régulier :** exporter le CSV depuis l'app périodiquement. iOS purge localStorage après 7 jours sans ouverture d'une webapp → pas de backup = perte de données.
7. **Supprimer le compte Nanocorp** une fois 1–5 validés et le CSV réimporté.

## Notes

- Aucune dépendance serveur, aucune API, aucun backend → zéro risque de vendor lock‑in. Tu peux déplacer `index.html` n'importe où (git, Dropbox, clé USB, Netlify), ça marche pareil.
- Les données restent en local sur l'appareil. Si tu utilises iPad + Mac, c'est deux bases séparées — l'export/import CSV est le pont entre les deux.
- Point d'attention : le site n'est pas responsive tablette spécifiquement testé au‑delà des meta tags iOS ; si un layout casse, on ajuste les breakpoints CSS.
