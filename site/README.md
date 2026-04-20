# Hostly — Site web

## 📁 Contenu

```
hostly-site/
├── index.html              → Accueil
├── offres.html             → Les 3 offres + comparatif + FAQ
├── offre-abonnement.html   → Fiche produit détaillée (carousel, finitions, Stripe)
├── apercu.html             → Aperçu de l'interface du guide voyageur
├── contact.html            → Formulaire de contact
├── mentions-legales.html   → Mentions + confidentialité + CGV
├── generateur.html         → ⚠️ Page INTERNE (voir ci-dessous)
├── style.css               → CSS partagé
├── script.js               → JS partagé
└── README.md
```

## 🎯 Flux client (important)

1. Client découvre le site → **index.html**
2. Il regarde les offres → **offres.html**
3. Il clique sur l'offre qui l'intéresse → **offre-abonnement.html** (ou fiche équivalente)
4. Il paie via Stripe
5. **APRÈS ACHAT**, tu lui envoies par email le lien vers **generateur.html**
   → Il remplit son guide, télécharge le ZIP, te l'envoie
6. Tu héberges le guide et configures son totem NFC

**Le générateur n'est PAS accessible depuis le menu public**, c'est volontaire. Il est accessible uniquement via lien direct (`tonsite.fr/generateur.html`), et il a un `noindex, nofollow` pour ne pas apparaître sur Google.

## 📧 Comment fonctionne le formulaire de contact

Le formulaire `contact.html` fonctionne **sans configuration** grâce à un fallback intelligent :

### Mode sans config (par défaut — fonctionne tout de suite)

Le formulaire utilise `mailto:` : quand le client clique "Envoyer", son logiciel de mail (Gmail, Apple Mail, Outlook) s'ouvre **avec le message déjà rédigé**, et il n'a qu'à cliquer "Envoyer". Tu reçois le mail sur `mj.thomasduboispro@gmail.com`.

✅ Avantages : marche immédiatement, sans compte, sans clé API
❌ Inconvénients : ouvre le logiciel de mail du client, il doit cliquer une fois de plus

### Mode avancé avec Web3Forms (optionnel — envoi automatique)

Si tu veux que le mail soit envoyé **automatiquement sans ouvrir de logiciel** :

1. Ton site doit être **en ligne** (pas en local) avec un vrai domaine (ex: `hostly.fr`, ou même `monsite.netlify.app`)
2. Va sur [web3forms.com](https://web3forms.com) → crée une clé avec `mj.thomasduboispro@gmail.com`
3. Dans `contact.html`, cherche `VOTRE_CLE_WEB3FORMS_ICI` et remplace par ta vraie clé
4. C'est tout — le formulaire bascule automatiquement en mode AJAX

**Tant que tu ne fais pas ça, le fallback mailto fonctionne.** Pas de précipitation.

## 🔗 Liens Stripe à mettre à jour

Dans `offre-abonnement.html`, le bouton "Acheter" pointe vers ton lien Stripe de test :
```
https://buy.stripe.com/test_00w8wRdifaFfcy3cW5bjW00
```
Quand tu passes en prod, remplace-le.

## 🎨 Personnalisation

### Logo
Remplace `<span class="nav__logo-mark">H</span>` par une image :
```html
<img src="logo.svg" alt="Hostly" style="width: 32px; height: 32px;">
```

### Photos fiche produit
Dans `offre-abonnement.html`, remplace les 3 slides placeholder :
```html
<div class="carousel__slide"><img src="photos/totem-noir.jpg" alt=""></div>
<div class="carousel__slide"><img src="photos/totem-blanc.jpg" alt=""></div>
<div class="carousel__slide"><img src="photos/totem-bois.jpg" alt=""></div>
```

### Mentions légales à compléter
SIRET, hébergeur, directeur de publication, numéro de téléphone réel.

## 🚀 Mettre en ligne

Le site est du HTML pur, héberge-le où tu veux :
- **Netlify** (gratuit, drag & drop sur app.netlify.com) — recommandé
- **Vercel** (gratuit)
- **GitHub Pages** (gratuit)
- Ton hébergeur existant via FTP

## 🛠️ Le générateur en détail

`generateur.html` est une page complète qui :
- Propose 9 sections dépliables : Propriété, Wi-Fi, Contacts, Arrivée/Départ, Infos essentielles, Règlement, Équipements, Recommandations, Documents
- Permet d'**ajouter/supprimer** dynamiquement : contacts, numéros d'urgence, étapes d'arrivée/départ, blocs d'info, règles, catégories d'équipements, catégories de recommandations
- Permet d'**uploader des fichiers** (drag & drop ou clic)
- Génère un **ZIP complet** côté client (JSZip) avec tous les fichiers HTML du guide + un `style.css` + les documents uploadés
- **Sauvegarde automatiquement** les données dans le navigateur pour qu'on ne perde rien

Le client remplit, clique "Générer", télécharge le ZIP, et te l'envoie par mail. Tu héberges le dossier et lui retournes l'URL + le totem.
