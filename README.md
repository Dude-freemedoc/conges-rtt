# Gestion Conges / RTT

Application web legere (HTML/CSS/JS, sans build) pour la gestion collaborative
des conges/RTT des equipes Support N1 et N2 sur la periode Juin 2026 -> Mai
2027.

## Fonctionnalites

- Planning mensuel par collaborateur (jours ouvrables)
- Selection du code d'absence avec journee, matinee, apres-midi
- Saisie en plage de dates (mode plage)
- Totaux mensuels par collaborateur et lignes "Presents" equipe/global
- Recap annuel avec :
	- date d'embauche
	- anciennete (ans)
	- CP anciennete (barreme Syntec)
	- soldes initiaux et soldes restants
- Gestion des collaborateurs (nom, equipe, date d'embauche)

## Stockage et resilence

- Stockage principal : Supabase
- Secours partage : JSONbin
- Secours local : localStorage (cache + historique roulant)

En cas d'indisponibilite du stockage principal, l'application bascule
automatiquement vers le secours puis vers le local si necessaire.

Configuration Supabase attendue :
- table `shared_state`
- colonne `id` integer primary key
- colonne `state` jsonb not null

## Modes utilisateur

- Modificateur : consultation + modification
- Lecture seule : consultation uniquement
- Local : utilisation sur le poste courant en cas d'indisponibilite du partage

## Regles de calcul

- Les demi-journees comptent 0.5
- CP provisoire et RTT provisoire sont comptes comme CP/RTT
- Anciennete calculee au 31/05
- CP anciennete Syntec : 5/10/15/20 ans = 1/2/3/4 jours

## Lancement

Ouvrir `index.html` dans un navigateur moderne.
