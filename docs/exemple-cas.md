# Cas pratiques — Validation de la taxonomie Deepfake MISP

Afin de clarifier nos propos établis dans [l'état de l'art](https://github.com/IaParInterim/Deepfake-taxonomy-for-MISP/blob/main/docs/etat-de-lart.md) et notre [taxonomie](https://github.com/IaParInterim/Deepfake-taxonomy-for-MISP/blob/main/taxonomy/machinetag.json), nous avons décidé d'interpréter trois exemples d'incidents deepfakes, réels et documentés. 

L'objectif est donc de valider que les prédicats et valeurs choisis couvrent des situations concrètes, et illustrer comment un analyste utiliserait nos tags dans MISP.

## Cas n°1 : deepfake de Volodymyr Zelensky (mars 2022)

### Contexte

Le 16 mars 2022, trois semaines après le début de l'invasion russe en Ukraine, une vidéo circule sur les réseaux sociaux et apparaît sur le site hacké de la chaîne de télévision ukrainienne *Ukraine 24*. On y voit le président Zelensky, debout devant un pupitre aux couleurs ukrainiennes, appeler ses soldats à déposer les armes et capituler face aux forces russes. ([BBC](https://www.bbc.com/news/technology-60780142))

La vidéo est rapidement démentie : Zelensky publie dans la foulée une vidéo authentique depuis Kiev, et les plateformes (Facebook, YouTube) la retirent. Dès le 2 mars, le Centre ukrainien pour les communications stratégiques avait d'ailleurs prévenu que des deepfakes de ce type étaient en préparation.

### Analyse technique

Le Pr. Hany Farid (UC Berkeley), expert en forensique numérique, identifie dans un [article ](https://www.ischool.berkeley.edu/news/2022/hany-farid-explains-signs-zelenskyy-surrender-video-deepfake) publiquement plusieurs indices de falsification :

- **faible résolution intentionnelle** : technique classique pour masquer les distorsions générées frame par frame
- **absence de mouvement des bras et de rotation de la tête** : les modèles de l'époque peinent à générer des deepfakes convaincants dès que les mains passent devant le visage ou que la tête tourne
- **incohérences visuelles inter-frames** : artéfacts périodiques liés à la génération image par image
- **Vvix légèrement désynchronisée** : accent perçu comme anormal par des locuteurs natifs ukrainiens

La faible qualité du deepfake a contribué à sa détection rapide, ce qui illustre que la pertinence contextuelle (diffusé pendant un conflit armé, sur un canal piraté) peut amplifier l'impact d'un deepfake même techniquement médiocre.

### Tags MISP applicables

> format : `namespace:predicate="value"`, namespace = `deepfake`, prédicats issus du `machinetag.json`

```
# detection
deepfake:detection="visual-artifacts"
# → artéfacts de bords visibles, incohérences d'éclairage
deepfake:detection="temporal-analysis"
# → incohérences inter-frames, scintillement périodique 
deepfake:detection="multimodal-analysis"
# → légère désynchronisation audio détectée par des locuteurs natifs
deepfake:detection="source-attribution"
# → signatures fréquentielles GAN identifiées a posteriori par des experts

# generation
deepfake:generation="manipulation-alteration"
# → modification d'une vidéo réelle existante par face reenactment 

# intent
deepfake:intent="malicious-deception"
# → intention délibérée de manipuler l'opinion publique en période de conflit 

# impact
deepfake:impact="institutional-erosion"
# → tentative de saper la légitimité du gouvernement ukrainien 
deepfake:impact="large-scale-narrative-manipulation"
# → diffusion virale sur réseaux sociaux et site TV piraté
```
```
# Galaxie DISARM (complément au niveau campagne)
galaxy:DISARM="T0019 — Fabricate content"
galaxy:DISARM="T0002 — Facilitate State Propaganda"
```

## Cas n°2 : fraude multimodale contre Arup (janvier 2024, Hong Kong)

### Contexte

En janvier 2024, un employé du bureau hongkongais du cabinet d'ingénierie britannique Arup reçoit un email prétendument envoyé par le directeur financier (CFO) de l'entreprise, basé à Londres, lui demandant d'effectuer des transactions financières confidentielles. Méfiant, suspectant initialement un phishing, l'employé rejoint une visioconférence censée rassurer ses doutes.

Lors de cet appel, il reconnaît visuellement et vocalement le CFO ainsi que plusieurs collègues habituels. Toutes ces personnes sont en réalité des deepfakes générés à partir de vidéos publiques (conférences en ligne, réunions d'entreprise). Convaincu de l'authenticité de la réunion, l'employé effectue 15 virements vers 5 comptes bancaires différents, pour un total de 200 millions HKD (≈ 25,6 millions USD). La fraude n'est découverte qu'une semaine plus tard, lors d'un contact avec le siège londonien ([CNN](https://www.cnn.com/2024/02/04/asia/deepfake-cfo-scam-hong-kong-intl-hnk)).

Arup confirme officiellement les faits en mai 2024 : *"We can confirm that fake voices and images were used."* (Rob Greig, Global CIO d'Arup) ([CNN](https://edition.cnn.com/2024/05/16/tech/arup-deepfake-scam-loss-hong-kong-intl-hnk)).

### Analyse technique

Ce cas est techniquement remarquable à plusieurs titres :

- **deepfake multimodal complet** : voix *et* visage synthétisés simultanément pour plusieurs individus dans un même appel vidéo — une première documentée à Hong Kong
- **source du deepfake** : les enquêteurs établissent que les fraudeurs ont entraîné leurs modèles à partir de vidéos publiquement disponibles des dirigeants d'Arup (interviews, webinaires, réunions en ligne)
- **stratégie d'évitement de détection** : les deepfakes évitent les mouvements de tête complexes et limitent l'interaction directe avec la victime au-delà des présentations initiales — ce qui réduit les risques d'artefacts visibles
- **vecteur hybride** : email + WhatsApp + visioconférence en one-to-one préalable pour installer la confiance, puis réunion de groupe falsifiée

Les indices de détection qui auraient pu alerter l'employé : demander aux participants de tourner la tête (les deepfakes de l'époque déforment le visage lors des rotations), changer la source lumineuse, ou demander à un participant de tenir un objet devant son visage ([coverlink](https://coverlink.com/case-study/case-study-25-million-deepfake-scam/)).

### Tags MISP applicables

```
# detection
deepfake:detection="visual-artifacts"
# → artéfacts de fusion aux bords du visage, détectables a postériori
deepfake:detection="multimodal-analysis"
# → analyse croisée audio/vidéo permettant d'identifier des désynchronisations
deepfake:detection="source-attribution"
# → identification des modèles génératifs utilisés lors de l'enquête

# generation
deepfake:generation="manipulation-alteration"
# → visages réels manipulés à partir de vidéos publiques 
deepfake:generation="voice-cloning"
# → clonage vocal du CFO et de plusieurs collègues 
deepfake:generation="multimodal"
# → synthèse simultanée audio + vidéo pour plusieurs individus 

# intent
deepfake:intent="malicious-deception"
# → tromperie délibérée dans un contexte de fraude financière 

# impact
deepfake:impact="identity-theft"
# → usurpation de l'identité du CFO et de collègues réels 
```
```
# Galaxie DISARM (complément au niveau campagne)
galaxy:DISARM="T0087 — Create Inauthentic Video Content"
galaxy:DISARM="T0021 — Impersonate existing narrative"
```

## Cas n°3 : deepfake audio du proviseur d'un lycée de Baltimore (janvier 2024)

### Contexte

En janvier 2024, un clip audio circule parmi les parents et enseignants du lycée Pikesville (Baltimore, Maryland). On y entend Eric Eiswert, proviseur de l'établissement, tenir des propos racistes et antisémites à l'égard de collègues et d'élèves. Le clip est diffusé via les réseaux sociaux et provoque un tollé immédiat : Eiswert reçoit des menaces, est mis en retrait et contraint de changer de poste.

L'enquête policière révèle que le clip est un deepfake audio fabriqué par Dazhon Darien, le directeur sportif de l'établissement, qui cherchait à se venger d'Eiswert après avoir été mis en cause pour des malversations financières. Darien est arrêté en avril 2024 et inculpé pour création et diffusion de contenu frauduleux ([CNN](https://edition.cnn.com/2024/04/26/us/pikesville-principal-maryland-deepfake-cec/index.html)).

### Analyse technique

Ce cas illustre plusieurs aspects particulièrement importants pour notre taxonomie :

- **deepfake audio uniquement** : contrairement aux deux cas précédents, il n'y a pas de manipulation vidéo, le deepfake repose exclusivement sur le voice cloning à partir d'enregistrements existants de la voix d'Eiswert
- **acteur interne (insider threat)** : le créateur du deepfake connaissait sa cible, avait accès à des enregistrements de sa voix, et maîtrisait suffisamment l'outil pour produire un résultat convaincant
- **intention ciblée** : il ne s'agit pas de désinformation de masse mais d'une attaque personnalisée visant à détruire la réputation professionnelle d'un individu précis
- **dommages durables** : même après la révélation du deepfake, une partie du public continue de croire à l'authenticité du clip, illustration directe du liar's dividend ([Citron & Chesney](https://www.jstor.org/stable/26798018), 2019)

Du point de vue forensic, les indices qui ont permis l'identification : analyse spectrale (MFCC) montrant des patterns anormaux, comparaison avec des enregistrements authentiques connus d'Eiswert, et enquête sur les métadonnées du fichier [BBC](https://www.bbc.com/news/articles/ckg9k5dv1zdo).

### Tags MISP applicables

```
# detection
deepfake:detection="spatial-analysis"
# → analyse spectrale MFCC révélant des patterns anormaux dans le signal audio
deepfake:detection="feature-extraction-analysis"
# → comparaison de caractéristiques prosodiques avec des enregistrements authentiques 
deepfake:detection="source-attribution"
# → identification du modèle de voice cloning utilisé lors de l'enquête policière 

# generation
deepfake:generation="manipulation-alteration"
# → voice cloning à partir d'enregistrements réels existants 
deepfake:generation="voice-cloning"
# → synthèse vocale ciblant un individu précis à partir de peu d'échantillons 

# intent
deepfake:intent="character-assassination"
# → destruction délibérée de la crédibilité du proviseur 

# impact
deepfake:impact="identity-theft"
# → usurpation de la voix et de l'identité d'un individu privé 
deepfake:impact="institutional-erosion"
# → atteinte à la confiance envers la direction de l'établissement
```
```
# Galaxie DISARM (complément au niveau campagne)
galaxy:DISARM="T0019 — Fabricate content"
```

## Synthèse comparative

| | Cas 1 — Zelensky | Cas 2 — Arup | Cas 3 — Baltimore |
|---|---|---|---|
| **`generation`** | `manipulation-alteration` | `manipulation-alteration` + `voice-cloning` + `multimodal` | `manipulation-alteration` + `voice-cloning` |
| **`detection`** | `visual-artifacts` + `temporal-analysis` + `multimodal-analysis` | `visual-artifacts` + `multimodal-analysis` | `spatial-analysis` + `feature-extraction-analysis` |
| **`intent`** | `malicious-deception` | `malicious-deception` | `character-assassination` |
| **`impact`** | `institutional-erosion` + `large-scale-narrative-manipulation` | `identity-theft` | `identity-theft` + `institutional-erosion` |
| **DISARM** | T0019 + T0002 | T0087 + T0021 | T0019 |

## En résumé

Les quatre prédicats `generation`, `detection`, `intent` et `impact` couvrent les trois cas sans ambiguïté majeure. La valeur `multimodal` du prédicat `generation` est indispensable, le cas Arup ne peut pas être décrit sans elle. Le prédicat `detection` est le plus riche et le plus opérationnel : ses dix valeurs couvrent toutes les méthodes d'analyse rencontrées dans la littérature.
