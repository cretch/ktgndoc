---
title: Jellyfin
description: 
published: true
date: 2024-04-22T18:56:44.331Z
tags: 
editor: markdown
dateCreated: 2024-04-14T15:04:33.113Z
---

# Jellyfin	
Jellyfin est un centre de lecture multimédia. Vous devez disposer d'un identifiant crée pour vous (il est partagé avec [Jellyseerr](/jellyseerr) ).

Il est accessible de plusieurs manières:
- Via le web, en cliquant sur [ce lien](https://jellyfin.ktgn.net)
- Via une app ([Android](/https://play.google.com/store/apps/details?id=org.jellyfin.mobile), [Apple](/https://apps.apple.com/us/app/jellyfin-mobile/id1480192618?mt=8) ou les [TV Android](/https://play.google.com/store/apps/details?id=org.jellyfin.androidtv))

Selon la configuration de la TV, vous pouvez aussi "caster" Jellyfin. Sur ma vieille Smart TV, j'ai du acquérir un Google Chromecast.

> La documentation est rédigée en s'appuyant sur la version web. Les grands principes sont identiques sur les applications
{.is-info}

## Connexion

On ne peut faire plus simple.  Veuillez entrer l'utilisateur et le mot de passe.
L'option de connexion rapide est réservée aux connexions pour, par exemple, les apps des TV et consoles, pour s'éviter la corvée des mots de passe.

![login.png](/jellyfin/login.png){.align-center}

## Accueil

1. Menu latéral (raccourci vers les bibliothèques et les paramètres utilisateur)
2. "Caster" vers un autre écran compatible. Utile si vous naviguez via le téléphone mais souhaitez regarder sur votre téléviseur
3. Rechercher un titre
4. Options utilisateur
5. Navigation principale

![accueil.png](/jellyfin/accueil.png){.align-center}

### Options utilisateur

Vous pourrez personnaliser ici votre expérience...  Normalement, vous ne devriez pas avoir besoin de changer quelque chose ici.

- Profil  (pour changer de mot de passe)
- Connexion rapide (à utiliser quand vous voudrez vous connecter par exemple sur une TV)
- Affichage (pour modifier certaines options graphiques, la langue de l'interface, l'heure, etc...)
- Accueil (pour personnaliser l'écran d'accueil, changer les ordres, supprimer certaines sections, etc...)
- Lecture (pour changer les paramètres de qualité des vidéos, la langue favorite,. etc...)
- Sous-titres (Préférences des sous-titres)
- Contrôle (pour activer la navigation à la manette)

### Navigation principale

> Attention, lorsque vous survolerez une vignette avec la souris, il y aura un bouton **Play** qui apparaitra.
Cela lance la lecture directement. Si vous voulez voir la fiche d'un media, cliquez sur le titre en-dessous. 
{.is-danger}


La première ligne contient les bibliothèques. Vous pouvez les faire défiler grâce à la petite flèche en haut à droite.
Cliquez sur la vignette pour voir le détail des bibliothèques.

Les autres lignes vous permettent de voir les derniers médias entamés, puis dessous les ajouts récents.

#### Livres
Dans Jellyfin, les livres sont classés par auteur. C'est assez basique, vous devez rentrer dans un auteur pour voir la liste de ses titres.

![livre1.png](/jellyfin/livre1.png){.align-center}

Si vous cliquez sur Play, le titre va se charger en mémoire, ce qui peut être sensiblement plus long pour les formats graphiques. La navigation entre les pages se fait au clavier, tout simplement. 

La coche est pour noter que vous avez déjà lu le leivre, le coeur vous permet de le mettre dans une liste de favoris.

Si vous cliquez sur le titre, vous entrerez sur la fiche du livre. Cela n'a d'intérêt que si vous souhaitez télécharger une copie locale (via la deuxième icône ⬇️)

![livre2.png](/jellyfin/livre2.png){.align-center}

#### Movies

L'interface est un peu plus touffue ici... Dans la navigation haute, il y a trois onglets importants :

- Films, celui dans lequel vous vous trouvez de base
- Suggestions, qui vous propose des films selon ce que vous avez déjà vus.
- Genres, pour une classification selon le genre de films. Bon, c'est un classement très... créatif va-on dire. 

Sur la droite de l'écran, vous pouvez n'afficher que les films d'une certaine lettre. 

![movies1.png](/jellyfin/movies1.png){.align-center}

##### Filtres et tri

- Flèches, pour naviger entre les pages
- Affichage, pour changer la présentation de la liste
- Tri
- Filtres

![movies2.png](/jellyfin/movies2.png){.align-center}

#### Music

#### TV shows



