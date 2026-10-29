# Résultats des exécutions — TP Agentic AI avec Gemini

> Ce document regroupe les sorties qui étaient enregistrées dans le notebook `AI_Agentic_Gemini(3).ipynb` au moment de sa réception. Les réponses du modèle sont reproduites telles qu’elles apparaissent dans le notebook. Les cellules sans sortie visible sont signalées comme telles. Aucune sortie manquante n’a été inventée.

## Résumé des tests

| Partie | Résultat observé |
|---|---|
| Configuration et agent simple | Réponses générées par Gemini |
| Sélection dynamique | Les sorties enregistrées mentionnent `gemini-3.6-flash` pour les contextes test et prod |
| Mémoire | L’agent se souvient du nom fourni dans la même conversation |
| Outils et Python REPL | La météo fictive et le calcul `5 + 9 = 14` fonctionnent |
| Gestion d’erreur | La division par zéro est transformée en message compréhensible |
| Prompt dynamique | Des réponses beginner/expert ont été générées |
| Sortie structurée | Nom, e-mail et téléphone extraits |
| Middleware / hooks | Les hooks avant et après le modèle sont visibles dans la sortie |
| RAG | L’agent retrouve les informations de la base documentaire de démonstration |
| PII, validation humaine et garde-fous | Les tests de masquage, d’approbation simulée et de blocage ont produit une sortie |
| Vérification finale | Agent prêt, modèle courant `gemini-3.5-flash-lite` |

### Remarque sur les avertissements

Certaines cellules affichent un avertissement de LangChain indiquant que `gemini-3.5-flash-lite` utilise des paramètres d’échantillonnage fixes et que `temperature` est ignoré. Une cellule RAG affiche aussi un avertissement de dépréciation concernant `langchain-community`. Ces avertissements sont conservés plus bas avec les sorties correspondantes.

## 0. Imports et configuration de Gemini

### Configuration initiale et test du modèle

*Cellule notebook : 3 — exécution n° 8*

**Résultat 1**

Un agent IA est un programme informatique autonome capable d'analyser son environnement, de prendre des décisions et de réaliser des actions pour atteindre un objectif précis.

## 1. Agent simple

### Présentation de l’agent

*Cellule notebook : 5 — exécution n° 9*

**Résultat 1**

Je suis un assistant virtuel conçu pour vous aider, répondre à vos questions et vous accompagner au quotidien. Mon objectif est de vous fournir des informations claires, précises et utiles à tout moment.

## 2. Configuration du modèle

### Explication de Python en trois points

*Cellule notebook : 7 — exécution n° 10*

**Résultat 1**

Voici ce qu'est Python en trois points simples :

1. **Un langage simple et facile à apprendre :** Python est un langage de programmation connu pour sa syntaxe claire et lisible, très proche de l'anglais. Il permet d'écrire du code rapidement avec moins de lignes que la plupart des autres langages, ce qui en fait le choix idéal pour les débutants.

2. **Extrêmement polyvalent :** C'est un langage "tout-terrain". On l'utilise aussi bien pour créer des sites web et automatiser des tâches répétitives, que pour des domaines avancés comme la science des données (Data Science), la finance ou l'intelligence artificielle (ex: ChatGPT a été développé principalement avec Python).

3. **Gratuit et soutenu par une immense communauté :** Python est un logiciel libre (*open-source*). Il dispose d'un écosystème gigantesque de "bibliothèques" (des modules de code prêts à l'emploi créés par la communauté) qui permettent de réaliser presque n'importe quel projet sans avoir à tout réinventer.

## 3. Sélection dynamique du modèle

### Appel avec contexte « test »

*Cellule notebook : 9 — exécution n° 11*

**Résultat 1**

Contexte=test; modèle=gemini-3.6-flash; plafond=4096 tokens; réflexion=low
Un **réseau neuronal** (ou réseau de neurones artificiels) est un système informatique inspiré du fonctionnement du **cerveau humain**. C'est la technologie clé derrière l'intelligence artificielle moderne et le *Deep Learning*.

Voici son fonctionnement résumé en 3 points :

1. **La Structure (Les couches) :** 
   Le réseau est composé de "neurones" virtuels connectés entre eux et organisés en couches :
   * **Couche d'entrée :** reçoit la donnée brute (ex: les pixels d'une photo).
   * **Couches cachées :** analysent l'information étape par étape (ex: détection des formes, des couleurs, puis des visages).
   * **Couche de sortie :** donne le résultat (ex: "C'est un chat").

2. **L'Apprentissage :** 
   Le réseau apprend par l'exemple. On lui montre des milliers d'images. Au début, il se trompe, mais il **ajuste la force de ses connexions** (appelées "poids") à chaque erreur pour devenir de plus en plus précis.

3. **À quoi ça sert ?**
   À reconnaître des schémas complexes dans de grands volumes de données. On l'utilise pour la reconnaissance vocale (Siri/Alexa), la traduction automatique, la détection de maladies sur des radios médicales ou les voitures autonomes.

**En résumé :** C'est un programme informatique qui apprend par lui-même à exécuter une tâche en s'entraînant sur des données, plutôt qu'en suivant des règles codées à la main.

### Appel avec contexte « prod »

*Cellule notebook : 10 — exécution n° 12*

**Résultat 1**

Contexte=prod; modèle=gemini-3.6-flash; plafond=8192 tokens; réflexion=low
Un **réseau de neurones artificiels** est un système informatique inspiré du fonctionnement du cerveau humain. C'est la technologie clé derrière l'intelligence artificielle moderne (l'apprentissage profond ou *Deep Learning*).

Voici comment cela fonctionne en résumé :

1. **La structure** : Il est composé de "neurones" virtuels organisés en **couches** :
   * **Couche d'entrée** : elle reçoit les données brutes (ex. : une photo).
   * **Couches cachées** : elles analysent les détails (ex. : formes, couleurs, contours) via des calculs mathématiques.
   * **Couche de sortie** : elle donne la réponse finale (ex. : "C'est un chat").

2. **L'apprentissage** : Au début, le réseau fait des erreurs. On lui donne des milliers d'exemples et, à chaque erreur, il ajuste la force des connexions entre ses neurones (appelées les "poids"). Avec la pratique, il devient de plus en plus précis.

**À quoi ça sert ?** 
À reconnaître des images, traduire des langues, conduire des voitures autonomes ou générer du texte (comme ChatGPT).

## 4. Mémoire de conversation

### Test de la mémoire de conversation

*Cellule notebook : 12 — exécution n° 14*

**Résultat 1**

Enchanté, Mouad ! Comment puis-je t'aider aujourd'hui ?
Tu t'appelles Mouad !

## 5. Outils (tools)

### Test de l’outil météo

*Cellule notebook : 14 — exécution n° 15*

**Résultat 1**

Voici un exemple de météo fictive pour Béni Mellal :

* **Conditions :** Ciel dégagé
* **Température :** 22 °C

*(Remarque : Il s'agit d'une donnée fictive générée pour la démonstration et non d'une information météo en temps réel.)*

## Configuration du modèle après le test de quota

### Changement du modèle principal

*Cellule notebook : 15 — exécution n° 20*

**Résultat 1**

Modèle configuré : gemini-3.5-flash-lite
Agent principal et configurations dynamiques mis à jour.

## 6. Recherche Web facultative avec DuckDuckGo

### Définition de l’outil de recherche Web

*Cellule notebook : 17 — exécution n° 21*

Aucune sortie affichée. La cellule définit l’outil de recherche Web ; elle n’exécute pas elle-même une recherche.

### Résultat de l’exécution de l’agent utilisant la recherche Web

**Résultat affiché dans le notebook**

Bien que la recherche en direct ait rencontré un problème technique momentané, je peux vous orienter vers les meilleurs tutoriels, concepts et frameworks de référence pour maîtriser l’**Agentic RAG** (*Retrieval-Augmented Generation* piloté par des agents).

L’Agentic RAG va au-delà du RAG traditionnel (linéaire : question → recherche → génération) en donnant à un LLM des capacités d’agent (planification, utilisation d’outils, auto-correction, boucles de rétroaction).

Voici où trouver les meilleurs tutoriels et comment aborder le sujet :

---

#### 1. Les tutoriels de référence par framework

Les deux écosystèmes majeurs pour le développement d’agents et de RAG proposent d’excellents tutoriels gratuits et documentés :

- **LlamaIndex (guides « Agentic RAG »)**
  - *Pourquoi y aller :* LlamaIndex est pionnier dans la gestion des données pour LLM. Leurs guides montrent comment créer des agents capables de choisir dynamiquement entre plusieurs sources de données, d’effectuer de la recherche récursive ou de valider les réponses.
  - *À chercher :* `LlamaIndex Agentic RAG tutorial`, `Advanced Agentic RAG workflows LlamaIndex`.
- **LangChain / LangGraph (tutoriels « Adaptive RAG » et « Corrective RAG »)**
  - *Pourquoi y aller :* LangGraph permet de structurer des agents sous forme de graphes d’état, ce qui est idéal pour l’Agentic RAG. Exemple : étape 1, analyser la question ; étape 2, chercher ; étape 3, évaluer la pertinence ; si les résultats sont insuffisants, reformuler et rechercher à nouveau.
  - *À chercher :* `LangChain Adaptive RAG tutorial`, `LangGraph Agentic RAG LangSmith`.

---

#### 2. Les approches clés à étudier dans les tutoriels

Un bon tutoriel d’Agentic RAG aborde généralement l’un de ces *design patterns* (popularisés notamment par des articles de recherche et des experts comme Harrison Chase ou Jerry Liu) :

1. **Router (routage) :** l’agent analyse la requête de l’utilisateur et décide de l’outil à utiliser : faut-il chercher dans une base vectorielle, lancer une recherche sur le Web, ou répondre directement sans document ?
2. **Corrective RAG (CRAG) :** l’agent évalue la qualité des documents récupérés *avant* de générer la réponse. Si les documents ne sont pas pertinents, il déclenche une recherche Web alternative.
3. **Self-RAG :** l’agent s’auto-critique pendant et après la génération pour s’assurer que la réponse est factuelle, s’appuie bien sur les sources et ne contient pas d’hallucinations.
4. **Recherche multi-pas (Multi-hop RAG) :** l’agent pose une première question, lit le résultat, puis formule une *deuxième* question plus précise basée sur la première découverte.

---

#### 3. Où trouver des exemples de code (GitHub / YouTube) ?

- **GitHub :** recherchez des dépôts comme `langchain-ai/langgraph` ou `run-llama/llama_index` qui contiennent des dossiers `examples/` ou `cookbook/` dédiés aux architectures d’agents autonomes.
- **YouTube :** des chaînes axées sur l’IA appliquée (comme *Rabbitmetrics*, *Codebasics*, ou les webinaires officiels de *LangChain* et *LlamaIndex*) publient régulièrement des tutoriels pas-à-pas de type `Build an Agentic RAG from Scratch`.

Si vous le souhaitez, je peux vous rédiger directement un exemple de code conceptuel en Python (avec LangChain ou LlamaIndex) pour illustrer la structure de base d’un Agentic RAG. Quel framework préférez-vous utiliser ?

> **Note sur le résultat :** la réponse de l’agent indique que la recherche en direct a rencontré un problème technique. Elle fournit donc des pistes générales et des termes de recherche, mais ne présente pas de résultats Web récupérés avec des liens vérifiés. La cellule a produit une réponse, sans démontrer que la recherche DuckDuckGo a effectivement renvoyé des résultats exploitables.

## 7. Outil Python REPL

### Test direct du Python REPL

*Cellule notebook : 19 — exécution n° 22*

**Résultat 1**

14

### Configuration de l’agent Python REPL

*Cellule notebook : 20 — exécution n° 43*

**Résultat 1**

Modèle : gemini-3.5-flash-lite
Délai d'attente : 120 secondes
Agent Python REPL recréé.

### Test de calcul avec l’agent

*Cellule notebook : 21 — exécution n° 44*

**Résultat 1**

La somme de 5 et 9 est 14.

## 8. Gestion des erreurs des outils

### Test de gestion de la division par zéro

*Cellule notebook : 23 — exécution n° 24*

**Sortie technique 1**

```text
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
```

**Résultat 2**

La division par zéro est impossible (division par zéro). C'est pourquoi l'outil a renvoyé une erreur.

## 9. System prompt dynamique selon le rôle

### Test de la réponse pour le rôle débutant

*Cellule notebook : 25 — exécution n° 25*

**Sortie technique 1**

```text
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
```

**Résultat 2**

Bonjour ! C'est une excellente question. 

Pour comprendre ce qu'est un **agent IA (Intelligence Artificielle)**, imagine un petit robot virtuel ou un assistant intelligent. 

### 🌟 Explication simple
Un agent IA est un programme informatique conçu pour **observer** son environnement, **réfléchir** à la situation et **agir** de manière autonome pour atteindre un objectif précis. 

Contrairement à un logiciel classique qui fait <u>toujours</u> exactement la même chose (comme une calculatrice), un agent IA peut s'adapter, apprendre de ses erreurs et prendre des décisions tout seul.

---

### 📖 Définition des termes clés
Pour bien saisir le concept, voici comment fonctionne un agent IA (le modèle "Perception-Raisonnement-Action") :
1. **L'environnement :** C'est le monde (réel ou virtuel) dans lequel évolue l'agent.
2. **La perception :** C'est la façon dont l'agent "voit" ou "entend" son environnement (grâce à des capteurs, des micros, ou en lisant des données).
3. **Le raisonnement :** C'est le "cerveau" de l'agent. Il analyse les données perçues pour décider de la meilleure chose à faire.
4. **L'action :** C'est ce que fait l'agent pour se rapprocher de son but (afficher un message, bouger, envoyer un email, etc.).

---

### 🚗 Un exemple concret : La voiture autonome
Pense à une voiture sans conducteur (comme une Tesla). C'est un **agent IA**.

* **Son objectif :** T'emmener d'un point A à un point B en sécurité.
* **Sa perception :** Elle utilise des caméras, des radars et des capteurs pour "voir" les autres voitures, les piétons, les feux tricolores et les panneaux.
* **Son raisonnement :** Le "cerveau" informatique analyse les images : *"Oh, un piéton traverse la rue !"*. Il calcule qu'il faut freiner.
* **Son action :** La voiture envoie l'ordre aux freins de s'activer pour s'arrêter.

**En résumé :** Un agent IA, c'est un système qui perçoit, réfléchit et agit pour nous aider ou accomplir une tâche de façon autonome. 

Est-ce que c'est plus clair pour toi ? N'hésite pas si tu as d'autres questions !

### Test de la réponse pour le rôle expert

*Cellule notebook : 26 — exécution n° 42*

**Sortie technique 1**

```text
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
```

**Résultat 2**

En intelligence artificielle, un **agent IA** est une entité autonome conçue pour percevoir son environnement au moyen de capteurs, traiter ces informations, prendre des décisions et agir sur cet environnement à l'aide d'actionneurs afin d'atteindre un objectif spécifique ou d'optimiser une fonction de performance.

Voici une décomposition détaillée de sa structure, de son fonctionnement et de sa taxonomie selon les standards de l'ingénierie des systèmes autonomes.

---

### 1. Architecture fondamentale d'un Agent IA

L'architecture d'un agent peut se résumer par la fonction mathématique suivante : à partir d'une séquence de perceptions (l'historique de ce que l'agent a capté depuis le début), l'agent associe une action. 

$$\text{Action} : P^* \rightarrow A$$
*(où $P^*$ est l'espace des séquences de perceptions et $A$ l'espace des actions).*

Dans le détail, un agent est généralement composé de quatre modules principaux :

*   **Les Capteurs (Perception) :** Ils permettent à l'agent de collecter des données brutes de son environnement (ex: flux vidéo, requêtes textuelles, flux de données IoT, API).
*   **Le Module de Traitement (État et Raisonnement) :** Il analyse les entrées. Selon la complexité de l'agent, il peut s'agir de simples règles logiques (If/Then) ou de modèles probabilistes et neuronaux profonds (Deep Learning, LLM). Ce module maintient souvent un **modèle du monde** (une représentation interne de l'état actuel de l'environnement).
*   **Le Moteur de Décision (Planification) :** En fonction de l'état perçu et de sa fonction objective (sa "récompense" ou son but), l'agent évalue différentes trajectoires d'action et sélectionne la plus optimale.
*   **Les Actionneurs (Action) :** Ils exécutent la décision dans l'environnement (ex: générer une réponse textuelle, exécuter du code, déplacer un bras robotique, envoyer une commande à un actionneur physique).

---

### 2. Taxonomie des Agents IA

On classe généralement les agents IA selon leur degré de complexité et d'autonomie :

#### A. Agents réactifs simples
Ces agents agissent selon des règles conditionnelles strictes du type **Condition-Action** (ou *réflexes*). Ils ne possèdent pas de mémoire de l'historique des perceptions et fonctionnent uniquement en se basant sur l'état actuel de l'environnement.
*   *Exemple :* Un thermostat intelligent qui déclenche le chauffage si la température descend sous un seuil.

#### B. Agents réactifs fondés sur des modèles
Contrairement aux précédents, ces agents intègrent un **état interne**. Ils gardent en mémoire une partie de l'historique pour gérer l'incomplètetude des perceptions (lorsque l'environnement n'est que partiellement observable).
*   *Exemple :* Un véhicule autonome qui doit estimer la position des angles morts (qu'il ne voit plus) en se basant sur sa trajectoire passée.

#### C. Agents fondés sur des objectifs (*Goal-based agents*)
En plus d'un état interne, ces agents possèdent un objectif explicite. Ils ne se contentent pas de réagir ; ils planifient leurs actions en évaluant plusieurs scénarios futurs pour déterminer lequel mène à l'objectif.
*   *Exemple :* Un agent de routage GPS qui calcule non pas la prochaine intersection, mais l'itinéraire complet pour arriver à destination.

#### D. Agents fondés sur l'utilité (*Utility-based agents*)
Atteindre un objectif binaire (réussi/échoué) ne suffit pas toujours. Ces agents utilisent une **fonction d'utilité** qui mesure à quel point un état est "désirable" (mesure de la performance, du coût, du risque). Cela leur permet de choisir entre plusieurs objectifs conflictuels ou d'optimiser le compromis qualité/rapidité.
*   *Exemple :* Un agent de trading algorithmique qui cherche à maximiser le profit tout en minimisant le risque de perte (gestion de portefeuille).

#### E. Agents apprenants (*Learning agents*)
Ces agents possèdent la capacité d'améliorer leurs performances au fil du temps grâce à l'expérience. Ils séparent l'élément d'apprentissage (qui modifie l'agent pour faire mieux) de l'élément de performance (qui choisit les actions).
*   *Technologie clé :* L'apprentissage par renforcement (*Reinforcement Learning*), où l'agent maximise un signal de récompense (*reward*) par essais-erreurs.

---

### 3. Les Agents IA modernes (Ere des LLM)

Récemment, l'émergence des **LLM (Large Language Models)** a donné naissance à une nouvelle classe d'agents dits "cognitifs" ou "autonomes" (ex: AutoGPT, LangChain Agents). 

Dans cette architecture :
*   Le LLM sert de **cerveau central** pour le raisonnement en langage naturel et la planification.
*   L'agent est doté d'**outils** (*tools*) : accès à des moteurs de recherche, interpréteurs de code Python, API externes.
*   Il utilise des boucles de rétroaction de type **ReAct** (*Reason + Act*) : il réfléchit à une tâche, écrit un plan, exécute une action (ex: lancer une requête SQL), analyse le résultat, puis ajuste son plan jusqu'à résolution complète de la tâche.

### Résumé technique
Un agent IA est donc un système cybernétique logiciel ou matériel capable de boucler dynamiquement les phases de **Perception $\rightarrow$ Cognition $\rightarrow$ Action** dans le but de maximiser une métrique de succès au sein d'un environnement stochastique ou déterministe.

## 10. Sortie structurée

### Extraction des données de contact

*Cellule notebook : 28 — exécution n° 27*

**Sortie technique 1**

```text
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
```

**Résultat 2**

Mouad Aguelloul
mouad@example.com
+212 600 000 000

## 11. Middleware personnalisé et hooks

### Exécution et ordre des hooks

*Cellule notebook : 30 — exécution n° 28*

**Résultat 1**

[hook] before_agent
[hook] before_model

**Sortie technique 2**

```text
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
```

**Résultat 3**

[hook] after_model
[hook] after_agent — durée approx. 2.19 s
12 + 30 font **42**.

## 12. RAG comme outil de l’agent

### Création de la base documentaire et recherche initiale

*Cellule notebook : 32 — exécution n° 29*

**Sortie technique 1**

```text
C:\Users\mouad\AppData\Local\Temp\ipykernel_1560\1269673553.py:1: DeprecationWarning: `langchain-community` is being sunset and is no longer actively maintained. See https://github.com/langchain-ai/langchain-community/issues/674 for details and migration guidance toward standalone integration packages.
  from langchain_community.vectorstores import FAISS
```

**Résultat 2**

- Le RAG recherche des passages pertinents avant de générer une réponse.
- Les données du projet sont stockées dans une base documentaire de démonstration.
- LangChain permet de connecter un modèle à des outils Python.

### Réponse de l’agent à partir du RAG

*Cellule notebook : 33 — exécution n° 30*

**Sortie technique 1**

```text
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
c:\Users\mouad\OneDrive\Desktop\AIDC\2emeannee\S3\DSD\Tps\AI_Agentic_Gemini\.venv\Lib\site-packages\langchain_google_genai\chat_models.py:3919: UserWarning: Model 'gemini-3.5-flash-lite' uses fixed sampling defaults; the sampling parameter(s) temperature will be ignored.
  request = self._build_request_config(
```

**Résultat 2**

**RAG** signifie **Retrieval-Augmented Generation** (génération augmentée par récupération). Il s'agit d'une technique qui consiste à rechercher des passages pertinents dans une base documentaire avant de les transmettre à un modèle pour générer une réponse précise.

La bibliothèque utilisée pour indexer les vecteurs et accélérer la recherche de similarité dans cette base est **FAISS**.

## 13. Garde-fous : détection de données personnelles (PII)

### Test de masquage des données personnelles

*Cellule notebook : 35 — exécution n° 33*

**Résultat 1**

Je ne peux pas voir votre e-mail ni les chiffres de votre carte car ces informations ont été masquées pour des raisons de sécurité. 

Il semble que vous ayez effectué un test en utilisant des données fictives, mais en l'absence de détails sur le contexte ou le système utilisé, je ne peux pas expliquer ce qui s'est passé. Pouvez-vous préciser de quel outil ou application il s'agit ?

## 14. Human-in-the-loop : validation avant l’outil

### Interruption en attente de validation humaine

*Cellule notebook : 37 — exécution n° 36*

**Sortie technique 1**

```text
[Interrupt(value={'action_requests': [{'name': 'send_email', 'args': {'email_address': 'mouadag10@gmail.com'}, 'description': "Tool execution requires approval\n\nTool: send_email\nArgs: {'email_address': 'mouadag10@gmail.com'}"}], 'review_configs': [{'action_name': 'send_email', 'allowed_decisions': ['approve', 'edit', 'reject', 'respond']}]}, id='2246cf67a1b5870929596436a20d50f2', response_schema=None)]
```

### Résultat après approbation de l’outil simulé

*Cellule notebook : 38 — exécution n° 37*

**Résultat 1**

[SIMULATION] Envoi d'e-mail à mouadag10@gmail.com
L'appel à l'outil `send_email` a été effectué pour l'adresse **mouadag10@gmail.com** (simulation pédagogique, aucun e-mail réel n'a été envoyé).

## 15. Garde-fou personnalisé avant l’agent

### Test du blocage par mot-clé

*Cellule notebook : 40 — exécution n° 38*

**Résultat 1**

Cette requête a été bloquée par le filtre de démonstration.

## 16. Garde-fou après l’agent

### Évaluation de la réponse finale

*Cellule notebook : 42 — exécution n° 39*

**Résultat 1**

Un **pare-feu** (ou *firewall* en anglais) est un système de sécurité informatique qui sert de **barrière de protection** entre votre ordinateur (ou votre réseau local) et l'extérieur (généralement Internet). 

Pour faire simple, il agit comme un **gardien de sécurité** ou un agent de douane à l'entrée de votre réseau. Son rôle principal est de filtrer le trafic de données pour empêcher les intrus d'entrer et d'empêcher les données sensibles de sortir sans autorisation.

Voici en détail à quoi il sert :

### 1. Contrôler le trafic entrant et sortant
Le pare-feu analyse toutes les données (sous forme de paquets) qui tentent d'entrer ou de sortir de votre appareil. Pour ce faire, il s'appuie sur des **règles de sécurité** prédéfinies :
* **Ce qui est autorisé :** Par exemple, il laisse passer le trafic de votre navigateur web (quand vous allez sur Google ou YouTube) parce que c'est vous qui l'avez demandé.
* **Ce qui est bloqué :** Il bloque les connexions suspectes ou non sollicitées provenant d'inconnus sur Internet qui essaient de pirater votre machine.

### 2. Bloquer les accès non autorisés (Piratage)
Si un hacker essaie de scanner votre ordinateur depuis Internet pour trouver une faille, le pare-feu rend votre machine "invisible" ou bloque l'accès, empêchant ainsi l'intrusion.

### 3. Empêcher la fuite de données (Malwares)
Si un logiciel malveillant (comme un virus ou un *trojan*) s'installe par mégarde sur votre ordinateur, il essaiera souvent de se connecter à Internet pour envoyer vos données personnelles (mots de passe, numéros de carte bancaire) à un pirate. Un bon pare-feu détecte cette tentative de communication anormale et **bloque la sortie**.

### 4. Protéger contre les attaques par déni de service (DDoS)
Le pare-feu peut bloquer des inondations de requêtes malveillantes conçues pour faire planter un serveur ou une connexion Internet.

---

### Les deux types de pare-feu

1. **Le pare-feu logiciel :** C'est celui qui est directement installé sur votre système d'exploitation (comme *Windows Defender Firewall* sur Windows ou le pare-feu intégré à macOS). Il protège un seul appareil.
2. **Le pare-feu matériel :** C'est un équipement physique indépendant (souvent intégré à votre box Internet ou routeur) qui protège **tous** les appareils connectés à votre réseau domestique ou d'entreprise.

### En résumé
Sans pare-feu, connecter un ordinateur à Internet, c'est un peu comme laisser la porte d'entrée de sa maison grand ouverte dans une grande ville. Le pare-feu ferme la porte aux étrangers malveillants tout en vous laissant aller et venir à votre guise.

## 17. Vérification finale

### État final du projet

*Cellule notebook : 44 — exécution n° 40*

**Résultat 1**

Agent prêt. Modèle utilisé : gemini-3.5-flash-lite
Vérifie les versions installées avec : uv tree

## Conclusion

D’après les sorties conservées, les tests principaux se sont terminés et l’agent a affiché le message final `Agent prêt. Modèle utilisé : gemini-3.5-flash-lite`. Les avertissements signalés ci-dessus ne sont pas, à eux seuls, des échecs d’exécution. La cellule de recherche Web n’a pas de résultat enregistré, car elle se limite à définir un outil.
