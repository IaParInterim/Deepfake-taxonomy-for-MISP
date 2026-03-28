# Deepfake-taxonomy-for-MISP
## Description
Le but de notre projet et de créer une taxonomie afin de renseigner et une galaxie permettant de caractériser de manière fine les deepfakes au sein de MISP. 

Notre travail fusionne deux approches existantes en proposant :
1. une **taxonomie technique** basée sur les artefacts forensics
2. une **galaxy contextuelle** axée sur l'impact

## Objectif
L'objectif de notre **taxonomie** est de décrire complètement un deepfake. Les contenus traités seront : 
- la **détection** → comment on le détecte (yeux, doigts, ...)
- la **génération** → comment il est fabriqué (faceswap, ...)
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

## Lancer le projet

### Prérequis

```bash
# MISP en local (Docker)
git clone https://github.com/MISP/misp-docker
cd misp-docker
docker compose up -d

# Python pour la génération d'UUID
python3 -c "import uuid; print(uuid.uuid4())"
```

## Déroulement
Nous avons tout d'abord effectué un état-de-l'art sur l'existant, celui-ci est disponible dans le projet : [état de l'art](https://github.com/IaParInterim/Deepfake-taxonomy-for-MISP/blob/main/docs/etat-de-lart.md). Il regroupe nos recherches académiques sur les détections de deepfake et les taxonomies existantes.

Nous avons ensuite créé machinetag.json et nous l'avons testé via une instance MISP locale (sur Docker). Les traductions des descriptions techniques vers l'anglais ont été affinées à l'aide d'outils d'IA pour garantir une terminologie internationale et une meilleure lisibilité pour les analystes. Pour finir, nous avons créé une galaxie deepfake.

## Lien avec DISARM

**[DISARM](https://www.disarm.foundation/)** (*Disinformation Analysis and Risk Management*) est le framework de référence pour décrire les opérations d'influence et de désinformation. Calqué sur MITRE ATT&CK, il est déjà intégré nativement dans MISP sous forme de quatre clusters de galaxie : *Actor Types*, *Techniques*, *Detections* et *Countermeasures*. Notre galaxie deepfake est une **couche de granularité forensic** qui s'articule sous DISARM, exactement comme les sous-techniques ATT&CK affinent une technique parent. Elle permet de documenter l'artefact lui-même, là où DISARM documente la campagne.


## Ce qui fonctionne / Ce qui ne fonctionne pas

### Ce qui fonctionne

- les **tests sur une instance MISP Docker** : essentiel pour valider le rendu des tags avant soumission
- une **approche multi-axe** : propre, modulaire, extensible et nativement compatible avec MISP
- le **lien DISARM** : permet de relier un incident deepfake à une campagne de désinformation plus large

### Ce qui ne fonctionne pas

- le **parsing automatique de texte brut** → trop rigide, une réflexion humaine est nécessaire pour sélectionner ce qui est pertinent
- les **algorithmes de détection ML** → ambitieux, hors scope pour ce projet, référencer les méthodes suffit
- les **valeurs mal formatées** : toujours utiliser des minuscules avec tirets (`face-swap`, jamais `Face Swap`)
- oublier les **UUID** : chaque entrée doit avoir un UUID unique
