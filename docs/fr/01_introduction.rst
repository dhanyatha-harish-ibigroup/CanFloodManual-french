.. _introduction:

===============
1. Introduction
===============

CanFlood  est une trousse d'outils de calcul du risque d'inondation axée sur des objets, transparente et de source ouverte qu’on a créée pour le Canada. CanFlood  facilite le calcul du risque d'inondation avec trois ‘toolsets’:

  1) Création d’un modèle  |buildimage|                      

  2) Exécution d’un modèle  |runimage|                       
  
  3) Visualisation et analyse des résultats  |visualimage|

Chacun de ces ensembles comporte une série d'outils pour aider le modélisateur du risque d'inondation à réaliser plusieurs tâches qu’on utilise fréquemment pour évaluer le risque d'inondation au Canada.

Les modèles de risque d'inondation de CanFlood sont axés sur des objets, alors que les conséquences d’une exposition à une inondation sont calculés pour chaque bien (comme une maison) au moyen d’une fonction de vulnérabilité unidimensionnelle fournie par l'utilisateur (comme la fonction de dommage en profondeur) avant d’additionner les conséquences sur chaque bien et d’intégrer une série d’événements dans le but de connaître le risque total d'inondation dans une région. Pour répondre à la diversité des besoins en matière d'évaluation du risque d'inondation et en fonction des données disponibles au Canada, CanFlood appuie trois cadres de modélisation de plus en plus complexes, les exigences en matière de données et l'effort (Section1.1_). Chacun de ces cadres a été conçu de manière à ce qu’il soit flexible et agnostique, permettant ainsi aux modélisateur de mettre en place un outil logiciel unique et une structure de données tout en entretenant les modèles de risque d'inondation qui tiennent compte de l’hétérogénéité des biens et des valeurs des Canadiens. Reconnaissant le sens des infrastructures de protection contre les inondations sur le risque d'inondation au sein de plusieurs communautés canadiennes, les modèles de CanFlood  peuvent intégrer le potentiel de défaillance aux calculs de risque. Pour utiliser la collection croissante d'ensemble de données de modélisation des dangers au Canada, CanFlood  aide les utilisateurs à accéder et à manipuler ces données dans les modèles de risque d'inondation.

Le plugin de CanFlood n’est PAS un modèle de risque d'inondation. Il s’agit plutôt d’une plate-forme de modélisation comportant un éventail d’outils pour aider les utilisateurs à créer, à exécuter et à analyser leurs propres modèles. CanFlood  exige des utilisateurs qu’ils recueillent d’avance et qu’ils assemblent les ensembles de données qui décrivent le risque d'inondation dans leur zone d'étude (voir :ref:`Section3 <03_applications_et_flux_des_travaux>`). Lorsque l'analyse dans CanFlood est terminée, les utilisateurs doivent faire appel à leur propre jugement et expérience pour ajouter le contexte et les conseils nécessaires à tout rapport avant de transmettre les résultats aux décideurs. Les résultats de CanFlood ne devraient pas servir à *rendre* des décisions. On devrait plutôt les utiliser pour *informer* les décisions, ainsi que tous les autres aspects et critères pertinents pour la communauté à risque.

.. _Section1.1:

**************
1.1 Contexte
**************

La dévastation causée en 2013 par les inondations qui sont survenues dans le sud de l'Alberta et à Toronto ont donné lieu à une transition au Canada, alors qu’on a délaissé l'approche axée sur les normes, alors que la protection des inondations correspond à un niveau de sécurité unique, en faveur d’une approche axée sur les risques. Cette nouvelle approche axée sur les risques reconnaît qu’une planification stricte doit tenir compte de la vulnérabilité et de l'éventail complet des inondations qui peuvent frapper une communauté plutôt que de s’attarder à un seul événement présentant un concept arbitraire. De plus, une vision axée sur le risque permet aux décideurs d’optimiser de manière quantitative les mesures d'atténuation pour leur communauté, aidant les autorités disposant de budgets de moins en moins importants à étendre davantage les mesures de protection. La base des décisions rendues en vertu d’un système de gestion des inondations axé sur les risques est une évaluation des risques, c'est-à-dire :

   *Une méthodologie qui permet de déterminer le risque en analysant les dangers possibles et en évaluant les conditions de vulnérabilité actuelles qui, ensemble, pourraient possiblement causer des torts aux personnes exposées, aux biens, aux services, aux moyens de subsistance, ainsi qu’à l'environnement dont elles dépendent (ONUSIPC 2009).*

Pour quantifier le risque, les évaluations modernes du risque intègrent les données sur l'environnement naturel et bâti à des modèles prédictifs. Appliquée dans le domaine de la gestion du risque d'inondation, une analyse du risque est très sensibles aux éléments spatiaux du risque, soit la vulnérabilité (ce qu’on a construit, où et le caractère dommageable des eaux?) et le danger (l’endroit et l’intensité des inondations). L’évaluation de ces éléments s’effectue normalement dans une chaîne d'activités, comme la collecte de données, le traitement, la modélisation et le post-traitement pour en arriver aux paramètres de risque souhaités. Les éléments de base d’une évaluation type du risque d'inondation comprennent l'évaluation des dangers pour synthétiser les ensembles de données sur l'exposition spatiale et la probabilité, ainsi qu’une évaluation des dommages pour estimes les dommages aux biens à partir des résultats de l'évaluation des dangers, le tout suivi d’une quantification du risque qui fait appel aux probabilités pour estimer les dommages moyens.


1.1.1 Motivation
================

En tenant compte des limites des outils actuels et du besoin croissant de minimiser le tort causé par les inondations au Canada en comprenant mieux le risque d'inondation, RNCan a cherché à développer et à entretenir un outil de source ouverte flexible qui est adapté au Canada. Un tel outil uniformisé :

  • réduira le coût de chaque évaluation du risque d'inondation (ÉRI) en consolidant les coûts de développement et d'entretien du logiciel;

  • augmentera la transparence et l’uniformisation des ÉRI afin d’établir des comparaisons croisées du risque dans la zone d’étude et des mises à jour;

  • encouragera les communautés à procéder à des ÉRI additionnelles en réduisant l’opacité et le coût, ainsi qu’en sensibilisant davantage les gens;

  • facilitera et encouragera l’uniformisation et la collecte d'ensemble de données liés au risque d'inondation; et

  • favorisera une modélisation plus sophistiquée et rationalisée.

.. _Section1.1.2:

1.1.2 Directives
================

**Guides d'orientation fédéraux sur la cartographie des zones inondables**

"La série Guides d’orientation fédéraux sur la cartographie des plaines inondables » a été développée sous la direction du Comité de la cartographie des inondations (CCI). Le CCI est un partenariat entre Sécurité publique Canada, Ressources naturelles Canada, Environnement et Changement climatique Canada, le Conseil national de recherches du Canada, Recherche et développement pour la défense Canada, les Forces armées canadiennes, Infrastructure Canada, Relations Couronne-Autochtones et Affaires du Nord Canada." Il " s’agit d’une série de directives évolutives qui contribueront à faire avancer les activités de cartographie des inondations partout au Canada" (Sécurité publique Canada 2018). Il est possible de trouver les documents publiés en cherchant `"Guides d'orientation fédéraux sur la cartographie des zones inondables" sur le Web. <https://www.publicsafety.gc.ca/cnt/mrgnc-mngmnt/dsstr-prvntn-mtgtn/ndmp/fldpln-mppng-en.aspx>`__ The following are particularly relevant to CanFlood:

• Directives fédérales d’estimation des dommages causés par les inondations aux édifices et aux infrastructures (en cours d'élaboration)

• Procédures fédérales d'évaluation du risque d'inondation (en cours d'élaboration)

**Directives internationales**

+--------------------------------+------------+----------+----------+----------+----------+----------+----------+
|Instance responsable / autorité |     Directive (référence)                                                    |          
+================================+============+==========+==========+==========+==========+==========+==========+
| Royaume-Uni                    | Flood and coastal erosion risk management – Manual                           |
|                                | (Penning-Rowsell et al. 2013)                                                |
+--------------------------------+------------+----------+----------+----------+----------+----------+----------+
| United States                  | Multi-Hazard Loss Estimation Methodology, Flood Model:                       |
|                                |                                                                              |
|                                | Hazus-MH MR2 Technical Manual (FEMA 2012)                                    |
|                                | Risk-Based Analysis For Flood Damage Reduction Studies (USACE 1996)          |
|                                |                                                                              |
|                                | Tying flood insurance to flood risk for low-lying structures in the          |
|                                | floodplain (National Research Council 2015)                                  |
|                                | Principles of Risk Analysis for Water Resources (IWR and USACE 2017)         |
+--------------------------------+------------+---------------------+----------+----------+----------+----------+


1.1.3 Modèles basés sur le risque ou sur un événement
==================================

Dans l'histoire, la gestion des inondations impliquait des décisions axées sur un seul ‘événement de référence’ hypothétique et souvent arbitraire (comme un refoulement jamais vu en 100 ans). En raison d’une telle approche, plusieurs communautés n’avaient pas les moyens de se défendre, ce qui a probablement contribué à l'augmentation des pertes attribuables aux inondations qu’on a vues récemment au Canada (Fréchette 2016). En réaction à ce phénomène, la gestion moderne des inondations reconnaît la nécessité de procéder à des évaluations détaillées axées sur le risque qui tiennent compte de différents événements, de leur probabilité et leurs conséquences lorsqu’il s’agit de planifier la gestion. CanFlood a été conçu pour faciliter la gestion moderne axée sur le risque en intégrant différents événements impliquant des inondations (comme des événements qui surviennent tous les 10 ans, tous les 50 ans, tous les 100 ans ou tous les 200 ans), ainsi que leurs probabilités à des modèles axés sur le risque qui calculent les paramètres liés au risque. Cependant, puisque CanFlood calcule les impacts d’un événement avant de calculer le risque, les utilisateurs peuvent utiliser CanFlood afin de procéder à des évaluations axés sur un événement ou sur les impacts en effectuant toutes les étapes de calcul du risque, sauf la dernière.  

******************
1.2 Utilisateurs prévus
******************

Le plugin de CanFlood s’adresse aux utilisateurs qui possèdent des données sur l’espace et sur la vulnérabilité afin de procéder à une évaluation du risque d'inondation (ÉRI) axé sur un objet au Canada. CanFlood s’adresse aux praticiens dans le domaine du risque d'inondation qui possèdent l'expertise suivante :

   • Analyse du risque d'inondation axé sur un objet
   • QGIS (débutant)

Voir à la Section1.1.2_ un résumé des directives et des procédures en matière d’ÉRI au Canada.

.. _Section1.3:

*********************
1.3 Niveaux des modèles de risque
*********************

Les objectifs et les applications de l'analyse du risque d'inondation sont aussi variés que les communautés qu’ils desservent. Pour s’adapter à ce vaste éventail, CanFlood renferme trois types de modèles de risque qui présentent une complexité accrue, comme on peut le voir au tableau Table1-1_ et comme on en discute dans la :ref:`Section5.2 <Section5.2>`. Pour faciliter la construction et l'analyse de ces modèles de risque, CanFlood comprend également les trousses d'outils ‘Build’ et ‘Results’ respectivement (:ref:`Section5.1 <Section5.1>` et :ref:`Section5.3 <Section5.3>`). La façon de relier tous des éléments pour effectuer une analyse est décrite dans la :ref:`Section4.5 <Section4.5>` et des didacticiels comparables sont présentés dans la :ref:`Section6 <Section6>`.

.. _Table1-1:

*Tableau 1-1 - Résumés du niveau des modèles CanFlood*

.. list-table::
    :header-rows: 1
    :stub-columns: 1

    * - Niveau d'analyse 
      - L1 : Initial
      - L2 : Intermédiaire 
      - L3 : Détaillé 
    * - Motivation :sup:`1`
      - ÉRI rapide, évaluations de type bureau: premières approximations dans le but d’identifier les domaines où un travail plus détaillé est nécessaire.
      - Des évaluations plus détaillées lorsqu’une évaluation plus poussée de la perte de potentiel est justifiée.
      - Étude détaillée des pertes possibles et quantification robuste de l’incertitude
    * - Flux des travaux 
      - :ref:`Section3.1 <Section3.1>`
      - :ref:`Section3.2 <Section3.2>`
      - Annexe B
    * - Noms des outils du modèle CanFlood
      - Risque (L1)
      - Impacts (L2) et Risque (L2)
      - Risque (L3) (appelé également SOFDA)
    * - Données requises 
      - bas
      - moyen
      - haut
    * - Niveau de l'effort de modélisation (par bien) 
      - bas
      - bas
      - haut
    * - Complexité du modèle
      - bas
      - moyen
      - haut
    * - Fonctions d’impact
      - aucune (inondation seulement)
      - par objet
      - par objet, non compilé
    * - Quantification de l’incertitude 
      - aucune
      - aucune
      - modélisation stochastique
    - MPLP  
      - oui
      - oui
      - oui
    * - Dynamique du risque 
      - non
      - non
      - oui
    * - Géométrie du bien
      - point, polygone, ligne
      - point, polygone, ligne
      - point
    * - Intrants 
      - inventaire des biens, événements de danger, DTM (facultatif), événements de défaillance connexes (facultatif)
      - identique à L1 plus: Ensemble de fonctions d’impact
      - inventaire des biens, tableaux WSL, fonctions de vulnérabilité (non compilées) paramètres dynamiques, autres
    * - Extrants primaires
      - impacts totaux (‘r_ttl’), impacts par bien (‘r_passet’), courbe de risque
      - identique à L1
      - table d’exposition, schéma sommaire des impacts annualisés (sommaire et pour chaque bien), autres 

1. Adapté de Penning-Rowsell et al. (2019)

.. _Section1.4:

*****************
1.4 Fichiers de commande
*****************

Les modèles de CanFlood sont conçus pour permettre l’écriture et la lecture à partir de petits fichiers de commande. Grâce à ces fichiers, il est facile de créer et de partager un modèle ou un scénario spécifique et de tenir un registre de la façon dont ces ensembles de résultats ont été générés. Ces fichiers facilitent également les petits changements à un fichier d’intrants commun (comme l’inventaire des biens) et la reproduction de ce changement dans tous les scénarios. Les fichiers de commande ne renferment pas de données (volumineuses), mais uniquement des paramètres et des pointeurs menant aux ensembles de données exigés dans un modèle de CanFlood. Des conventions diligentes et uniformes de stockage et d’appellation des fichiers sont essentielles afin de connaître une expérience agréable en matière de modélisation. La plupart des paramètres des fichiers de commande et des fichiers de données peuvent être configurés dans la trousse d'outils ‘Built’. Cependant, certains paramètres avancés doivent être configurés manuellement (voir :ref:`Section5.2 <Section5.2>` pour connaître la description complète des paramètres des fichiers de commande) (Tous les intrants SOFDA doivent être saisie et configurés manuellement). La collection des intrants des modèles et le fichier de commande configurée portent le nom ‘d’ensemble modèle’ comme on peut le voir à la figure 1-1_. Pour en savoir davantage sur les fichiers d’intrants, voir :ref:`Section3 <applications_and_workflows>`.

.. _Figure1-1:

Figure 1-1. Pour en savoir davantage sur les fichiers d’intrants, voir :ref:`Section3 <applications_and_workflows>`.

.. image:: /_static/intro_1_4_conrol_files.jpg

*Figure 1-1: Schéma de la relation entre l'ensemble de modèle et les intrants de données L2 de CanFlood.*

.. |buildimage| image:: /_static/build_image.jpg
   :align: middle
   :width: 22

.. |runimage| image:: /_static/run_image.jpg
   :align: middle
   :width: 22

.. |visualimage| image:: /_static/visual_image.jpg
   :align: middle
   :width: 22
