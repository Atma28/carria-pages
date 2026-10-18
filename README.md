# carria-pages

Pages statiques de **Carria**, hébergées sur GitHub Pages. Ce dépôt est
**public**.

## Ce qu'il contient

| | |
|---|---|
| `index.html` | page d'accueil : le nom, une phrase. Rien d'autre. |
| `email-confirmed/index.html` | la page vers laquelle Supabase renvoie après qu'un utilisateur a cliqué sur le lien de confirmation de son adresse. Français et anglais, l'un sous l'autre. |
| `404.html` | page d'erreur, même style. |
| `.nojekyll` | désactive Jekyll sur GitHub Pages : sans ce fichier, les dossiers et fichiers commençant par `_` sont ignorés. |

## Ce qu'il ne doit JAMAIS contenir

Ce dépôt est public, et tout ce qui y entre est public pour toujours — même
retiré par un commit suivant, il reste dans l'historique.

- **Aucune clé, aucun jeton.** Pas même la clé « anon » de Supabase : elle est
  publique par conception, mais elle n'a rien à faire ici, puisque ces pages
  n'appellent aucune interface de programmation.
- **Aucune adresse de projet Supabase**, aucune URL de base de données.
- **Aucune référence** à un fichier du dépôt privé de l'application.
- **Aucune donnée d'utilisateur**, aucune capture d'écran en contenant.

## La page de confirmation

Supabase renvoie vers elle après la confirmation d'une adresse. **L'adresse de
retour peut porter un jeton de session** dans son fragment (`#access_token=…`)
ou un code dans sa requête (`?code=…`), selon le flux d'authentification.

La page **ne s'en sert pas, ne le transmet pas, ne l'affiche pas**. Son script
en ligne ne lit qu'une chose : la PRÉSENCE d'une clé d'erreur, pour choisir
entre deux messages déjà écrits dans la page. Puis il nettoie l'adresse par
`history.replaceState`, afin que le jeton ne reste ni dans la barre d'adresse
ni dans l'historique du navigateur.

### Si vous modifiez ce script

La politique de sécurité (`Content-Security-Policy`) n'autorise ce script que
par son **empreinte sha256**, et elle interdit tout le reste : aucune
ressource externe, aucune police en ligne, aucune image distante, aucun autre
script.

L'empreinte porte sur le contenu exact de l'élément `<script>`, octet pour
octet. **Un seul espace de différence et le navigateur refuse de l'exécuter —
en silence.** La page s'afficherait quand même, et seul le cas du lien expiré
serait muet : il montrerait « adresse confirmée » à quelqu'un dont le lien a
expiré.

Après toute modification du script, recalculez l'empreinte et remplacez-la
dans la directive `script-src` :

```sh
# Sur une seule ligne, le contenu du script entre <script> et </script>.
# Le plus sûr reste de passer par un outil qui l'extrait mécaniquement.
openssl dgst -sha256 -binary script.txt | openssl base64
```

## Pour plus tard

Les pages légales — politique de confidentialité, conditions d'utilisation —
viendront ici, et leurs adresses iront dans `PRIVACY_POLICY_URL` et
`TERMS_URL` de l'application. Elles ne font pas partie de ce premier jet, et
c'est pour cela qu'aucune page ne porte de lien vers elles : un lien mort est
pire que pas de lien.
