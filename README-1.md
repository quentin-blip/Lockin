# LOCKIN
App mobile (PWA) : cadenas interactif (3 s), journal d’habitudes sans culpabilisation, suivi du poids, sport, statistiques. Design Liquid Glass, **100 % hors ligne**, données privées sur l’appareil, synchronisation Supabase **facultative**.

## Structure
`index.html` (app) · `config.js` (URL + clé anon Supabase) · `manifest.webmanifest` + `sw.js` + icônes PNG (PWA) · `schema.sql` (table + RLS) · `.github/workflows/pages.yml` (déploiement)

## Mise en route
1. **Supabase** : SQL Editor → colle et exécute `schema.sql`.
2. **Clé** : Project Settings → API → copie la clé *anon public* dans `config.js` (remplace `COLLE_ICI_TA_CLE_ANON`). Jamais la clé `service_role`.
3. **Auth** : Authentication → URL Configuration → *Site URL* = l’URL GitHub Pages (`https://<utilisateur>.github.io/<repo>/`). Providers → Email activé.
4. **GitHub** : crée le dépôt, pousse tout le contenu (fichiers cachés inclus), puis Settings → Pages → Source : **GitHub Actions**. Chaque push sur `main` redéploie.
5. **Android** : ouvre l’URL dans Chrome → « Installer l’application » (ou bouton dans Profil).

## Données et confidentialité
- Stockage local (localStorage) ; fonctionne sans réseau après la première visite (service worker).
- Avec un compte : l’état complet est synchronisé dans `user_data` (une ligne par utilisateur, protégée par RLS). Stratégie : dernière écriture gagnante.
- Export JSON (téléchargement), import, suppression totale (local + ligne Supabase) depuis Profil.
- Limites : les rappels ne s’affichent que lorsque l’app est ouverte (pas de push en arrière-plan). La suppression du compte Auth se fait dans le dashboard Supabase.
