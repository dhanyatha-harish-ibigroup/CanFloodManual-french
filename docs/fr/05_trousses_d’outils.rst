.. _toolsets:

===============
5. Trousses d'outils
===============

Cette section concerne l'utilisation et le fonctionnement des trousses d'outils de CanFlood.

.. _Section5.1:

**********
5.1. Construction
**********

.. image:: /_static/build_image.jpg
   :align: right

La trousse d'outils de construction renferme un éventail d'outils dont on présente un résumé au Table5-1_ dans le but d’aider le modélisateur du risque d'inondation à fabriquer les modèles L1 et L2 de CanFlood.

.. _Table5-1:

*Tableau 5-1 : Résumé des outils de construction*

+-----------------+------------------------+------------------------+-----------------+-------------------+
| Tab Name        | Tool Name              | Description            | Inputs          | Outputs           |
+=================+========================+========================+=================+===================+
| Setup           | Start Control File     | Creates a Control      | name and        | Control File      |
|                 |                        | File template          | precision       | Template          |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Inventory       | Inventory Constructor  | Builds a finv          | vector layer,   | inventory vector  |
|                 |                        | template               | attributes      | layer (‘finv’)    |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Inventory       | Vuln. Function Library | GUI for selecting a    |                 | Vulnerability     |
|                 |                        | Function set           |                 | Function Set      |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Inventory       | Inventory Compiler     | Clip and extract finv  | ‘finv’,         | inventory tabular |
|                 |                        | data to tabular format | parameters      | data (‘finv’)     |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Hazard Sampler  | Raster Preparation     | Manipulate hazard      | hazard rasters  | hazard rasters    |
|                 |                        | rasters                |                 |                   |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Hazard Sampler  | Sample Rasters         | Sample hazard raster   | hazard rasters, | exposure dataset  |
|                 |                        | values                 | ‘finv’, DTM     | ('expos')         |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Event Variables | Store Evals            | Write event            | hazard event    | event variables   |
|                 |                        | probabilities to file  | probabilities   | (‘evals’)         |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Conditional P   | Conditional P          | Resolve conditional    | ‘finv’, failure | exposure          |
|                 |                        | exposure probabilities | polygons        | prob.(‘exlikes’)  |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| DTM Sampler     | DTM Sampler            | Sample DTM raster at   | ‘finv’, DTM     | ground elevations |
|                 |                        | asset geometry         |                 | (‘gels’)          |
+-----------------+------------------------+------------------------+-----------------+-------------------+
| Validation      | Validation             | Validate against       | complete model  |                   |
|                 |                        | model requirements     | package         |                   |
+-----------------+------------------------+------------------------+-----------------+-------------------+


Nom de l’onglet	Nom de l'outil	Description	Intrants	Extrants
Setup	Start Control File	Crée un modèle de fichier de commande	Nom et précision	Modèle de fichier de commande
Inventory	Inventory Constructor	Créer un modèle finv	Couche vectorielle, attributs	Couche vectorielle d’inventaire (‘finv’)
Inventory	Vuln. Function Library	IUG permettant de choisir un ensemble de fonctions 		Ensemble de fonctions de vulnérabilité
Inventory	Inventory Compiler	Couper et extraire les données finv sous forme de tableau	‘finv’, paramètres	Données tabulaires d'inventaire (‘finv’)
Hazard Sampler	Raster Preparation	Manipule les trames de risque	Trames de risque	Trames de risque
Hazard Sampler	Sample rasters	Échantillonne les valeurs de trame de risque	Trames de risque, ‘finv’. DTM	Ensemble de données d'exposition (‘expos’)
Event Variables	Store Evals	Écrit les probabilités d’événement dans un fichier	Probabilité d’événement à risque	Variables d’événement (‘evals’)
Conditional P	Conditional P	Résout les probabilités d'exposition conditionnelle 	‘finv’, polygones de défaillance	Prob. d'exposition (‘exlikes’)
DTM Sampler	DTM Sampler	Échantillonne la trame et la géométrie du bien DTM	‘finv’. DTM	Élévations du sol (‘gels’)
Validation	Validation	Valide les données par rapport aux exigences du modèle	Ensemble de modèle complet	

5.1.1. Setup
============

Cet onglet facilite la création d’un fichier de commande à partir des paramètres indiqués par l'utilisation et de l'inventaire, en plus de fournir les variables de commande de fichier générales pour les autres outils de la trousse d'outils.

5.1.2. Inventory
================

L’onglet Inventory renferme un ensemble d'outils permettant de construire et de convertir les inventaires de biens d'inondation (‘finv’; :ref:`Section4.1 <Section4.1>`). Le reste de cette section est consacré aux outils d'inventaire.

**Inventory Construction Helper** (Aide pour la création d'inventaire)

L’outil facultatif ‘Inventory Construction Helper’ aide la créer un modèle d'inventaire de biens inondés à partir d’une géométrie vectorielle à l'intérieur du cadre de ‘fonction imbriquée’ de CanFlood (:ref:`Section4.1 <Section4.1>`). Une analyse de données additionnelle à l'extérieur de la plate-forme CanFlood est généralement nécessaire afin de remplir ces champs.

**Vulnerability Function Library** (Ensemble de fonctions de vulnérabilité)

Pour faciliter la création de modèles de risque préliminaire, le plugin de CanFlood procure une série de bibliothèques de fonctions de vulnérabilité qu’on utilise fréquemment au Canada. On recommande aux utilisateurs d’étudier attentivement les fonctions de vulnérabilité existantes et leurs méthodes de construction avant de les intégrer à une analyse du risque. Les fonctions devraient être mises à l’échelle à tout le moins pour tenir compte des transferts spatiaux et temporels.

**Inventory Compiler** (Compilateur d'inventaire)

Le compilateur d'inventaire est un outil simple qu’on utilise pour préparer une couche vectorielle d'inventaire en vue de l’inclure dans un modèle CanFlood en procédant comme suit :

  1. Découper la couche vectorielle en fonction de la ZI (un une telle zone a été sélectionnée sur l’onglet Setup);
  2. Extraire le données non spatiales dans le répertoire de travail dans la forme csv; et
  3. Écrire l'emplacement du fichier de ce csv et le nom de fichier d’index dans le fichier de commande.

.. _Section5.1.3:

5.1.3. Hazard Sampler (échantillonneur de risque)
=====================

L’outil Hazard Sampler génère les ensembles de données d'exposition (‘expositions’) à partir d’un ensemble de trames d’événements à risque. De façon générale, ces trames d'événements à risque représentent les résultats WSL de certains modèles de risque (par exemple HEC-RAS) à des probabilités spécifiques. L’échantillonneur de risque présente deux modes de base :

  • **Value Sampling**: (Échantillonnage de valeur) Échantillonne les valeurs de trame pour chaque bien; il s’agit généralement de la valeur par défaut lorsqu’on s’intéresse à la profondeur d’un bien. En ce qui concerne les biens de types ligne et polygone, l'utilisateur doit préciser une statistique d'échantillonnage (globalement ou par bien).
  • **Area-Threshold Sampling**: (Échantillonnage de seuil de zone) Calcule le pourcentage de longueur de chaque bien ou zone au-dessus d’une valeur de trame de seuil; utile pour calculer le pourcentage d'inondation de segments de la route ou de polygones de culture agricole. Cela demande qu’on précise une couche DTM et un ‘seuil de profondeur’.

.. _Figure5-1:

.. image:: /_static/toolsets_5_1_3_haz_sampler.jpg

*Figure 5-1: Schéma de définition du calcul de risque où la ligne pointillée représente la valeur WSL d’un événement ‘ei’*

En utilisant les définitions présentées à la Figure5-1_, l’exposition WSL d’un événement i pour un bien unique j avec une hauteur *elv* :sub:`j` se calcule comme suit :
 
                           *expo* :sub:`i,j` = *WSL* :sub:`bl, ei` - *elv* :sub:`j`

L’échantillonneur de risque réalise les étapes générales suivantes au niveau de l'ensemble de couches de risques et de la couche d'inventaire fournis par l'utilisateur.

  1) Trancher la couche d'inventaire en fonction de la ZI (si on précise ‘Project AOI’).
  2) Pour chaque couche, échantillonner la valeur de trame ou calculer le pourcentage d'inondation de chaque bien;
  3) Sauvegarder les résultats dans le fichier csv ‘expositions’ dans le répertoire de travail et écrire ce chemin dans le fichier de commande;
  4) Charger la couche de résultats sur le canevas (facultatif).
  
**Value Sampling for Complex Geometries** (Échantillonnage de valeurs pour les géométries complexes)

Contrairement aux géométries par points, les inventaires présentant des géométries de type ligne ou polygone ont besoin de *statistiques d'échantillonnage* (par exemple, 'Min', 'Max', 'Mean') pour informer CanFlood de la façon dont la valeur de trame devrait se calculer à partir de la géométrie de chaque bien. Deux options sont prévues pour préciser les statistiques d'échantillonnage :

  • **Global**: (Globale) Une statistique d'échantillonnage unique est prescrite et utilisée pour toutes les géométries des biens (par exemple, prendre la valeur de trame ‘Max’ qui se trouve à l'intérieur de chaque polygone).  
  • **Per-Asset**: (Par bien) Une statistique d'échantillonnage est indiquée pour chaque bien par l'entremise d’une valeur de terrain sur l'inventaire (par exemple, prendre la valeur ‘Max’ pour certains biens et la valeur ‘Min’ pour d’autres). Cette façon de faire est la plus utile pour les géométries des biens plus gros et les trames présentant une variance élevée (par exemple, pour créer des DTM d'échantillonnage de polygones dans les zone présentant un terrain important).
  
  
**Raster Preparation** (Préparation de la trame)

L’échantillonneur de trame s’attend à ce que toutes les couches de risque présentent les propriétés suivantes:

  • le CRS de la couche correspond au CRS du projet;
  • les valeurs en pixels de couche correspondent aux fonctions de vulnérabilité (par exemple, les valeurs sont habituellement exprimées en mètres);
  • la source de données de la couche est ‘gdal’ (c'est-à-dire que l'outil ne prend pas en charge le traitement des couches Web).

Pour aider à rendre les trames conformes à ces attentes, CanFlood comporte une option ‘Raster Preparation’ (préparation de trame) sur l’onglet ‘Hazard Sampler’ dont les outils sont résumés au Table5-2_.

.. image:: /_static/toolsets_5_1_3_hazsamp_ras_prep.jpg

.. _Table5-2:

*Tableau 5-2 : Outils de préparation de trame*

+------------------------+---------------------------+-----------------------+--------------------------------+
| Tool Name              | Handle                    | Description                                            |
+========================+===========================+=======================+================================+
| Downloader             | Allow dataProvider        | If the layer’s dataProvider is not ‘gdal’              | 
|                        | conversion                | (i.e., web-layers), a local copy of the layer is       |
|                        |                           | made to the user’s ‘TEMP’ directory.                   |
+------------------------+---------------------------+-----------------------+--------------------------------+
| Re-projector           | Allow re-projection       | If the layer’s CRS does not match that of the project, | 
|                        |                           | the ‘gdalwarp’ utility is used to re-project the layer.|
+------------------------+---------------------------+-----------------------+--------------------------------+
| AOI clipper            | Clip to AOI               | This uses the ‘gdalwarp’ utility to clip the           |
|                        |                           | raster by the AOI mask layer.                          |
+------------------------+---------------------------+-----------------------+--------------------------------+
| Value Scaler           | ScaleFactor               | For ScaleFactors not equal to 1.0, this uses the Raster|
|                        |                           | Calculator to scale the raster values by the passed    |
|                        |                           | ScaleFactor (useful for simple unit conversions).      |
+------------------------+---------------------------+-----------------------+--------------------------------+


Nom de l'outil	Fonction	Description
Downloader	Permet la conversion dataProvider	Si le dataProvider de la couche n’est pas ‘gdal’ (c'est-à-dire les couches Web), une copie locale de la couche est effectuée dans le répertoire ‘TEMP’ de l'utilisateur.
Re-Projector	Permet d’effectuer la re-projection.	Si le CRS de la couche ne correspond pas à celui du projet, l’utilitaire ‘gdalwarp’ est utilisé pour projeter de nouveau la couche.
AOI clipper	Découper vers la ZI	Cette fonction fait appel à l'utilitaire ‘gdalwarp’ pour découper la trame et fonction de la couche du masque de la ZI.
Value Scale	Facteur d’échelle	Pour les facteurs d’échelle qui ne sont pas égaux à 1.0, cette fonction utilise le calculateur de trame pour mettre à l'échelle les valeurs de trame par le facteur d’échelle passé (utile pour les conversions d’unité simple).

Après avoir exécuté ces outils, un nouvel ensemble de trames est chargé dans le projet.

**Sampling Geometry and Exposure Type** (Échantillonnage du type de géométrie et d'exposition)

Pour prendre en charge un vaste éventail d'analyses de vulnérabilité, l’outil Hazard Sampler est capable de développer des variables WSL et d'exposition d'inondation à partir des trois types de géométrie de base, comme on peut le voir au Table5-3_. Pour les géométries de types *ligne* et *polygone*, l'outil exige de l'utilisateur qu’il indique les statistiques d'échantillonnage pour les calculs de WSL et un seuil de profondeur pour les calculs d'inondation en pour cent.

.. _Table5-3:

*Tableau 5-3 : Configuration de l'échantillonneur de risque en fonction du type de géométrie et du [didacticiel pertinent.*]

+------------------------+---------------------------------------------+---------------------------------------------+
| Géométrie              |                       WSL                   |                 Inundation                  |
|                        +------------------------+--------------------+------------------------+--------------------+
|                        | Paramètres             | Exposure           | Paramètres             | Exposure           |
+========================+========================+====================+========================+====================+
| Point                  | Default                | WSL                | Default                | WSL :sup:`1`       |
|                        | [Tutorial 2a]          |                    | [Tutorial 1a]          |                    |
+------------------------+------------------------+--------------------+------------------------+--------------------+
| Ligne:sup:`4`          | Sample Statistic       | WSL Statistic      | % inundation,          | % inundation       |  
|                        | :sup:`3, 5`            |                    | Depth Thresh :sup:`2`  |                    |
|                        |                        |                    | [Tutorial 4b]          |                    |
+------------------------+------------------------+--------------------+------------------------+--------------------+
| Polygone:sup:`4`       | Sample Statistic       | WSL Statistic      | % inundation,          | % inundation       |
|                        | :sup:`3`               |                    | Depth Thresh :sup:`2`  |                    |
|                        |                        |                    | [Tutorial 4a]          |                    |
+------------------------+------------------------+--------------------+------------------------+--------------------+
| 1. Pour appliquer une profondeur de seuil, les valeurs f_elv peuvent être manipulées manuellement. Les valeurs     |
|    d'exposition WSL sont converties en exposition binaire (c'est-à-dire inondé ou non inondé) par le modèle de     |
|    isque (L1).                                                                                                     |
| 2. Une trame DTM doit être indiquée sur l’onglet ‘DTM Sampler’. Les outils du modèle prévoient que l'inventaire    |
|    de biens (‘finv’) comporte une colonne ‘f_elv’ avec tous les zéros et le paramètre .felv=’datum’. Respecte les  |
|    valeurs des cellules de trame NULLE comme n’étant pas inondées.                                                 |
| 3. Ignore les valeurs NoData lors du calcul des statistiques.                                                      |
| 4. Les valeurs M et Z ne sont pas prises en charge.                                                                |
| 5. Affiche l’erreur ‘feature(s) from input layer could not be matched’ lorsque des valeurs zéro sont rencontrées.  |
|    Il est possible d’ignorer cette erreur sans danger.                                                             |
+------------------------+-------------------------+--------------------+------------------------+-------------------+

Géométrie	WSL
	Paramètres
Point	Défaut (didacticiel 2A)
Ligne	Échantillon de statistiques 3.5 
Polygone	Échantillon de statistiques 3




.. _Section5.1.4:

5.1.4. Event Variables (variables d’événement)
======================

L’outil ‘Store Evals’ des variables d'événement enregistre les probabilités d'événement indiquées par l'utilisateur dans l'ensemble de données des variables d'événement (‘evals’). L’outil d'échantillonnage des données doit être exécuté en premier lieu pour remplir le tableau des variables d'événement.

**Remarques et limites**

Les éléments suivants s’appliquent aux variables d'événement et aux outils connectés :

  • Les modules de risque (L1 et L2) ont besoin d’au moins 3 événements présentant des probabilités uniques.

.. _Section5.1.5:

5.1.5. Conditional P (P conditionnel)
====================

Pour intégrer la défaillance des moyens de défense (:ref:`Section1.4 <Section1.4>`), les modèles ‘Risk (L1)’ et ‘Risk (L2)’ de CanFlood s’attendent à un ensemble de données de probabilités d'exposition résolues (‘exlikes’) qui indiquent la probabilité d'exposition conditionnelle de chaque bien par rapport à la trame de défaillance de chaque danger. L’outil ‘Conditional P’ permet une conversion d’une série de polygones et de trames de zone d’influence de défaillance (c'est-à-dire les extrants d’une analyse de fiabilité de protection contre les inondations) vers les ensembles de données des probabilités d'exposition résolues (‘exlikes’). Pour chaque événement de défaillance conditionnelle, l’outil ‘Conditional P’ s’attend à ce que l'utilisateur fournisse une paire composée des couches suivantes :

  • Trame de WSL qui serait réalisée au cours de l'événement de défaillance
  • Couche vectorielle avec éléments polygonaux indiquant l’ampleur de la probabilité des défaillances d’élément pendant l'événement à risque (‘polygones de défaillance’). Ces caractéristiques peuvent ne pas se chevaucher (conditionnelles simples) ou se chevaucher (conditionnelles complexes) comme on le verra ci-dessous.

L’utilisateur peut préciser jusqu’à huit jumelages de trame d’événement/polygone de probabilité d'exposition conditionnelle avec l’IUG.

CanFlood fait la distinction entre des polygones de probabilité d'exposition conditionnelle ‘complexes’ et ‘simples’ en fonction du chevauchement géométrique de leurs caractéristiques, comme on peut le voir au Table5-4_ et à la Figure5-2_.

.. _Table5-4:

*Tableau 5-4 : Sommaire du traitement des polygones de probabilité d'exposition conditionnelle.*
Type	Caractéristiques	Traitement	Exemple (Figure 5-5)
Trivial	Aucune	Les défaillances ne sont pas prises en compte, aucune probabilité d'exposition résolue (‘exlikes’) n’est requise.	s/o
Simple	Aucun chevauchement	L’outil ‘Conditional P’ joint la valeur d’attribut prescrite de la caractéristique polygonale sur chaque bien pour créer des probabilités d'exposition résolues (‘exlikes’).	F2, f3
Complexe	Avec chevauchement	Voir ci-dessous.	F1

.. _Figure5-2:

.. image:: /_static/toolsets_5_1_5_conditionalp.jpg

*Figure 5-2: Schéma conceptuel de polygone de probabilité d'exposition conditionnelle simple [gauche] ou complexe [droit] montrant une seule couche avec quatre caractéristiques.*

Pour les conditionnels complexes, l'outil ‘Conditional P’ présente deux algorithmes pour résoudre les polygones de défaillance qui se chevauchent à une seule probabilité de défaillance (pour un bien donné sur une trame de défaillance donnée) basée sur deux hypothèses alternatives pour la relation mécanistique entre les mécanismes de défaillance qu’on résume au Table5-5_.

.. _Table5-5:

*Tableau 5-5 : Algorithmes de résolution de polygone de probabilité d'exposition conditionnelle pour un conditionnel complexe*

Relation	Résumé de l’algorithme
Mutuellement exclusive	
Indépendante 1	
Où P(X) représente la probabilité de défaillance résolue pour un seul bien sur un événement donné, alors que P(i) représente la valeur probable de défaillance échantillonnée à partir d’une caractéristique d’un polygone de défaillance.
1.	Bedford and Cooke (2001)


5.1.6. DTM Sampler (Échantillonneur DTM)
==================

L’outil d'échantillonnage DTM utilise le même module que l'échantillonneur de risque pour échantillonner les valeurs de trame DTM au niveau de chaque bien qu’on retrouve sur la couche vectorielle d'inventaire. Cet outil produit l'ensemble de données d’élévation du terrain (‘gels’) et écrit la référence correspondante sur le fichier de commande. Cet ensemble de données est exigé par tout modèle lorsque les paramètres de hauteur ou d’élévation des données d'inventaire (‘finv’) sont indiqués par rapport au terrain (felv=’ground’).

5.1.7. Validation
=================

L’outil de validation effectue une série de vérifications sur le fichier de commande prescrit pour s’assurer qu’on répond aux exigences en matière de données du modèle indiqué. Si on satisfait les vérifications, la marque de validation correspondante est réglée dans le fichier de commande, permettant ainsi l'exécution du modèle.

.. _Section5.2:

**********
5,2. Modèle
**********

.. image:: /_static/run_image.jpg
   :align: right

La trousse d'outils ‘Model’ comporte une IUG pour faciliter l’accès aux trois modèles de risque d'inondation de CanFlood. Les modèles L2 de CanFlood sont répartis entre l'exposition et le risque pour faciliter les applications personnalisées (qu’il est possible de relier en cochant la case ‘Run Risk Model (L2)’). Les onglets suivants sont utilisés dans la trousse d'outils de modèles de CanFlood :

  • *Setup*: Chemin des fichiers, descriptions d'exécution et paramètres facultatifs utilisés par tous les outils du modèle;
  • *Risk (L1)*: Analyse de la probabilité d'inondation;
  • *Impacts (L2)*: Première partie des modèles L2, exposition par événement calculée avec les fonctions de vulnérabilité;
  • *Risk (L2)*: Deuxième partie des modèles L2, valeur attendue de tous les impacts d’un événement;
  • *Risk (L3)*: Modèle de recherche SOFDA

**Batch Runs** (Exécutions par lots)

Afin de faciliter la simulation des lots pour les utilisateurs avancés, tous les modules de modélisation de CanFlood ont réduit les exigences en matière de dépendance (par exemple, l’IPA de QGIS n’est pas nécessaire).

**Parameter Summary** (Résumé des paramètres)

Le tableau suivant renferme un résumé des paramètres pertinents pour la trousse d'outils de modèle CanFlood qu’il est possible d’indiquer dans le fichier de commande.

*Résumé des paramètres de fichier de commande CanFlood*
Nom 	Option	Attente type	Valeur par défaut	Description
Nom		Str		Nom du scénario/modèle exécuté
cid				Colonne d’index pour les 3 ensembles de données inventoriés (finv expos gels)
Prec		Int		Précision flottante pour les calculs
Ground_water		Bool		Marque devant inclure les profondeurs négatives dans l'analyse
Felv		Str		Plan horizontal de référence ou terrain
Event_probs		Str	ari	Format de probabilités d'événement (dans le fichier de données evals).
	Aep			Probabilité d'événement dans le fichier aeps exprimé sous forme de probabilités de dépassement annuel
	Ari			Exprimé sous forme d’intervalles de récurrence annuelle
Itail		Aucune	extrapoler	Gestion d'événement à probabilité zéro
	Flat			Événement à probabilité zéro égal aux impacts les plus extrêmes dans les séries passées
	Extrapoler			Régler l'événement à probabilité zéro en extrapolant à partir de l’impact le plus extrême (interp1d)
	Aucune			Ne pas extrapoler (non recommandé)
	Flotter			Utiliser la valeur passée en tant que valeur d’impact de probabilité zéro.
rtail		Aucune	0,5	Traitement d'événement à zéro impact
	Extrapoler			Régler l'événement à zéro impact en extrapolant à partir de l’impact le moins extrême.
	Aucune			Non-exécution d’un événement à zéro impact (non recommandé).
	Flat			Reproduit l’AEP minimal en tant qu’événement à zéro dommage (NON UTILISÉ)
	Flotter			Utiliser la valeur passée comme valeur AEP à zéro impact.
Drop_tails		Bool	faux	Extrapolation d’EAD : à savoir si on doit enlever les valeurs extrapolées avant d’écrire les résultats par bien.
intégrer		Str		Méthode d'intégration NumPy qu’on doit appliquer (trapz par défaut)
As_inun		Bool		Marque à savoir si on doit traiter les expositions en fonction du % d'inondation.
Event_rels		Str		Hypothèse permettant de calculer la valeur attendue pour les événements complexes.
	Max			Valeur maximale attendue des impacts par bien à partir des événements dupliqués Dommage résolu = dommage maximal sans défaillance * prob de défaillance) valeur par défaut jusqu’au 2020-12-30
	mutEx			Tenir pour acquis que chaque événement est mutuellement exclusif (un seul peut survenir) (limite inférieure)
	Indep			Tenir pour acquis que chaque événement est indépendant (la défaillance d’un n’influence pas l’autre) (limite supérieure)
Impact_units		Str		Valeur d'étiquetage de l’axe des impacts avec (généralement réglé par Dmg2)
Apply_miti		Bool		Appliquer ou non les algorithmes d'atténuation.
Déviation de courbe		str		pour les bibliothèques de fonctions de dommages L1, préciser la déviation (facultatif).

.. csv-table:: 
   :file: /tables/52_controlFileDesc.csv
   :widths: auto
   :header-rows: 1

*Résumé des fichiers de données et des tracés des fichiers de commande de CanFlood*
Section 	Nom	Attente type	Description
Dmg_fps	Courbes	Str	Pour le chemin de fichier L2 vers la bibliothèque de fonctions de dommage .xls
	Finv	str	
	Expos	Str	
	Gels	Str	
Risk_fps	Dmgs	Str	Chemin de fichier des résultats des données sur les dommages (valeur par défaut S/O)
	Exlikes	Str	Chemin de fichier de données de probabilité d'exposition secondaire (valeur par défaut S/O)
	Evals	Str	Chemin de fichier de données de probabilité d'événement (valeur par défaut S/O)
validation	Risk1	Bool	
	Dmg2	Bool	
	Risk2	Bool	Marque de validation Risk2 (Faux est la valeur par défaut)
	Risk3	Bool	
Results_fps	Attrimat02	Str	fp de matrice d'attribution lvl2 (modèle après dommage)
	Attrimat03	Str	fp de matrice d'attribution lvl2 (modèle après risque)
	R_passet	Str	résultats par_bien à partir du modèle risk2
	R_ttl	Str	résultats totaux des modèles de risque
	Eventypes	Str	df des aep noFail et rEventName
Tracé	Couleur	Str	
	Style de ligne	Srt	
	Largeur de ligne	Flotter	
	Alpha	Flotter	
	Marqueur	Str	
	Taille du marqueur	Flotter	
	Fillstyle	Str	
	Impactfmt_str	str	Formateur python qu’on doit utiliser pour formater les valeurs des résultats des impacts





.. csv-table:: 
   :file: /tables/52b_controlFileDesc_filepaths.csv
   :widths: auto
   :header-rows: 1
   
Certains peuvent être configurés avec l’IU de la trousse d'outils *Build* de CanFlood, alors que d’autres doivent être indiqués manuellement dans le fichier de commande.

.. _Section5.2.1:

5.2.1. Risque (L1)
================

L’outil de risque L1 de CanFlood permet une évaluation préliminaire du risque d'inondation avec exposition binaire comme on le mentionne dans :ref:`Section3.1 <Section3.1>`. Cet outil prend également en charge les intrants de probabilité conditionnelle pour intégrer les défaillances de protection contre les inondations. Le Table5-6_ résume les exigences en matière d’intrants pour le modèle de risque (L1), qui sont généralement prêt à utiliser les outils ‘Build’ (:ref:`Figure3-1 <Figure3-1>`).

.. _Table5-6:

*Tableau 5-6 : Exigences en matière d'ensemble de modèle CanFlood pour le risque (L1).*
Nom	Description	Outil de construction	Code	Nécessaire
Fichier de commande	Chemins et paramètres du fichier de données	Start Control File		Oui
Inventory	Données d'inventaire de bien sous forme de tableau	Inventory Compiler	Finv	Oui
Exposition	WSL ou * de données d'exposition inondées	Hazard Sampler	Expos	Oui
Probabilité de l'événement 	Probabilité de chaque événement à risque	Variables d'événement applicables	Evals	Oui
Probabilité d'exposition	Probabilité conditionnelle de chaque bien composant la trame de défaillance	Conditional P	exlikes	Pour défaillance
Élévations du terrain	Élévation du terrain au niveau de chaque bien	Échantillonneur DTM	gels	Pour felv=rgound

Le module de risque (L1) peut être utilisé pour estimer un ensemble de paramètres simples par une utilisation créative des champs de l'inventaire de biens (‘finv’) abordés dans :ref:`Section4.1 <Section4.1>`. Lorsque le facteur ‘d’échelle’ est réglé à 1, la ‘hauteur’ à zéro et lorsqu’aucune probabilité conditionnelle n’est utilisée (typique pour l'analyse des inondations), la majeure partie du calcul devient banale, puisque le résultat repose simplement dans les valeurs d’impact fournies par le tableau ‘expositions’ (à l’exception du calendrier des valeurs attendues).

Les extrants fournis par cet outil sont résumés dans le tableau suivant :

.. _Table5-7:

*Tableau 5-7 : Résumé du fichier de sortie du modèle de risque.*
Code de sortie	Nom	Description
Résultats totaux	r_ttl	tableau de la somme des impacts (pour tous les biens) par événement et valeur attendue de tous les événements (EAD)
Résultats par bien	R_passet	tableau des impacts par bien par événement et valeur attendue de tous les événements par bien
Courbe du risque		Courbe de risque des impacts totaux

.. _Section5.2.2:

5.2.2. Impacts (L2)
===================

L’outil *Impacts (L2)* de CanFlood est conçu afin de procéder à une évaluation déterministe classique des dommages causés par les inondations basée sur un objet en utilisant des courbes de vulnérabilité, les hauteurs des biens et les valeurs WSL pour estimer les impacts des inondations attribuables à des événements multiples. Cet outil calcule les impacts sur chaque bien attribuables à un événement à risque (si le WSL de trame fourni a été réalisé). Les ‘Impacts (L2)’ ne tiennent pas compte des probabilités des événements (conditionnelles ou autres), puisqu’elles sont traitées dans le module de risque (L2) (voir la Section5.2.3_). Les exigences en matière d'ensemble de modèles sont résumées dans le tableau suivant :


*Tableau 5-8 : Exigences de l'ensemble du modèle d’impacts (L2).*
Nom 	Description 	Outil de construction	Code	Nécessaire
Fichier de commande	Chemins et paramètres du fichier de données	Start Control File		Oui
Inventory	Données d'inventaire de bien sous forme de tableau	Inventory Compiler	Finv	oui
Exposition	WSL ou données d'exposition %inondé	Hazard Sampler	Expos	Oui
Élévations du terrain	Élévation du terrain au niveau de chaque bien	DTM Sampler	Gels	pour felv=terrain
Ensemble de fonctions de vulnérabilité	Collection de fonctions concernant l’exposition aux impacts	Bibliothèque de fonctions de vulnérabilité	courbes	Oui

Les extrants des impacts (L2) sont résumés dans le tableau suivant, où seul l’extrant ‘dmgs’ est exigé par le modèle de risque (L2) :

*Tableau 5-9 : Extrants des impacts (L2).*
Nom de l’extrant	Code 	Description
Impacts totaux	dmgs	Les impacts totaux sont calculés pour chaque bien
Impacts développés des composants	dmgs_expnd	Impacts complets calculés pour chaque fonction imbriquée de chaque bien (voir ci-dessous)
résumé du calcul des impacts	bdmg_smry	classeur résumant les composants du calcul d’impact (voir ci-dessous)
profondeurs	depths_df	valeurs de profondeur calculées pour chaque bien
résumé de l’histogramme des impacts		tracé sommaire des valeurs d’impact totales par bien
tracé de boîte des impacts		tracé sommaire des valeurs d’impact totales par bien


**Nested Functions** (Fonctions imbriquées)


Pour favoriser les biens complexes (comme une maison vulnérable aux dommages touchant sa structure et son contenu), le paramètre Impacts (L2) favorise les fonctions de vulnérabilité composite paramétrées avec les 4 attributs clés (‘tag’, ‘scale’, ‘cap’, ‘elv’) avec le préfixe ‘f’ et le numérateur ‘nestID’ (par exemple, f0, f1, f2, etc.) qu’on aborde dans la :ref:`Section4.1 <Section4.1>`. CanFlood peut ainsi simuler une fonction de vulnérabilité complexe en combinant l'ensemble de fonctions de composants simples pour estimer les dommages causés par une inondation. Une entrée simple dans l'inventaire de biens (‘finv’) pour une habitation unifamiliale peut ressembler à ce qui suit :

+-------+--------+----------+--------+--------+--------+--------+----------+--------+
| xid   | f0_tag | f0_scale | f0_cap | f0_elv | f1_cap | f1_elv | f1_scale | f1_tag |
+-------+--------+----------+--------+--------+--------+--------+----------+--------+
| 14879 | BA_S   | 117.99   | 91300  | 11.11  | 20000  | 11.11  | 117.99   | BA_C   |
+-------+--------+----------+--------+--------+--------+--------+----------+--------+

Où BA_S correspond à une fonction de vulnérabilité pour estimer le nettoyage ou la réparation des structures, alors que BA_C estime les dommages causés au contenu du foyer (les deux en fonction de la superficie). On pourrait ajouter d’autres colonnes jX en tant que fonctions de vulnérabilité de composant pour les sous-sols, les garages, et ainsi de suite. Chaque groupe de quatre attributs clés est qualifié de ‘fonction imbriquée’ alors que la collection de fonctions imbriquées comprend la fonction de vulnérabilité complète d’un bien.

Le paramètre Impacts (L2) calcule l’impact d’un événement *ei* pour un seul bien *j* à partir de sa collection de fonctions de vulnérabilité imbriquées *k*. Ainsi :

.. image:: /_static/toolsets_model_5_2_2_impacts.jpg

Où chaque fonction de vulnérabilité imbriquée est paramétrée comme suit à partir de ‘l'inventaire des biens (finv)' (:ref:`Section4.1 <Section4.1>`) :

  • *tag*: variable établissant un lien entre le bien et la courbe de vulnérabilité correspondante dans la série de courbes de vulnérabilité (‘curves’);
  • *cap*: valeur maximale imposée au résultat de la courbe de vulnérabilité;
  • *scale*: valeur d’échelle appliquée au résultat de la courbe de vulnérabilité;
  • *elv*: distance verticale provenant de la valeur d'exposition;

les paramètres suivants de ‘l'ensemble de données d'exposition (expos)’ :

  • *expo*: ampleur de l'exposition à une inondation échantillonnée au niveau du bien.
  
et le paramètre facultatif suivant du ‘fichier de contrôle’:

  • *curve_deviation*: soit la courbe de déviation qu’on doit utiliser. 


La routine ‘Impacts (L2)’ calcule premièrement les impacts de chaque fonction imbriquée et met ensuite les valeurs à l'échelle et établit le maximum de ces valeurs avant de combiner toutes les valeurs imbriquées pour connaître l’impact total d’un bien donné.

De façon générale, l'ensemble de données d'exposition (‘expos’) est construit au moyen de l'outil ‘Hazard Sampler’ (Section5.1.3_) et renferme un WSL échantillonné pour chaque bien et chaque événement. Cependant, les seules exigences qui concernent le fichier ‘expos’ l’obligent à répondre aux attentes des fonctions de vulnérabilité auxquelles les paramètres ‘curves’ font référence (:ref:`Section4.3 <Section4.3>`).

**Ground Water** (Eau souterraine)

Pour améliorer le rendement, le paramètre Impacts (L2) n’évalue que les biens qui présentent des profondeurs positives (lorsque ‘ground_water’=Faux) et des profondeurs réelles. En spécifiant ‘ground_water’= *Vrai* , les profondeurs négatives (en-deçà de la profondeur minimale trouvée dans toutes les fonctions de dommage chargées) peuvent être comprises dans le calcul.

**Object Level Mitigation Measures** (Mesures d’atténuation du niveau de l’objet)

Le modèle ‘Impacts (L2)’ facilite la modélisation des réductions d'exposition provoquées par les mesures d'atténuation du niveau de l’objet (ou de la propriété) (PLPM) comme les clapets antiretour ou l'installation de sacs de sable. L’effet véritable de telles interventions sur l'exposition hydraulique des édifices ou des biens est complexe et peut être influencé par les facteurs suivants : 1) nature active ou passive du PLPM; 2) heure de l'avertissement et heure du jour ou année (pour les PLPM actifs); 3) charge hydraulique sur le PLPM; 4) qualité de l'installation du PLPM; 5) expérience ou erreur de l'opérateur (pour les PLPM actifs); 6) entretien du PLPM. CanFlood ne tient pas compte de cette complexité; CanFlood aide plutôt l'utilisateur à procéder à des calculs approximatifs en utilisant des seuils simples, des facteurs d’échelle et des valeurs d’addition. Cette paramétrisation devrait être utilisée pour chaque bien dans la couche vectorielle de l'inventaire (‘finv’) avec la Section5.2.2_ les champs suivants :

  • Seuil inférieur (*mi_Lthresh*): Toutes les profondeurs moins élevées produiront une valeur d’impact égale à zéro.
  • Seuil supérieur (*mi_Uthresh*): Toutes les profondeurs plus élevées n’entraîneront PAS l'application de facteurs d’échelle d’impact ou de valeur d’addition des impacts.
  • Facteur d’échelle d’impact (*mi_iScale*): Pour les profondeurs en dessous du ‘seuil supérieur’, les valeurs d’impact seront mises à l'échelle au moyen de ce facteur.
  • Valeur d’addition des impacts (*mi_ iVal*): Pour les profondeurs en dessous du ‘seuil supérieur’, cette valeur sera ajoutée aux valeurs d’impact.

**Additional Outputs** (Extrants additionnels)

Pour une analyse avancée, les utilisateurs peuvent choisir l'option ‘dmgs_expnd’ afin de produire les impacts complets calculés pour chaque fonction imbriquée de chaque bien. Cet imposant fichier de données intermédiaires présente les valeurs d’impact brutes, mises à l'échelle, plafonnées et résolues (les valeurs ‘plafonnées’ présentant un traitement nul et d’arrondissement) pour chaque bien et chaque fonction imbriquée. Cela peut être utile afin de procéder à une analyse additionnelle des données et au diagnostic des erreurs, mais il n’est pas nécessaire de le produire quelque routine que ce soit du modèle (parce qu’il est fourni à titre d'information seulement).

Un autre extrant facultatif est fourni par l'entremise de la fonction ‘bdmg_smry’ et du paramètre correspondant qui résume les résultats de chaque étape ou routine dans le module ‘Impacts (L2)’. Le premier onglet du chiffrier, ‘_smry’, montre les impacts totaux pour chaque événement au niveau de chaque routine du module. Le groupe suivant d’onglets résume les impacts calculés de chaque marque pour la routine correspondante (par exemple, ‘brutes’, ‘mises à l'échelle’, ‘plafonnées’, ‘dmg’, ‘mi_Lthresh’, ‘mi_iScale’, ‘mi_iVal’). Deux onglets additionnels existent pour résumer les calculs de la routine de plafonnement (c'est-à-dire ‘cap_cnts’ et ‘cap_data’).

.. _Section5.2.3:

5.2.3. Risque (L2)
================

L’outil ‘Risk (L2) de CanFlood a été conçu pour procéder à une évaluation ‘classique’ du risque d'inondation déterministe basé sur un objet à partir des estimations et des probabilités des impacts pour calculer un paramètre de risque annualisé. Au-delà de ce modèle de risque classique, l'outil ‘Risk (L2)’ facilite également les estimations du risque qui tiennent compte des événements de risque conditionnel, comme la défaillance d’une digue lors d’une inondation de 100 ans. Il est possible de conceptualiser ceci en faisant appel au cadre de ‘source-chemin-récepteur’ de Sayers (2012) qu’on peut voir à la Figure5-3_, où :

  • *Source*: (Source) Prédiction de WSL (en format de trame) pour les niveaux derrière la défense (comme la digue) d’un événement qui présente une probabilité quantifiée.
  • *Pathway*: (Chemin) Élément d’infrastructure qui sépare les récepteurs (c'est-à-dire les biens) de la prédiction brute du WSL. Il s’agit habituellement d’une digue, mais il pourrait s’agir de tout élément dont il est possible de quantifier la probabilité de ‘défaillance’ et le WSL (comme des portes d'évacuation de l'eau de pluie, des pompes pour l'eau de pluie).
  • *Receptor*: (Récepteur) Biens vulnérables aux inondations, lorsque l'emplacement et les variables pertinentes sont catalogués dans l'inventaire et lorsque la vulnérabilité est quantifiée au moyen d’une fonction de profondeur-dommage.

.. _Figure5-3:

.. image:: /_static/toolsets_5_2_3_sayers.jpg

*Figure 5-3: Cadre source-chemin-récepteur de Sayers (2012).*

Les exigences en ce qui concerne l'ensemble du modèle pour l'outil Risk (L2) sont résumées dans le tableau suivant :

*Tableau 5-10 : Exigences en ce qui concerne l'ensemble du modèle pour l'outil Risk (L2).*
Nom 	Description	Outil de construction	Code	Nécessaire
Fichier de commande	Chemins et paramètres du fichier de données	Start Control File		Oui
Probabilité de l'événement	Probabilité de chaque événement à risque	Event Variables	Evals	Oui
Probabilité d'exposition	Probabilité conditionnelle de chaque bien composant la trame de défaillance	Conditional P	Exlikes	Pour défaillance
Total des impacts	Modèle d’extrant des impacts (L2)	s/o	dmgs	oui

Les extrants que fournit cet outil sont résumés au Table5-7_.

**Events without Failure** (Événements sans défaillance)

Une simple application du modèle ‘Risk (L2)’ représente un domaine d'étude qui ne présente aucune infrastructure importante de protection contre les inondations (comme une plaine inondable sans digue), comme on l’a vu dans le didacticiel 2a (:ref:`Section6.2 <Section6.2>`). Dans ce cas, chaque événement à risque présente une probabilité unique et une trame unique, alors qu’on doit simplement intégrer les résultats de l'outil ‘Risk (L2)’ pour connaître le paramètre de risque annualisé. Le paramètre de risque principal calculé par CanFlood est la valeur attendue des impacts des inondations (appelé également *Expected Annual Damages* (EAD), ou *Average Annual Damages* (AAD), ou *Annualized Loss*) et il concerne les événements discrets du genre :

.. image:: /_static/toolsets_5_2_3_eq_1.jpg

Où x :sub:`i` représente l’impact total de l'événement i et p :sub:`i` signifie la probabilité que cet événement ait lieu. Alors que les modèles d'inondation discrétisent les événements par nécessité (par exemple, 100yr, 200yr), les inondations réelles génèrent des variables continues de risque (par exemple, 100 - 200yr). Par conséquent, la forme continue de l’équation précédente est nécessaire.

.. image:: /_static/toolsets_5_2_3_eq_2.jpg

Où *f(x)* re présente la fonction décrivant la probabilité d’un éventail *x* (c'est-à-dire la fonction de densité de probabilité) (USACE 1996). Pour l’harmoniser avec les expressions types de probabilité de rejet qu’on utilise fréquemment dans l'analyse du risque d'inondation, on manipule l’équation précédente pour obtenir :

.. image:: /_static/toolsets_5_2_3_eq_3.jpg

Où *Fx(x)* représente la probabilité cumulative d’un événement *x* (par exemple, la fonction de distribution cumulative). Reconnaissant que le complément de *Fx(x*) représente la probabilité de dépassement annuel (AEP) (la probabilité de réalisation d’un événement d’ampleur *x* ou plus), cette équation donne la ‘courbe de risque’ classique qu’on retrouve dans les évaluations du risque d'inondation qu’on peut voir à la Figure5-4_.

.. _Figure5-4:

.. image:: /_static/toolsets_model_fig_5_4.jpg

*Figure 5-4: Courbe de probabilité de dommage de Messner (2007).*

L’algorithme suivant est utilisé dans les modèles ‘Risk (L1)’ et ‘Risk (L2)’ de CanFlood pour calculer la valeur attendue:

  1. Réunir une série d’AEP et d’impacts totaux pour chaque événement;
  2. Extrapoler cette série avec les pseudos (‘rtail’ et ‘ltail’);
  3. Utiliser la méthode d'intégration NumPy <https://docs.scipy.org/doc/scipy/reference/integrate.html>`__ prescrite par l'utilisateur pour calculer la zone sous la série.

Le même algorithme est utilisé pour calculer la valeur attendue totale de tous les biens et pour connaître la valeur attendue de chaque bien à titre individuel (si ‘res_per_asset’=True).

**Events with Failure** (Événements avec défaillance)

Lorsqu’elle résout un événement à risque présentant une certaine défaillance, CanFlood combine la valeur attendue (E(X)) de chaque événement de défaillance complémentaire avec celle d’une valeur sans défaillance de base pour connaître la valeur attendue totale de l'événement exigée dans l’équation du paramètre de risque (formule 4). Pour bénéficier d’une certaine flexibilité dans les exigences relatives aux données dans l'analyse de fiabilité d’une défense, CanFlood fait la distinction entre deux dimensions de l'analyse de l'événement de défaillance en se basant sur la géométrie des polygones de probabilité d’exposition conditionnelle (‘polygones de défaillance’) et le nombre d'événements de défaillance qu’on résume à la Figure5-5_. La complexité des ‘polygones de défaillance’ est abordée dans la Section5.1.5_ et on résout la question dans l'ensemble de données des probabilités d'exposition résolues (‘exlikes’) en calculant une seule probabilité d'exposition pour chaque événement de défaillance complémentaire (Figure5-5_ ‘b1’ and ‘b2’ into ‘f1’). Après avoir simplifié le tout dans l'ensemble de données des probabilités d'exposition résolues (‘exlikes’), la relation, le nombre et la complexité de l'ensemble de polygones de défaillance de l'événement de défaillance sont ignorés.

.. _Figure5-5:

.. image:: /_static/toolsets_model_fig_5_5.jpg

*Figure 5-5: Exemple de schéma montrant trois événements à risque, un sans défaillance (e3), un présentant un événement de défaillance simple (e2) et un présentant un événement de défaillance complète (e1), ainsi que deux événements de défaillance complémentaires avec polygones de probabilité d'exposition conditionnelle simple (f2, f3) et complexe (f1) (polygones de défaillance).*

Le table5-11_ résume le traitement des événements à risque en fonction du nombre d'événements de défaillance qui sont attribués à chacun.

.. _Table5-11:

*Tableau 5-11 : Traitement de l'événement à risque en fonction du nombre d'événements de défaillance.*
Type	Nombre	Traitement	Exemple (figure 5-5)
Trivial	0	E(X)fail=0 E(X)nofail de l’équation 2	e3
Simple	1	‘max’ ou ‘mutEx’	e2
Complexe	˃0	‘max’, ‘mutEx’ ou ‘indep’	e1
1. Voir le tableau 5-12			

**Events with Complex Failure** (Événements présentant une défaillance complexe)

Le Table5-12_ contient un résumé des algorithmes mis en place par CanFlood pour calculer la valeur attendue de ces événements à risque avec plus d’un événement de défaillance complémentaire, c'est-à-dire des événements de défaillance ‘complexes’.

.. _Table5-12:

*Table5-12: Algorithmes de valeur attendue pour les événements de défaillance.*
Nom	Nombre	Résumé
Maximum modifié	Max	
Mutuellement exclusifs	mutEx	
Indépendant	indep	a.	Créer une matrice de toutes les combinaisons possibles d'événement de défaillance (positifs=1 et négatifs=0)
b.	Remplacer les valeurs de la matrice par P et (1-P)
c.	Multiplier l'ensemble pour obtenir la probabilité de la combinaison (P comb)
d.	Multiplier P comb par l’impact maximal des événements à l'intérieur de l'ensemble pour connaître l’impact de la combinaison (C comb)

P(o) = 1-sum(C i )



.. _Section5.2.4:

5.2.4. Risque (L3)
================

Bryant (2019) a élaboré le cadre modèle stochastique d'évaluation dynamique des dommages causés par les inondations à partir d’objets (SOFDA) pour simuler le risque d’inondation dans le temps à partir des courbes de l'Alberta et d’une prévision de réaménagement résidentiel. L’élaboration du cadre a été motivée par un désir de quantifier les avantages du Règlement sur les risques d'inondation (RRI) et pour aider à intégrer la dynamique du risque dans le processus décisionnel. La SOFDA quantifie le risque d'inondation d’un bien par l'utilisation de fonctions de dommages directs et d’une probabilité liée à la profondeur. De cette façon, le risque d'inondation peut être quantifié (c'est-à-dire monétisé) à des résolutions spatiales précises pour obtenir un soutien robuste au niveau de la prise de décisions.

La SOFDA présente les capacités suivantes :

  • Estimer la baisse de vulnérabilité du Règlement sur le risque d'inondation;
  • Estimer la baisse de vulnérabilité des Mesures de protection du niveau des propriétés;
  • Estimer l’influence de hausser les caractéristiques liées aux dommages (par exemple, augmenter la hauteur des chauffe-eau);
  • Simuler les changements dans la typologie des édifices concernés en raison du réaménagement (par exemple, des maisons plus grandes présentant des sous-sols plus profonds);
  • Modélisation dynamique et flexible de plusieurs éléments du modèle (comme des chauffe-eau plus dispendieux);
  • Quantification de l’incertitude (c'est-à-dire la modélisation stochastique);
  • Présentation d’extrants détaillés pour faciliter l'analyse des mécanismes sous-jacents.

Pour de plus amples renseignements et pour obtenir des conseils, voir :ref:`Appendix B <appendix_b>`.

.. _section5.3:

************
5.3. Résultats
************

.. image:: /_static/visual_image.jpg
   :align: right

La trousse d'outils ‘Results’ est une collection d’outils qui aident l'utilisateur à procéder à l'analyse et à la visualisation des données secondaires sur les modèles de CanFlood. Le reste de cette section décrit la fonction des outils qui font partie de cette trousse.

5.3.1. Join Geo (Liaison géométrique)
===============

Cet onglet comporte un outil permettant de relier les résultats du risque non spatial à la géométrie d'inventaire pour le post-traitement spatial. Une version de base de cet outil peut être exécutée automatiquement au moyen des outils ‘Risk (L1)’ et ‘Risk (L2)’. Sur l’onglet ‘Join Geo’, l'utilisateur peut procéder à une personnalisation additionnelle de ces couches, incluant l'application de styles de couches préemballées.

5.3.2. Risk Plot (Tracé du risque)
================

Cet onglet renferme plusieurs outils permettant de générer des tracés non spatiaux sur un scénario de modèle simple. Les tracés générés sur cet onglet utilisent tous l'information de style du groupe du fichier de commande [tracé] et les données des résultats du groupe ‘[results_fp]’. Les tracés sont disponibles dans les deux formats de courbe de risque standard :

  • ARI par rapport aux Impacts
  • Impacts par rapport à l’AEP

Voir les exemples à :ref:`Section6.3.3 <Section6.3.3>`.

**Plot Total** (Tracé total)

Cet outil génère un tracé simple des résultats totaux. Il est possible d’exécuter une version de base de cet outil à partir des outils ‘Risk (L1)’ et ‘Risk (L2)’ pour des raisons pratiques.

**Plot Stack** (Empilage des tracés)

Cet outil génère des courbes de risque montrant les contributions totales de chacune des fonctions de vulnérabilité composites qu’on aborde dans la  :ref:`Section4.1 <Section4.1>` sur un seul tracé.

**Plot Fail Split** (Répartition de défaillance des tracés)

Cet outil génère une courbe du risque composite montrant les résultats totaux et une deuxième courbe montrant la contribution de la partie ‘aucune défaillance’ de chaque événement (c'est-à-dire qu’on soustrait les contributions des événements de défaillance complémentaires) sur un seul tracé.

5.3.3. Compare/Combine (Comparer/combiner)
======================

Cet onglet renferme deux outils permettant de combiner ou de comparer plusieurs modèles CanFlood à l'intérieur d’une même analyse. Par exemple, une analyse du risque d'inondation tenant compte des pertes agricoles et des dommages aux immeubles résidentiels permettrait généralement de créer deux modèles distincts (c'est-à-dire des fichiers de commande séparés) et de combiner les résultats à la fin pour comprendre le risque total. Ou encore, une analyse peut souhaiter comparer deux mesures d'atténuation alternatives.

**Compare** (Comparer)

L'outil de comparaison recueille l'ensemble de données des résultats totaux (‘r_ttl’) et les paramètres de l'ensemble de fichiers de commande spécifiés pour produire deux extrants de comparaison :

  • *Control file comparison* (comparaison des fichiers de commande) : génère un fichier de données comportant les paramètres de chaque fichier de commande sélectionné et une dernière colonne précisant si le paramètre varie à l'intérieur de l’ensemble. Cette fonction peut être utile pour indiquer ce qui sépare deux modèles CanFlood.
  • *Plot comparison* (comparaison des tracés) : crée un tracé de courbe de risque comparant l'ensemble de données des résultats totaux (‘r_ttl’) de tous les fichiers de commande sélectionnés. Les valeurs de tracé par défaut proviennent du fichier de commande indiqué sur l’onglet ‘Setup’.

**Combine** (Combiner)

L’outil de combinaison recueille les ensembles de données sur les résultats totaux (‘r_ttl’) et les paramètres du fichier de commande principal (de l’onglet ‘Setup’) afin de générer deux types d’extrants :

  • *Composite scenario*: (Scénario composite) Choisir cette option au moment d’exécuter l'outil ‘Combine’ pour générer un nouveau fichier de commande composite et le fichier de résultats ‘r_ttl’ pour une analyse plus poussée.
  • *Plot combine* (combiner les tracés) : Crée une courbe de risque cumulative montrant la contribution à l'égard du risque total de chaque fichier de commande sélectionné.

5.3.4. Analyse des coûts-avantages
============================

Cet onglet renferme deux outils pour soutenir les calculs de base des coûts-avantages qui sont communément utilisés lors des évaluations des options d'atténuation des inondations. L’analyse des coûts-avantages (ACA) est un processus complexe qu’on aborde ailleurs (Merz et al. 2010; Smith et al. 2016; IWR et USACE 2017) et qui s’accompagne de nombreux défis et lacunes lorsqu’on l’applique aux décisions touchant l’atténuation des inondations (O’Connell and O’Donnell 2014; Hosein 2016). En résumé, l’ACA compare la valeur actualisée nette des coûts d’une intervention (comme la construction, l'entretien) pour le profit ou pour évider les inondations grâce à l'intervention. Par l'application d’un taux d’escompte à des calculs de la valeur nette actualisée, l’ACA est sensible au moment ou à l’accroissement des avantages et des coûts. Un flux des travaux typique lors de la mise en oeuvre d’une ACA dans CanFlood est présenté ci-dessous :

.. image:: /_static/toolsets_model_fig_5_3_4.jpg

Pour soutenir des calculs simples de l'ACA, l’onglet ‘BCA’ de CanFlood comporte les outils suivants :

**Copy BCA Template** (Copier modèle d’ACA)

Cet outil copie le modèle d’ACA de CanFlood (‘cf_bca_template_01.xlsx’, voir ci-dessous), qui présente des onglets ‘smry’ et ‘data’ et inscrit sur l’onglet ‘smry’ des métadonnées provenant du fichier de commande principal. Le fichier .xlsx renferme un modèle générique pour inscrire les séries temporelles des coûts et avantages du projet et calculer les valeurs financières sommaires, comme le rapport des coûts-avantages, en utilisant des formules intégrées dans EXCEL. Le cahier d'exercices renferme des ‘notes’ Excel et met en oeuvre les styles suivants pour guider les utilisateurs lorsqu’ils remplissent le modèle :

.. image:: /_static/toolsets_model_fic_5_3_4_legend.jpg

Une partie de l’onglet ‘données’ est présentée ci-dessous. Les utilisateurs devraient remplir les cellules des intrants en utilisant les valeurs de développement, d'exploitation et de perte attribuable aux inondations pour l'option concernée. Les principales cellules sur l’onglet ‘input’ sont nommées de manière à faciliter le processus qui consiste à générer les données de manière dynamique.

.. image:: /_static/toolsets_model_fig_5_6.jpg

*Figure 5-6: Onglet ‘data’ du modèle d'ACA de CanFlood.*

Une fois l’onglet ‘data’ complété, on recommande d’inscrire un taux d’escompte approprié sur l’onglet ‘smry’. Des taux d’escompte positifs sont fréquemment utilisés dans les analyses financières pour tenir compte du fait que les choses de valeur (comme les capitaux) valent plus aujourd'hui qu’ils ne vaudront à l’avenir. À ne pas confondre avec l’inflation. L’application de taux d’escompte positifs est inappropriée lorsqu’on évalue des biens qui sont de plus en plus rares, comme la fonction des écosystèmes et les espaces sauvages. Certains auteurs et directives proposent des taux d’escompte variables (Smith et el. 2016). Guidance on selecting an appropriate discounting rate is provided elsewhere (Farber 2016).

Après avoir rempli les onglets ‘data’ et ‘smry’, le cahier d’exercices devrait afficher les résultats résumés ci-dessous :

:PV benefits $:                             Valeur actualisée des avantages totaux
:PV costs $:                                Valeur actualisée des coûts totaux
:NPV $:                                     Valeur actualisée nette des coûts et des avantages
:B/C ratio:                                 Rapport entre les avantages de la VA et les coûts de la VA

**Plot Financials** (Tracé des données financières)

Cet outil génère un tracé temporel des données financières en ce qui concerne les données des avantages et des coûts qu’on retrouve dans le feuille de travail de l'ACA.

*********************
5.4. Outils additionnels
*********************

La section suivante décrit certains outils additionnels qu’on retrouve sur la plate-forme CanFlood et qui favorisent la modélisation du risque d'inondation au Canada. Ces outils sont accessibles à partir du menu CanFlood (Plugins > CanFlood).

.. _Section5.4.1:

5.4.1. Outil de cartographie de fragilité des digues
============================

Pour les modèles de risque qui prévoient une défaillance du système de défense des digues, un ensemble de données contenant les probabilités conditionnelles de chaque bien responsable de la défaillance, qu’on appelle ensemble de données de probabilité d'exposition résolue (‘exlikes’), est exigé pour les modules de risque (L1) et de risque (L2). De façon générale, cet ensemble de données est généré à partir d’une liste de ‘polygones de défaillance’ en utilisant l'outil ‘Conditional P’ dans la trousse d'outils de construction (Section5.1.5_). Alors que ces ‘polygones de défaillance’ peuvent être disponibles dans certains projets, il arrive souvent que seules les trames et l'information sur les digues qu’on trouve dans la :ref:`Section4.5 <Section4.5>` soient disponibles. Dans de tels cas, le flux des travaux résumé à la Figure5-7_ peut être utilisé, en commençant par l’outil de cartographie de fragilité des digues, qui présente une série d’algorithmes qu’on peut utiliser afin de générer des polygones de défaillance à partir d'information sur les digues types.

.. _Figure5-7:

.. image:: /_static/toolsets_5_4_1_fig_5_7.jpg

*Figure 5-7: Le flux des travaux types des outils de CanFlood, qui tient compte de la fragilité des digues, dont l'outil de cartographie de fragilité des digues, est utilisé pour développer la couche de données du polygone de défaillance.*

L’outil de cartographie de fragilité des digues ressemble à bien des égards au module Impacts (L2) appliqué aux biens présentant une géométrie linéaire, mais avec l’ajout d’un échantillonnage spécial des trames décalées, la jonction intelligente des résultats aux polygones, ainsi que les considérations de segmentation spécifiques à l'analyse des digues. Cet outil est exécuté en trois étapes qui sont résumées ci-dessous. Pour en savoir davantage sur la façon d’utiliser cet outil, voir le didacticiel 6a (:ref:`Section6.11 <Section6.11>`).

**Dike Exposure** (Exposition des digues)

Le sous-outil d’exposition des digues détermine l’emplacement de la vulnérabilité la plus élevée sur chaque segment de digue et retourne la valeur de franc-bord correspondante pour chaque trame d'événement, produisant ainsi l'ensemble de données d’exposition du segment de digue (‘dexpo’). Cela s’effectue au moyen de la séquence suivante :

  1) Générer des transects aux intervalles indiqués sur le côté indiqué de chaque segment de digue (lignes rouges à la Figure5-8_);
  2) Échantillonner l’élévation de la crête de la digue à partir de la trame DTM à la tête de chaque transect;
  3) Échantillonner la trame WSL de chaque événement sur chaque transect;
  4) Calculer les valeurs de franc-bord sur chaque transect comme étant la différence entre le WSL échantillonné et les valeurs d’élévation de la crête;
  5) Calculer la valeur de franc-bord du segment en appliquant les statistiques sommaires aux valeurs des transects concernés (la valeur par défaut est la valeur minimale).

.. _Figure5-8:

.. image:: /_static/toolsets_5_4_1_fig_5_8.jpg

*Figure 5-8: Exemple d’éléments d’un algorithme pour la routine d'exposition de l'outil de cartographie de fragilité de digue*

Ce sous-outil procure les extrants suivants :

  • *dike segment exposure (‘dexpo’) dataset* (ensemble de données d'exposition de segment de digue (‘dexpo’)): extrant .csv du franc-bord et intrant principal dans le sous-outil de vulnérabilité de la digue;
  • *processed dikes layer* (couche des digues traitée) (facultatif) : il s’agit d’une version modifiée du fichier d’intrants original montrant les données ‘dexpo’ sur la géométrie originale des digues;
  • *transects layer* (couche de transect) (facultatif): il s’agit des segments perpendiculaires dont la longueur et l’espacement sont précisés par l'utilisateur lorsqu’on procède à l'échantillonnage de l’élévation de la crête et du WSL au niveau de la tête et de la queue, respectivement;
  • *transect exposure points* (points d'exposition du transect) (facultatif): chaque tête d’un transect présentant toutes les valeurs calculées;
  • *breach points layer* (couches des points de bris) (facultatif): têtes de transect présentant des valeurs de franc-bord négatives;
  • *dike segment profile plots* (tracés de profil de segment de digue) (facultatif): tracé du profil de segment de digue montrant les éléments de crête échantillonnés et le WSL (voir ci-dessous).

.. image:: /_static/toolsets_5_4_1_fig_5_8_2.jpg

**Dike Vulnerability** (Vulnérabilité des digues)

Le sous-outil ‘Dike Vulnerability’ inscrit l’entrée correspondante de l'ensemble de données d'exposition de segment de digue (‘dexpo’) sur la courbe de fragilité associée avec chaque segment de digue. Le sous-outil produit un fichier .csv des données de probabilité de défaillance tabulaire (‘pfail’).

Les algorithmes suivants sont disponibles pour ajuster les probabilités de défaillance résultantes en ce qui concerne l’effet de longueur :

  • URS (2008): normaliser toutes les probabilités de défaillance par l'ensemble des longueurs de segment.

Un extrant secondaire semblable est fourni pour des valeurs ajustées en fonction de la longueur.

**Dike Failure Probability Results Join** (Jonction des résultats de probabilité de défaillance de digue)

Cet outil combine simplement les données de probabilité de défaillance tabulaire aux polygones d’influence de digue fournis afin de générer les ‘polygones de défaillance’ exigés par l’outil ‘Conditional P’ (Section5.1.5_).

**Notes and Considerations** (Remarques et considérations)

Lors de l'application de l'outil de cartographie de fragilité des digues à votre projet, on recommande de tenir compte des éléments suivant :

  • CanFlood n’effectue aucune analyse hydraulique. L’utilisateur doit fournir les polygones d’influence précisant la zone au-dessus de laquelle les biens devraient présenter leur probabilité de réaliser le WSL de la trame de défaillance correspondante. Sachant cela, les polygones d’influence peuvent se prolonger en toute sécurité au-delà des étendues des trames sans influencer le calcul des impacts de défaillance.
  • Les fonctions de fragilité devraient être développées et jumelées à chaque segment de trame par un expert qualifié en géotechnique à partir des données recueillies sur le terrain.

5.4.2. Add Connections (Ajouter connexions)
======================

L’outil ‘Add Connections’ de CanFlood |addConnectionsImage| ajoute un ensemble déjà compilé de ressources Web au profil QGIS d’un utilisateur pour faciliter l’accès et pour la configuration (comme l’ajout de justificatifs d'identité). L’ensemble de ressources Web ajouté par cet outil est configuré dans le fichier ‘canflood\_pars\WebConnections.ini’ (répertoire de plugins de l'utilisateur). La :ref:`Appendix A <appendix_a>` comporte un résumé des connexions Web ajoutées par cet outil.

On explique, dans le Guide de l'utilisateur de QGIS <https://docs.qgis.org/3.10/en/docs/user_manual/working_with_ogc/ogc_client_support.html#wms-wmts-client>`__ la façon de gérer ces connexions et d’y accéder. Après avoir ajouté les ressources à un profil d'utilisateur, deux méthodes de base peuvent être utilisées pour ajouter les données au projet.

  • **Browser Panel**: (Tableau de navigateur) Il s’agit de la méthode la plus simple, mais elle ne permet pas de préciser la demande de données. Sur le tableau du navigateur, développer le type d’intérêt du fournisseur (par exemple, ArcGisFeatureServer) > développer la connexion d’intérêt > sélectionner la couche d’intérêt > faire un clic droit > ajouter une couche au projet.

  • **Data Source Manager**: (Gestionnaire de la source de données) Il s’agit de la méthode recommandée, puisqu’elle est plus polyvalente lorsqu’on additionne à partir de connexions de données. Ouvrir le gestionnaire de source de données (Crtl + L) > sélectionner le type de fournisseur d’intérêt > sélectionner le serveur d’intérêt > sélectionner la couche d’intérêt > spécifier les paramètres additionnels de la demande > cliquer sur ‘Add’ pour charger la couche dans le projet.

Plusieurs plugins et outils utilisés par QGIS (et CanFlood) ne prennent pas en charge de telles couches Web (en particulier les trames). Par conséquent, des opérations de conversion et de téléchargement pourraient être nécessaires.

5.4.3. RFDA Converter (Convertisseur RFDA)
=====================

L’outil d’évaluation rapide des dommages causés par une inondation (RFDA) a été développé par la province de l’Alberta en 2014 en tant que plugin QGIS 2. Le RFDA ne comprend aucune analyse spatiale ni aucun calcul des risques. Les inventaires du RFDA sont présentés dans un format de chiffrier Excel (.xls) indexé en fonction de leur position dans la colonne (et non des étiquettes). Les courbes sont étiquetées par rapport aux biens par une concaténation des colonnes 11 et 12. Plusieurs colonnes de l'inventaire sont ignorées dans le RFDA. Il s’agit des colonnes fonctionnelles :

  • 0:'id1',
  • 10:'class',
  • 11:'struct_type',
  • 13:'area',
  • 18:'bsmt_f',
  • 19:'ff_height',
  • 20:'lon',*
  • 21:'lat',*
  • 25:'gel'

\* qui ne sont pas utilisées par le RFDA, mais qui sont nécessaires pour l'analyse spatiale.

Le RFDA utilise un format déjà existant pour la lecture des fonctions liées aux dommages. Ce format est basé sur les emplacements alternatifs des colonnes. Un exemple est présenté ci-dessous :

.. image:: /_static/toolsets_5_4_3_img.jpg

Le RFDA a été développé parallèlement à une série de fonctions de dommage 1D à partir d’une étude des structures des édifices à Edmonton et Calgary, AB en 2014. Les courbes de remplacement/réparation aux édifices et de dommages au contenu ont été développées séparément. Les courbes résidentielles pour l'étage principal et le sous-sol ont été développées séparément.

Lors de l'exécution d’un modèle, le RFDA applique une courbe de contenu et de structure à chaque bien, alors que le sous-sol correspondant est jumelé avec ‘bsmt_f’=Vrai.

Pour faciliter la conversion des inventaires du RFDA au format CanFlood, deux outils ont été prévus :

  1) Convertisseur d'inventaire; et
  2) Convertisseur de courbe de dommages.

**Inventory Conversion** (Conversion d'inventaire)

La conversion d'inventaire du RFDA nécessite une couche vectorielle de points en guise d’intrant (peut être créée à partir d’un fichier .xls en l’exportant vers csv pour ensuite créer une couche csv dans QGIS à partir des valeurs de lat/long). Pour les inventaires résidentiels (ceux dont le type de structure ne commence pas par ‘S’), chaque bien se voit attribuer un champ f0_tag avec le suffixe ‘_M’ pour signifier qu’il s’agit de la courbe de l'étage principal (par exemple BD_M) basé sur les valeurs concaténées ‘class’ et ‘struct+type’ dans l'inventaire. En utilisant la valeur ‘bsmt_f’, l'étiquette f1_tag se voit également attribuer un suffixe ‘_B’. Ces suffixes correspondent à la dénomination de courbe de l'outil DamageCurves (décrit ci-dessous). Le f1_elv est attribué à partir de f0_elv – bsmt_ht.

Pour les inventaires commerciaux (ceux sont la struct_type commence par ‘S’), l'étiquette f1_ et les champs f0_tag et f1_tag f reçoivent les valeurs ‘struct_type’ et ‘class’ séparément. Lorsque ‘bsmt_f’ = True, un troisième f2_tag=’ nrpUgPark’ est ajouté pour indiquer la présence d’un stationnement souterrain (une simple courbe $/m2 correspondante est créée par l'outil de conversion DamageCurves). Une fois la conversion effectuée, l'utilisateur peut démarrer le processus de construction du modèle CanFlood.

**DamageCurves Converter** (Convertisseur des courbes de dommage)

Cet outil convertit les courbes dans le format RFDA en un ensemble de courbes CanFlood (une courbe par onglet). Les combinaisons suivantes de courbes RFDA sont construites :

  • Individuelle (comme le contenu de l'étage principal)
  • Étage combiné (comme la structure et le contenu de l'étage principal)
  • Type combiné (comme le sous-sol et l'étage principal du point de vue de la structure)
  • Tous ces éléments combinés

Cela permet à l'utilisateur de personnaliser les courbes qui sont appliquées et la façon dont chaque bien (avec la caractéristique ‘fonction de vulnérabilité composite’ de CanFlood).

.. _Section5.4.4:

5.4.4. Add Styles (Ajouter des styles)
=================

Pour augmenter les styles de symboles contenus dans le QGIS afin de modifier l'affichage des caractéristiques des couches vectorielles, CanFlood a prévu une petite bibliothèque des styles types pour les projets d'inondation de GIS. Cette bibliothèque est un fichier .xml dans le répertoire des plugins. Il est possible d’ajouter cette bibliothèque à votre gestionnaire de styles à partir du menu CanFlood de la manière indiquée ci-dessous :

.. image:: /_static/toolsets_5_4_4_img.jpg

Après les avoir exécutés, ces symboles devraient être disponibles pour déterminer le style des couches vectorielles pertinentes par l'entremise d’un des dialogues de style de couche du QGIS. Par exemple, il est possible d’accéder au groupe ‘CanFlood’ par le volet ‘Layer Styling’ (F7) qu’on peut voir ci-dessous.

.. image:: /_static/toolsets_5_4_4_layer_styling.jpg

La fonction ‘Styling Manager’ |stylingManager| (gestionnaire de styles) de QGIS comporte une interface pour l'organisation et pour les autres tâches en lien avec les styles.

.. |stylingManager| image:: /_static/styling_manager_image.jpg
   :align: middle
   :width: 30

.. |addConnectionsImage| image:: /_static/add_connections_image.jpg
   :align: middle
   :width: 22
   
.. _Section5.4.5:

5.4.5. Sensitivity Analysis (Analyse de la sensibilité)
===========================

Le dialogue *Sensitivity Analysis* |targetImage| de CanFlood présente un flux des travaux et des outils permettant d’effectuer l'analyse de sensibilité sur un modèle CanFlood de type L1 ou L2. Ce dialogue peut être utile pour comprendre et faire connaître l’incertitude de votre modèle, ainsi que pour aider à déterminer les paramètres qu’on devrait prioriser lors de la collecte des données. Pour utiliser cette trousse d'outils, l'utilisation doit fournir premièrement un modèle de ‘base’ à partir duquel on procédera à l'analyse. À partir de ce modèle de base, il est possible d’utiliser la trousse d'outils d'analyse de la sensibilité pour : 1) construire un éventail de modèles candidats, alors que chaque candidat présente un seul paramètre ou une perturbation du fichier de données (dataFile); 2) exécuter la nouvelle suite de modèles; et 3) évaluer l'effet de la perturbation de chaque paramètre sur le paramètre d’impact annualisé ('ead_tot'). 

.. |targetImage| image:: /_static/target.png
   :align: middle
   :width: 22

Pour faciliter cette analyse, les onglets suivants sont prévus :
   
   1) Préparer l'analyse et charger le fichier de commande

   2) Assembler, configurer et terminer l'éventail des modèles candidats

   3) Manipuler les fichiers de données (facultatif)

   4) Exécuter la suite candidate

   5) Analyser les résultats

**Compile** (Compiler)

Cet onglet présente un tableau des paramètres des fichiers de commande pour chacun de vos modèles candidats. Pour remplir ce tableau, commencez par charger (*Load*) un fichier de commande principal à partir de l’onglet *Setup*. D’autres candidats peuvent être ajoutés et enlevés au moyen des boutons correspondants. Les valeurs des paramètres peuvent être éditées directement à l'intérieur du tableau; alors qu’une méthode pratique pour randomiser toutes les couleurs est prévue (cette méthode crée des chaînes de couleurs hex qu’il est possible de lire au moyen de matplotlib). On recommande d’utiliser des couleurs distinctes pour chaque candidat en vue de votre travail futur sur l’onglet *Analysis* (voir ci-dessous).

Pour construire chacun de ces modèles candidats et une copie de travail du modèle de base (dans leur propre sous-répertoire à l'intérieur de votre répertoire de travail), utilisez le bouton *Compile Candidates*. Cela a également pour effet d’activer l’onglet *DataFiles* et de remplir l’onglet *Run* avec chacun des fichiers de commande compilés. Les utilisateurs souhaitent généralement créer des copies séparées de chaque fichier de données (plutôt que de voir chaque candidat revenir aux fichiers de données du modèle de base) au moyen de l'option ‘Copy all candidate datafiles’. Cela permet d’examiner la sensibilité du paramètre annualisé des fichiers de données en manipulant chaque fichier de données dupliqué (par exemple, en ajoutant 1m à toutes les hauteurs). Précisons que cette option aura pour effet de remplir le tableau avec les nouveaux chemins des fichiers de données, incluant les chemins du modèle de base. Tous les modèles candidats utilisent des chemins de fichier absolus, et ce, peu importe la configuration sur l’onglet *Setup*. 

**DataFiles** (Fichiers de données)

L’onglet *DataFiles* facilite la manipulation des fichiers de données candidats. Lorsque tous les candidats ont été compilés (c'est-à-dire copiés dans leurs propres répertoires), il est possible d’accéder à chaque fichier de données à partir des boîtes combinées *Candidate Name* et *Parameter*. Le chemin de fichiers des données se remplira automatiquement. Il est ensuite possible de charger le fichier de données dans le projet (en tant que couche de mémoire sans géométrie) à partir de l’endroit où les champs peuvent être manipulés au moyen du tableau des attributs et du calculateur de terrain de QGIS. Les fonctions d’expression personnalisées sont également préchargées sous le menu ‘CanFlood’ dans le calculateur de terrain. Lorsque la manipulation désirée des valeurs d’attribut est appliquée, on peut utiliser le bouton *Save Datafile* pour réécrire la couche de mémoire sur un csv.

**Run** (Exécuter)

L’onglet *Run* affiche les chemins des fichiers de commande de chaque modèle de candidat chargé au moyen de la commande *Compile Candidates*. La suite de modèles peut être exécutée en vrac au moyen du bouton *Run*. Les résultats de cette exécution en vrac sont enregistrés dans un fichier python .pickle qu’il est possible de sauvegarder pour plus tard et de charger sur l’onglet *Analysis*.


**Analysis** (Analyse)

L’onglet *Analysis* résume les extrants de l'exécution en vrac chargée à partir du fichier python .pickle (voir la section précédente). Le tableau présente certaines statistiques simples, les paramètres qui ont été perturbés, ainsi que le rang du modèle candidat. Le rang correspond à la sensibilité du paramètre annualisé (ead_tot) au niveau du paramètre perturbé, où le candidat rank=1 a produit la plus grande différence par rapport au modèle de base.

Pour visualiser ces valeurs, il est possible d’utiliser le bouton *Plot Risk Curves* pour créer une courbe de risque combinée (semblable à la fonction *Compare* sur la trousse d'outils Results. On peut également utiliser le bouton *Plot Box* pour créer une boîte simple de toutes les valeurs ‘ead_tot’.  






