# Cracken LockR Vercel

Projet ultra leger pour servir la page de validation LockR sur Vercel. La page appelle le launcher local (`http://127.0.0.1:{port}`) pour lancer le telechargement apres la validation LockR.

## Structure

```
vercel-lockr/
├── public/
│   └── lockr/
│       └── index.html
└── vercel.json
```

- `public/lockr/index.html` : page HTML + JS qui fait du polling vers le launcher.
- `vercel.json` : configuration Vercel pour servir la page en mode statique et mapper `/lockr` vers `/lockr/index.html`.

## Deploiement

1. Installer la CLI Vercel puis lancer `vercel` dans ce dossier.
2. Definir l’URL produite, par exemple `https://mon-lockr.vercel.app/lockr`.
3. Dans le launcher, ajouter la variable d’environnement `CRACKEN_LOCKR_CALLBACK_URL` :
   ```text
   https://mon-lockr.vercel.app/lockr/index.html?token={token}&port={port}&local_callback={local_callback}
   ```
   Les placeholders `{token}`, `{port}` et `{local_callback}` seront remplis automatiquement par le launcher.
4. Dans le dashboard LockR, mettre ce meme lien en “Target URL”.

## Utilisation

- Quand l’utilisateur valide LockR, il est redirige vers cette page.
- La page lit `token`, `port`, `local_callback` puis ping `http://127.0.0.1:{port}/api/poll-download?token=...` toutes les 1.5 secondes.
- Le launcher repond et lance le telechargement; le message se met a jour dans la page.
- La page affiche aussi un lien de secours direct vers l’URL locale en cas de blocage navigateur.

## Tests locaux

Pour tester sans deployer :

```bash
npx serve public
```

Ouvrir `http://localhost:3000/lockr/index.html?token=fake&port=51732&local_callback=http%3A%2F%2F127.0.0.1%3A51732%2Flockr%2Fcomplete%3Ftoken%3Dfake` et ajuster `port` ou `token`.

## Personnalisation

- Couleurs / texte : modifier directement `public/lockr/index.html`.
- Intervalle de polling : changer `1500` et `3000` dans le script.

