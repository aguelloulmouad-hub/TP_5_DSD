<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.11" />
  <img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter Notebook" />
  <img src="https://img.shields.io/badge/uv-Environment-6C3FB5?style=for-the-badge&logo=astral&logoColor=white" alt="uv" />
  <img src="https://img.shields.io/badge/Google-Gemini--API-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white" alt="Google Gemini API" />
  <img src="https://img.shields.io/badge/LangChain-Agentic--AI-1C3C3C?style=for-the-badge&logo=chainlink&logoColor=white" alt="LangChain Agentic AI" />
  <img src="https://img.shields.io/badge/FAISS-Vector--Database-0A7B83?style=for-the-badge" alt="FAISS Vector Database" />
  <img src="https://img.shields.io/badge/LangGraph-Middleware--Hooks-2E6F95?style=for-the-badge" alt="LangGraph Middleware and Hooks" />
  <img src="https://img.shields.io/badge/Pydantic-Structured--Output-E92063?style=for-the-badge&logo=pydantic&logoColor=white" alt="Pydantic Structured Output" />
  <img src="https://img.shields.io/badge/LLM-Judge--Evaluation-0A7B83?style=for-the-badge" alt="LLM Judge Evaluation" />
  <img src="https://img.shields.io/badge/Status-Terminé-success?style=for-the-badge" alt="Status" />
</p>

# TP — Agentic AI avec LangChain et Gemini

## 1. Présentation

Ce TP met en pratique la création d'agents IA avec **LangChain**, **LangGraph** et l'API **Gemini** de Google. Un agent peut recevoir une requête, consulter le modèle de langage, choisir un outil Python, exploiter une mémoire de conversation et appliquer des contrôles avant ou après son exécution.

Le notebook a été adapté pour utiliser Gemini à la place d'un modèle exécuté localement avec Ollama. Les requêtes au modèle et les embeddings sont donc envoyés à l'API Google : une connexion Internet et une clé API valide sont nécessaires.

## 2. Objectifs

- Créer un agent à partir d'un modèle et d'un prompt système.
- Configurer les paramètres de génération et sélectionner une configuration selon le contexte.
- Conserver l'historique d'une conversation.
- Déclarer des outils Python et laisser l'agent décider quand les appeler.
- Gérer les erreurs d'outil.
- Adapter les instructions selon le profil de l'utilisateur.
- Obtenir des résultats structurés avec Pydantic.
- Observer le cycle d'exécution grâce aux middlewares/hooks.
- Réaliser un RAG avec des embeddings Gemini et une base vectorielle FAISS.
- Tester la détection de données personnelles, les validations humaines et des garde-fous personnalisés.

## 3. Environnement utilisé

- Python 3.11.4 (environnement virtuel créé avec `uv`)
- `uv` pour gérer le projet et les dépendances
- Jupyter Notebook / VS Code pour exécuter les cellules
- LangChain et LangGraph pour les agents et les middlewares
- `langchain-google-genai` pour accéder à Gemini
- `python-dotenv` pour lire la clé API depuis `.env`
- `langchain-experimental` pour le Python REPL
- `ddgs` pour essayer la recherche DuckDuckGo
- `FAISS` et `GoogleGenerativeAIEmbeddings` pour la partie RAG

### Installation reproductible

Dans PowerShell, à la racine du projet :

```powershell
uv init --python 3.12
uv add langchain langchain-google-genai langgraph ipykernel jupyterlab langchain-experimental ddgs langchain-community faiss-cpu python-dotenv
uv run jupyter lab
```

Si le projet et son environnement existent déjà, il n'est pas nécessaire de relancer `uv init`.

Créer le fichier `.env` à la racine du projet, au même niveau que `.venv` :

```dotenv
GOOGLE_API_KEY=VOTRE_CLE_API_GEMINI
```

La clé se crée depuis [Google AI Studio](https://aistudio.google.com/api-keys). Elle ne doit pas être ajoutée au notebook ni publiée sur GitHub. Le fichier `.gitignore` doit contenir au minimum :

```gitignore
.env
.venv/
```

## 4. Modèle Gemini et configuration

Le notebook utilise **`gemini-3.5-flash-lite`** comme modèle de langage pour les agents et les exemples du TP. Le modèle est configuré avec `ChatGoogleGenerativeAI` et la clé API est chargée depuis le fichier `.env`.

Pendant les essais, des erreurs temporaires d'indisponibilité du service (`503`), de quota (`429 RESOURCE_EXHAUSTED`) et de délai d'attente (`504 DEADLINE_EXCEEDED`) ont été rencontrées. Le test de connexion avec `gemini-3.5-flash-lite` a ensuite réussi. Ces erreurs montrent que la disponibilité et les quotas de l'API peuvent affecter l'exécution, sans indiquer nécessairement un défaut dans le code du TP.

Le modèle peut afficher un avertissement indiquant que certains paramètres d'échantillonnage, comme `temperature`, ne sont pas pris en compte. Ces avertissements n'ont pas empêché les tests de fonctionner.

## 5. Réalisation et résultats

### 5.1 Agent simple

Un agent a été créé sans outil avec un prompt système lui demandant de répondre clairement en français.

**Résultat observé :** Gemini s'est présenté comme un assistant virtuel conçu pour répondre aux questions et fournir des informations utiles. Le test confirme le fonctionnement de l'invocation d'un agent simple.

### 5.2 Configuration du modèle

Le notebook teste une génération limitée à trois points pour expliquer Python.

**Résultat observé :** la réponse a présenté Python comme un langage simple à apprendre, polyvalent et open source, avec un vaste écosystème de bibliothèques. La réponse a été générée sans erreur visible.

### 5.3 Sélection dynamique de configuration

Le middleware `@wrap_model_call` lit le contexte `env` et choisit une configuration `basic_model` ou `advanced_model` :

- contexte `test` : plafond de 4 096 tokens ;
- contexte `prod` : plafond de 8 192 tokens.

**Résultat observé :** les contextes `test` et `prod` ont tous deux produit une explication des réseaux neuronaux. Le modèle reste `gemini-3.5-flash-lite` ; la différence porte sur la configuration et la limite de sortie.

### 5.4 Mémoire de conversation

L'agent utilise `InMemorySaver` avec le même identifiant de conversation (`thread_id`) entre deux appels.

**Résultat observé :** après le message « Je m'appelle Mouad », l'agent a répondu à la question suivante « Comment je m'appelle ? » par « Tu t'appelles Mouad ! ». La mémoire fonctionne pendant la durée du processus. Elle est temporaire et disparaît lorsque le processus est arrêté.

### 5.5 Outils Python

Trois outils de démonstration sont déclarés avec `@tool` : une recherche dans des résultats fictifs, une météo fictive et un profil fictif d'employé.

**Résultat observé :** l'agent a appelé l'outil météo pour Béni Mellal et renvoyé « ciel dégagé, 22 °C », en précisant que cette donnée est fictive et non issue d'une source météo en temps réel.

### 5.6 Recherche Web DuckDuckGo

Un outil `web_search` s'appuyant sur `DDGS` a été défini puis testé avec la question : « Recherche des tutoriels sur Agentic RAG ».

**Résultat observé :** l'agent a bien renvoyé une réponse textuelle détaillée. Il a proposé les ressources et pistes suivantes :

- les tutoriels Agentic RAG de **LlamaIndex** ;
- les approches **Adaptive RAG** et **Corrective RAG** avec **LangChain/LangGraph** ;
- les concepts **Router**, **Corrective RAG (CRAG)**, **Self-RAG** et **Multi-hop RAG** ;
- les dépôts GitHub `langchain-ai/langgraph` et `run-llama/llama_index` ;
- des tutoriels vidéo, notamment ceux de Rabbitmetrics, Codebasics et les webinaires de LangChain/LlamaIndex.

La réponse indique toutefois explicitement : « Bien que la recherche en direct ait rencontré un problème technique momentané... ». Le résultat fourni est donc une **réponse de repli utile générée par l'agent**, et non une liste de résultats Web dont les titres, URL et sources auraient été affichés et vérifiés par l'outil.

**Évaluation :** la cellule a produit une réponse et le scénario a été exécuté, mais la récupération de résultats Web en direct n'est pas entièrement validée. Pour démontrer cette fonctionnalité sans ambiguïté, il faudrait afficher directement les résultats de `web_search` (titres, liens et extraits) avant de laisser l'agent les résumer.

### 5.7 Python REPL

Le TP définit un outil Python REPL pour exécuter des calculs simples.

**Résultats observés :**

- l'exécution directe de `a = 5; b = 9; print(a + b)` renvoie `14` ;
- l'agent, lorsqu'on lui demande de calculer la somme de 5 et 9 avec Python, répond « La somme de 5 et 9 est 14. »

Le REPL peut exécuter du code Python. Dans ce TP, il doit rester limité aux calculs de démonstration et ne pas recevoir de code non fiable.

### 5.8 Gestion des erreurs d'outils

L'outil `divide` lève une exception si le diviseur vaut zéro. Le middleware `@wrap_tool_call` intercepte l'exception et transmet un message d'erreur compréhensible à l'agent.

**Résultat observé :** pour `10 / 0`, l'agent explique que la division par zéro est impossible. Le test démontre que l'erreur est transformée en message au lieu de rester une exception brute affichée à l'utilisateur.

### 5.9 Prompt dynamique selon le rôle

Le middleware `@dynamic_prompt` utilise `user_role` pour choisir des instructions différentes.

- `beginner` : explications simples, définitions et exemples ;
- `expert` : terminologie et explications techniques.

**Résultat observé :** pour la même question « Explique un agent IA », le profil débutant a reçu une explication intuitive illustrée par une voiture autonome, tandis que le profil expert a obtenu une réponse plus technique sur l'architecture, les perceptions, les actions et les types d'agents.

### 5.10 Sortie structurée avec Pydantic

Le schéma `ContactInfo` contient trois champs : `name`, `email` et `phone`. L'agent utilise `ToolStrategy` pour structurer l'information extraite.

**Résultat observé :**

```text
Mouad Aguelloul
mouad@example.com
+212 600 000 000
```

Les champs sont récupérés séparément, plutôt que dans une réponse textuelle libre.

### 5.11 Middleware personnalisé et hooks

La classe `HooksDemo` journalise les moments clés du cycle de l'agent.

**Résultat observé :**

```text
[hook] before_agent
[hook] before_model
[hook] after_model
[hook] after_agent — durée approx. 2.19 s
12 + 30 font 42.
```

Le test confirme les hooks `before_agent`, `before_model`, `after_model` et `after_agent`. Comme cet agent n'a aucun outil, cette cellule ne passe pas par `wrap_tool_call`. Les wrappers de modèle et d'outil sont démontrés séparément dans les autres sections.

### 5.12 RAG avec FAISS

Cinq textes de démonstration sont transformés en vecteurs par `GoogleGenerativeAIEmbeddings` avec le modèle `gemini-embedding-001`. FAISS indexe les vecteurs et le retriever récupère les trois passages les plus proches (`k=3`). L'agent peut ensuite interroger ce retriever comme un outil.

**Résultats observés :** la recherche a renvoyé des passages sur le RAG, la base documentaire et LangChain. L'agent a expliqué que RAG signifie **Retrieval-Augmented Generation** (génération augmentée par récupération) et que **FAISS** indexe les vecteurs.

**Limite :** la base documentaire est petite et créée uniquement pour le TP. Les résultats démontrent le mécanisme, pas la qualité d'un système RAG de production. Un avertissement de dépréciation de `langchain-community` a également été affiché.

### 5.13 Protection des données personnelles (PII)

Le middleware `PIIMiddleware` est configuré pour masquer les e-mails (`redact`) et masquer les numéros de carte (`mask`) avant l'appel au modèle.

**Résultat observé :** l'agent a indiqué que l'e-mail et les chiffres de la carte de test avaient été masqués. Le test illustre le mécanisme, mais il ne prouve pas à lui seul que toutes les formes de données personnelles seraient détectées dans tous les contextes.

### 5.14 Human-in-the-loop

Le middleware `HumanInTheLoopMiddleware` interrompt l'agent avant l'appel d'un outil `send_email`. Une décision d'approbation est ensuite envoyée avec `Command(resume=...)`.

**Résultats observés :**

1. L'agent s'est interrompu et a présenté l'appel à examiner.
2. Après l'approbation, le message `[SIMULATION] Envoi d'e-mail...` est apparu.
3. Le résultat a précisé qu'aucun e-mail réel n'avait été envoyé.

Cet outil est volontairement simulé ; il ne réalise aucune opération d'envoi réelle.

### 5.15 Garde-fou avant l'agent

Le hook `@before_agent` bloque les requêtes qui contiennent les expressions de démonstration `mot-interdit-demo` ou `requete-bloquee-demo`.

**Résultat observé :**

```text
Cette requête a été bloquée par le filtre de démonstration.
```

Le blocage a lieu avant le traitement normal par le modèle. Ce filtre est déterministe mais limité aux mots configurés ; il ne représente pas une modération générale.

### 5.16 Garde-fou après l'agent

Le hook `@after_agent` envoie la réponse finale à un modèle utilisé comme juge, qui doit renvoyer `SAFE` ou `UNSAFE`. La cellule demande une explication sur le pare-feu informatique.

**Résultat observé :** l'agent a fourni une explication du pare-feu et de son rôle dans le filtrage du trafic, la prévention d'accès non autorisés et la réduction de certains risques réseau. La réponse a été conservée.

**Limite :** la sortie enregistrée ne montre pas explicitement l'étiquette `SAFE` ni un cas rejeté `UNSAFE`. Le test prouve que le chemin d'une réponse sûre a fonctionné, mais pas que le rejet d'une réponse dangereuse a été démontré.

## 6. Schéma conceptuel des hooks

Le schéma du TP représente le cycle suivant :

1. `before_agent` : traitement initial de l'invocation.
2. `before_model` : contrôle avant l'appel au modèle.
3. `wrap_model_call` : enveloppe l'appel au modèle.
4. `wrap_tool_call` : enveloppe l'exécution d'un outil lorsqu'un outil est appelé.
5. `after_model` : traitement après la réponse du modèle.
6. `after_agent` : traitement final de l'agent.

Les appels aux outils et au modèle peuvent former plusieurs cycles jusqu'à ce que l'agent termine. Dans ce TP, les hooks sont démontrés dans plusieurs sections et non tous dans une seule invocation.

## 7. Problèmes et limites rencontrés

- **Erreur `503 UNAVAILABLE` :** le service Gemini a temporairement indiqué une forte demande.
- **Erreur `404 NOT_FOUND` :** un identifiant de modèle testé n'était pas disponible pour le projet utilisé.
- **Erreur `429 RESOURCE_EXHAUSTED` :** le quota gratuit journalier de l'API a été atteint. Ce type d'erreur dépend du quota associé au projet et au modèle.
- **Erreur `504 DEADLINE_EXCEEDED` :** un appel au modèle a dépassé le délai alloué. Le délai a ensuite été augmenté pour l'exemple Python REPL.
- **Avertissements `temperature` :** `gemini-3.5-flash-lite` indique que certains paramètres d'échantillonnage sont ignorés. Ce sont des avertissements et non des échecs de génération.
- **Recherche Web :** l'agent a fourni une réponse de repli détaillée malgré un problème technique de recherche en direct. La sortie n'affiche toutefois pas les résultats bruts avec titres et URL ; la récupération en direct reste à vérifier.
- **PII et garde-fou final :** les sorties sont cohérentes avec le scénario de test, mais des tests négatifs supplémentaires seraient nécessaires pour valider le blocage dans des cas non sûrs.

Le quota et la disponibilité de Gemini dépendent du projet Google et peuvent varier. Consulter [les limites de l'API Gemini](https://ai.google.dev/gemini-api/docs/rate-limits) avant de relancer de nombreuses cellules.

## 8. Conclusion

Le TP a permis de mettre en pratique les principaux composants d'un agent IA : modèle, prompt, mémoire, outils, middlewares, recherche documentaire et garde-fous. Les sorties enregistrées confirment le fonctionnement des agents simples, de la mémoire, des outils simulés, du Python REPL, des prompts dynamiques, des sorties structurées, des hooks principaux, du RAG et de l'approbation humaine.

Les limites principales concernent la validation complète de la recherche Web en direct (la cellule a fourni une réponse de repli mais pas de résultats bruts vérifiables), la vérification complète du garde-fou après l'agent et la nécessité de conserver une configuration unique avec `gemini-3.5-flash-lite`. Pour une livraison reproductible, il est conseillé de fixer le modèle dans la première cellule de configuration et de relancer le notebook du début à la fin.

## 9. Références utiles

- [Google AI Studio — création de clé API](https://aistudio.google.com/api-keys)
- [Documentation Gemini API](https://ai.google.dev/gemini-api/docs)
- [Documentation LangChain](https://docs.langchain.com/)
- [LangGraph — dépôt GitHub](https://github.com/langchain-ai/langgraph)
- [LlamaIndex — dépôt GitHub](https://github.com/run-llama/llama_index)
- [Quotas et limites Gemini](https://ai.google.dev/gemini-api/docs/rate-limits)

---

<p align="center">
  <strong>Réalisé par : Aguelloul Mouad</strong><br>
  <strong>Encadré par : Lhoussaine Daoudi</strong>
</p>
