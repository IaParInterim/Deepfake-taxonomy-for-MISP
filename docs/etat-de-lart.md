# État de l'art — Deepfake Taxonomy for MISP

## 1. Introduction et périmètre

Ce document regroupe l'ensemble de nos recherches effectuées pour notre projet *Deepfake Taxonomy for MISP*. Il vise donc à synthétiser les connaissances académiques et techniques disponibles sur les deepfakes, afin de fonder des bases solides pour la construction de notre taxonomie afin qu'elle soit fonctionnelle sur la plateforme MISP.

Notre étude couvre trois dimensions complémentaires :

- la **génération** : comment les deepfakes sont produits, les techniques et manières de faire (faceswap, ...)
- la **détection** : comment les identifier, avec l'inspection visuelle manuelle jusqu'aux approches par apprentissage automatique
- l'**impact** et l'**intention** : les usages malveillants documentés, leurs cibles et leurs conséquences

Pour chaque dimension, nous identifions les catégories pertinentes qui seront encodées dans les `machinetag.json` de notre propre taxonomie.

## 2. Définitions et terminologie

### 2.1 Origine et évolution du terme

Le terme *deepfake* est une contraction de *deep learning* et *fake*, apparu en 2017 sur la plateforme Reddit. Ce terme désigne un contenu multimédia (vidéo, image ou son) généré ou altéré par des algorithmes d'IA de manière réaliste qui a pour but de tromper l'œil ou l'oreille humaine. ([Oracle](https://www.oracle.com/fr/security/definition-deepfake-risques/),[Le Monde](https://www.lemonde.fr/idees/article/2024/03/06/deepfake-un-terme-imparfait-pour-une-realite-appelee-a-durer_6220414_3232.html)). 
 
Du point de vue technique, les deepfakes correspondent à la *"génération de contenu numérique faux ou à la manipulation de contenu authentique par le biais de techniques de deep learning"* ([Gambín et al., 2024](https://link.springer.com/article/10.1007/s10462-023-10679-x), cité dans [Polidoro, 2025](https://www.unilim.fr/actes-semiotiques/9095)).

L'article de référence de [Polidoro]((https://www.unilim.fr/actes-semiotiques/9095)) (2025) soulève une question terminologique importante : le label *deepfake* porte en lui une connotation négative dès sa création, d'une part par le terme *deep*, associé au *dark web* et à ce qui est obscur et dangereux, d'autre part par le terme *fake*, qui lui est intrinsèquement péjoratif. L'auteur suggère que des expressions comme *AI-generated content* ou *synthetic media* seraient plus neutres et plus précises, en réservant le terme *deepfake* aux seuls cas de tromperie délibérée.

Cette distinction est importante pour notre taxonomie : il ne faut pas limiter la classification aux seuls usages malveillants, mais il est important d'inclure les usages légitimes (satire, pédagogie, art) comme valeurs possibles.

### 2.2 Origine du contenu : création vs altération

L'article de [Polidoro]((https://www.unilim.fr/actes-semiotiques/9095)) (2025) propose deux approches taxonomiques complémentaires. La première distingue deux grandes familles selon le rapport au contenu source :

| Famille | Définition | Exemples technologiques |
|---|---|---|
| **Synthetic creation** (*production*) | Contenu généré entièrement *ex nihilo* (à partir de rien)| Sora, Midjourney, DALL-E |
| **Manipulation / altération** | Modification de contenu réel existant | Face-swap, lip-sync, voice cloning |

La distinction entre *production* (créer une image de toutes pièces) et *altération* (modifier une image existante) est fondamentale dans toute tentative de taxonomie, car elle détermine les traces laissées dans le contenu et donc les méthodes de détection applicables. Cette distinction est a appliquer dans notre taxonomie.

### 2.3 Des objectifs multiples

La seconde contribution taxonomique de [Polidoro]((https://www.unilim.fr/actes-semiotiques/9095)) (2025) introduit la notion de *régime de croyance* : un deepfake n'a pas nécessairement pour but de tromper. L'auteur rappelle, à travers les exemples de Tom Cruise dansant en peignoir ou de la chanteuse Dalida interviewée en 2022 (alors qu'elle est décédée en 1987), que certains usages des deepfakes ont une visée purement ludique ou pédagogique, et que toute taxonomie doit rendre compte de toutes ces intentions.

Pour la taxonomie MISP, cela justifie d'inclure des valeurs comme `satire`, `docudrama`, ou `legitimate-entertainment`, en plus des valeurs malveillantes.

## 3. Techniques de génération

### 3.1 L'ère des GANs

Les *Generative Adversarial Networks* (GANs), introduits par [Goodfellow et al.](https://arxiv.org/abs/1406.2661) en 2014, constituent le socle technique historique des deepfakes. Leur architecture met en compétition deux réseaux : un réseau générateur, qui produit des échantillons synthétiques, et un réseau discriminateur, qui tente de distinguer les échantillons réels des faux. Ce processus, incluant deux algorithmes adversaires, pousse donc le générateur à produire des contenus de plus en plus convaincants.

Les principales variantes de GANs utilisées pour les deepfakes incluent :

- le **face-swap GAN** : remplacement du visage d'une personne par celui d'une autre dans une vidéo existante
- le **face reenactment** : animation du visage d'une personne cible à partir des mouvements d'un acteur source
- le **full-body synthesis** : génération complète d'une silhouette humaine inexistante

La limite majeure des GANs est qu'ils laissent des signatures détectables dans le domaine fréquentiel, exploitées par de nombreux détecteurs (voir section [4](#4-techniques-de-détection)).

### 3.2 L'ère des modèles de diffusion

Depuis 2022-2023, les modèles de diffusion (Stable Diffusion, DALL-E, Sora) ont progressivement remplacés les GANs pour certaines tâches. L'étude de [Croitoru et al.](https://arxiv.org/abs/2411.19537) (2024) recense les développements les plus récents dans le domaine, incluant les modèles de diffusion et les *Neural Radiance Fields*, et souligne que le réalisme des contenus deepfake augmente à un rythme constant, au point que les personnes peinent souvent à détecter des contenus manipulés en ligne, les exposant ainsi à diverses formes d'escroqueries.

L'apport technique majeur des modèles de diffusion pour notre projet est le suivant : ils ne laissent pas les mêmes artefacts que les GANs. Cette distinction est à inclure dans notre taxonomie.

### 3.3 Modalités : image, vidéo, son, ...

L'étude de Croitoru et al.](https://arxiv.org/abs/2411.19537) (2024) couvre l'ensemble des types de médias deepfake : image, vidéo, son, texte, contenu multimodal (audio-visuel), et identifie différents types de deepfakes selon la procédure utilisée pour altérer ou générer le contenu faux.

Le contenu multimodal représente la frontière la plus avancée : une vidéo dont à la fois l'image et le son ont été manipulés, ou un deepfake couplé à un texte synthétique. Les deepfakes synthétiques ont ainsi transcendé la simple manipulation visuelle pour englober les modalités textuelles et audio, ce qui élargit considérablement leur potentiel de mauvais usage.

**Tableau récapitulatif des techniques par modalité :**

| Modalité | Techniques principales | Exemple de tag |
|---|---|---|
| Vidéo | Face-swap, face reenactment, lip-sync | `face-swap`, `face-reenactment`, `lip-sync` |
| Image | Full synthesis, attribute alteration | `full-body-synthesis`, `attribute-alteration` |
| Audio | Voice cloning, text-to-speech | `voice-cloning`, `text-to-speech` |
| Texte | LLM generation, style transfer | `llm-generation` |
| Multimodal | Combinaison des techniques ci-dessus | `multimodal` |

## 4. Techniques de détection

La détection des deepfakes constitue le cœur de notre travail de taxonomie. L'objectif n'est pas de développer de nouveaux algorithmes, mais d'encoder dans MISP les méthodes et indices de détection existants, afin qu'un analyste puisse documenter comment il a identifié un deepfake.

### 4.1 Détection basée sur les artéfacts visuels

La première famille de méthodes repose sur l'inspection des **anomalies visuelles** laissées par le processus de génération. Ces artéfacts sont exploitables aussi bien manuellement (par un analyste) qu'automatiquement (par un algorithme).

La revue critique de [Sandotra et al.](https://www.researchgate.net/publication/382253628_Deep_Learning_based_Model_for_Deepfake_Image_Detection_An_Analytical_Approach) (2024) répertorie plusieurs catégories d'artéfacts visuels caractéristiques : les anomalies de fusion (*blending artifacts*) visibles aux bords du visage substitué, les incohérences d'éclairage, et les distorsions anatomiques sur les mains et les oreilles, souvent mal restituées par les modèles génératifs.

Les indices visuels les plus documentés dans la littérature sont :

| Indice | Description | Exemple de tag |
|---|---|---|
| Anomalie des mains | Mauvais nombre de doigts ou déformés | `hand-anomaly` |
| Artefact de fusion | Bords flous autour du visage | `blending-artifact` |
| Incohérence d'éclairage | Ombres ou reflets dans les yeux incohérents | `lighting-inconsistency` |
| Distorsion des dents | Texture lissée ou forme anormale | `teeth-anomaly` |
| Asymétrie du visage | Oreilles ou sourcils asymétriques | `facial-asymmetry` |

### 4.2 Détection par signaux biologiques

Une approche complémentaire repose sur l'analyse des signaux physiologiques involontaires, absents ou anormaux dans les contenus synthétiques.

L'article de [Passos et al.](https://www.researchgate.net/publication/378400333_A_review_of_deep_learning-based_approaches_for_deepfake_content_detection) (2024) identifie deux signaux biologiques principaux exploitables :

- le **clignement des yeux** (*eye blinking*) : les premiers modèles de deepfake produisaient des sujets ne clignant pas ou clignant anormalement, car les données d'entraînement comprenaient peu de frames avec les yeux fermés
- les **variations du rythme cardiaque** (*heart rate / rPPG*) : les fluctuations de couleur de la peau liées aux pulsations cardiaques (*remote photoplethysmography*, rPPG) sont absentes des vidéos synthétiques. Des travaux comme ceux de [Çiftçi et al.](https://link.springer.com/article/10.1007/s00371-023-02981-0) (2024) ont exploré cette piste, en cherchant à détecter des deepfakes à partir de la signature cardiaque visible dans la vidéo

### 4.3 Analyse fréquentielle et forensic

Au-delà de la détection, la traçabilité de l'origine des médias synthétiques est également critique afin d'identifier les responsables et comprendre les processus génératifs derrière les deepfakes. Les techniques d'attribution de deepfake et de fingerprinting de modèle visent à identifier les sources de médias synthétiques et les modèles génératifs responsables de leur création.

L'analyse forensic dans le domaine fréquentiel repose notamment sur :

- la **transformée de Fourier** : les GANs laissent des signatures périodiques dans le spectre fréquentiel, absentes dans les images réelles
- l'**analyse PRNU** (*Photo Response Non-Uniformity*) : chaque capteur photo laisse une signature unique dans les images qu'il produit ; les images synthétiques n'en ont pas
- l'**analyse DCT** : les artéfacts de compression JPEG sont différents dans les images générées par IA

La portée de l'analyse forensic dans le domaine des deepfakes s'étend au-delà de la détection, en englobant des domaines de recherche critiques comme l'attribution des deepfakes, l'authentification passive et active, et les méthodes conçues pour fonctionner efficacement dans des scénarios réels, comme les médias fortement compressés tels qu'on en trouve sur les réseaux sociaux.

### 4.4 Détection temporelle et multimodale

L'étude de [Amerini et al.](https://www.researchgate.net/publication/389456404_Deepfake_Media_Forensics_Status_and_Future_Challenges) (2025) structure les approches de détection en trois grandes familles :

| Famille | Description | Exemple de tag |
|---|---|---|
| **Spatial-based** | Analyse des anomalies pixel par pixel dans le domaine spatial | `spatial-analysis` |
| **Temporal-based** | Analyse du flux vidéo, incohérences inter-frames, scintillement | `temporal-analysis` |
| **Multimodal-based** | Analyse croisée audio/vidéo, désynchronisation lèvres/son | `multimodal-analysis` |

L'analyse multimodale est particulièrement efficace pour détecter des deepfakes avancés qui manipulent plusieurs dimensions du contenu original. Les recherches futures devraient explorer l'interopérabilité des différentes modalités de données et le développement d'approches qui combinent analyse visuelle, vocale et textuelle.

La désynchronisation audio-vidéo est l'un des indices les plus facilement détectables à l'œil nu : les approches multimodales analysent les composantes visuelles et audio pour identifier des incohérences, telles que des décalages dans les mouvements des lèvres.

### 4.5 Détection par deep learning

La prolifération de la technologie deepfake a soulevé d'importantes préoccupations en raison de son potentiel de détournement, notamment dans le contexte des élections et des plateformes de réseaux sociaux. Pour y répondre, les chercheurs ont développé des méthodes exploitant des réseaux de neurones convolutifs (CNN) pour analyser les anomalies spatiales et temporelles, allant des distorsions au niveau des pixels aux incohérences dans les signaux biologiques ou comportementaux.

Les principaux modèles de détection par deep learning référencés dans la littérature sont :

- **XceptionNet** : architecture basée sur les convolutions séparables en profondeur, utilisée comme baseline sur le dataset FaceForensics++
- la **combinaison CNN + LSTM** : CNN pour l'analyse spatiale frame par frame, LSTM pour la modélisation temporelle de la séquence vidéo
- les **Siamese Networks** : adaptés à la comparaison pairée de contenus
- les **approches par transformers** : de plus en plus utilisées pour capturer les dépendances longue portée dans les vidéos

### 4.6 Limites actuelles de la détection

Les résultats du benchmark multimodal développé par [Croitoru et al.](https://arxiv.org/abs/2411.19537) (2024) montrent que les détecteurs à l'état de l'art échouent à généraliser à des contenus deepfake produits par des générateurs non vus lors de l'entraînement, ce qui souligne la fragilité des approches actuelles face à l'évolution rapide des techniques de génération.

Les recherches actuelles soulignent plusieurs lacunes critiques dans les approches forensics : la nature adaptative des algorithmes de deepfake, qui évoluent rapidement pour contourner les systèmes de détection existants, le manque de scalabilité et de robustesse des méthodes actuelles pour un déploiement à grande échelle, les préoccupations liées à la vie privée, qui freinent les efforts collaboratifs pour améliorer les frameworks de détection.

De plus, les humains ont tendance à surestimer leur capacité à détecter les deepfakes, ce qui augmente le risque d'être trompé. Les algorithmes de détection par IA, bien que plus performants, ne sont pas accessibles à l'utilisateur lambda qui rencontre du contenu deepfake sur les réseaux sociaux.

C'est précisément dans ce contexte que la taxonomie MISP prend tout son sens : en permettant aux analystes de partager structurellement leurs observations, même partielles, elle contribue à l'intelligence collective face à ces limitations.

## 5. Impact et usages malveillants

### 5.1 Désinformation et manipulation politique

Le cas le plus documenté de deepfake à visée politique reste la vidéo du président ukrainien Volodymyr Zelensky diffusée en mars 2022, dans laquelle il semblait appeler ses soldats à déposer les armes. Cet exemple illustre parfaitement la catégorie de deepfake que [Polidoro](https://www.unilim.fr/actes-semiotiques/9095) (2025) nomme **manipulation-alteration** avec un **régime de croyance** malveillant : le contenu vise à tromper sur les actions réelles d'une figure publique dans un contexte de guerre.

Les deepfakes politiques exploitent principalement les techniques de face reenactment et de lip-sync pour faire dire à une personnalité réelle des propos qu'elle n'a pas tenus. Leur impact est amplifié par la viralité des plateformes sociales.

### 5.2 Fraude et usurpation d'identité

La fraude par deepfake audio représente une menace croissante pour le secteur financier et les entreprises. Les escroqueries dites **CEO fraud** exploitent le clonage vocal pour imiter la voix d'un dirigeant et ordonner des virements frauduleux.

La revue de [Sandotra et al.](395089993_Enhancing_Digital_Authenticity_A_Detailed_Review_of_Deepfake_Detection_Technologies) (2025) recense les principales méthodes d'extraction de caractéristiques pour la détection audio des deepfakes, incluant les coefficients MFCC (*Mel-Frequency Cepstral Coefficients*), les spectrogrammes Mel et les caractéristiques prosodiques, autant d'indices que peut exploiter un analyste pour qualifier un deepfake audio.

### 5.3 Atteintes à la vie privée et surveillance

L'article de [Lee et al.](https://scispace.com/papers/deepfakes-phrenology-surveillance-and-more-a-taxonomy-of-ai-4crwnkiq1b) (2023) introduit deux dimensions d'impact rarement abordées dans les taxonomies techniques :
- la **deepfake surveillance** : l'utilisation de l'IA pour surveiller et classer les individus en fonction de leur apparence (émotions, ethnicité, intentions supposées). Ce cas dépasse le deepfake traditionnel pour toucher à la biométrie et au profilage automatisé
- l'**AI Phrenology** : la fabrication de preuves visuelles de comportements supposément déviants basées sur l'apparence. Cette pratique peut être utilisée pour nuire à la réputation d'un individu ou pour fabriquer des preuves à charge

### 5.4 Impact sur la réputation scientifique et institutionnelle

L'article de [De Jong et al.](https://academic.oup.com/ae/article-abstract/71/1/20/8116419?login=false) (2025) sur les deepfakes dans le domaine de l'entomologie illustre une dimension souvent négligée : l'impact des deepfakes sur l'intégrité scientifique.

Dans un contexte où une fausse preuve scientifique (image de spécimen manipulée, vidéo de résultat d'expérience falsifiée) peut influencer des politiques publiques ou des financements de recherche. Ainsi, la classification de ces usages dans MISP représente une réelle valeur ajoutée pour les communautés CTI et de recherche.

Ces cas enrichissent notre taxonomie.

## 6. Taxonomies existantes

### 6.1 Taxonomies académiques de la détection

Deux schémas taxonomiques de référence sont disponibles sur ResearchGate :

- [1er schéma](https://www.researchgate.net/figure/Taxonomy-of-deepfake-detection-approaches_fig1_362203739) → classe les méthodes en trois familles hiérarchiques avec des sous-catégories fines : *Deep Learning Based*, *Biological Signals*, *Visual Artifacts*
- [2e schéma](https://www.researchgate.net/figure/Taxonomy-of-Deepfake-Detection-Methods-Detection-techniques-are-categorized-into-four_fig3_389456404) → structure plus synthétique en trois axes, directement transposable en prédicats MISP : *Spatial-based*, *Temporal-based*, *Multimodal-based*

L'étude de [Croitoru et al.](https://arxiv.org/abs/2411.19537) (2024) propose une taxonomie unifiée qui fait le lien entre méthodes de génération et méthodes de détection, et est à ce jour la référence la plus complète et à jour (elle couvre les modèles de diffusion de 2024).

### 6.2 Taxonomies contextuelles et sémiotiques

La taxonomie de [Polidoro](https://www.unilim.fr/actes-semiotiques/9095) (2025) est la seule à aborder les deepfakes depuis un angle sémiotique et communicationnel. Elle propose une grille théorico-opérationnelle fondée sur les variables d'expression et de contenu, croisées avec les types de situations communicatives faisant intervenir la conscience et la relation énonciative entre les acteurs, en distinguant notamment les deepfakes qui trompent de ceux qui illustrent ou amusent.

Cette approche est complémentaire des taxonomies techniques : là où les autres disent comment un deepfake est fait, Polidoro dit dans quelle intention de communication il s'inscrit.

### 6.3 Ce qui existe dans MISP

MISP dispose de plusieurs taxonomies partiellement pertinentes, mais aucune taxonomie dédiée aux deepfakes n'existe à ce jour dans le dépôt officiel :

| Taxonomie MISP | Pertinence | Limite |
|---|---|---|
| `information-origin` | Origine humaine vs IA du contenu | Trop générale, pas spécifique aux deepfakes |
| `DFRLab-dichotomies-of-disinformation` | Campagnes de désinformation, vecteurs | Ne couvre pas les techniques de génération |
| `AM!TT` | Narratives et tactiques d'opérations d'influence | Niveau stratégique, pas forensique |
| `admiralty-scale` | Fiabilité et crédibilité d'une source | Complémentaire, à utiliser en combinaison |
| `tlp` | Niveau de partage | Modèle de format à suivre |

La **galaxie DISARM** constitue le point d'articulation le plus pertinent avec notre projet. Elle encode les TTPs (*Tactics, Techniques and Procedures*) des opérations d'influence, et certains de ses clusters correspondent directement à des usages de deepfakes (T0019 — Fabricate content, T0021 — Impersonate existing narrative).

### 6.4 DISARM : le framework de référence pour la désinformation

#### Présentation générale

[DISARM](https://www.disarm.foundation/) (*Disinformation Analysis and Risk Management*) est un framework conçu pour adapter les pratiques de la sécurité de l'information (*infosec*) à la lutte contre la désinformation. Son style est délibérément calqué sur les frameworks MITRE ATT&CK et STIX, afin de s'intégrer nativement dans les outils de cybersécurité existants.

Développé en 2022 par des experts de MITRE, de la Florida International University (FIU) et du Cognitive Security Collaborative (CogSec-Collab), DISARM fournit une méthodologie transparente et une taxonomie détaillée des Tactics, Techniques and Procedures (TTPs) utilisées par les acteurs malveillants dans les campagnes de désinformation et de manipulation de l'information.

Son rôle dans l'écosystème CTI est stratégique : tout comme MITRE ATT&CK a fourni un standard pour les informations contextuelles sur les tactiques et techniques adversariales basées sur des observations réelles, DISARM ambitionne de faire de même pour les opérations d'influence et de désinformation.

#### Structure du framework

DISARM est organisé en deux frameworks complémentaires :

- **DISARM Red Framework** : TTPs des créateurs de désinformation, listées par phase tactique. C'est la version "classique" du framework, celle qui est intégrée dans MISP
- **DISARM Blue Framework** : TTPs des défenseurs et contre-mesures face à la désinformation, listées par phase d'intervention la plus précoce possible

Dans MISP, DISARM est représenté par quatre clusters de galaxie distincts : **Actor Types** (33 éléments), **Countermeasures** (139 éléments), **Detections** (94 éléments) et **Techniques**.

Les phases tactiques du DISARM Red Framework couvrent l'ensemble du cycle de vie d'une opération d'influence : planification des objectifs (*Plan Objectives*), développement du contenu (*Develop Content*), établissement des assets (*Establish Assets*), diffusion (*Deliver Content*), amplification (*Maximise Exposure*), et persistance.

#### Techniques DISARM directement liées aux deepfakes

Plusieurs techniques DISARM correspondent précisément à des usages de deepfakes. Le tableau suivant établit la correspondance entre les TTPs DISARM et des exemples de tags possible :

| Technique DISARM | ID | Description | Exemple de tag cohérent |
|---|---|---|---|
| Develop Text-Based Content | T0085 | Création de contenus textuels faux ou trompeurs | `deepfake-generation:technique="llm-generation"` |
| Create Inauthentic Video Content | T0087 | Production de vidéos synthétiques | `deepfake-generation:media-type="video"` |
| Impersonate existing narrative | T0021 | Usurpation d'une voix ou d'un visage existant | `deepfake-generation:technique="face-reenactment"` |
| Fabricate content | T0019 | Fabrication de contenu de toutes pièces | `deepfake-generation:type="synthetic-creation"` |
| Facilitate State Propaganda | T0002 | Coordination de contenus pro-État | `deepfake-intent:intent="political-manipulation"` |

DISARM encode notamment la création et l'édition de contenu textuel faux ou trompeur, souvent aligné sur des narratives spécifiques, pour usage dans une campagne de désinformation, ce qui inclut les textes générés par IA de manière autonome (bot-created content generation), qui représentent une extension naturelle des deepfakes vers la dimension textuelle.

### 6.5 Notre galaxie comme complément à DISARM

#### Le problème : DISARM s'arrête au niveau stratégique

DISARM est un framework puissant, mais il opère à un niveau d'abstraction stratégique. Il dit qu'une opération d'influence a utilisé du contenu synthétique, mais il ne dit pas comment ce contenu a été techniquement produit, ni quels indices forensics permettent de l'identifier.

Concrètement, la technique DISARM `T0019 — Fabricate content` couvre toute fabrication de contenu, qu'il s'agisse d'un texte rédigé par un humain, d'une image Photoshoppée ou d'un deepfake généré par un modèle de diffusion. DISARM ne discrimine pas entre ces cas, alors que du point de vue forensic et de la réponse à incident, la différence est fondamentale.

#### Notre apport : la couche forensique et technique

Notre galaxie deepfake joue le rôle d'une couche de granularité technique qui s'articule sous DISARM, de la même manière que les sous-techniques ATT&CK (ex. `T1566.001 — Spearphishing Attachment`) affinent une technique parent (ex. `T1566 — Phishing`).

Le schéma d'articulation est en théorie le suivant :

```
DISARM (niveau campagne / stratégie)
    └── T0019 — Fabricate content
    └── T0021 — Impersonate existing narrative
            │
            ▼
Exemple Galaxie Deepfake (niveau artéfact / forensic)
    ├── deepfake-generation:technique="face-reenactment"
    ├── deepfake-generation:model-family="diffusion-based"
    ├── deepfake-detection:detection-clue="audio-video-desync"
    └── deepfake-detection:confidence="high"
```

#### Exemple concret : campagne de désinformation politique

Afin de mieux comprend le tout, prenons le cas de la fausse vidéo du président Zelensky (mars 2022). Dans MISP, un analyste pourrait attacher à l'événement :

**Via DISARM (niveau campagne) :**
```
galaxy:DISARM — Techniques = "T0019 — Fabricate content"
galaxy:DISARM — Techniques = "T0002 — Facilitate State Propaganda"
```

**Via un exemple de Galaxie Deepfake (niveau artéfact) :**
```
deepfake-generation:technique="face-reenactment"
deepfake-generation:media-type="video"
deepfake-detection:detection-clue="blending-artifact"
deepfake-intent:intent="political-manipulation"
deepfake-intent:target-type="political-figure"
deepfake-context:distribution-platform="social-media"
```

Les deux couches se complètent sans se chevaucher : DISARM contextualise l'opération d'influence globale, notre galaxie documente le deepfake en tant qu'artéfact technique. Ainsi, un analyste qui remonte un incident peut ainsi naviguer du niveau forensic (notre galaxie) au niveau campagne (DISARM) et inversement.

#### Positionnement défendable

Cette articulation est précisément ce que les auteurs de DISARM anticipent : DISARM dispose de standards MISP pour les objets de bas niveau comme les images et les publications sur les réseaux sociaux, et travaille à la construction de connecteurs vers des outils comme MISP afin de permettre l'envoi d'alertes rapides sur la désinformation sans remplir de nombreux champs de données.

Notre projet s'inscrit donc dans cette même logique en fournissant exactement ces objets de bas niveau pour les deepfakes, comblant ainsi une lacune explicitement identifiée par la DISARM Foundation elle-même.


## 7. Gap identifié et positionnement du projet

### Ce qui manque actuellement

La revue de la littérature révèle un gap structurel : les chercheurs en sécurité qui détectent ou observent des deepfakes ne disposent d'aucun format standardisé pour partager leurs observations au sein d'une plateforme de CTI comme MISP. Les taxonomies académiques existantes sont conçues pour la recherche, non pour l'opérationnel.

Concrètement, si un analyste découvre une vidéo deepfake ciblant un dirigeant d'entreprise sur LinkedIn, il ne peut actuellement pas documenter dans MISP :
- la technique probable de génération (voice cloning ? lip-sync ?)
- les indices de détection qu'il a observés (désynchronisation audio-vidéo, artefacts de fusion)
- l'intention de l'attaquant (fraude ? extorsion ?)
- la plateforme de diffusion et le type de compte utilisé

### Notre positionnement

Notre projet veut combler ce gap en proposant une approche multi-axe native MISP, structurée autour de quatre taxonomies indépendantes mais complémentaires :

```
deepfake-detection     → comment l'identifier (artéfacts, méthodes)
deepfake-generation    → comment le deepfake a été produit
deepfake-intent        → pourquoi il existe (intention, cible)
deepfake-impact        → quel impact a-t-il sur son environnement
```

Cette approche est :
- **compatible avec MISP** : un objet MISP peut recevoir des tags de plusieurs axes
- **extensible** : chaque axe peut évoluer indépendamment au fil des avancées technologiques

## 8. Références

### Articles académiques

| Référence | URL | Axe couvert |
|---|---|---|
| Croitoru et al. (2024). *Deepfake Media Generation and Detection in the Generative AI Era: A Survey and Outlook*. arXiv:2411.19537. | https://arxiv.org/abs/2411.19537 | Génération + Détection |
| Polidoro, P. (2025). *Two Proposals for a Semiotic Taxonomy of Fake News and Deepfakes*. Actes Sémiotiques, n°133, Université de Limoges. | https://www.unilim.fr/actes-semiotiques/9095 | Définition + Intention |
| Gambín et al. (2024). *Deepfakes: current and future trends*. Artificial Intelligence Review, 57(3). | https://link.springer.com/article/10.1007/s10462-023-10679-x | Génération |
| Sandotra, N. et al. (2024). *A comprehensive evaluation of feature-based AI techniques for deepfake detection*. ResearchGate. | https://www.researchgate.net/publication/376519032 | Détection |
| Çiftçi, U.A., Demir, I., Yin, L. (2024). *Deepfake source detection in a heart beat*. The Visual Computer, 40(4). | https://link.springer.com/article/10.1007/s00371-023-029 | Détection biologique |
| Tolosana et al. (2020). *Deepfakes and Beyond: A Survey of Face Manipulation and Fake Detection*. Information Fusion, 64. | https://arxiv.org/abs/2001.00179 | Génération + Détection |
| *A Comprehensive Review of Deepfake Detection Methods and Challenges in Digital Forensics*. ResearchGate, 2025. | https://www.researchgate.net/publication/391325974 | Détection + Forensique |
| *Deepfake Media Forensics: Status and Future Challenges*. MDPI Journal of Imaging, 2025. | https://www.mdpi.com/2313-433X/11/3/73 | Forensique |
| *Deepfakes, Phrenology, Surveillance and More: A Taxonomy of AI*. SciSpace. | https://scispace.com/pdf/deepfakes-phrenology-surveillance-and-more-a-taxonomy-of-ai-4crwnkiq1b.pdf | Impact + Vie privée |
| *Deepfakes and Insect Taxonomy: Problematic Implications*. American Entomologist, Oxford Academic, 71(1), 2024. | https://academic.oup.com/ae/article-abstract/71/1/20/8116419 | Impact scientifique |

### Ressources en ligne

| Ressource | URL |
|---|---|
| Webinar IEC : *Deepfake Threats and Detection Standards* | https://iec.ch/academy/webinars/deepfake-threats-and-detection-standards-digital-authenticity |
| Dépôt GitHub : classification vidéo deepfake | https://github.com/AKASH2907/deepfakes_video_classification |
| Documentation taxonomies MISP | https://www.circl.lu/doc/misp/taxonomy/ |
| Dépôt officiel MISP Taxonomies | https://github.com/MISP/misp-taxonomies |
| DISARM Galaxy MISP | https://github.com/MISP/misp-galaxy/blob/main/clusters/disarm.json |

### Projets étudiants de référence (promotions précédentes)

| Projet | URL |
|---|---|
| DORA Taxonomy + Galaxy (2024–2025) | https://github.com/Malphas4/MISP_taxonomy_DORA_-_Galaxy |
| Galaxy of Modern Tanks | https://github.com/PorteEtoile/MIPS_Galaxy_Of_Modern_Tanks |
| Hazardous Substances | https://github.com/Lise-Lebrun/MISP_Hazardous_Substances |
| Cloud Security Threats Galaxy | https://github.com/Dyslate/MIPS-CloudSecurityThreatsGalaxy |

## 9. Méthode de travail
#### Étape 1 ─ Recherche bibliographique
  └─ lecture des articles listés
  └─ exploration des taxonomies MISP existantes
  └─ découverte de DISARM

#### Étape 2 ─ Structure
  └─ mise en place du plan
  └─ corrélation des articles

#### Étape 3 ─ Brouillon
  └─ explication globale voulue pour chaque partie
  └─ mise en place de la structure par paragraphe

#### Étape 4 ─ Enrichissement par IA
  └─ enrichissement des paragraphes (Claude, Gemini, ChatGPT)
  └─ relecture manuelle de chaque ajout
  └─ ajout des liens des sources à la main