# CLAUDE.md — nautilus-app-passager

## Rôle

Appli des passagers et des réservants (aidant, établissement) : inscription, profil(s) de besoins, recherche de chauffeurs compatibles, réservation planifiée (>= 12 h), suivi, bouton d'urgence, notation.

## État

Dépôt vide. Première tâche : `flutter create --org fr.nautilus --project-name nautilus_passager --platforms=ios,android .`, puis dépendance vers `nautilus-flutter-commun` (git, tag versionné).

## Écrans du lot 0

1. Connexion (Keycloak, Authorization Code + PKCE via le package commun)
2. Profil de besoins (questionnaire guidé, plusieurs profils pour un aidant)
3. Nouvelle course : départ, arrivée (saisie vocale possible, favoris), date/heure, aller-retour avec attente, accompagnant
4. Liste des chauffeurs compatibles : prix estimé détaillé, note + critères PMR, équipements, badge certifié
5. Suivi de réservation (statuts, approche du chauffeur, bouton d'urgence 112, alerte à la personne à prévenir)
6. Notation

Mineurs : uniquement accompagnés d'un adulte. Pas de paiement dans l'appli (paiement direct au chauffeur).

## Règles Flutter

- Gestion d'état : Riverpod (avec génération de code si adoptée dans `nautilus-flutter-commun`).
- Composants visuels, client API, authentification : toujours depuis `nautilus-flutter-commun`. Ne pas dupliquer.
- Navigation : go_router (à confirmer dans le package commun).

## Accessibilité (critère d'acceptation)

- Chaque élément interactif a un libellé `Semantics` explicite, en français.
- Cibles tactiles >= 48 x 48 dp. Aucune action accessible uniquement par geste complexe.
- Respect de la taille de texte système jusqu'à 200 % sans perte d'information.
- Contrastes >= 4,5:1 (texte), >= 3:1 (composants). Thème sombre et contraste élevé.
- La carte n'est jamais la seule source d'information : adresse et temps d'approche en texte.
- Toute info importante en texte + son + vibration.
- Tests : `meetsGuideline(androidTapTargetGuideline)`, `iOSTapTargetGuideline`, `labeledTapTargetGuideline`, `textContrastGuideline` dans les tests de widgets.
- Tester sous VoiceOver et TalkBack avant de considérer un écran terminé.

## Contexte projet (commun)

- Projet : **Nautilus** (nom de code), appli VTC marketplace dédiée aux PMR, lancement à Nantes (métropole + aéroport + gare).
- Side project, développeur unique, budget quasi nul, < 5 h/semaine, démarrage janvier 2027.
- Référence fonctionnelle : `nautilus/docs/cahier-des-charges.md`. Glossaire : `nautilus/docs/glossaire.md`. Décisions transverses : `nautilus/docs/decisions.md`.
- Lot en cours : **lot 0 – prototype** avec données fictives. Objectif : un prototype fonctionnel et démontrable. La validation terrain est faite par un autre intervenant.
- Licence : PolyForm Noncommercial 1.0.0. Ne jamais ajouter de dépendance dont la licence interdit cet usage.

## Règles transverses

- Langue : identifiants de code en anglais (voir glossaire FR -> EN) ; docs, commits et messages à l'utilisateur en français.
- Commits : Conventional Commits.
- Mode de travail : Claude développe seul et pilote le projet en Scrum autogéré. Fouad est le client (MOA/MOE) : il supervise et teste, il ne lit pas les tickets. Processus complet : `nautilus/.claude/scrum/PROCESS.md` (à lire avant toute action sur le backlog).
- Autonomie : branches, commits, push de branches, PR, fusion après CI verte, gestion des issues et du tableau de projet sont autorisés sans demander. Demander à Fouad avant : déploiement en production, secret ou service payant, publication sur les stores, opération destructive (force push, suppression), changement de périmètre ou de règle métier du cahier des charges.
- Accessibilité WCAG 2.2 AA obligatoire sur toute interface : c'est un critère d'acceptation, pas une option.
- Données : profil de besoins = besoins fonctionnels, jamais de pathologie. Aucune donnée personnelle réelle dans le code, les tests ou les fixtures.
- Secrets : jamais dans le dépôt (même privé).
- Contrat d'API : la source de vérité est `nautilus-contrat-api`. Ne jamais modifier un client généré à la main.

## Sessions Claude Code

- Au démarrage, le dernier résumé est injecté automatiquement (hook SessionStart). Commande `/reprise` pour reprendre proprement.
- En fin de session, ou quand l'utilisateur annonce une pause : `/cloture`.
- Protocole : `.claude/sessions/_PROTOCOLE.md`. Ce dossier est réservé à Claude Code ; ne pas le mentionner dans la documentation destinée aux humains.
