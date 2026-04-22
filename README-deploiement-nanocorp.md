# Déploiement SafeTide via NanoCorp — guide d'intégration

Tu n'as pas accès direct au Vercel de Nanocorp. Tout passe par les agents Nanocorp. Voici la démarche propre pour intégrer les pages légales et les fichiers techniques sur safetide.app.

---

## 1. Avant d'envoyer la demande aux agents

**Vérifications préalables chez Nanocorp :**

- [ ] Récupérer l'adresse postale exacte de **Phospho Inc.** depuis https://www.nanocorp.so/privacy ou /terms. La copier dans `mentions-legales.md` à la place du placeholder.
- [ ] Vérifier la liste des sous-traitants que Phospho mentionne (Stripe, Google, fournisseurs d'inférence IA, etc.) et ajouter ceux qui sont effectivement actifs sur ton instance dans `privacy.md` si nécessaire.
- [ ] Confirmer que ton domaine `safetide.app` pointe bien vers l'infra Nanocorp/Vercel (DNS apex + www).

**Vérifications côté ton compte GoDaddy :**

- [x] WHOIS privacy confirmé actif (Domains By Proxy, déjà fait).
- [ ] Enregistrement CAA à envisager pour restreindre qui peut émettre des certifs TLS — **à voir avec Nanocorp** : quelle autorité de certification utilisent-ils ? (probablement Let's Encrypt via Vercel)
- [ ] MX + SPF si tu ne veux pas que quelqu'un usurpe safetide.app pour envoyer des mails — **à voir avec Nanocorp** : est-ce qu'ils gèrent déjà ces records ?

---

## 2. Demande à envoyer aux agents Nanocorp

Voici un bloc prêt à coller dans ton interface agent. Adapte les chemins et formats selon ce que l'agent te confirme faisable.

```
Bonjour,

Je souhaite intégrer les éléments suivants sur mon site safetide.app :

1. Trois pages légales accessibles publiquement :
   - /mentions-legales
   - /privacy (politique de confidentialité)
   - /terms (conditions d'utilisation)
   Le contenu de chaque page est fourni en pièce jointe (fichiers .md).

2. Un lien vers ces 3 pages dans le footer de toutes les pages du site,
   sous la forme : « Mentions légales · Confidentialité · Conditions ».

3. Un fichier security.txt accessible publiquement à l'URL :
   https://safetide.app/.well-known/security.txt
   Contenu fourni dans security.txt ci-joint.

4. Un fichier robots.txt à la racine :
   https://safetide.app/robots.txt
   Contenu fourni dans robots.txt ci-joint.

5. Si possible, configuration des headers HTTP de sécurité recommandés :
   - Strict-Transport-Security: max-age=31536000; includeSubDomains
   - X-Content-Type-Options: nosniff
   - X-Frame-Options: DENY
   - Referrer-Policy: strict-origin-when-cross-origin
   - Permissions-Policy: geolocation=(), camera=(), microphone=(), payment=()
   - Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self'; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'

Peux-tu me confirmer lesquels de ces éléments sont faisables sur la plateforme,
et m'indiquer si certains nécessitent une intervention humaine de votre côté ?

Merci.
```

---

## 3. Questions ouvertes à poser explicitement à Nanocorp

- **Pages custom** : peuvent-elles être ajoutées avec du contenu Markdown ou uniquement via leur template ?
- **Footer global** : modifiable sur toutes les pages ou page par page ?
- **Fichiers statiques à la racine** : supportés ou faut-il passer par un redirect / hack ?
- **Headers de sécurité** : configurables par utilisateur, ou appliqués globalement par Vercel/Nanocorp sans possibilité de surcharge ?
- **DNS / MX / SPF / CAA** : gérés côté Nanocorp, ou doivent-ils être configurés chez GoDaddy ?
- **Propriété intellectuelle du code généré** : qui possède le code de la landing page ? Toi, Nanocorp, ou usage partagé ? (important pour la section "Propriété intellectuelle" des mentions légales)

---

## 4. Si Nanocorp ne permet pas d'ajouter /mentions-legales ni footer custom

Plan B — publication alternative, conforme à l'esprit de l'art. XII.6 CDE :

- **Mettre les 3 sections légales directement sur la home page**, accessibles sans clic (en bas, en petit). C'est moche mais légal.
- **Utiliser une section repliable** ("Mentions légales" avec un `<details>`) en bas de chaque page si le système accepte du HTML inline.
- **Héberger les pages ailleurs** (GitHub Pages, Netlify, Cloudflare Pages) sur un sous-domaine `legal.safetide.app`, et simplement mettre un lien dans le footer Nanocorp. Nécessite la création d'un sous-domaine chez GoDaddy.
- **En dernier recours** : quitter Nanocorp pour un hébergeur où tu contrôles le déploiement. Pour un site gratuit avec juste une landing, Cloudflare Pages ou Netlify font le job.

---

## 5. Une fois les pages en ligne — vérifications

```
# Depuis n'importe quel poste avec curl
curl -sI https://safetide.app/mentions-legales | head -5
curl -sI https://safetide.app/privacy | head -5
curl -sI https://safetide.app/terms | head -5
curl -s https://safetide.app/.well-known/security.txt
curl -s https://safetide.app/robots.txt
curl -sI https://safetide.app | grep -iE "strict-transport|x-frame|content-security"
```

Tester aussi :
- https://securityheaders.com/?q=safetide.app → grade attendu : A ou A+
- https://www.ssllabs.com/ssltest/analyze.html?d=safetide.app → grade attendu : A ou A+

Screenshots des grades à archiver dans ton dossier projet pour traçabilité.

---

Produit le 21/04/2026. À re-vérifier après chaque modification substantielle de SafeTide.
