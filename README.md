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
cp template.env .env
docker compose up -d
```

### Installer la taxonomie

```bash
git clone https://github.com/IaParInterim/Deepfake-taxonomy-for-MISP
cd Deepfake-taxonomy-for-MISP
```

### Installer la taxonomie

```bash
 # Copier la taxonomie dans MISP
docker cp taxonomy/taxonomy.json misp-docker-misp-core-1:/tmp/taxonomy.json

# Importer via l'API
curl -X POST https://localhost/taxonomies/import \
  -H "Authorization: VOTRE_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d "@/tmp/taxonomy.json" \
  -k

# Lister les taxonomies pour trouver l'ID de deepfake
curl -X GET https://localhost/taxonomies.json \
  -H "Authorization: VOTRE_API_KEY" \
  -H "Accept: application/json" \
  -k

# Activer la taxonomie
curl -X POST https://localhost/taxonomies/enable/ID_TROUVE \
  -H "Authorization: VOTRE_API_KEY" \
  -H "Accept: application/json" \
  -k
```

### Installer la galaxy

```bash
# Copier les fichiers de la galaxie
docker cp galaxy/clusters/deepfake.json misp-docker-misp-core-1:/var/www/MISP/app/files/misp-galaxy/clusters/deepfake.json
docker cp galaxy/galaxies/deepfake.json misp-docker-misp-core-1:/var/www/MISP/app/files/misp-galaxy/galaxies/deepfake.json

# Mettre à jour les galaxies via l'API
curl -X POST https://localhost/galaxies/update \
  -H "Authorization: VOTRE_API_KEY" \
  -H "Accept: application/json" \
  -k
```


## Déroulement
Nous avons tout d'abord effectué un état-de-l'art sur l'existant, celui-ci est disponible dans le projet : [état de l'art](https://github.com/IaParInterim/Deepfake-taxonomy-for-MISP/blob/main/docs/etat-de-lart.md). Il regroupe nos recherches académiques sur les détections de deepfake et les taxonomies existantes.

Nous avons ensuite créé taxonomy.json et nous l'avons testé via une instance MISP locale (sur Docker). Pour finir, nous avons créé une galaxie deepfake. Les traductions des descriptions techniques vers l'anglais ont été affinées à l'aide d'outils d'IA pour garantir une terminologie internationale et une meilleure lisibilité pour les analystes.

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
