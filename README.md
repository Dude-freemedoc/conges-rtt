# Gestion Congés / RTT

Application web légère (HTML/CSS/JS, sans build) pour la gestion collaborative
des congés/RTT des équipes Support N1 et N2 sur la période Juin 2026 -> Mai
2027.

## Fonctionnalités

- Planning mensuel par collaborateur (jours ouvrables)
- Ouverture par défaut sur le mois courant (si présent dans la période)
- Affichage des numéros de semaine ISO dans l'en-tête mensuel
- Repérage visuel de la date du jour (colonne discrètement surlignée)
- Repérage visuel de la semaine en cours dans la ligne des semaines
- Jours fériés distingués en gris pour éviter la confusion avec le jour courant
- Sélection du code d'absence avec journée, matinée, après-midi
- Possibilité de saisir deux demi-journées différentes sur un même jour
  (exemple : matin CP, après-midi RTT)
- Navigation rapide entre les mois via les flèches ‹ / › encadrant le sélecteur
- Saisie en plage de dates (mode plage)
- Picker d'absence positionné automatiquement pour rester visible dans la page
- Totaux mensuels par collaborateur et lignes "Présents" équipe/global
- Fusion visuelle des zones vides d'en-tête et des fins de lignes "Présents"
  pour une lecture plus compacte
- Récap annuel avec :
	- date d'embauche
	- ancienneté (ans)
	- CP ancienneté (barème Syntec)
	- soldes initiaux et soldes restants
	- séparateurs verticaux pour délimiter les blocs (identitaire / données / totaux)
- Gestion des collaborateurs (nom, équipe, date d'embauche)

## Stockage et résilience

- Stockage principal : Supabase
- Secours partagé : JSONbin
- Secours local : localStorage (cache + historique roulant)

En cas d'indisponibilité du stockage principal, l'application bascule
automatiquement vers le secours puis vers le local si nécessaire.

Mirroring et rattrapage automatique :
- Quand Supabase est disponible, l'écriture est faite sur Supabase avec un
	miroir best-effort vers JSONbin.
- Si Supabase tombe pendant une écriture, l'état est écrit sur JSONbin et un
	miroir "en attente" est conservé localement.
- Au retour de Supabase, ce miroir en attente est automatiquement repoussé vers
	Supabase, puis nettoyé.

Configuration Supabase attendue :
- table `shared_state`
- colonne `id` integer primary key
- colonne `state` jsonb not null

## Modes utilisateur

- Modificateur : consultation + modification
- Lecture seule : consultation uniquement
- Local : utilisation sur le poste courant en cas d'indisponibilité du partage

## Règles de calcul

- Les demi-journées comptent 0.5
- CP provisoire et RTT provisoire sont comptés comme CP/RTT
- Ancienneté calculée au 31/05
- CP ancienneté Syntec : 5/10/15/20 ans = 1/2/3/4 jours (barème)

## Lancement

Ouvrir `index.html` dans un navigateur moderne.
