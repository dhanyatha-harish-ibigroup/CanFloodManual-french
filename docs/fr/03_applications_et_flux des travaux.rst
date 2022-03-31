.. _applications_and_workflows:

=============================
3. Applications et flux des travaux
=============================

Le plugin de CanFlood  renferme une collection d'outils conçus pour aider les modélisateurs du risque d'inondation à réaliser différentes tâches fréquentes. Pour ce faire, CanFlood  est flexible, alors qu’il permet aux utilisateurs de relier ensemble les outils et les séquences dont ils ont besoin pour réaliser la tâche en cours. Une évaluation du risque d'inondation au moyen de CanFlood  demande de l'expertise dans le domaine de la modélisation du risque d'inondation, la connaissance de procédures, comme celles auxquelles on fait référence dans la :ref:`Section1.1.2 <Section1.1.2>` sans compter qu’elle fait généralement appel aux étapes suivantes :

  1. Identification des objectifs, de la portée et du but de l'évaluation.
  2. Sélection du niveau de modèle CanFlood approprié ((:ref:`Section1.3 <Section1.3>`) suivie de l'identification des données d’entrée nécessaires.
  3. Collecte et préparation des données d’entrée nécessaires (:ref:`Section3 <applications_and_workflows>`).
  4. Création de l'ensemble du modèle CanFlood  (voir ci-dessous).
  5. Éxécution de l'ensemble du modèle CanFlood à partir de l'outil du modèle approprié (:ref:`Section5.2 <Section5.2>`).
  6. Utilisation des outils ‘Results’ de CanFlood pour préparer des schémas et des cartes (:ref:`Section5.3 <Section5.3>`).
  7. Évaluation, documentation et communication des résultats, du contexte et des éléments d’incertitude.

Comme on le mentionne dans les références de la section, plusieurs de ces étapes doivent être réalisées à l'extérieur de la plate-forme de CanFlood .

**Création de modèles**

La plupart des flux des travaux dans CanFlood  exigent de l'utilisateur qu’il emploie une séquence comparable des outils ‘Build’ qui sont décrits dans :ref:`Section5.1 <Section5.1>` pour préparer l'ensemble de modèle CanFlood . La Figure3-1_ présente un flux des travaux type de l'étape ‘Setup’ à l'étape ‘Validation’. L’intégration ou non des étapes facultatives apparaissant sur le schéma dépendra de ce qui suit :

  • *Niveau de modèle de risque*: Les modèles L2 ont besoin de fonctionnement de vulnérabilité (‘courbes’) (voir la section3.2_).
  • *Bris de défense*: Les modèles L1 ou L2 comportant une certaine faille de protection ont besoin d’événements de faille compagnons (trames de faille et polygones de faille (voir la Section3.3_).
  • *Hauteurs des biens*: Les modèles L1 ou L2 comportant des inventaires de biens accompagnés de données de hauteur d’objet par rapport au sol ont besoin de données d’élévation du sol (‘gels’).
  • *Type d'exposition*: Les modèles L1 ou L2 qui présentent des biens géométriques sans point évaluant l'exposition en pourcentage d'inondation (:ref:`Section5.1.3 <Section5.1.3>`) ont besoin d’une couche DTM.

.. _Figure3-1:

.. image:: /_static/app_workflow_build_model.jpg

*Figure 3-1 : Flux des travaux de construction d’un modèle type faisant appel aux outils ‘Build’ de CanFlood  de l'étape ‘Setup’ à l'étape ‘Validation’. Les intrants de données sont décrites dans* :ref:`Section3 <applications_and_workflows>` *, alors que les outils et les extrants sont décrits dans*  :ref:`Section5.1 <Section5.1>` 

D’autres informations et des outils additionnels pour faciliter la construction des modèles sont présentés dans la :ref:`Section5.1 <Section5.1>`.

D’ici la fin de cette section, on résume certains types d'analyse typiques ou flux des travaux faisant appel aux modèles de risques énoncés dans la :ref:`Section1.3 <Section1.3>` et qu’on aborde en détail dans la :ref:`Section5.2 <Section5.2>` . Tous ces flux des travaux sont axés sur les risques, puisqu’ils comportent un vaste éventail de probabilités d’événements en plus de calculer les paramètres de risque. Les didacticiels qu’on retrouve dans la :ref:`Section6 <Section6>` présentent les instructions étape par étape et les données d’intrant correspondantes pour faire la démonstration de ces flux des travaux.

.. _Section3.1:

****************************************
3.1. Évaluation basée sur l'exposition au risque (L1)
****************************************

Les évaluations basées sur l'exposition (L1) quantifient la probabilité d'exposition binaire des biens aux inondations (mouillés ou secs). Ces données peuvent être utiles afin de procéder aux évaluations initiales, lorsque les ressources et les données sont limitées, pour identifier les zones devant faire l'objet d’une étude plus poussée. Dans le système CanFlood , cette opération s’effectue en recueillant les données, en élaborant un modèle de risque (L1), en exécutant le modèle et en évaluant les résultats. Contrairement aux évaluations axées sur la vulnérabilité (L2, Section3.2_), les évaluations axées sur l'exposition (L1) ne tiennent pas compte de l’influence de la profondeur d'inondation sur le risque. Autrement dit, une maison qui comporte un étang dans sa cour seraient considérée au même titre qu’une maison complètement engloutie par l’eau. Cependant, les évaluations basées sur l'exposition (L1) peuvent être utilisées pour estimer les paramètres de risque additionnels en utilisant les paramètres de mise à l'échelle de CanFlood  (par exemple, en estimant une perte de cultures en multipliant la zone inondée par une constante de perte/zone). Les évaluations basées sur l'exposition (L1) peuvent comporter une évaluation de la faille de défense si des données de probabilité d'exposition sont disponibles (Section3.3_). La Figure3-1_ et la Figure3-2_ présentent un flux des travaux sommaire d’un risque typique (L1). Pour en apprendre davantage sur le modèle de risque (L1), voir la :ref:`Section5.2.1 <Section5.2.1>`.

.. _Figure3-2:

.. image:: /_static/app_wrkflw_3_1_risk_ecp.jpg

*Figure 3-2 : Flux des travaux d’un risque typique (construction après le modèle).*

.. _Section3.2:

*********************************************
3.2. Évaluation axée sur la vulnérabilité su risque (L2)
*********************************************

Les évaluations axées sur la vulnérabilité (L2) quantifient le risque d’impact de certaines inondations sur les biens, lorsqu’il est possible de lier l’impact à la profondeur. Les modèles de risque qui tiennent compte de la vulnérabilité en fonction de la profondeur de l'inondation sont fréquemment utilisés pour évaluer le risque d'inondation pour les édifices, le contenu des édifices et les infrastructures. Dans CanFlood , une telle évaluation s’effectue en recueillant des données, en créant ou en recueillant des fonctions de vulnérabilité, en créant un modèle de risque (L2) , en exécutant ce modèle et en évaluant ensuite les résultats. L’élément de ce processus qui représente souvent le principal défi consister à colliger des fonctions de construction ou de vulnérabilité (:ref:`Section4.3 <Section4.3>`) que les version futures de CanFlood  peuvent prendre en charge : Les évaluations axées sur la vulnérabilité (L2) comportent généralement une évaluation de la faille de défense (Section3.3_). La Figure3-1_ et la Figure3-3_ présentent un flux des travaux sommaire d’un risque typique (L2). Pour de plus amples renseignements sur le modèle de risque (L2), voir :ref:`Section5.2.3 <Section5.2.3>`.

.. _Figure3-3:

.. image:: /_static/app_wrkflw_3_2_vuln.jpg

*Figure 3-3 : Flux des travaux du risque type (L2) (après la construction du modèle).*

.. _Section3.3:

********************
3.3. Faille de défense
********************

Plusieurs zones développées au Canada compte sur une forme quelconque d'infrastructure de défense contre les inondations (comme des levées ou des pompes de drainage) pour réduire l'exposition des biens. Chacune de ces infrastructures peut se briser lors d’une inondation. Si on ignore le potentiel de défaillance (P :sub:`fail` =0), on sous-estimera le risque réel d'inondation dans une zone (biais négatif du modèle). En présumant qu’une telle infrastructure présentera toujours une défaillance (P :sub:`fail` =1), il est possible de surestimer de manière drastique le risque d'inondation (biais positif du modèle). Une ou l’autre hypothèse réduira la confiance à l'égard du modèle et la qualité de toute décision liée à la gestion des inondations qu’on prend en s’y basant. Dans plusieurs régions au Canada, la protection contre les inondations joue un tel rôle important dans la mécanique d'exposition qu’un traitement binaire de la probabilité de défaillance (P :sub:`fail` = 0 or 1) rendrait inutile les paramètres de risque calculés du modèle. En reconnaissant l'importance des infrastructures de protection contre les inondations dans le domaine de la gestion des risques d'inondation au Canada, le risque (L1) ou le risque (L2) de CanFlood  facilite l’intégration de la défaillance des moyens de défense au calcul du risque.

Une application fréquente de cette capacité consiste à intégrer la fragilité de la levée à un modèle de risque. Il arrive souvent que les domaines d'étude présentent des groupes de biens protégés au moyen d’une levée, alors que chaque bien est vulnérable à un point de brèche n’importe où le long d’un anneau de la levée. Cette situation peut être analysée en discrétisant la levée en segments, en estimant la zone d’influence d’une brèche le long de chaque segment (pour l'événement *j*), en estimant la probabilité conditionnelle qu’une brèche survienne (pendant l'événement *j*) et en élaborant des trames de danger pour les conditions de la brèche. On recommande de faire appel à des professionnels qualifiés dans le domaine hydrotechnique et géotechnique pour réaliser cette analyse et générer les intrants dont CanFlood  a besoin et qu’on retrouve résumés à :ref:`Section4.2 <Section4.2>`.

3.3.1. Flux des travaux
===============

La défaillance des défenses est intégrée aux calculs de risque lors des flux des travaux du risque (L1) et du risque (L2) de CanFlood, incluant les étapes générales suivantes :

  1) recueillir la série de trames de l'événement dangereux (:ref:`Section4.2 <Section4.2>`), ainsi que l'information sur le profil de la digue, la fragilité et la zone d’influence (:ref:`Section4.5 <Section4.5>`).

  2) Calculer le risque de défaillance de la digue pour chaque événement dangereux et le cartographier dans la zone d’influence de la digue au moyen de l'outil ‘Dike Fragility Mapper’ (:ref:`Section5.4.1 <Section5.4.1>`) pour obtenir l'ensemble des polygones de défaillance.

  3) À partir des polygones de défaillances, extraire, résoudre et attribuer les probabilité de défaillance conditionnelles pour chaque événement de défaillance dans l'ensemble de données des probabilités d'exposition résolues (‘exlikes’) au moyen de l'outil ‘Conditional P’ (:ref:`Section5.1.5 <Section5.1.5>`).

  4) Exécuter le modèle de risque (L1) ou de risque (L2) pour employer les algorithmes de CanFlood dans le but de calculer les valeurs attendues avec une défaillance des défenses (:ref:`Section5.2.3 <Section5.2.3>` *Events with Failure*).

La Figure3-4_ présente un résumé de l’algorithme complet des valeurs attendues de CanFlood .

.. _figure3-4:

.. image:: /_static/app_wrkflw_3_3_1_wrkflw.jpg

*Figure 3-4 : Algorithme de calcul de la valeur totale attendue (E(X)) de l'outil de risque (L1 et L2) de CanFlood*

3.3.2. Relations avec l'événement
======================

Pour calculer les valeurs attendues (dans des modèles plus complexes), l'application de l'outil ‘Conditional P’ et des modèles de risque repose sur la prise en compte de la relation entre les événements fournis par l'utilisateur. Autrement dit, lorsque plusieurs défaillances sont indiquées, on doit préciser la façon dont elles devraient/ne devraient pas être combinées. Pour calculer et intégrer les corrélations des défaillances entre les éléments d’un système de défense, il est important de posséder une compréhension sophistiquée et mécanistique du système, ce qui déborde des compétences de CanFlood . En guise d’approximation alternative, CanFlood  présente deux hypothèses de base, qui sont résumées à la Figure3-5_, en ce qui concerne la relation entre les éléments de la défaillance. Ces hypothèses alternatives sont présentées afin de permettre à l'utilisateur d’essayer la sensibilité des corrélations entre le modèle et les éléments de défaillance; si on constate que le modèle présente une sensibilité à ce paramètre, on recommande de procéder à une analyse plus sophistiquée du système de défense.

.. _Figure3-5:

.. image:: /_static/app_wrkflw_3_3_2_event_relations.jpg

*Figure 3-5 : Exemple de schéma d’espace de probabilité montrant deux événements, soit [gauche] indépendant ou [droit] qui sont mutuellement exclusifs et où ‘P(o)’ représente la problème qu’il n’y ait aucune défaillance.*
