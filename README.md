# Deepfake-taxonomy-for-MISP
## Description
Le but de notre projet et de créer une taxonomie afin de renseigner et de caractériser de manière fine les deepfakes au sein de MISP. 

Notre travail fusionne deux approches existantes en proposant :
1. une **taxonomie technique** basée sur les artefacts forensics
2. une **galaxy contextuelle** axée sur l'impact

## Objectif
Notre objectif de décrire complètement un deepfake. Les contenus traités seront : 
- les **indices** de génération
- les **méthodes** de génération
- les **intentions** et **impacts**

## Problématique
Les deepfakes posent un défi majeur en cybersécurité et en OSINT :
- difficulté de détection
- multiplicité des techniques de génération
- variété des objectifs 

C'est pourquoi nous avons jugé utile de créer une taxonomie structurée et exploitable à ce sujet.

Notre défi principal est donc :
_Comment structurer une taxonomie riche sans la rendre inutilisable ?_

Nous avons choisi une approche en axes indépendants :

- **detection** → comment on le détecte
- **creation** → comment il est fabriqué
- **technique** → méthode précise
- **intent** → pourquoi il existe
- **impact** → conséquences

Chaque axe est indépendant, ce qui permet une combinaison flexible de tags

## Déroulement
Nous avons tout d'abord effectué un état-de-l'art sur l'existant, celui-ci est disponible dans le projet : [état de l'art](https://github.com/IaParInterim/Deepfake-taxonomy-for-MISP/blob/main/docs/etat-de-lart.md). Il regroupe nos recherches académiques sur les détections de deepfake et les taxonomies existantes.

Nous avons ensuite créé machinetag.json et nous l'avons testé via une instance MISP locale (sur Docker). Pour finir, nous avons créé une Galaxy deepfake.
