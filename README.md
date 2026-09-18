# Spotify Control — applet Cinnamon

Un applet [Cinnamon](https://github.com/linuxmint/cinnamon) (Linux Mint) qui ajoute des contrôles Spotify dans la barre de tâches : précédent, lecture/pause, suivant, et un bouton favoris — avec la pochette, le titre et l'artiste du morceau en cours affichés au survol.

![Cinnamon](https://img.shields.io/badge/Cinnamon-6.x-2f9e44) ![License](https://img.shields.io/badge/license-GPL--3.0-blue)

## Fonctionnalités

- **Précédent / Lecture-pause / Suivant** via l'interface MPRIS de Spotify (D-Bus), sans dépendance externe type `playerctl`.
- **Survol** : affiche la pochette de l'album, le titre et l'artiste du morceau en cours.
- **Bouton favoris (★)** : ajoute ou retire le morceau en cours de vos "Titres likés" Spotify. Cette fonctionnalité passe par l'API Web officielle de Spotify (OAuth), car Spotify n'expose pas cette action via D-Bus.
- Si Spotify n'est pas lancé, un clic sur un des boutons lance l'application.
- Interface compacte : les boutons s'adaptent (le bouton favoris n'apparaît que pour un vrai morceau en cours de lecture).

## Prérequis

- Linux Mint / Cinnamon 5.x ou supérieur.
- Le client Spotify officiel installé (`/usr/bin/spotify`).
- `curl`, `wget`, `openssl`, `xdg-open` (présents par défaut sur la plupart des installations).

## Installation

```bash
git clone https://github.com/ghislaingaillot/cinnamon-spotify-applet.git
ln -s "$(pwd)/cinnamon-spotify-applet/spotify-control@ghislaingaillot" ~/.local/share/cinnamon/applets/spotify-control@ghislaingaillot
```

Puis, dans Cinnamon : **Paramètres → Applets**, retrouvez « Spotify Control » dans l'onglet *Téléchargé* et ajoutez-le à votre panel (ou activez-le directement s'il apparaît déjà dans la liste).

## Configurer le bouton favoris (optionnel)

Le bouton favoris nécessite une autorisation OAuth auprès de Spotify (gratuite, quelques minutes) :

1. Rendez-vous sur [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard) et créez une application.
2. Dans les paramètres de l'application, ajoutez l'URI de redirection suivante :
   ```
   http://127.0.0.1:43127/callback
   ```
3. Copiez le **Client ID** de l'application.
4. Dans Cinnamon, faites un clic droit sur l'applet → **Configurer**, collez le Client ID.
5. Cliquez sur **Se connecter à Spotify** : votre navigateur s'ouvre pour autoriser l'application, puis se referme automatiquement.

Le jeton d'accès est ensuite rafraîchi automatiquement ; vous pouvez vous déconnecter à tout moment depuis le même panneau de configuration.

Sans configuration, l'applet fonctionne normalement pour la lecture (précédent/pause/suivant/pochette) ; seul le bouton favoris reste inactif.

## Structure du projet

```
spotify-control@ghislaingaillot/
├── applet.js            # UI de l'applet, intégration MPRIS, pochette
├── spotifyAuth.js        # Flux OAuth (PKCE) et appels à l'API Web Spotify
├── settings-schema.json  # Page de configuration (Client ID, connexion)
├── stylesheet.css        # Styles
└── metadata.json          # Métadonnées de l'applet
```

## Licence

Ce projet est distribué sous licence [GPL-3.0](LICENSE).
