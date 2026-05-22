# Contexte du projet HireCue

_Date de consolidation : 2026-05-22_

## 1. Objectif du projet

Relire et reformuler le rapport LaTeX afin d'améliorer son originalite, son style humain et sa qualité académique, sans chercher a tromper les détecteurs IA. L'objectif est de produire un contenu authentique, professionnel et fidèle au travail réellement réalisé dans le projet HireCue.

Le recrutement moderne genere une quantité importante de données : CV, offres d'emploi, résultats de tests, historiques de candidature, scores, interactions et sorties de services intelligents. Le probleme ne consiste plus seulement à collecter ces informations, mais à les organiser, les expliquer et les restituer sous une forme exploitable.

Dans l'état observe avant les évolutions du stage, plusieurs limites apparaissaient :

- certains scores restaient difficiles a interpréter pour les utilisateurs ;
- les recruteurs ne disposaient pas toujours d'une aide à la décision suffisamment explicite ;
- les candidats avaient besoin d'un accompagnement plus clair sur leurs écarts de compétences ;
- le reengagement et certaines relances restaient insuffisamment structures ;
- une partie des intégrations dependait encore d'opérations manuelles ou peu fiabilisées ;
- l'usage des services IA devait être mieux encadre par des quotas, du cache et des mécanismes de contrôle.

La problématique centrale peut ainsi être formulee comme suit :

Comment faire évoluer une plateforme de recrutement existante vers une solution plus intelligente, plus explicable et plus automatisée, capable d'aider les recruteurs dans l'analyse des candidatures et d'offrir aux candidats un suivi plus utile, sans fragiliser l'architecture ni perdre la maîtrise des traitements engagés ?

### Objectifs fonctionnels

Les objectifs fonctionnels du projet portent sur l'amélioration des parcours recruteur et candidat à partir des données déjà présentés dans HireCue.

Les principales cibles sont les suivantes :

- rendre le rapport de score plus lisible et plus exploitable ;
- proposer une roadmap de compétences plus claire pour le candidat ;
- permettre un échange contextuel autour de cette roadmap via un chatbot ;
- fournir au recruteur des recommandations de candidats plus utiles ;
- générer des questions personnalisées pour préparer l'entretien ;
- améliorer le suivi candidat, les notifications et le reengagement ;
- automatiser l'import d'offres et certaines intégrations externes ;
- fournir une lecture décisionnelle des données RH et opérationnelles à travers des dashboards Power BI ;
- explorer une logique de prévision RH avec `AI Workforce Forecasting`.

### Objectifs techniques

Les objectifs techniques concernent l'intégration et la fiabilisation de ces fonctionnalités dans l'existant.

Ils peuvent être résumés ainsi :

- intégrer les services IA sans rompre la cohérence du backend principal ;
- encadrer les appels LLM par des quotas et des mécanismes de contrôle ;
- réutiliser un cache pour limiter les recalculs coûteux ;
- mettre en place ou consolider des traitements asynchrones lorsque la charge le justifie ;
- maintenir une traçabilité suffisante des traitements et des intégrations ;
- articuler les workflows externes avec la plateforme sans dégrader l'expérience utilisateur ;
- connecter MySQL à Power BI pour construire un modèle analytique exploitable ;
- intégrer les dashboards décisionnels dans l'interface web via React / Node.js et iframe.

## 2. Présentation de HireCue

### Présentation de HireCue

HireCue est une plateforme SaaS de recrutement assiste par l'IA. Elle centralise plusieurs étapes du processus de recrutement dans un même environnement applicatif : gestion des offres, suivi des candidatures, évaluation des profils, analyse de scores, accompagnement candidat et outils d'aide à la décision pour les recruteurs.

Le projet de stage ne consistait pas à concevoir une plateforme à partir de zéro. Il s'agissait plutôt de faire évoluer un existant déjà structure, afin de renforcer son niveau d'explicabilité, d'automatisation et de personnalisation, tout en conservant une architecture exploitable en conditions réelles.

### Domaine

Le projet se situe à l'intersection de quatre dimensions :

- le recrutement numérique, avec des besoins de gestion d'offres, de candidatures et de suivi des parcours ;
- l'intelligence artificielle, utilisée pour enrichir l'analyse, générer des explications et assister la décision ;
- la Business Intelligence, utilisée pour consolider les données RH et opérationnelles dans des dashboards décisionnels ;
- le modèle SaaS, qui impose des exigences de robustesse, de scalabilité, de traçabilité et de maintien en production.

HireCue ne se limite donc pas a un simple job board. La plateforme s'oriente vers une logique d'orchestration du recrutement et de pilotage analytique, dans laquelle les données existantes sont exploitées pour produire des informations plus utiles aux recruteurs, aux candidats, aux administrateurs et aux décideurs.

### État initial de la plateforme

Avant les évolutions traitées pendant le stage, HireCue disposait déjà d'un socle applicatif fonctionnel :

- gestion des utilisateurs, des offres et des candidatures ;
- dashboards pour plusieurs profils d'utilisateurs ;
- base de données MySQL stockant les informations liées aux candidats, aux jobs, aux scores et aux évaluations ;
- backend métier expose via des APIs ;
- premiers mécanismes de scoring et de parcours d'évaluation.

Cet existant constituait une base solide, mais plusieurs briques restaient à clarifier, fiabiliser ou enrichir, en particulier autour de l'explication des scores, de l'accompagnement candidat, de l'automatisation et de l'exploitation des services IA.

## 3. Architecture générale

### Architecture générale

L'architecture de HireCue repose sur une séparation claire entre interface, logique métier, persistence et services externes :

- le frontend React porte les parcours utilisateur ;
- le backend Express centralise les routes, l'orchestration métier et l'accès aux données ;
- MySQL assure la persistence ;
- les services IA produisent certaines sorties textuelles ou analytiques ;
- n8n orchestre plusieurs automatisations externes ;
- Power BI, Power Query et DAX ajoutent une couche décisionnelle exploitant les données RH et opérationnelles ;
- l'interface React / Node.js permet l'affichage web de dashboards analytiques via iframe ;
- RabbitMQ prend en charge certains traitements asynchrones, notamment pour les recommandations.

Cette architecture a influence toute la démarche du stage, car les fonctionnalités ajoutées devaient s'intégrer a un système déjà composé de plusieurs briques techniques et métier.

### Intégration frontend/backend

Le travail ne s'est pas limite aux services backend. Une partie importante de la valeur du stage tient à l'intégration des fonctionnalités dans les parcours visibles par l'utilisateur.

Composants frontend significatifs observés :

- score report et roadmap :
  - `src/jsx/components/ScoreReport/ScoreDetailsReportPage.js`
  - `src/jsx/components/ScoreReport/ScoreDetailsRoadmapPage.js`
  - `src/jsx/components/ScoreReport/roadmap/RoadmapAssistantChat.jsx`
- recruteur :
  - `src/jsx/components/ScoreReport/AIRecommendationsPanel.jsx`
  - `src/jsx/components/RecruiterRisk/RiskAlertsPage.jsx`
  - `src/jsx/components/user/ReEngagementPoolSection.jsx`
  - `src/jsx/components/Dashboard/CandidateSearchForm.jsx`
- admin :
  - `src/jsx/components/Dashboard/ExternalJobsImportPanel.jsx`.
- analytique :
  - intégration web des dashboards Power BI dans une application React / Node.js via iframe.

Cette intégration est importante dans le rapport, car elle montre que les fonctionnalités ne sont pas restees au stade de services isolés. Elles ont été rattachées a des parcours utilisateur identifiables.

## 4. Environnement technique

### Environnement technique

L'environnement technique observe dans les repositories du projet repose principalement sur les composants suivants :

- frontend principal en React dans `repotinitialnpx` ;
- backend principal en Node.js / Express dans `hr-BackendPfeHirecue` ;
- base de données MySQL avec Sequelize ;
- services IA en Python dans `hr-AiHirecue` et `hr-intelligencePFE` ;
- workflows d'automatisation via `n8n` ;
- sourcing LinkedIn via `PhantomBuster` ;
- couche Business Intelligence avec `Power BI`, `Power Query` et `DAX` ;
- intégration web des dashboards analytiques via `React.js`, `Node.js` et `iframe` ;
- traitements asynchrones via `RabbitMQ` pour certaines fonctionnalités ;
- usage de cache, logs applicatifs et quotas IA pour encadrer les traitements coûteux ;
- expérimentation analytique `AI Workforce Forecasting` basée sur Python.

### Technologies complémentaires pour l'analytique

Les technologies ajoutées autour du module décisionnel doivent être présentées de manière séparée des modules IA / LLM afin d'éviter toute confusion entre génération intelligente et analyse décisionnelle.

Business Intelligence :

- `Power BI` pour la création des rapports et dashboards ;
- `Power Query` pour le nettoyage, la transformation et la préparation des données ;
- `DAX` pour les mesures analytiques et les KPIs.

Frontend / Web :

- `React.js` pour l'affichage côté interface ;
- `Node.js` pour l'intégration web dans l'environnement applicatif ;
- intégration `iframe` pour embarquer les dashboards Power BI.

Base de données :

- `MySQL` comme source principale des données RH et opérationnelles.

Deep Learning / IA :

- `Python` pour l'expérimentation analytique ;
- `AI Workforce Forecasting` comme prototype de prévision RH ;
- `TensorFlow` ou `scikit-learn` uniquement si l'implémentation confirmée du prototype les utilise réellement.

### Repositories principaux analysés

- `repotinitialnpx` : frontend principal des dashboards et parcours applicatifs ;
- `hr-BackendPfeHirecue` : backend Node.js / Express / Sequelize / MySQL ;
- `hr-AiHirecue` : services IA séparés pour certains workflows candidats et analyses ;
- `hr-intelligencePFE` : referentiel IA plus large, hétérogène, incluant des briques d'explication, de roadmap et de génération.

## 5. Fonctionnalités réalisées

### Vue d'ensemble

Le travail du stage a porté sur l'évolution progressive de HireCue autour de fonctionnalités déjà reliées à des besoins métier concrets. L'objectif n'était pas d'ajouter des modules de façon decorative, mais d'améliorer la lecture des résultats, d'assister la décision et de rendre plusieurs parcours plus fluides.

Les développements ou consolidations les plus importants concernent le score explicable, les recommandations IA, la roadmap de compétences, le chatbot associé, les workflows n8n, le reengagement candidat, l'intégration frontend/backend de ces briques et l'ajout d'une couche analytique Power BI.

Cette section a pour objectif de clarifier le périmètre réel du projet afin d'éviter de présenter de manière uniforme des briques qui n'ont pas toutes atteint le même niveau de maturité. Certaines fonctionnalités ont été effectivement intégrées dans les parcours applicatifs et documentees par des endpoints, des tables ou des composants frontend. D'autres apparaissent comme des automatisations mises en place, consolidées partiellement ou encore en cours de stabilisation. Le rapport doit donc employer des formulations prudentes et distinguer ce qui est réalisé, consolide, documenté ou encore en cours.

### 1. Score explicable pour le candidat et le recruteur

Le rapport de score explicable fait partie des fonctionnalités les plus concrètes du projet. Cette brique a été mise en place afin de justifier l'attribution d'un score à partir des données disponibles dans la candidature et dans les évaluations associées. Le résultat ne se limite pas a une note globale : il s'appuie sur plusieurs composantes, met en avant des sous-scores et visé a fournir une répartition plus lisible du résultat.

Pour le candidat, cette restitution prend une dimension pédagogique. Elle doit permettre de mieux comprendre les facteurs ayant influence l'évaluation et d'identifier des pistes d'amélioration. Pour le recruteur, la même logique renforce l'aide à la décision en rendant le score plus défendable, plus interprétable et moins opaque qu'une valeur numérique isolée. L'export PDF du rapport de score est également présent dans le périmètre observe.

### 2. Roadmap candidat

La roadmap candidat constitue une autre fonctionnalité effectivement intégrée dans le périmètre du stage. Elle est associée a une candidature et à une offre cible, puis visé à identifier les compétences manquantes ou insuffisamment visibles par rapport au poste. À partir de cet écart, la plateforme propose un guide personnalisé d'amélioration.

Cette roadmap peut être prolongee par un chatbot contextuel s'appuyant sur le contenu déjà genere. Le projet montre également la présence d'un accès PDF lié à la roadmap, ce qui permet soit de consulter, soit de générer un support partageable à partir du parcours d'amélioration. La couche conversationnelle existe dans le code observe ; en revanche, une couche RAG documentaire plus riche doit être présentée avec prudence lorsqu'elle n'est pas pleinement documentee.

### 3. Recommandations IA dans le dashboard recruteur

Le dashboard recruteur intègre un module de recommandations IA destine à faire ressortir les profils les plus pertinents pour une offre. Dans le périmètre observe, cette fonctionnalité visé a recommander un nombre restreint de candidats pertinents, typiquement trois profils mis en avant, avec des raisons explicites reliées aux compétences rapprochées, aux écarts identifiés et au score d'adéquation.

Cette brique ne doit pas être décrite comme un mécanisme autonome de sélection. Le classement reste encadre par la logique métier, tandis que la couche IA sert surtout à enrichir l'explication. Le module est également relié a des usages concrets du recruteur, notamment la génération de questions personnalisées pour l'entretien et, lorsque le parcours le permet, l'invitation de candidats à passer un test complémentaire.

### 4. Réengagement du talent pool

Le reengagement du talent pool fait partie des fonctionnalités intégrées pour mieux exploiter les candidats déjà présents dans la plateforme. Cette brique permet au recruteur de retrouver d'anciens profils, d'identifier ceux qui restent pertinents pour une offre et de préparer une relance structurée.

Selon les scénarios documentes dans le projet, cette relance peut prendre la forme d'un message, d'une notification in-app ou d'un email. L'objectif n'est pas de remplacer le sourcing externe, mais de mieux valoriser le vivier déjà constitue dans HireCue.

### 5. Alertes de risque candidat

Le dashboard recruteur comprend également des alertes de risque destinées à faire remonter certains cas qui meritent une analyse plus attentive. Ces alertes peuvent signaler un faible engagement, des informations incomplètes, des incohérences ou un risque potentiel d'échec dans la suite du processus.

Il est important de préciser dans le rapport que ces alertes relèvent de l'aide à la décision. Elles ont pour fonction d'attirer l'attention du recruteur sur des points de vigilance, mais elles ne doivent ni remplacer son jugement ni être présentées comme un mécanisme automatique d'exclusion.

### 6. Dashboard Admin - candidats inactifs

Le périmètre fonctionnel comprend également un besoin de relance des candidats inactifs depuis une période significative, généralement autour de 60 jours. Dans le rapport, cette fonctionnalité doit être présentée soit comme un bouton admin déjà ajoute, soit comme une brique documentee et intégrée dans le périmètre de gestion des candidats inactifs, selon le niveau de finalisation réel confirme par les éléments du projet.

Sa finalite est claire : permettre à l'administration de relancer des candidats peu actifs afin d'améliorer l'engagement global sur la plateforme. Si le niveau de maturité reste partiel, une formulation du type "documenté" ou "en cours de consolidation" est plus juste qu'une présentation comme fonctionnalité totalement stabilisée.

### 7. Workflow n8n - récupération des profils candidats

Le projet comprend un workflow n8n connecte a PhantomBuster pour le sourcing de profils candidats. Le recruteur peut saisir un critère de recherche, par exemple un intitulé de poste comme "Software Engineer", puis déclencher une collecte de profils correspondants.

Les données récupérées sont ensuite nettoyées, dédoublonnées et restructurees avant leur transmission au dashboard recruteur. Cette chaîne d'automatisation fait partie des briques les mieux rattachées au projet, même si certains comportements peuvent rester dependants des limites des services tiers.

### 8. Workflow n8n - import d'offres externes

Un autre volet du périmètre concerné l'import d'offres d'emploi externes. Des workflows sont prévus pour récupérer des offres depuis des APIs ou des plateformes tierces, les mapper vers la structure interne de HireCue, puis les insérer dans la base après vérification.

Le dashboard Admin peut être associé à des paramètres de recherche ou de sélection de source afin de piloter cette importation. Cette fonctionnalité doit être présentée comme une automatisation mise en place et consolidée sur la partie import, sans surevaluer les parties qui dépendent encore de la stabilité des intégrations externes.

### 9. Workflow n8n - reverse flow vers LinkedIn

Le périmètre du projet inclut également un flux inverse de publication d'offres vers LinkedIn. Dans cette logique, le backend HireCue prépare les données de l'offre à publier, puis transmet les informations nécessaires a un workflow n8n charge d'orchestrer la publication externe.

Ce volet doit être décrit avec mesure. Les éléments observés montrent surtout une préparation backend et une intention d'orchestration externe. Si la publication complète n'est pas intégralement documentee ou stabilisée, il convient de là présenter comme un flux intègre ou prévu dans le périmètre, en cours de consolidation.

### 10. Workflow n8n - scraping CEO / propriétaires + email

Le projet couvre enfin un workflow de prospection ciblée à partir de profils de type CEO, founder ou owner récupérés depuis LinkedIn via PhantomBuster. Les données recueillies doivent ensuite être nettoyées, structurées et préparées pour un usage de communication.

Une fois les leads qualifies, un envoi d'email peut être déclenché afin de présenter ou de rappeler les services de HireCue. La traçabilité des envois et le suivi de statut doivent être mentionnes dans le rapport lorsqu'ils sont documentes. Toutefois, cette brique doit rester présentée comme une automatisation complémentaire, souvent en cours de consolidation, plutôt que comme le cœur fonctionnel du projet.

### 11. Module décisionnel Power BI

Le module décisionnel Power BI ajoute une couche analytique à HireCue. Il exploite les données RH et opérationnelles déjà stockées dans MySQL afin de produire des indicateurs, des mesures et des visualisations utiles aux décideurs, aux administrateurs et au suivi produit.

Les tâches réalisées ou documentées dans ce périmètre sont les suivantes :

- connexion de la base MySQL de HireCue avec Power BI ;
- récupération des données RH et opérationnelles depuis MySQL ;
- nettoyage et transformation des données avec Power Query ;
- création des relations entre tables et préparation du modèle analytique ;
- création de KPIs et de mesures analytiques, notamment avec DAX ;
- création de visualisations et de dashboards interactifs ;
- réalisation de deux dashboards : `Executive Overview` et `Product & Operations Analytics` ;
- intégration des dashboards dans une application React / Node.js via iframe ;
- affichage web des dashboards analytiques dans l'interface applicative.

Cette brique ne remplace pas les modules IA / LLM. Elle complète HireCue par une couche décisionnelle orientée analyse, suivi et pilotage.

### 12. Expérimentation AI Workforce Forecasting

L'expérimentation `AI Workforce Forecasting` correspond à une exploration analytique complémentaire autour de la prévision RH. Elle vise à tester une logique de prévision basée sur les données de la plateforme, par exemple l'évolution de l'activité, des candidatures, des offres ou de certains besoins opérationnels.

Cette partie doit être présentée comme une expérimentation Deep Learning ou machine learning selon le niveau réel de maturité du prototype. Elle s'appuie sur Python et peut mobiliser TensorFlow ou scikit-learn uniquement si ces bibliothèques sont effectivement utilisées dans l'implémentation confirmée.

Dans le contexte du rapport, cette expérimentation montre que HireCue évolue aussi vers une lecture prédictive des données, sans confondre cette brique avec les modules LLM déjà intégrés au score, aux recommandations ou à la roadmap.

### Backlog fonctionnel enrichi

Le backlog a été détaillé autour des briques effectivement traitées ou consolidées dans le projet :

- score explicable ;
- AI recommendations ;
- skill gap roadmap ;
- roadmap chatbot contextuel ;
- questions personnalisées d'entretien ;
- candidate engagement \& motivation insights ;
- smart candidate risk alerts ;
- reengagement des candidats et re-engagement pool ;
- scraping des données externes ;
- scraping candidats via n8n + PhantomBuster ;
- pipeline d'import des offres LinkedIn ;
- workflows de prospection et d'envoi d'emails, à présenter comme automatisations complémentaires lorsqu'ils ne sont pas totalement stabilisés ;
- connexion de MySQL avec Power BI ;
- nettoyage et transformation des données dans Power Query ;
- modélisation des relations analytiques et création de mesures DAX ;
- réalisation des dashboards `Executive Overview` et `Product & Operations Analytics` ;
- intégration web des dashboards Power BI via React / Node.js et iframe ;
- expérimentation `AI Workforce Forecasting` pour explorer une logique de prévision RH.

## 6. Modules IA / LLM

### Scoring IA et score report explicable

Une partie importante du travail a concerné le rapport de score explicable. L'enjeu était de transformer un résultat numérique en une sortie plus compréhensible pour l'utilisateur, notamment en mettant en valeur le score global, certains sous-scores et une explication du résultat.

Elements observés dans le projet :

- endpoint principal : `GET /api/score-report` ;
- export PDF disponible via `GET /api/candidate/score-report/:candidateId/:jobId/pdf` ;
- lecture de plusieurs scores sources comme `Cscore`, `Pscore`, `Ptscore`, `Tscore` et `final_tests_score` ;
- calcul du score global à partir de poids stockés dans `final_scoring` ;
- persistence et cache dans la table `score_reports`.

Cette brique apporte une valeur directe, car elle rend l'évaluation plus lisible et donné un support plus défendable à la lecture du profil.

### Recommandations IA côté recruteur

Le projet comprend également un module de recommandations destine au recruteur. Son objectif est d'aider à identifier les profils les plus pertinents pour une offre, sans remplacer la décision humaine.

Elements observés dans le projet :

- endpoint principal : `GET /api/recruiter/jobs/:jobId/ai-recommendations` ;
- suivi d'état par `GET /api/recruiter/ai-recommendations/status/:jobRunId` ;
- cache dans `ai_recommendations_cache` ;
- suivi des traitements dans `ai_recommendation_jobs` ;
- exécution asynchrone vià une queue RabbitMQ `hirecue.ai-recommendations.v1`.

Les recommandations combinent des éléments comme le `fit score`, les compétences rapprochées, les compétences manquantes, la source et une explication associée.

### Roadmap de compétences

Le stage a aussi porte sur la génération d'une roadmap de compétences côté candidat. Cette fonctionnalité visé a rendre plus concret l'écart entre le profil du candidat et les attentes du poste, en proposant un parcours d'amélioration plus structure.

Elements observés dans le projet :

- endpoints :
  - `GET /api/candidate/roadmap/general`
  - `GET /api/candidate/roadmap/job/:jobId`
  - `GET /api/candidate/roadmap/pdf`
- service principal : `services/roadmapService.js` ;
- construction métier : `utils/roadmapBuilder.js` ;
- appels IA : `utils/roadmapAiClient.js` ;
- persistence dans `candidate_skill_roadmaps` ;
- cache fondé sur `candidate_uid + job_id + input_hash` ;
- consommation de quota `quotaRoadmapGeneration`.

Cette fonctionnalité répond à un besoin académiquement et metierement important : ne pas seulement évaluer, mais aussi expliquer comment progresser.

### Chatbot RAG lié à la roadmap

Le chatbot associé à la roadmap permet au candidat de poser des questions contextuelles sur sa progression. Cette brique a pour rôle de prolonger la roadmap en une interaction plus souple, tout en restant centrée sur le contenu déjà calculé.

Elements observés dans le projet :

- endpoints :
  - `GET /api/candidate/roadmap/chat/history`
  - `POST /api/candidate/roadmap/chat`
- services principaux :
  - `services/roadmapChatService.js`
  - `repositories/roadmapChatRepository.js`
- table d'historique : `candidate_roadmap_chat_messages` ;
- dépendance au contenu de `candidate_skill_roadmaps` ;
- quota dédié : `quotaRoadmapChat`.

Le code montre une logique de chatbot contextuel exploitable, avec historique et encadrement des appels. Une vraie couche RAG documentaire complète n'a toutefois pas été clairement documentee dans les repositories observés.

### Questions personnalisées pour l'entretien

Une autre brique du projet concerné la génération de questions personnalisées pour le recruteur, à partir du profil du candidat et du poste cible.

Elements observés dans le projet :

- endpoint principal : `POST /api/recruiter/jobs/:jobId/candidates/:candidateId/personalized-questions` ;
- cache dans `recruiter_personalized_question_cache` ;
- objectif métier : préparer un entretien plus cible et moins générique.

Cette fonctionnalité renforce l'aide à la décision en prolongeant l'analyse du profil vers la phase d'entretien.

### Candidate engagement et alertes

Le stage a également porté sur le reengagement du talent pool et le suivi de certaines notifications. L'enjeu était de mieux exploiter les profils déjà présents dans la plateforme au lieu de se limiter aux candidatures les plus récentes.

Elements observés dans le projet :

- endpoints principaux :
  - `GET /api/recruiter/jobs/:jobId/reengagement-pool`
  - `GET /api/recruiter/reengagement/status/:jobRunId`
  - `POST /api/recruiter/jobs/:jobId/reengage/draft`
  - `POST /api/recruiter/reengage`
  - `POST /api/candidate/reengagement/decline`
- services :
  - `services/reengagementService.js`
  - `services/reengagementJobService.js`
- tables :
  - `reengagement_log`
  - `reengagement_pool_jobs`.

Ces mécanismes apportent une valeur opérationnelle au recruteur, en structurant la relance et en amenant plus de continuite dans le suivi.

Le dashboard recruteur intègre également des alertes de risque destinées a attirer l'attention sur certains cas demandant une lecture plus fine.

Elements observés dans le projet :

- endpoint principal : `GET /api/recruiter/risk-alerts` ;
- actions associées :
  - `POST /api/recruiter/risk-alerts/ai-summaries`
  - `POST /api/recruiter/risk-alerts/:id/resolve`
  - `POST /api/recruiter/risk-alerts/:id/snooze`.

Cette brique prolonge la logique d'aide à la décision en ne se limitant pas aux seuls scores.

### Principes techniques à respecter dans le rapport

- La détection des écarts de compétences reste déterministe ; le LLM sert surtout à générer une roadmap pédagogique.
- Le classement des candidats dans les recommandations reste \textit{rules-first} ; le LLM intervient pour l'explication et non pour la décision finale.
- Les sorties importantes doivent être présentées avec leurs mécanismes de cache, de queue, de fallback et de validation de format lorsque ces points existent dans le projet.
- Les modules IA doivent être décrits comme des briques d'assistance encadrees par des quotas, des contrôles et de la persistence.

## 7. Workflows n8n et automatisations

### Workflows n8n et automatisation

Une partie du travail concerné l'automatisation de flux métier via n8n. Ces workflows ne sont pas accessoires : ils participent à l'exploitation opérationnelle de la plateforme.

Les usages observés concernent notamment :

- l'import d'offres externes vers `joblist` ;
- le reverse flow de publication vers LinkedIn, partiellement prépare dans le backend ;
- le candidate sourcing à partir de recherchés LinkedIn ;
- certains flux de communication et d'outreach.

Points techniques releves :

- webhook d'import d'offres base sur `N8N_EXTERNAL_JOBS_WEBHOOK_URL` ;
- lancement de recherche sourcing via `POST /api/recruiter/candidate-search-runs/start` ;
- import des profils sources vers `sourced_candidates` ;
- suivi de runs dans `candidate_search_runs`.

Ces intégrations montrent que HireCue cherche aussi a agir sur l'acquisition de profils et non seulement sur l'analyse interne des candidatures.

## 8. Intégrations LinkedIn / UniPile / PhantomBuster

### Synthèse des intégrations externes

Les intégrations LinkedIn, UniPile et PhantomBuster doivent être présentées comme des briques d'orchestration externes autour de HireCue, avec un niveau de maturité précise selon les cas. PhantomBuster est surtout rattaché au sourcing, au scraping de profils, à l'enrichissement de leads et à la récupération d'offres ou de contacts. UniPile est plus adapté aux interactions API avec LinkedIn, notamment la messagerie, la publication de contenus, le suivi de conversations et certains flux sortants plus structures.

Dans le rapport, ces intégrations ne doivent pas être présentées comme interchangeables. PhantomBuster convient davantage aux flux d'extraction et de collecte, tandis qu'UniPile est plus pertinent pour les actions LinkedIn suivies et synchronisees. n8n joue le rôle de couche d'orchestration entre ces services, le backend HireCue et les supports de suivi comme Gmail, Google Sheets ou les dashboards internes.

Les limites des services tiers doivent rester visibles : dépendance aux quotas, variations de formats, disponibilité variable des emails, échec possible de certains agents, restrictions LinkedIn et besoin de dédoublonnage avant insertion dans HireCue.

## 9. Module décisionnel et analytique Power BI

### Rôle du module décisionnel

Le module décisionnel et analytique Power BI ajoute une couche de Business Intelligence à HireCue. Son rôle est d'exploiter les données RH et opérationnelles produites par la plateforme afin de les transformer en indicateurs, en KPIs et en dashboards interactifs.

Cette partie s'inscrit dans la continuité des modules existants, mais avec un objectif différent. Les modules IA / LLM assistent l'analyse individuelle, la génération d'explications et l'accompagnement candidat. La couche Power BI vise plutôt le pilotage global, la lecture opérationnelle et l'aide à la décision à partir d'indicateurs agrégés.

### Tâches réalisées

Les travaux réalisés autour de Power BI couvrent l'ensemble de la chaîne analytique :

- connexion de la base MySQL de HireCue avec Power BI ;
- récupération des données RH et opérationnelles depuis MySQL ;
- nettoyage, filtrage et transformation des données avec Power Query ;
- préparation du modèle analytique et création des relations entre tables ;
- création de KPIs et de mesures analytiques avec DAX ;
- création de visualisations et de dashboards interactifs ;
- réalisation des dashboards `Executive Overview` et `Product & Operations Analytics` ;
- intégration des dashboards dans une application React / Node.js via iframe ;
- affichage web des dashboards analytiques dans l'interface HireCue.

### Architecture analytique

La chaîne analytique peut être résumée comme suit :

`MySQL -> Power BI -> Power Query -> KPIs / Dashboards -> React / Node.js -> Interface Web`

Dans cette architecture :

- MySQL fournit les données sources liées aux candidats, aux jobs, aux workflows, aux événements et aux indicateurs de plateforme ;
- Power Query prépare, nettoie et transforme les données avant leur exploitation analytique ;
- Power BI génère les mesures, KPIs, graphiques et tableaux de bord ;
- React / Node.js permet l'intégration web des dashboards via iframe ;
- `AI Workforce Forecasting` constitue une expérimentation analytique complémentaire autour de la prévision RH.

### Dashboards réalisés

`Executive Overview` est destiné aux décideurs, aux administrateurs et aux profils ayant besoin d'une vue synthétique de l'activité globale.

KPIs possibles :

- nombre de candidats ;
- nombre de jobs ;
- statistiques globales de la plateforme ;
- activité globale ;
- indicateurs de performance du recrutement.

`Product & Operations Analytics` vise le suivi opérationnel des workflows, des événements et de l'activité système.

KPIs possibles :

- événements applicatifs ;
- workflows et automatisations ;
- jobs externes ;
- statistiques opérationnelles ;
- suivi d'activité et tendances d'utilisation.

### Positionnement dans HireCue

Cette partie ne remplace pas les modules IA / LLM existants. Elle complète HireCue par une couche décisionnelle et analytique permettant de suivre l'activité, de comparer les indicateurs et de préparer des décisions à partir de données agrégées.

Le projet combine désormais plusieurs briques complémentaires :

- IA / LLM ;
- automatisation n8n ;
- scraping LinkedIn et intégrations externes ;
- workflows backend ;
- dashboards décisionnels Power BI ;
- expérimentation Deep Learning ou machine learning autour de `AI Workforce Forecasting`.

## 10. AI Workforce Forecasting

### Objectif de l'expérimentation

`AI Workforce Forecasting` représente une expérimentation analytique complémentaire au module Power BI. L'objectif est d'explorer une logique de prévision RH à partir des données déjà disponibles dans HireCue.

Cette expérimentation peut porter sur des tendances comme le volume de candidatures, l'évolution des offres, l'activité de la plateforme, les besoins de recrutement ou certains signaux opérationnels. Elle doit être décrite avec prudence si le prototype n'a pas atteint le même niveau de maturité que les fonctionnalités intégrées au cœur applicatif.

### Positionnement technique

La brique de prévision peut s'appuyer sur :

- les données MySQL extraites de la plateforme ;
- une préparation des variables et indicateurs utiles à la prévision ;
- Python pour l'expérimentation ;
- TensorFlow ou scikit-learn uniquement si ces bibliothèques sont réellement utilisées ;
- une restitution web expérimentale dans l'interface applicative.

Cette expérimentation prolonge la logique analytique de HireCue. Elle ne doit pas être confondue avec les modules LLM, qui produisent des explications, des recommandations ou des contenus textuels contextualisés.

### Apport dans le rapport

Dans le rapport PFE, cette partie peut être utilisée pour montrer l'ouverture du projet vers l'analyse prédictive. Elle permet aussi de relier la démarche CRISP-DM à une chaîne de données concrète : compréhension du besoin RH, extraction des données, préparation, expérimentation, évaluation et restitution.

## 11. Méthodologie (Scrum + CRISP-DM)

### Démarche itérative

La démarche adoptée pendant le stage repose sur une progression itérative. Ce choix était nécessaire, car les travaux portaient sur une plateforme déjà existante, avec plusieurs repositories, des intégrations externes et des dépendances entre modules.

Une telle approche permettait :

- de comprendre l'existant avant d'ajouter de nouvelles briques ;
- de limiter les régressions ;
- de tester progressivement la cohérence des intégrations ;
- de traiter les priorités selon leur valeur métier et leur maturité technique.

### Agile Scrum

Pour la dimension applicative, la logique Scrum est adaptée au projet, car les évolutions ont été menées par lots fonctionnels successifs : score report, roadmap, chatbot, recommandations, automatisation, reengagement, dashboards applicatifs et dashboards décisionnels Power BI.

Cette approche permet de :

- prioriser les fonctionnalités à plus forte valeur ;
- articuler frontend, backend et intégrations ;
- ajuster progressivement les travaux selon les retours et les contraintes rencontrees.

### CRISP-DM

Pour les briques IA, LLM et analytiques, une logique proche de CRISP-DM reste pertinente. Les sorties générées ou prédites ne peuvent pas être évaluées uniquement sur leur faisabilité technique. Elles doivent aussi être reliées au besoin métier, à la qualité des données disponibles et à la cohérence des résultats retournés.

Dans le cadre de HireCue, cette logique se retrouve dans :

- la clarification du besoin avant génération d'une explication ou d'une roadmap ;
- l'analyse des données disponibles : scores, informations de poste, résultats de tests, CV ;
- la structuration des données d'entrée ;
- l'évaluation de la qualité des sorties produites ;
- l'intégration dans une plateforme avec quotas, cache et gestion d'erreurs ;
- la préparation de données analytiques pour Power BI et l'expérimentation `AI Workforce Forecasting`.

## 12. Résultats obtenus

### Fonctionnalités livrées ou consolidées

Au terme du travail analysé, les principales fonctionnalités livrées ou consolidées dans HireCue peuvent être résumées ainsi :

- rapport de score explicable ;
- roadmap de compétences ;
- chatbot contextuel lié à la roadmap ;
- recommandations IA pour les recruteurs ;
- questions personnalisées pour l'entretien ;
- reengagement du talent pool ;
- alertes de risque dans le dashboard recruteur ;
- automatisation de certains flux n8n ;
- intégration visible de ces briques dans le frontend et le backend ;
- couche décisionnelle Power BI connectée à MySQL ;
- dashboards `Executive Overview` et `Product & Operations Analytics` ;
- intégration web des dashboards via React / Node.js et iframe ;
- expérimentation `AI Workforce Forecasting`.

### Améliorations apportées

Les améliorations les plus marquantes portent sur trois plans.

D'abord, la plateforme devient plus lisible. Les scores, les recommandations et les parcours candidats sont mieux structures et plus faciles à exploiter.

Ensuite, la plateforme devient plus utile operationnellement. Les automatisations, le sourcing, le reengagement et certains traitements asynchrones réduisent la part de manipulation manuelle.

Enfin, la plateforme devient plus maîtrisable techniquement. Le cache, les quotas IA, la persistence des résultats et la traçabilité renforcent la fiabilité de l'ensemble.

La plateforme gagne aussi une capacité de pilotage analytique. Les données RH et opérationnelles ne sont plus seulement exploitées dans les parcours métier ; elles peuvent aussi être consolidées dans Power BI pour produire des indicateurs globaux, des dashboards et des analyses de tendance.

### Valeur apportée

Pour le recruteur, la valeur se situe dans une meilleure aide à la décision, une lecture plus rapide des profils et des outils plus concrets pour prioriser, analyser et relancer.

Pour les administrateurs et décideurs, la valeur se situe dans une meilleure visibilité sur l'activité globale, les performances de recrutement, les workflows et les indicateurs opérationnels.

Pour le candidat, la valeur se situe dans une meilleure compréhension des résultats, un accompagnement plus structure à travers la roadmap et un échange plus contextualise via le chatbot.

### Compétences techniques

Le stage a permis de consolider plusieurs compétences techniques :

- intégration frontend/backend sur une plateforme existante ;
- structuration de services backend Node.js / Express ;
- manipulation d'une base MySQL via Sequelize ;
- intégration de services IA et de fonctionnalités LLM ;
- modélisation de données analytiques dans Power BI ;
- nettoyage et transformation de données avec Power Query ;
- création de mesures DAX et de KPIs décisionnels ;
- intégration web de dashboards via React / Node.js et iframe ;
- expérimentation de prévision RH avec Python ;
- mise en place de cache, quotas et traitements asynchrones ;
- orchestration de workflows n8n et interaction avec des services tiers.

### Compétences organisationnelles

Au-dela des aspects techniques, le projet a aussi permis de renforcer :

- la capacité a analyser un existant avant intervention ;
- la priorisation de fonctionnalités selon leur valeur et leur faisabilité ;
- la coordination entre modules hétérogènes ;
- la prise en compte des contraintes de robustesse et de production dans les choix techniques ;
- l'articulation entre développement applicatif, automatisation, analyse décisionnelle et expérimentation prédictive.

### Compréhension métier

Le stage a également apporte une meilleure compréhension du domaine du recrutement numérique :

- lecture des besoins des recruteurs ;
- importance de l'explicabilité dans les scores et les recommandations ;
- rôle de l'accompagnement candidat dans l'acceptabilite de la plateforme ;
- intérêt de l'automatisation pour fluidifier le traitement des candidatures.

## 13. Difficultés rencontrées

### Difficultés techniques

Le projet a implique plusieurs difficultés techniques liées à la coexistence entre frontend, backend, services IA, base de données et workflows externes. Cette hétérogénéité impose une vigilance constante sur la cohérence des appels, des formats et des états de traitement.

### Intégrations API et services externes

Parmi les points sensibles observés dans le projet :

- écarts de configuration entre environnements, notamment autour de la connexion à la base ;
- comportements différents entre appels testes via Postman et appels déclenchés depuis le frontend ;
- limitations ou erreurs d'intégration côté LinkedIn ;
- incidents liés a PhantomBuster, comme `Agent not found` ou `auto launch disabled`.

### Workflows et orchestration

Les workflows n8n apportent de la valeur, mais ils introduisent aussi des difficultés de fiabilisation :

- dépendance a des webhooks et services tiers ;
- gestion des retours d'exécution et des échecs ;
- suivi incomplet de certains diagnostics d'erreur ;
- besoin de mieux documenter certaines chaînes d'automatisation.

### Données et persistance

Le projet met également en évidence plusieurs enjeux liés aux données :

- besoin de garder des scores sources cohérents ;
- nécessité de persister correctement les sorties générées ;
- suivi des historiques utiles au chatbot et au reengagement ;
- vérification de certaines tables ou nomenclatures non totalement homogènes entre code et documentation ;
- besoin de préparer un modèle de données exploitable pour Power BI sans dégrader les données sources de HireCue.

## 14. Perspectives

Les pistes d'évolution identifiées à partir du projet sont les suivantes :

- stabiliser davantage les workflows n8n en production ;
- renforcer la documentation des intégrations externes ;
- clarifier certaines parties du reverse publishing LinkedIn ;
- mieux documenter ou compléter la couche RAG si une base documentaire plus riche est mise en place ;
- renforcer la persistance de certains marqueurs de progression métier dans les roadmaps ;
- consolider le modèle analytique Power BI et documenter les mesures DAX les plus importantes ;
- stabiliser l'intégration web des dashboards Power BI selon les contraintes d'accès, d'authentification et de partage ;
- poursuivre l'évaluation de l'expérimentation `AI Workforce Forecasting` avec des données historiques suffisantes ;
- poursuivre l'amélioration des diagnostics d'erreur et de la traçabilité ;
- valider plus complètement certains flux de sourcing et de publication externes.

## 15. Règles de rédaction académique

### Style et originalité

- Le rapport doit être redige dans un style humain, académique et personnel.
- Chaque paragraphe doit être lié au projet réel HireCue.
- Les phrases génériques doivent être evitees.
- Les textes inspires d'exemples doivent être reformules.
- Les definitions générales doivent être sourcees.
- Le rapport doit présenter uniquement le travail réellement réalisé.
- L'objectif est d'améliorer l'originalite, la clarté et la qualité académique, sans chercher a contourner des détecteurs automatiques.

### Consigne sur les accents français

Tous les mots français du rapport doivent être correctement accentués. Les caractères spéciaux français doivent être respectés dans tous les fichiers LaTeX visibles par le lecteur. Les commandes LaTeX, URLs, endpoints API, chemins de fichiers et noms techniques ne doivent pas être modifiés.

## 16. Points de vigilance pour le rapport

| Element | Chapitre concerné | Action à faire | Priorite |
| --- | --- | --- | --- |
| PDF associé à la roadmap | Chapitre 2 et 3 | Ajouter explicitement dans les besoins fonctionnels, le backlog et la réalisation de la roadmap | Moyenne |
| Recommandation explicite de trois candidats | Chapitre 2 et 3 | Preciser le format attendu des recommandations côté recruteur et la logique de sélection présentée dans le rapport | Moyenne |
| Invitation à passer un test depuis les recommandations IA | Chapitre 2 et 3 | Ajouter dans les besoins fonctionnels, le backlog et le module recommandations/recruteur | Haute |
| Bouton admin de relance des candidats inactifs depuis environ 60 jours | Chapitre 1, 2 et 3 | Integrer la fonctionnalité dans la solution proposée, le backlog et la réalisation admin | Haute |
| Reverse flow de publication d'offres vers LinkedIn | Chapitre 1, 2 et 3 | Mieux expliciter le périmètre du flux sortant, son orchestration n8n et son niveau de maturité | Haute |
| Workflow CEO / founder / owner avec envoi d'email et traçabilité | Chapitre 2 et 3 | Developper la partie prospection et suivi des envois sans là présenter comme totalement stabilisée | Moyenne |
| Repartition détaillée du score, sous-scores et conseils d'amélioration | Chapitre 3 | Renforcer la description fonctionnelle et la réalisation du score explicable | Haute |
| Roadmap liée à chaque candidature et à chaque offre | Chapitre 3 | Preciser le rattachement de la roadmap au binôme candidat-offre et l'usage du PDF associé | Moyenne |
| Parametres de recherche de l'import d'offres externes depuis le dashboard Admin | Chapitre 2 et 3 | Completer la description fonctionnelle et technique du workflow d'import | Moyenne |
| Module décisionnel Power BI | Chapitre 2, 3 et 4 | Ajouter la connexion MySQL, Power Query, DAX, les KPIs, les dashboards et l'intégration iframe | Haute |
| Architecture analytique | Chapitre 3 | Présenter la chaîne MySQL -> Power BI -> Power Query -> KPIs / Dashboards -> React / Node.js -> Interface Web | Haute |
| Dashboards Executive Overview et Product & Operations Analytics | Chapitre 3 et 4 | Décrire les objectifs, les indicateurs et les usages de chaque dashboard | Haute |
| AI Workforce Forecasting | Chapitre 3 et 4 | Présenter cette partie comme une expérimentation analytique complémentaire, sans la confondre avec les modules LLM | Moyenne |
| Distinction entre fonctionnalités réalisées, documentees et en cours de consolidation | Chapitre 1, 2 et 3 | Uniformiser les formulations pour éviter de sur-promettre certains workflows ou intégrations externes | Haute |

- Le rapport doit présenter le travail réellement réalisé sur HireCue et non une plateforme théorique.
- Les descriptions de l'entreprise d'accueil doivent rester courtes et utiles au cadrage.
- La problématique, la mission, les moyens utilisés et les résultats obtenus doivent rester au centre du contenu.
- Les fonctionnalités non confirmees dans le code ne doivent pas être présentées comme des acquis certains.
- Les modules IA doivent être décrits comme des briques d'assistance et non comme des mécanismes de décision autonome.
- Le module Power BI doit être présenté comme une couche décisionnelle complémentaire, distincte des modules IA / LLM.
- `AI Workforce Forecasting` doit être décrit comme une expérimentation analytique si son niveau de maturité reste exploratoire.

## 17. Références techniques utiles

### Images à insérer dans le chapitre 3

- `Diagramme d’architecture.png`
- `diagramme de classe.png`
- `Casdutilisation_globale.png`
- `casdutilisation_raffine.png`
- `Diagramme de séquence — Génération de roadmap.png`
- `Diagramme de séquence — Recommandations IA.png`
- `Diagramme de séquence — Score explicable.png`
- `diagramme d’activité.png`
- `N8n-logo-new.svg.png`
- `PhantomBuster.png`

### Dashboards Power BI à documenter

- `Executive Overview`
- `Product & Operations Analytics`

### Chaîne analytique à décrire

- `MySQL -> Power BI -> Power Query -> KPIs / Dashboards -> React / Node.js -> Interface Web`
- connexion MySQL comme source de données ;
- transformations Power Query ;
- mesures DAX et KPIs ;
- intégration React / Node.js via iframe ;
- expérimentation `AI Workforce Forecasting`.

### Endpoints importants

- `GET /api/score-report`
- `GET /api/candidate/score-report/:candidateId/:jobId/pdf`
- `GET /api/candidate/roadmap/general`
- `GET /api/candidate/roadmap/job/:jobId`
- `POST /api/candidate/roadmap/chat`
- `GET /api/recruiter/jobs/:jobId/ai-recommendations`
- `GET /api/recruiter/ai-recommendations/status/:jobRunId`
- `POST /api/recruiter/jobs/:jobId/candidates/:candidateId/personalized-questions`
- `GET /api/recruiter/risk-alerts`
- `GET /api/recruiter/jobs/:jobId/reengagement-pool`
- `POST /api/recruiter/candidate-search-runs/start`
- `POST /api/integrations/candidates/import`

### Tables importantes

- `score_reports`
- `candidate_skill_roadmaps`
- `candidate_roadmap_chat_messages`
- `ai_recommendation_jobs`
- `ai_recommendations_cache`
- `recruiter_personalized_question_cache`
- `reengagement_log`
- `reengagement_pool_jobs`
- `candidate_search_runs`
- `sourced_candidates`
- `LLMConsumptions`

### Fichiers backend significatifs

- `hr-BackendPfeHirecue/services/scoreReportService.js`
- `hr-BackendPfeHirecue/utils/scoreReportBuilder.js`
- `hr-BackendPfeHirecue/services/roadmapService.js`
- `hr-BackendPfeHirecue/services/roadmapChatService.js`
- `hr-BackendPfeHirecue/utils/roadmapAiClient.js`
- `hr-BackendPfeHirecue/services/aiRecommendationsService.js`
- `hr-BackendPfeHirecue/services/aiRecommendationsJobService.js`
- `hr-BackendPfeHirecue/services/personalizedRecruiterQuestionsService.js`
- `hr-BackendPfeHirecue/services/reengagementService.js`
- `hr-BackendPfeHirecue/services/recruiterCandidateSearchService.js`

### Fichiers frontend significatifs

- `repotinitialnpx/src/jsx/components/ScoreReport/ScoreDetailsReportPage.js`
- `repotinitialnpx/src/jsx/components/ScoreReport/ScoreDetailsRoadmapPage.js`
- `repotinitialnpx/src/jsx/components/ScoreReport/roadmap/RoadmapAssistantChat.jsx`
- `repotinitialnpx/src/jsx/components/ScoreReport/AIRecommendationsPanel.jsx`
- `repotinitialnpx/src/jsx/components/RecruiterRisk/RiskAlertsPage.jsx`
- `repotinitialnpx/src/jsx/components/user/ReEngagementPoolSection.jsx`
- `repotinitialnpx/src/jsx/components/Dashboard/CandidateSearchForm.jsx`
- `repotinitialnpx/src/jsx/components/Dashboard/ExternalJobsImportPanel.jsx`
