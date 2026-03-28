# Deepfake-taxonomy-for-MISP
## Description
Le but de notre projet et de créer une taxonomie afin de renseigner et une galaxie permettant de caractériser de manière fine les deepfakes au sein de MISP. 

Notre travail fusionne deux approches existantes en proposant :
1. une **taxonomie technique** basée sur les artefacts forensics
2. une **galaxy contextuelle** axée sur l'impact

## Objectif
L'objectif de notre **taxonomie** est de décrire complètement un deepfake. Les contenus traités seront : 
- la **détection** → comment on le détecte (yeux, doigts, ...)
- la **création** → comment il est fabriqué (faceswap, ...)
- l'**intention** → pourquoi il existe (se moquer, calomnier, nuire,...)
- l'**impact** → conséquences (faire perdre des contrats, isoler, se venger, ...)

Chaque axe est indépendant, ce qui permet une combinaison flexible de tags

L'objectif de notre **galaxie** est d'apporter du contexte sur l'impact du deepfake, en prennant appui sur des études effectuées à ce sujet.

## Problématique
Les deepfakes posent un défi majeur en cybersécurité et en OSINT :
- difficulté de détection
- multiplicité des techniques de génération
- variété des objectifs 

C'est pourquoi nous avons jugé utile de créer une taxonomie structurée et exploitable à ce sujet.

Notre défi principal est donc :
_Comment structurer une taxonomie cohérente et une galaxie riche ?_

## Déroulement
Nous avons tout d'abord effectué un état-de-l'art sur l'existant, celui-ci est disponible dans le projet : [état de l'art](https://github.com/IaParInterim/Deepfake-taxonomy-for-MISP/blob/main/docs/etat-de-lart.md). Il regroupe nos recherches académiques sur les détections de deepfake et les taxonomies existantes.

Nous avons ensuite créé machinetag.json et nous l'avons testé via une instance MISP locale (sur Docker). Pour finir, nous avons créé une galaxie deepfake.
