.. _inputs_and_data_requirements:

===============================
4. Exigences en matière d’intrants et de données
===============================

Les modèles CanFlood sont utiles uniquement en fonction des ensembles de données dont ils sont constitués. Voici un résumé des principaux ensembles de données que l'utilisateur doit recueillir et compiler avant de créer un modèle CanFlood.

.. _Section4.1:

********************
4.1. Liste des biens
********************

La liste des biens (‘finv’) est une liste détaillée des objets ou des biens dont l'exposition sera évaluée par les routines du modèle CanFlood . La liste des biens est un ensemble de données spatiales qui requiert les champs suivants lorsqu’on l’utilise dans les modèles de risque (L1) :

  • *fX_scale*: valeur permettant d’établir la fonction de vulnérabilité (par exemple, la superficie);
  • *fX_elv*: élévation pour ancrer la fonction de vulnérabilité (par exemple, hauteur du premier étage + DTM);
  • *geometry*: données géospatiales utilisées pour localiser le bien en vue de procéder à son échantillonnage;
  • *FieldName Index (cid)*: entier identifiant un bien unique et utilisé pour lier les ensembles de données.

En ce qui concerne les modèles d’impacts (L2) et de risque (L2), les champs additionnels suivants sont nécessaires :

  • *fX_tag*: valeur correspondant au modèle de la fonction de vulnérabilité qu’on doit attribuer à ce bien;
  • *fX_cap*: valeur servant à limiter la vulnérabilité prévue (par exemple, la valeur d'amélioration).

D’autres champs sont permis, mais CanFlood  les ignore. Le paramètre fictif ‘X’ présenté cdu est appelé ‘nestID’ et sert à regrouper les quatre principaux attributs servant à paramétrer une ‘fonction imbriquée’ dont le modèle Impacts (L2) a besoin (:ref:`Section5.2.2 <Section5.2.2>`). La trousse d'outils ‘Build’ renferme un outil ‘Inventory Constructor’ capable de remplir un modèle d’inventaire pratique. Afin de remplir ce modèle pour une zone d'étude, on doit généralement procéder à une vaste analyse de données à l'extérieur du plugin de CanFlood .

.. _Section4.2:

******************
4.2. Événements à risque
******************

CanFlood a besoin d’un ensemble d’événements à risque pour calculer l'exposition et le risque d'inondation. Pour calculer le risque, chaque événement devrait présenter les paramètres suivants :

  • **Event probability**: probabilité que l'événement survienne. On peut inscrire de paramètre comme étant les probabilités de dépassement annuel (AEP) ou les intervalles de récurrence annuelle (AR(). Il arrive souvent qu’on obtienne ces paramètres en procédant à une analyse statistique des inondations passées. Cette information n’apparaît pas dans les données de trame en tant que tel. Par conséquent, une pratique exemplaire consiste à l’inclure dans le nom de la couche.

  • **Event raster**: emplacement et WSL de l'inondation. L’outil ‘Hazard Sampler’ de CanFlood  (:ref:`Section5.1.3 <Section5.1.3>`) prévoit qu’il s’agit d’un fichier de données de trame, mais les routines du modèle de CanFlood  n’ont besoin que des données d'exposition sous forme de tableau (‘expos’). Les valeurs doivent correspondre aux données du projet (WSL) et sont habituellement développées à partir du logiciel de modélisation hydraulique.

  • **Companion failure events (optional)**: Renferme l'information sur la probabilité et sur l'exposition résultants d’un bris du système de protection contre les inondations pendant l'événement à risque. Chaque événement à risque peut se voir attribuer plusieurs événement d’échec (voir: :réf:`Section1.4 <Section1.4>`) en précisant la même probabilité d’événement pour chaque ensemble de données ‘evals’ (voir :réf:`Section5.1.4 <Section5.1.4>`).

      o Trame de bris: emplacement et WSL de l'événement de bris compagnon.

      o Polygone de bris : Couche du polygone de probabilité d'exposition conditionnelle présentant les caractéristiques indiquant l’ampleur et la probabilité des bris d’élément pendant l'événement. L’outil ‘Dike Fragility Mapper’ tool (:réf:`Section5.1.5 <Section5.1.5>`) présente un ensemble d’algorithmes servant à préparer ces polygones à partir de l'information sur la fragilité des digues types et des trames d’événement. L’outil ‘Conditional P’ a besoin de ces polygones de bris pour générer les ensembles de données des probabilités d’exposition résolues (‘exlikes’) dont les modules de risques (L1) et de risque (L2) ont besoin.

.. _Section4.3:

*******************************
4.3. Ensemble de fonctions de vulnérabilité
*******************************

En ce qui concerne le modèle des impacts (L2), CanFlood  a besoin d’une bibliothèque des fonctions d’impact comportant une fonction pour chaque étiquette de bien qui fait partie de l'inventaire. Le fichier de données est un chiffrier .xls, alors que chaque onglet correspond à une fonction d’impact séparée. Chaque onglet renferme :

  • des métadonnées sur la fonction (qui ne sont pas utilisées par CanFlood ; et
  • une fonction 1D qui traduit l'exposition en impact.

Un exemple est présenté ci-dessous avec une description. Dans le modèle des impacts (L2), on assiste à l’interpolation de la fonction de vulnérabilité de chaque bien à la valeur d'exposition (à partir de l'ensemble de données ‘expos’) pour estimer la valeur d’impact. Les variables d'exposition sont habituellement la profondeur, alors que les variables d’impact sont les dommages, mais l'utilisateur peut personnaliser le modèle en inscrivant dans l'ensemble de données ‘expositions’ des variables d'exposition alternatives et en développant des fonctions de vulnérabilité avec des extrants alternatifs (comme les personnes déplacées = f(pourcentage inondés)).

*Tableau 4-1 : Exigences et description du format de la fonction d’impact de CanFlood .*

   
.. csv-table:: 
   :file: /tables/41_impactFunctionFormat.csv
   :widths: auto
   :header-rows: 1

.. _Section4.4:

********************************
4.4. Modèle de terrain numérique (DTM)
********************************

Un DTM de projet est nécessaire uniquement pour les modèles qui présentent des hauteurs de biens relatives (elv).

.. _Section4.5:

*********************
4.5. Information sur les digues
*********************

Pour utiliser le module ‘Dike Fragility Mapper’ (:réf:`Section5.4.1 <Section5.4.1>`) afin de générer l'ensemble ‘failure polygon’, on a besoin des renseignements suivants sur le système de digues de la zone d'étude :

    • **Dike alignment**: Ce niveau de la ligne contient les renseignements suivants sur les digues étudiées :
        o face de la digue : indiquée par le sens de l’élément, ce qui indique à CanFlood le côté de l’élément dont on devrait échantillonner le WSL;

        o emplacement horizontal de la crête de la digue (c'est-à-dire la position des éléments);

        o façon dont chaque digue devrait être segmentée dans l'analyse (où chaque élément représente un segment);

        o identifiant de la digue (pour combiner plusieurs segments sur une seule courbe);

        o tampons de franc-bord qu’on devrait utiliser (par exemple, pour simuler l'installation de sacs de sable);

        o courbe de fragilité qu’on devrait utiliser pour calculer la probabilité de bris de ce segment.

    • **Dike fragility function library**: Ce type spécial de fonction d’impact (Section4.3_) concerne le WSL par rapport à l'élévation de la crête du segment (c'est-à-dire le franc-bord) par rapport à la probabilité de bris de ce segment (et pour réaliser le WSL du bris prévu).  L’élaboration de ces relations fait souvent appel à des propriétés mécaniques (comme la fondation, le cœur) et de réparation d'urgence (comme l’accessibilité aux véhicules d'entretien), ainsi qu’une analyse et une expertise géotechniques sophistiquées. Alors que le rendement des digues est généralement sensible à plus d’un type de charge que le franc-bord, CanFlood  ne prend en charge que les calculs de fragilité à variable unique.

    • **Dike segment influence areas**: Ces polygones présentent la géométrie de la zone où les biens seraient touchés par le bris d’un segment. De façon générale, cela ressemble à l’étendue de la trame du bris (comme les résultats d’une longueur de brèche sur le modèle hydraulique).

    • **Digital Terrain Model (DTM) of dike crest**: Il s’agit généralement du même ensemble de données que celui qu’on décrit dans la section 4.4. Cependant, l'évaluation d’une digue est particulièrement sensible aux petits changements d’élévation et les DTM présentent fréquemment des erreurs ou des artéfacts autour de la crête des digues si elles n’ont pas été construites pour permettre la modélisation des inondations. Pour cette raison, les utilisateurs devraient insister sur la qualité des DTM autour de la crête des digues lorsqu’ils procèdent à une analyse de la fragilité.
