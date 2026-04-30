# Contexte du projet HireCue

_Date de consolidation : 2026-04-30_

## Objectif

Relire et reformuler le rapport LaTeX afin d'ameliorer son originalite, son style humain et sa qualite academique, sans chercher a tromper les detecteurs IA. L'objectif est de produire un contenu authentique, professionnel et fidele au travail reellement realise dans le projet HireCue.

## Regles de redaction academique et d'originalite

- Le rapport doit etre redige dans un style humain, academique et personnel.
- Chaque paragraphe doit etre lie au projet reel HireCue.
- Les phrases generiques doivent etre evitees.
- Les textes inspires d'exemples doivent etre reformules.
- Les definitions generales doivent etre sourcees.
- Le rapport doit presenter uniquement le travail reellement realise.
- L'objectif est d'ameliorer l'originalite, la clarte et la qualite academique, sans chercher a contourner des detecteurs automatiques.

## Contexte du projet

### Presentation de HireCue

HireCue est une plateforme SaaS de recrutement assiste par l'IA. Elle centralise plusieurs etapes du processus de recrutement dans un meme environnement applicatif : gestion des offres, suivi des candidatures, evaluation des profils, analyse de scores, accompagnement candidat et outils d'aide a la decision pour les recruteurs.

Le projet de stage ne consistait pas a concevoir une plateforme a partir de zero. Il s'agissait plutot de faire evoluer un existant deja structure, afin de renforcer son niveau d'explicabilite, d'automatisation et de personnalisation, tout en conservant une architecture exploitable en conditions reelles.

### Domaine

Le projet se situe a l'intersection de trois dimensions :

- le recrutement numerique, avec des besoins de gestion d'offres, de candidatures et de suivi des parcours ;
- l'intelligence artificielle, utilisee pour enrichir l'analyse, generer des explications et assister la decision ;
- le modele SaaS, qui impose des exigences de robustesse, de scalabilite, de tracabilite et de maintien en production.

HireCue ne se limite donc pas a un simple job board. La plateforme s'oriente vers une logique d'orchestration du recrutement, dans laquelle les donnees existantes sont exploitees pour produire des informations plus utiles aux recruteurs et aux candidats.

### Etat initial de la plateforme

Avant les evolutions traitees pendant le stage, HireCue disposait deja d'un socle applicatif fonctionnel :

- gestion des utilisateurs, des offres et des candidatures ;
- dashboards pour plusieurs profils d'utilisateurs ;
- base de donnees MySQL stockant les informations liees aux candidats, aux jobs, aux scores et aux evaluations ;
- backend metier expose via des APIs ;
- premiers mecanismes de scoring et de parcours d'evaluation.

Cet existant constituait une base solide, mais plusieurs briques restaient a clarifier, fiabiliser ou enrichir, en particulier autour de l'explication des scores, de l'accompagnement candidat, de l'automatisation et de l'exploitation des services IA.

### Environnement technique

L'environnement technique observe dans les repositories du projet repose principalement sur les composants suivants :

- frontend principal en React dans `repotinitialnpx` ;
- backend principal en Node.js / Express dans `hr-BackendPfeHirecue` ;
- base de donnees MySQL avec Sequelize ;
- services IA en Python dans `hr-AiHirecue` et `hr-intelligencePFE` ;
- workflows d'automatisation via `n8n` ;
- sourcing LinkedIn via `PhantomBuster` ;
- traitements asynchrones via `RabbitMQ` pour certaines fonctionnalites ;
- usage de cache, logs applicatifs et quotas IA pour encadrer les traitements couteux.

### Repositories principaux analyses

- `repotinitialnpx` : frontend principal des dashboards et parcours applicatifs ;
- `hr-BackendPfeHirecue` : backend Node.js / Express / Sequelize / MySQL ;
- `hr-AiHirecue` : services IA separes pour certains workflows candidats et analyses ;
- `hr-intelligencePFE` : referentiel IA plus large, heterogene, incluant des briques d'explication, de roadmap et de generation.

### Architecture generale

L'architecture de HireCue repose sur une separation claire entre interface, logique metier, persistence et services externes :

- le frontend React porte les parcours utilisateur ;
- le backend Express centralise les routes, l'orchestration metier et l'acces aux donnees ;
- MySQL assure la persistence ;
- les services IA produisent certaines sorties textuelles ou analytiques ;
- n8n orchestre plusieurs automatisations externes ;
- RabbitMQ prend en charge certains traitements asynchrones, notamment pour les recommandations.

Cette architecture a influence toute la demarche du stage, car les fonctionnalites ajoutees devaient s'integrer a un systeme deja compose de plusieurs briques techniques et metier.

## Problematique

Le recrutement moderne genere une quantite importante de donnees : CV, offres d'emploi, resultats de tests, historiques de candidature, scores, interactions et sorties de services intelligents. Le probleme ne consiste plus seulement a collecter ces informations, mais a les organiser, les expliquer et les restituer sous une forme exploitable.

Dans l'etat observe avant les evolutions du stage, plusieurs limites apparaissaient :

- certains scores restaient difficiles a interpreter pour les utilisateurs ;
- les recruteurs ne disposaient pas toujours d'une aide a la decision suffisamment explicite ;
- les candidats avaient besoin d'un accompagnement plus clair sur leurs ecarts de competences ;
- le reengagement et certaines relances restaient insuffisamment structures ;
- une partie des integrations dependait encore d'operations manuelles ou peu fiabilisees ;
- l'usage des services IA devait etre mieux encadre par des quotas, du cache et des mecanismes de controle.

La problematique centrale peut ainsi etre formulee comme suit :

Comment faire evoluer une plateforme de recrutement existante vers une solution plus intelligente, plus explicable et plus automatisee, capable d'aider les recruteurs dans l'analyse des candidatures et d'offrir aux candidats un suivi plus utile, sans fragiliser l'architecture ni perdre la maitrise des traitements engages ?

## Objectifs du projet

### Objectifs fonctionnels

Les objectifs fonctionnels du projet portent sur l'amelioration des parcours recruteur et candidat a partir des donnees deja presentes dans HireCue.

Les principales cibles sont les suivantes :

- rendre le rapport de score plus lisible et plus exploitable ;
- proposer une roadmap de competences plus claire pour le candidat ;
- permettre un echange contextuel autour de cette roadmap via un chatbot ;
- fournir au recruteur des recommandations de candidats plus utiles ;
- generer des questions personnalisees pour preparer l'entretien ;
- ameliorer le suivi candidat, les notifications et le reengagement ;
- automatiser l'import d'offres et certaines integrations externes.

### Objectifs techniques

Les objectifs techniques concernent l'integration et la fiabilisation de ces fonctionnalites dans l'existant.

Ils peuvent etre resumes ainsi :

- integrer les services IA sans rompre la coherence du backend principal ;
- encadrer les appels LLM par des quotas et des mecanismes de controle ;
- reutiliser un cache pour limiter les recalculs couteux ;
- mettre en place ou consolider des traitements asynchrones lorsque la charge le justifie ;
- maintenir une tracabilite suffisante des traitements et des integrations ;
- articuler les workflows externes avec la plateforme sans degrader l'experience utilisateur.

## Travail realise

### Vue d'ensemble

Le travail du stage a porte sur l'evolution progressive de HireCue autour de fonctionnalites deja reliees a des besoins metier concrets. L'objectif n'etait pas d'ajouter des modules de facon decorative, mais d'ameliorer la lecture des resultats, d'assister la decision et de rendre plusieurs parcours plus fluides.

Les developpements ou consolidations les plus importants concernent le score explicable, les recommandations IA, la roadmap de competences, le chatbot associe, les workflows n8n, le reengagement candidat et l'integration frontend/backend de ces briques.

### Scoring IA et score report explicable

Une partie importante du travail a concerne le rapport de score explicable. L'enjeu etait de transformer un resultat numerique en une sortie plus comprehensible pour l'utilisateur, notamment en mettant en valeur le score global, certains sous-scores et une explication du resultat.

Elements observes dans le projet :

- endpoint principal : `GET /api/score-report` ;
- export PDF disponible via `GET /api/candidate/score-report/:candidateId/:jobId/pdf` ;
- lecture de plusieurs scores sources comme `Cscore`, `Pscore`, `Ptscore`, `Tscore` et `final_tests_score` ;
- calcul du score global a partir de poids stockes dans `final_scoring` ;
- persistence et cache dans la table `score_reports`.

Cette brique apporte une valeur directe, car elle rend l'evaluation plus lisible et donne un support plus defendable a la lecture du profil.

### Recommandations IA cote recruteur

Le projet comprend egalement un module de recommandations destine au recruteur. Son objectif est d'aider a identifier les profils les plus pertinents pour une offre, sans remplacer la decision humaine.

Elements observes dans le projet :

- endpoint principal : `GET /api/recruiter/jobs/:jobId/ai-recommendations` ;
- suivi d'etat par `GET /api/recruiter/ai-recommendations/status/:jobRunId` ;
- cache dans `ai_recommendations_cache` ;
- suivi des traitements dans `ai_recommendation_jobs` ;
- execution asynchrone via une queue RabbitMQ `hirecue.ai-recommendations.v1`.

Les recommandations combinent des elements comme le `fit score`, les competences rapprochees, les competences manquantes, la source et une explication associee.

### Roadmap de competences

Le stage a aussi porte sur la generation d'une roadmap de competences cote candidat. Cette fonctionnalite vise a rendre plus concret l'ecart entre le profil du candidat et les attentes du poste, en proposant un parcours d'amelioration plus structure.

Elements observes dans le projet :

- endpoints :
  - `GET /api/candidate/roadmap/general`
  - `GET /api/candidate/roadmap/job/:jobId`
  - `GET /api/candidate/roadmap/pdf`
- service principal : `services/roadmapService.js` ;
- construction metier : `utils/roadmapBuilder.js` ;
- appels IA : `utils/roadmapAiClient.js` ;
- persistence dans `candidate_skill_roadmaps` ;
- cache fonde sur `candidate_uid + job_id + input_hash` ;
- consommation de quota `quotaRoadmapGeneration`.

Cette fonctionnalite repond a un besoin academiquement et metierement important : ne pas seulement evaluer, mais aussi expliquer comment progresser.

### Chatbot RAG lie a la roadmap

Le chatbot associe a la roadmap permet au candidat de poser des questions contextuelles sur sa progression. Cette brique a pour role de prolonger la roadmap en une interaction plus souple, tout en restant centree sur le contenu deja calcule.

Elements observes dans le projet :

- endpoints :
  - `GET /api/candidate/roadmap/chat/history`
  - `POST /api/candidate/roadmap/chat`
- services principaux :
  - `services/roadmapChatService.js`
  - `repositories/roadmapChatRepository.js`
- table d'historique : `candidate_roadmap_chat_messages` ;
- dependance au contenu de `candidate_skill_roadmaps` ;
- quota dedie : `quotaRoadmapChat`.

Le code montre une logique de chatbot contextuel exploitable, avec historique et encadrement des appels. Une vraie couche RAG documentaire complete n'a toutefois pas ete clairement documentee dans les repositories observes.

### Questions personnalisees pour l'entretien

Une autre brique du projet concerne la generation de questions personnalisees pour le recruteur, a partir du profil du candidat et du poste cible.

Elements observes dans le projet :

- endpoint principal : `POST /api/recruiter/jobs/:jobId/candidates/:candidateId/personalized-questions` ;
- cache dans `recruiter_personalized_question_cache` ;
- objectif metier : preparer un entretien plus cible et moins generique.

Cette fonctionnalite renforce l'aide a la decision en prolongeant l'analyse du profil vers la phase d'entretien.

### Reengagement candidat et notifications

Le stage a egalement porte sur le reengagement du talent pool et le suivi de certaines notifications. L'enjeu etait de mieux exploiter les profils deja presents dans la plateforme au lieu de se limiter aux candidatures les plus recentes.

Elements observes dans le projet :

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

Ces mecanismes apportent une valeur operationnelle au recruteur, en structurant la relance et en amenant plus de continuite dans le suivi.

### Risk alerts dans le dashboard recruteur

Le dashboard recruteur integre egalement des alertes de risque destinees a attirer l'attention sur certains cas demandant une lecture plus fine.

Elements observes dans le projet :

- endpoint principal : `GET /api/recruiter/risk-alerts` ;
- actions associees :
  - `POST /api/recruiter/risk-alerts/ai-summaries`
  - `POST /api/recruiter/risk-alerts/:id/resolve`
  - `POST /api/recruiter/risk-alerts/:id/snooze`.

Cette brique prolonge la logique d'aide a la decision en ne se limitant pas aux seuls scores.

### Workflows n8n et automatisation

Une partie du travail concerne l'automatisation de flux metier via n8n. Ces workflows ne sont pas accessoires : ils participent a l'exploitation operationnelle de la plateforme.

Les usages observes concernent notamment :

- l'import d'offres externes vers `joblist` ;
- le reverse flow de publication vers LinkedIn, partiellement prepare dans le backend ;
- le candidate sourcing a partir de recherches LinkedIn ;
- certains flux de communication et d'outreach.

Points techniques releves :

- webhook d'import d'offres base sur `N8N_EXTERNAL_JOBS_WEBHOOK_URL` ;
- lancement de recherche sourcing via `POST /api/recruiter/candidate-search-runs/start` ;
- import des profils sources vers `sourced_candidates` ;
- suivi de runs dans `candidate_search_runs`.

Ces integrations montrent que HireCue cherche aussi a agir sur l'acquisition de profils et non seulement sur l'analyse interne des candidatures.

### Integration frontend/backend

Le travail ne s'est pas limite aux services backend. Une partie importante de la valeur du stage tient a l'integration des fonctionnalites dans les parcours visibles par l'utilisateur.

Composants frontend significatifs observes :

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

Cette integration est importante dans le rapport, car elle montre que les fonctionnalites ne sont pas restees au stade de services isoles. Elles ont ete rattachees a des parcours utilisateur identifiables.

## Methodologie

### Demarche iterative

La demarche adoptee pendant le stage repose sur une progression iterative. Ce choix etait necessaire, car les travaux portaient sur une plateforme deja existante, avec plusieurs repositories, des integrations externes et des dependances entre modules.

Une telle approche permettait :

- de comprendre l'existant avant d'ajouter de nouvelles briques ;
- de limiter les regressions ;
- de tester progressivement la coherence des integrations ;
- de traiter les priorites selon leur valeur metier et leur maturite technique.

### Agile Scrum

Pour la dimension applicative, la logique Scrum est adaptee au projet, car les evolutions ont ete menees par lots fonctionnels successifs : score report, roadmap, chatbot, recommandations, automatisation, reengagement et dashboards.

Cette approche permet de :

- prioriser les fonctionnalites a plus forte valeur ;
- articuler frontend, backend et integrations ;
- ajuster progressivement les travaux selon les retours et les contraintes rencontrees.

### CRISP-DM

Pour les briques IA et LLM, une logique proche de CRISP-DM reste pertinente. Les sorties generees ne peuvent pas etre evaluees uniquement sur leur faisabilite technique. Elles doivent aussi etre reliees au besoin metier, a la qualite des donnees disponibles et a la coherence des resultats retournes.

Dans le cadre de HireCue, cette logique se retrouve dans :

- la clarification du besoin avant generation d'une explication ou d'une roadmap ;
- l'analyse des donnees disponibles : scores, informations de poste, resultats de tests, CV ;
- la structuration des donnees d'entree ;
- l'evaluation de la qualite des sorties produites ;
- l'integration dans une plateforme avec quotas, cache et gestion d'erreurs.

## Resultats obtenus

### Fonctionnalites livrees ou consolidees

Au terme du travail analyse, les principales fonctionnalites livrees ou consolidees dans HireCue peuvent etre resumees ainsi :

- rapport de score explicable ;
- roadmap de competences ;
- chatbot contextuel lie a la roadmap ;
- recommandations IA pour les recruteurs ;
- questions personnalisees pour l'entretien ;
- reengagement du talent pool ;
- alertes de risque dans le dashboard recruteur ;
- automatisation de certains flux n8n ;
- integration visible de ces briques dans le frontend et le backend.

### Ameliorations apportees

Les ameliorations les plus marquantes portent sur trois plans.

D'abord, la plateforme devient plus lisible. Les scores, les recommandations et les parcours candidats sont mieux structures et plus faciles a exploiter.

Ensuite, la plateforme devient plus utile operationnellement. Les automatisations, le sourcing, le reengagement et certains traitements asynchrones reduisent la part de manipulation manuelle.

Enfin, la plateforme devient plus maitrisable techniquement. Le cache, les quotas IA, la persistence des resultats et la tracabilite renforcent la fiabilite de l'ensemble.

### Valeur apportee

Pour le recruteur, la valeur se situe dans une meilleure aide a la decision, une lecture plus rapide des profils et des outils plus concrets pour prioriser, analyser et relancer.

Pour le candidat, la valeur se situe dans une meilleure comprehension des resultats, un accompagnement plus structure a travers la roadmap et un echange plus contextualise via le chatbot.

## Difficultes rencontrees

### Difficultes techniques

Le projet a implique plusieurs difficultes techniques liees a la coexistence entre frontend, backend, services IA, base de donnees et workflows externes. Cette heterogeneite impose une vigilance constante sur la coherence des appels, des formats et des etats de traitement.

### Integrations API et services externes

Parmi les points sensibles observes dans le projet :

- ecarts de configuration entre environnements, notamment autour de la connexion a la base ;
- comportements differents entre appels testes via Postman et appels declenches depuis le frontend ;
- limitations ou erreurs d'integration cote LinkedIn ;
- incidents lies a PhantomBuster, comme `Agent not found` ou `auto launch disabled`.

### Workflows et orchestration

Les workflows n8n apportent de la valeur, mais ils introduisent aussi des difficultes de fiabilisation :

- dependance a des webhooks et services tiers ;
- gestion des retours d'execution et des echecs ;
- suivi incomplet de certains diagnostics d'erreur ;
- besoin de mieux documenter certaines chaines d'automatisation.

### Donnees et persistance

Le projet met egalement en evidence plusieurs enjeux lies aux donnees :

- besoin de garder des scores sources coherents ;
- necessite de persister correctement les sorties generees ;
- suivi des historiques utiles au chatbot et au reengagement ;
- verification de certaines tables ou nomenclatures non totalement homogenes entre code et documentation.

## Apports du stage

### Competences techniques

Le stage a permis de consolider plusieurs competences techniques :

- integration frontend/backend sur une plateforme existante ;
- structuration de services backend Node.js / Express ;
- manipulation d'une base MySQL via Sequelize ;
- integration de services IA et de fonctionnalites LLM ;
- mise en place de cache, quotas et traitements asynchrones ;
- orchestration de workflows n8n et interaction avec des services tiers.

### Competences organisationnelles

Au-dela des aspects techniques, le projet a aussi permis de renforcer :

- la capacite a analyser un existant avant intervention ;
- la priorisation de fonctionnalites selon leur valeur et leur faisabilite ;
- la coordination entre modules heterogenes ;
- la prise en compte des contraintes de robustesse et de production dans les choix techniques.

### Comprehension metier

Le stage a egalement apporte une meilleure comprehension du domaine du recrutement numerique :

- lecture des besoins des recruteurs ;
- importance de l'explicabilite dans les scores et les recommandations ;
- role de l'accompagnement candidat dans l'acceptabilite de la plateforme ;
- interet de l'automatisation pour fluidifier le traitement des candidatures.

## Perspectives

Les pistes d'evolution identifiees a partir du projet sont les suivantes :

- stabiliser davantage les workflows n8n en production ;
- renforcer la documentation des integrations externes ;
- clarifier certaines parties du reverse publishing LinkedIn ;
- mieux documenter ou completer la couche RAG si une base documentaire plus riche est mise en place ;
- renforcer la persistance de certains marqueurs de progression metier dans les roadmaps ;
- poursuivre l'amelioration des diagnostics d'erreur et de la tracabilite ;
- valider plus completement certains flux de sourcing et de publication externes.

## Points de vigilance pour la generation du rapport

- Le rapport doit presenter le travail reellement realise sur HireCue et non une plateforme theorique.
- Les descriptions de l'entreprise d'accueil doivent rester courtes et utiles au cadrage.
- La problematique, la mission, les moyens utilises et les resultats obtenus doivent rester au centre du contenu.
- Les fonctionnalites non confirmees dans le code ne doivent pas etre presentees comme des acquis certains.
- Les modules IA doivent etre decrits comme des briques d'assistance et non comme des mecanismes de decision autonome.

## References techniques utiles pour le rapport

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
