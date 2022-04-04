.. _Section6:

============
6. Didacticiels
============

Cette section renferme quelques didacticiels permettant à l'utilisateur de commencer à utiliser CanFlood. On suggère de parcourir ces didacticiels dans l’ordre en consultant la :ref:`Section5 <toolsets>` si on désire obtenir des renseignements plus détaillés. Pour tous les didacticiels, le projet CRS peut être configuré en toute sécurité à partir de n’importe laquelle des couches de données, sauf indication contraire. Les didacticiels sont rédigés en présumant que les utilisateurs connaissent le QGIS et la modélisation du risque d'inondation axé sur les objets. Toutes les données des didacticiels sont présentées dans la plus récente version du document `‘tutorial_data’ zip <https://github.com/IBIGroupCanWest/CanFlood/blob/master/tutorial_data_20210315.zip>`__ sur la page du projet.

.. _Section6.1:

***************************
6.1. Didacticiel 1a: Risque (L1)
***************************

Ce didacticiel guide l'utilisateur en procédant à l'application simple de la modélisation de risque dans CanFlood, ce qu’on qualifie d'analyse de niveau 1 (L1), alors que seule l'exposition binaire est calculée. Cette analyse ‘exposée ou non exposée’ peut être utile afin de procéder à l'analyse préliminaire lorsqu’on a suffisamment d'information pour modéliser la vulnérabilité des objets plus complexes.

6.1.1. Charger les données dans le projet
===============================

Télécharger les couches de données pour le didacticiel 1:

  • *haz_rast*: les trames d'événement à risque avec prédictions de valeur WSL pour la zone d'étude présentent quatre probabilités.

    o *haz_0050.tif*

    o *haz_0100.tif*

    o *haz_0200.tif*

    o *haz_1000.tif*

  • *finv_tut1a.gpkg* : couche spatiale de l'inventaire des biens d'inondation (’finv’).

Assurez-vous que le CRS de votre projet est réglé à ‘EPSG:3005’ (Tout dépendant de vos paramètres, ce réglage peut s’être déroulé automatiquement lorsque vous avez chargé les fichiers de données.) Sauf indication contraire, tous les didacticiels utilisent le CRS ‘EPSG:3005’. Voir le lien suivant pour une explication des projections dans le QGIS. `https://docs.qgis.org/3.10/en/docs/user_manual/working_with_projections/working_with_projections.html <https://docs.qgis.org/3.10/en/docs/user_manual/working_with_projections/working_with_projections.html>`_ ) et charger les couches téléchargées dans un nouveau projet QGIS (tout dépendant de vos réglages du QGIS, on pourrait vous demander de choisir la transformation si le CRS n’a pas été réglé correctement d’avance). Le canevas de votre toile devrait ressembler un peu à ce qui suit :

.. image:: /_static/tutorials_6_1_1_tiff.jpg

Explorez les attributs de la couche d'inventaire des biens d'inondation (‘finv’) (F6). Ce que vous verrez devrait ressembler à ce qui suit:

.. image:: /_static/tutorials_6_1_1_table.jpg

Les 4 champs sont 

  • *fid*: identificateur des caractéristiques intégrées (non utilisé) ;
  • *xid*: Nom de fichier d’index, identificateur unique du bien (n’importe quel champ présentant des valeurs entières uniques peut être utilisé comme index de noms de fichier (sauf les identificateurs des caractéristiques intégrées));
  • *f0_scale*: valeur permettant de mettre à l'échelle les résultats du calcul ‘f0’ pour ce bien;
  • *f0_elv*: hauteur (au-dessus de la référence du projet) à laquelle le bien est vulnérable aux inondations.

Pour cet exemple, chaque entrée ou ‘bien’ d'inventaire pourrait représenter une maison dont l’élévation de l'étage principal est inscrite dans ‘f0_elv’. Toutes les eaux d'inondation au-dessus de cette élévation seront considérées comme un impact pour ce bien. Pour l'analyse L1 de CanFlood, en fonction de la probabilité de chaque événement selon l'utilisateur, le ‘risque total d'inondation’ pour chaque bien et pour la zone d'étude complète sera calculé.

.. _Section6.1.2:

6.1.2. Créer le modèle
======================

Appuyer sur le bouton ‘Build’ |buildimage| pour commencer à créer un modèle CanFlood.

**Setup** (Configuration)

Sur l’onglet ‘Setup’, configurez votre ‘Séance de création’ en créant premièrement ou en sélectionnant un répertoire de travail facile à localiser au moyen du bouton ‘Browse’. CanFlood placera tous les fichiers de données assemblés au moyen de la trousse d'outils ‘Build’ dans ce répertoire. Assurez-vous que les autres commandes de création sont indiquées de la manière décrite ci-dessous.

Utilisez maintenant la partie inférieure du dialogue pour préciser les paramètres que CanFlood devrait utiliser pour assembler votre nouveau fichier de commande de la manière décrite ci-dessous.

.. image:: /_static/tutorials_6_1_2_img_1.jpg

Après avoir saisi correctement les paramètres, **cliquez sur ‘Start Control File’** pour créer votre fichier de commande dans le répertoire de travail. Un message devrait apparaître sur la barre d'outils du QGIS pour indiquer que le processus s’est déroulé avec succès.

Si vous affichez l’onglet des messages enregistrés ‘CanFlood’ (View > Panels > Log Messages), vous pouvez voir les messages enregistrés du processus que vous venez de terminer. Ceux-ci devraient ressembler à ce qui suit :

.. image:: /_static/tutorials_6_1_2_img_2.jpg

De retour sur l’onglet ‘Steup’ de CanFlood, près du chemin de fichiers du répertoire de travail, cliquez sur ‘Open’ pour ouvrir le répertoire de travail indiqué. Le fichier de commande ‘CanFlood_Tut1.txt’ devrait être créé dans votre répertoire de travail. Ouvrez le fichier de commande. Il s’agit d’un modèle comportant des espaces vides, des valeurs par défaut et des valeurs indiquées. Alors que vous parcourez le reste de la section ‘Build’ de ce didacticiel, les paramètres manquants seront ajoutés par les outils de CanFlood. Notez le commentaire ‘#’ qui vous permet de savoir la façon dont et le moment où ce fichier de commande a été créé. Le programme ignore les lignes du commentaire ‘#’ lors de la lecture à partir du fichier de commande et ces lignes sont écrites par des outils pour aider l'utilisateur à suivre les mesures prises par CanFlood sur le fichier de commande.

**Store Inventory** (Enregistrer l'inventaire)

Aller à l’onglet ‘Inventory’. Dans la section du compilateur d'inventaire, sélectionnez la couche d'inventaire (*finv_tut1a*) et assurez-vous que le type d’élévation est réglé à ‘datum’ pour refléter le fait que les valeurs ‘f0_elv’ sont mesurées à partir de la référence du projet (plutôt que du terrain). Sélectionnez maintenant la couche du vecteur d'inventaire et l’option ‘Index FieldName’ appropriée. **Cliquez ensuite sur ‘Store’**.

.. image:: /_static/tutorials_6_1_2_img_3.jpg

Vous devriez voir le fichier csv d'inventaire enregistré dans le répertoire de travail. Il s’agit d’une version simplifiée de la couche d'inventaire dont on a retiré les données spatiales. Ouvrez de nouveau le fichier de commande. Vous devriez remarquer que le paramètre d'inventaire des biens (‘finv’) présente maintenant le nom de fichier du document csv nouvellement créé.

**Hazard Sampler** (Échantillonneur de risque)

Allez à l’onglet ‘Hazard Sampler’. Cochez toutes les trames de risque dans la boîte d'affichage de la manière indiquée (si les couches de risque n’apparaissent pas dans le dialogue, appuyez sur ‘Refresh’), laissant ainsi les paramètres restants vides ou intacts :

.. image:: /_static/tutorials_6_1_2_img_4.jpg

**Cliquez sur ‘Sample Rasters’** afin de générer l'ensemble de données d'exposition (‘expos’). Vous devriez voir un nouveau fichier csv dans le répertoire de travail et son chemin de fichiers sera ajouté au fichier de commande sous ‘expos’. Il s’agit des WSL échantillonnés au niveau de chaque bien à partir de chaque trame d'événement à risque.

**Event Variables** (Variables d'événement)

Maintenant que les WSL ont été enregistrés, nous devons aviser CanFlood de la probabilité de réalisation de chacun de ces événements. Allez à l’onglet ‘Event Variables’. Précisez les valeurs exactes en ce qui concerne la probabilité de chaque événement (à partir des noms des événements) tel qu'indiqué:

.. image:: /_static/tutorials_6_1_2_img_5.jpg

**Appuyez sur ‘Store’**. L’ensemble de données des probabilités d'événement (‘evals’) devrait avoir été créé et son chemin de fichiers écrit dans le fichier de commande sous ‘evals’.

**Validation** (Validation)

Allez à l’onglet ‘Validation’, **cochez ‘Risk (L1)’** et **cliquez ensuite sur ‘Validate’**. Cela aura pour effet de cocher tous les intrants contenus dans le fichier de commande et de régler la marque de validation ‘risk1’ à ‘True’ dans le fichier de commande. Sans cet indicateur, le modèle CanFlood sera voué à l’échec.

Le fichier de commande devrait maintenant être créé complètement pour une analyse L1 et les intrants nécessaires devraient être réunis. Une fois terminé, le fichier de commande devrait ressembler à ce qui suit (mais sans vos répertoires):

.. image:: /_static/tutorials_6_1_2_img_6.jpg

6.1.3. Exécuter le modèle
====================

Cliquez sur le bouton ‘Model’ |runimage| pour lancer le dialogue de la trousse d'outils du modèle.

**Setup** (Configuration)

Sur l’onglet ‘Setup’, sélectionnez un répertoire de travail (il n’est pas nécessaire de sélectionner le même répertoire que dans l'étape précédente) où tous vos résultats seront enregistrés. Sélectionnez également le fichier de commande que vous avez créé dans la section précédente, au besoin.

Votre dialogue devrait ressembler à ce qui suit (CanFlood tentera d’identifier automatiquement la couche du vecteur d'inventaire; cependant, ce didacticiel n’utilise pas cette couche, de sorte qu’on peut ignorer la sélection dans ce cas-ci) :

.. image:: /_static/tutorials_6_1_3_img_1.jpg

**Execute** (Exécuter)

Aller à l’onglet ‘Risk (L1)’, Cochez les deux premières cases de la manière décrite ci-dessous et **appuyez sur ‘Run risk1’**:

.. image:: /_static/tutorials_6_1_3_img_2.jpg

6.1.4. Afficher les résultats
===================

Allez au répertoire de travail que vous avez sélectionné. Vous devriez vois 3 fichiers créés:

  • *risk1_run1_tut1a_passet.csv*: valeur attendue de l'inondation par bien;
  • *risk1_run1_tut1a_ttl.csv*: résultats totaux, valeur attendue de l'inondation totale par événement (et pour tous les événements);
  • *tut1a.run1 Impact-ARI plot on 6 events.svg*: schéma des résultats totaux (voir ci-dessous).

.. image:: /_static/tutorials_6_1_4_img_1.jpg

Il n’existe pas de résultats non spatiaux qui sont directement générés par les routines de modèle de CanFlood. Pour faciliter une analyse plus détaillée et la visualisation, CanFlood présente une troisième et dernière trousse d'outils ‘Results’.

**Join Geometry** (Géométrie conjointe)

Ouvrez la trousse d'outils de résultats en **cliquant sur le bouton ‘Results’** |visualimage2| . Les modèles CanFlood sont conçus pour fonctionner indépendamment de l’IPA du QGIS. Par conséquent, si vous souhaitez afficher les résultats dans un format spatial, d’autres mesures sont nécessaires pour fixer de nouveau les résultats du modèle tabulaire à la géométrie vectorielle de l'inventaire des biens (‘finv’). Pour ce faire, allez à l’onglet ‘Join Geo’ et sélectionnez la couche d'inventaire des biens (‘finv’). Sélectionnez ensuite ‘r_passet’ sous ‘results parameter to load’ afin d’inscrire dans le champ ci-dessous un chemin de fichiers menant à votre fichier des résultats par bien (si le chemin de fichiers n’apparaît pas automatiquement, essayez de modifier les menus déroulants ‘finv’ et ‘parameter’. Ou encore, inscrivez le chemin de fichiers manuellement). Enfin, sélectionnez ‘Results Layer Style’ et ‘Field re-label option’ tel qu'indiqué:

.. image:: /_static/tutorials_6_1_4_img_2.jpg

**Cliquez sur ‘Join’**. Une nouvelle couche ‘djoin’ temporaire devrait avoir été chargée sur le canevas de carte et le style sélectionné appliqué. Déplacez cette couche au haut du panneau de vos couches et fermez la couche ‘finv’ pour voir la nouvelle couche ‘djoin’. La couche ‘djoin’ devrait être une couche de points, alors que la taille de chaque point varie en fonction de la valeur attendue d'inondation (c'est-à-dire le nombre moyen d’inondations par année) semblable à ce qui suit:

.. image:: /_static/tutorials_6_1_4_img_3.jpg

Ouvrez le tableau des attributs pour la couche ‘djoin’ (F6). Vous devriez voir quelque chose de semblable au tableau suivant :

.. image:: /_static/tutorials_6_1_4_img_4.jpg

Notez les six champs d’impact (entourés de rouge ci-dessous) dont les noms ont été convertis à ‘’ari_probability’ et dont les valeurs de champ présentent les résultats de l'exposition binaire (0=non exposé; 1=exposé). Vous devrez sauvegarder cette couche si vous souhaitez qu’elle soit disponible lors d’une autre séance du QGIS (Layers Pane > Clic droit sur la couche > Save As…). Félicitations pour votre première exécution CanFlood!

.. |visualimage2| image:: /_static/visual_image.jpg
   :align: middle
   :width: 26

.. _Section6.2:

**********************************************
6.2. Didacticiel 2a: Risque (L2) avec des événements simples
**********************************************

Le didacticiel 2 fait la démonstration du modèle de ‘Risque (L2) de CanFlood (:ref:`Section5.2.3 <Section5.2.3>`). Cela reprend une évaluation plus détaillée du risque, alors qu’on connaît la vulnérabilité de chaque bien qui est décrite en fonction de la profondeur d'inondation (plutôt que de la simple présence binaire d’une inondation comme dans le didacticiel 1). Ce didacticiel fait également la démonstration d’un inventaire présentant des hauteurs ‘relatives’ et la caractéristique de fonction de vulnérabilité composite de CanFlood lorsque plusieurs fonctions sont appliquées au même bien.

6.2.1. Charger les données dans le projet
===========================

Télécharger les données du didacticiel 2 à partir du dossier ‘tutorials\2\data’ :

  • *haz_rast*: les trames d'événement à risque avec prédictions de valeur WSL pour la zone d'étude présentent quatre probabilités.

      o *haz_0050.tif*

      o *haz_0100.tif*

      o *haz_0200.tif*

      o *haz_1000.tif*

  • *finv_tut2.gpkg*: couche spatiale de l'inventaire des biens d'inondation (’finv’)
  • *dtm_tut2.tif*: trame numérique du modèle de terrain avec prédictions des élévations du terrain
  • |ss| *haz_frast*: trames d’accompagnement des événements de bris |se| (non utilisées dans le didacticiel 2a)
  • |ss| *haz_fpoly*: polygones d’accompagnement des événements de bris |se| (non utilisés dans le didacticiel 2a)

Chargez le tout dans le projet QGIS. Le résultat devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_2_1_img_1.jpg

6.2.2. Créer le modèle
======================

Ouvrez la trousse d'outils ‘Build’ |buildimage|.

**Scenario Setup** (Préparation du scénario)

Sur l’onglet ‘Setup’, configurez la séance de la manière décrite au moyen de vos propres chemins. **Cliquez ensuite sur ‘Start Control File’**:

.. image:: /_static/tutorials_6_2_2_img_1.jpg

**Select Vulnerability Function Set** (Sélectionner l’ensemble de fonctions de vulnérabilité)

Allez à l’onglet ‘Inventory’ et **cliquez sur ‘Select From Library** pour lancer l’IUG de sélection de bibliothèque indiquée ci-dessous. Sélectionnez la bibliothèque ‘IBI_2015’ dans la fenêtre supérieure gauche et ensuite ‘IBI2015_DamageCurves.xls’ dans la fenêtre inférieure gauche. **Cliquez ensuite sur ‘Copy Set’** pour régler ces fonctions de vulnérabilité dans votre répertoire de travail. L’inventaire qu’on retrouve dans ce didacticiel a été créé spécifiquement pour ces fonctions ‘IBI2015’. De façon générale, les modélisateurs de risque d'inondation doivent développer ou fournir leurs propres fonctions de vulnérabilité.

.. image:: /_static/tutorials_6_2_2_img_2.jpg

Fermez l’IUG ‘vFunc Selection’. Vous devriez maintenant voir le nouveau chemin de fichiers .xls inscrit sous ‘Vulnerability Functions’. Enfin, **cliquez sur ‘Update Control File’** pour enregistrer une référence à ses ensembles de fonctions de vulnérabilité dans le fichier de commande.

**Inventory** (Inventaire)

Sur le même onglet ‘Inventory’ sélectionnez la couche de vecteurs d'inventaire, le nom de champ d’index approprié et **réglez le type d’élévation à ‘ground’** tel qu'indiqué. **Cliquez ensuite sur ‘Store’**.

.. image:: /_static/tutorials_6_2_2_img_3.jpg

Vous devriez maintenant voir le fichier csv d'inventaire enregistré dans le répertoire de travail.

**Hazard Sampler** (Échantillonneur de risque)

Allez à l’onglet ‘Hazard Sampler’. Assurez-vous que les quatre trames de risque apparaissent dans la fenêtre et que tous les autres champs présentent la valeur par défaut. **Cliquez ensuite sur ‘Sample Rasters’**. Vous devriez maintenant voir le fichier de données ‘expos’ créé dans le répertoire de travail.

**Event Variables** (Variables d'événement)

Allez à l’onglet ‘Event Variables’. Vous devriez maintenant voir les 4 événements à risque de la tâche précédente apparaître dans le tableau. Inscrivez les valeurs de probabilité tel qu'indiqué (ignorez le paramètre ‘Failure Event Relation’ pour l’instant). **Cliquez ensuite sur ‘Store’** pour générer l'ensemble de données des variables d'événement (‘evals’).

.. image:: /_static/tutorials_6_2_2_img_4.jpg

**Échantillonneur DTM**

Allez à l’onglet ‘DTM Sampler’. Sélectionner la trame ‘dtm_tut2’ et **cliquez ensuite sur ‘Sample DTM’** pour générer l'ensemble de données d’élévation du terrain (‘gels’) dans votre répertoire de travail et créer une référence à cet ensemble dans le fichier de commande.

**Validation** (Validation)

Allez à l’onglet ‘Validation’. **Cochez les cases des deux modèles L2** et **cliquez ensuite sur ‘Validate’**. Vous devriez recevoir un message enregistré ‘passed 1 (of 2) validations. see log’. Pour enquêter sur la tentative de validation qui a échoué, ouvrez le panneau des messages enregistrés, qui devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_2_2_img_5.jpg

Cela démontre que le fichier de données ‘dmgs’ est absent du modèle de risque (L2), de sorte qu’il ne fonctionnera pas. Il s’agit là du comportement attendu, puisque CanFlood sépare les calculs d'exposition (Impacts L2) du calcul de risque. Nous calculerons ce fichier de données ‘dmgs’ et le validerons pour le risque (L2) dans la prochaine section. Vous êtes maintenant prêt à exécuter le modèle d’impacts (L2)!

6.2.3. Exécuter le modèle
====================

Ouvrez le dialogue ‘Model’ |runimage|. Configurez l’onglet 'Setup’ de la manière décrite ci-dessous en choisissant vos propres chemins et votre fichier de commande. Assurez-vous également que le répertoire des extrants est un sous-répertoire de votre répertoire de travail précédent (Certains outils ‘Results’ fonctionnent mieux lorsque les fichiers de données des extrants du modèle font partie de la même arborescence des fichiers que le fichier de commande) :

.. image:: /_static/tutorials_6_2_3_img_1.jpg

**Impact (L2)**

Allez à l’onglet ‘Impacts (L2),. Assurez-vous que la case ‘Run Rixk (L2)’ n’est **pas** cochée (nous exécuterons ce modèle de risque manuellement dans la prochaine étape), mais que l’option ‘Output expanded component impacts’ **est** cochée. **Cliquez sur ‘Run dmg2’**.

Cela devrait entraîner la création d’un fichier de données des impacts (‘dmgs’) dans votre répertoire de travail, alors que l’entrée correspondante apparaîtra dans le fichier de commande. Ouvrez ce fichier csv. Il devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_2_3_img_2.jpg

Il s’agit là des impacts bruts par événement et par bien qu’on calcule avec chaque fonction de vulnérabilité, de l'élévation WSL et de l'élévation DTM échantillonnées. Le deuxième extrant représente les ‘impacts expansés des composants’, un volumineux fichier de référence d’extrants en option utilisé par CanFlood qui renferme la tabulation de chaque fonction imbriquée, ainsi que les valeurs de mise à l'échelle et maximales qui sont appliquées. Voir la :ref:`Section5.2.2 <Section5.2.2>` pour de plus amples renseignements. Vous êtes maintenant prêt à calculer le risque d'inondation!

**Risque (L2)**

Allez à l’onglet ‘Risk (L2)’. Cochez toutes les cases montrées ci-dessous et **cliquez sur ‘Run risk2’.**

.. image:: /_static/tutorials_6_2_3_img_3.jpg

Une série de fichiers de résultats devraient avoir été générés (un sujet qu’on aborde ci-dessous). Pour une description complète du risque (L2), voir :ref:`Section5.2.3 <Section5.2.3>`.

6.2.4. Afficher les résultats
===================

Après avoir exécuté le risque (L2), naviguez jusqu’à votre répertoire de travail. Celui-ci devrait contenir les fichiers suivants :

  • *eventypes_run1_tut2a.csv*: paramètres dérivés pour chaque trame;
  • *risk2_run1_tut2a_r2_passet.csv*: valeur attendue pour chaque risque (L2) étendu;
  • *risk2_run1_tut2a_ttl.csv*: valeur totale attendue de tous les résultats des événements et des biens du risque (L2);
  • *dmgs_tut2a_run1.csv*: résultats en ce qui concerne les impacts (L2) par bien;
  • *dmgs_expnd_tut2a_run1.csv*: résultats élargis des impacts (L2) des composants;
  • *run1 Impacts-ARI plot for 6 events.svg*: voir ci-dessous.

.. image:: /_static/tutorials_6_2_4_img_1.jpg

*Figure 6-1: Courbe du risque sommaire provenant des résultats du risque (L2) total.*

**Risk Plot** (Tracé du risque)

Alors que les modules de risque comportent certaines courbes de risque de base (voir ci-dessus), CanFlood permet une personnalisation additionnelle des courbes au moyen de l'outil ‘Risk Plot’ dans la trousse d'outils ‘Results’. **Ouvrez la **trousse d'outils** ‘Results’  |visualimage1| **. Configurez la séance en sélectionnant un répertoire de travail, soit le fichier de commande, et en réglant ‘Plot Handling’ à ‘Save to file’ tel qu'indiqué :

.. image:: /_static/tutorials_6_2_4_img_2.jpg

Pour générer des tracés personnalisés, allez à l’onglet ‘Risk Plot’ et sélectionnez les deux types de tracés tel qu'indiqué ci-dessous :

.. image:: /_static/tutorials_6_2_4_img_3.jpg

Pour personnaliser le tracé, ouvrez le fichier de commande et sous ‘[plotting]’, modifiez les paramètres suivants :

  • couleur = rouge
  • impactfmt_str = ,.0f

Ces paramètres contrôlent la couleur du tracé et le format appliqué aux valeurs d’impact. Sauvegardez les changements. Retournez ensuite à la fenêtre CanFlood et **appuyez sur ‘Plot Total’**. Vous devriez voir les deux tracés ci-dessous qui apparaîtront dans votre répertoire de travail.

.. image:: /_static/tutorials_6_2_4_img_4.jpg

.. image:: /_static/tutorials_6_2_4_img_5.jpg

Ces tracés représentent les deux formats standard de courbe de risque pour les mêmes données sur les résultats totaux. Ou encore, modifiez le paramètre ‘Plot Handling’ en le réglant à ‘Launch Separate Window’ sur l’onglet ‘Setup’ pour lancer après le tracé une fenêtre de dialogue qui renferme certains outils intégrés afin de personnaliser davantage le tracé.

.. |visualimage1| image:: /_static/visual_image.jpg
   :align: middle
   :width: 28

*********************************************
6,3. Didacticiel 2b: Risque (L2) avec bris de digue
*********************************************

Les utilisateurs devraient terminer premièrement les didacticiels 1 et 2a. Le didacticiel 2b fait appel aux mêmes quotas que le 2a, mais on élabore davantage l'analyse pour démontrer l'analyse de risque de bris d’une levée simple en intégrant un seul événement de bris d’accompagnement au modèle. Cet événement de bris d’accompagnement comporte deux couches :

  • *haz_1000_fail_A_tut2*: ‘Failure raster’ indiquant le WSL qui serait réalisé si certains des segments de la levée devaient se briser pendant l'événement; et
  • *haz_1000_fail_A_tut2*: Couche du polygone de probabilité d'exposition conditionnelle présentant les caractéristiques indiquant l’ampleur et la probabilité de bris de chaque segment de levée lors d’une inondation (« polygones de bris »). Remarquez que cette couche renferme deux caractéristiques qui se recoupent à certains endroits, correspondant ainsi aux inondations possibles des deux sites de bris du système de levée. Cette couche sera utilisée pour informer CanFlood du moment et de la façon d'échantillonner la trame du bris.

Cette simplification en utilisant ces deux couches facilite la détermination de plusieurs probabilités de bris, mais dans les cas où un bris (ou une combinaison de bris) présenterait le même WSL (:ref:`Section5.1.5 <Section5.1.5>`’s ‘complex conditionals’). Assurez-vous que ces couches sont chargées dans le même projet QGIS que celui qu’on a utilisé pour le didacticiel 2a.

Pour mieux comprendre la couche ‘failure polygons’, appliquons le style ‘red fill transparent’ de CanFlood. Commençons en chargeant ce modèle de style dans votre profil avec l'outil ‘Add Styles’ (Plugins > CanFlood > Add Styles). Appliquez-le ensuite au moyen du panneau des styles de couches (F7). Enfin, ajoutez une seule étiquette pour ‘p_fail’ et déplacez la couche tout juste sous la couche de points de l'inventaire de biens (‘finv’) sur le panneau des couches. Votre canevas devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_3_img_1.jpg

6.3.1. Créer le modèle
======================

Suivez les étapes de création du modèle dans le didacticiel 2a, mais en incluant la trame de bris (‘haz_1000_fail_A_tut2’, probability=1000ARI) dans les étapes de l'échantillonneur de risque et des variables d'événement. À l'étape des variables d'événement, assurez-vous que le paramètre ‘Failure Event Relation Treatment’ est réglé à ‘Mutually Exclusive’.

**Conditional Probabilities** (Probabilités conditionnelles)

Allez à l’onglet ‘Conditional P’ pour résoudre la question des polygones de bris qui se chevauchent dans l'ensemble de données des probabilités d'exposition résolues (‘exlikes’) pour aviser CanFlood de la probabilité qu’on devrait attribuer à chaque bien au moment de réaliser la trame de bris d’accompagnement. Commencez en jumelant les polygones de bris à la trame de bris. Sélectionnez ensuite ‘Probability FieldName’, ‘Event Relation Treatment’ et ‘Summary Plots’ tel qu'indiqué et **cliquez ensuite sur ‘Sample’**:

.. image:: /_static/tutorials_6_3_1_img_1.jpg

Un fichier de données de probabilités d'exposition résolues (‘exlikes’) devrait avoir été créé dans votre répertoire de travail et ce fichier devrait présenter des entrées comparables à ceci :

.. image:: /_static/tutorials_6_3_1_img_2.jpg

Deux tracés sommaires non spatiaux de ces données devraient également avoir été créés dans votre répertoire de travail, le plus utile pour ce modèle particulier étant l’histogramme :

.. image:: /_static/tutorials_6_3_1_img_3.jpg

Ces valeurs représentent les probabilités conditionnelles de chaque bien atteignant le WSL de l'événement de bris d’accompagnement de 1000 ans (essayez d’exécuter l'outil de nouveau, mais en sélectionnant cette fois-ci l'option ‘Max’. Si vous regardez de près les tracés des boîtes, vous devriez constater une légère différence dans les probabilités résolues. Cela porte à croire que ce modèle n’est pas très sensible à l'hypothèse relationnelle de ces polygones de bris qui se chevauchent). Voir :ref:`Section5.2.3 <Section5.2.3>` pour une description complète de cet outil. Complétez la création du modèle en exécutant les outils ‘DTM Sampler’ et ‘Validation’.

6.3.2. Exécuter le modèle
====================

Ouvrez le dialogue ‘Model’ |runimage| et configurez votre séance de manière semblable au didacticiel 2a, mais assurez-vous que ‘Generate attribution matrix’ est coché sous ‘Run Controls’ (nous l’utiliserons pour créer des tracés montrant les différents composants qui contribuent aux risques totaux).

**Impacts et risque**

Allez à l’onglet ‘Impacts (L2)’. Cochez la case ‘Run Risk (L2) upon completion’ pour exécuter les modèles d'exposition et de risque dans l’ordre à partir de votre fichier de commande. Allez à l’onglet ‘Risk (L2’ et assurez-vous de cocher ‘Calculate expected values per asset’. Revenez ensuite à l’onglet ‘Impacts (L2)’ et **cliquez sur ‘Run dmg2’**. Vous devriez voir les mêmes types d’extrants que dans le didacticiel 2a, mais avec deux ensembles de données additionnels de type ‘attribution matrix’.

.. _Section6.3.3:

6.3.3. Afficher les résultats
===================

Pour mieux comprendre l’influence de l'intégration d’un bris de levée, on fera la démonstration, dans cette section, de la façon de gérer un tracé montrant le risque total et de la partie de ce risque total qui en résulte lorsqu’on présume qu’il n’y a pas de bris. Ouvrez la trousse d'outils ‘Results’ et configurez votre séance en sélectionnant un répertoire de travail et le même fichier de commande que celui que vous avez utilisé ci-dessus. Allez maintenant à l’onglet ‘Risk Plot’, assurez-vous que les deux commandes de tracé sont cochées et **cliquez ensuite sur ‘Plot Fail Split**. Cela devrait générer deux formulations de tracé de risques, incluant la figure suivante :

.. image:: /_static/tutorials_6_3_3_img_1.jpg

Dans ce tracé, la ligne rouge représente la contribution au risque sans les événements de bris d’accompagnement, qui devraient être presque identiques aux résultats du didacticiel 2a, alors qu’une deuxième ligne montre les résultats totaux (ou encore, on peut utiliser l'outil ‘Compare’ pour générer un tracé de comparaison entre les deux didacticiels). La zone entre ces deux lignes montre la contribution au risque lorsqu’on intègre le bris d’une levée au modèle.

************************************************
6,4. Didacticiel 2c: Risque (L2) avec bris complexe.
************************************************

On recommande aux utilisateurs de terminer premièrement le didacticiel 2b. Le didacticiel 2c fait appel aux mêmes données d’intrant que le 2b, mais on élabore l'analyse pour démontrer que l'intégration d’un bris de levée plus complexe avec deux événements de bris d’accompagnement dans le modèle.

Dans le même projet de QGIS que celui qu’on a utilisé pour le didacticiel 2b, assurez-vous d’ajouter également ce qui suit au projet :

  • *haz_1000_fail_B_tut2.gpkg*: polygone de bris ‘B’;
  • *haz_1000_fail_B_tut2.tif*: trame de bris ‘B’.

Ces couches représentent un événement de bris d’accompagnement ‘B’ additionnel pour l'événement de 1000 ans, lors que le WSL de bris et les probabilités sont différents, mais complémentaires à ceux de l'événement de bris d’accompagnement ‘A’ du didacticiel 2b. Celles-ci pourraient représenter des extrants des deux scénarios de bris modélisés.

6.4.1. Créer le modèle
======================

Suivre les étapes du didacticiel 2b ‘Créer le modèle’, mais en incluant l'événement de bris d’accompagnement ‘B’ additionnel dans les étapes échantillonneur de risque, variables d'événement et P conditionnel : Pour les deux dernières, assurez-vous que les traitements des relations entre les événements sont réglés à ‘Mutually Exclusive’. En regardant la case du tracé ‘Conditional P’, on voit la diffusion dans les probabilités de bris prescrites par les deux événements de bris d’accompagnement :

.. image:: /_static/tutorials_6_4_1_img_1.jpg

Complétez la création du modèle en exécutant les outils ‘DTM Sampler’ et ‘Validation’.

6.4.2. Exécuter le modèle
====================

Ouvrez le dialogue ‘Model’ |runimage| et suivez les étapes du didacticiel 2b pour configurer l'exécution de ce modèle.

**Impacts et risque**

Exécutez les modèles ‘Impacts (L2)’ et ‘Risk (L2)’ de manière semblable au didacticiel 2b, mais assurez-vous de désélectionner l’option ‘Generate attribution matrix’.

Pour explorer l’influence du paramètre ‘event_rels’, ouvrez le fichier de commande, modifiez le paramètre ‘event_rels’ à ‘max’, changez le paramètre ‘name’ en attribuant un nom unique (par exemple ‘tut2c_max’) et sauvegardez ensuite le fichier sous un nom différent. Sur l’onglet ‘Setup’, pointez vers ce fichier de commande modifié, un nouveau répertoire des extrants, et exécutez de nouveau les deux modèles de la manière décrite ci-dessus (les utilisateurs avancés devraient éviter d’exécuter de nouveau le modèle ‘Impacts (L2)’ en manipulant le fichier de commande de manière à pointer les résultats ‘dmgs’ de l'exécution précédente, puisque ceux-ci resteront inchangés entre les deux formulations). 

6.4.3. Afficher les résultats
===================

Après avoir exécuté le modèle ‘Risk (L2) pour les fichiers de commande ‘event_rels=mutEx’ et ‘event_rels=max’ , deux séries de fichiers d’extrants comparables devraient avoir été produites dans les deux répertoires d’extrants séparés qu’on a précisés lors de la configuration du modèle. Pour visualiser la différence entre ceux deux configurations du modèle, **ouvrez la trousse d'outils ‘Results’** et sélectionnez un répertoire de travail, ainsi que le fichier de commande ‘event_rels=mutEx’ original comme étant le fichier de commande principal sur l’onglet 'Setup’. (Le fichier de commande indiqué sur l’onglet ‘Setup’ sera utilisé pour les styles de tracés communs (par exemple). Avant de générer des fichiers de comparaison, configurez le style de tracé en ouvrant le même fichier de commande principal et en modifiant les paramètres ‘[plotting]’ suivants :

  • ‘color = red’
  • ‘linestyle = solid’
  • ‘impactfmt_str = ,.0f’

Pour générer un tracé de comparaison de ces deux scénarios, allez à l’onglet ‘Compare/Combine’, sélectionnez le fichier de commande des deux configurations de modèles créées à l'étape précédente et assurez-vous que l'option ‘Control Files’ est cochée sous ‘Comparison Controls’, de la manière décrite ci-dessous.

.. image:: /_static/tutorials_6_4_3_img_1.jpg

Cliquez sur ‘Compare’ pour effectuer la comparaison. Vous devriez voir deux fichiers qui ont été créés dans votre répertoire de travail :

  • Tracé de comparaison montrant les deux courbes de risque sur le même axe; et
  • Chiffrier de comparaison des fichiers de commande.

Le chiffrier de comparaison des fichiers de commande est présenté ci-dessous. Il représente un moyen facile d’identifier rapidement les distinctions entre les scénarios du modèle.

.. image:: /_static/tutorials_6_4_3_img_2.jpg

Sur le tracé de comparaison (présenté ci-dessous), remarquez que la différence dans les courbes de risque et les valeurs annualisées est négligeable, ce qui révèle que les relations entre les événements ne sont pas très importantes pour ce modèle.

.. image:: /_static/tutorials_6_4_3_img_3.jpg

En exécutant de nouveau l'outil de comparaison des quatre fichiers de commande du didacticiel 2 qu’on a créés jusqu’à présent, on obtient ce qui suit :

.. image:: /_static/tutorials_6_4_3_img_4.jpg

*******************************************
6,5. Didacticiel 2d: Risque (L2) avec mesure d'atténuation
*******************************************

On recommande aux utilisateurs de terminer premièrement le didacticiel 2a avant d’aller plus loin. Le didacticiel 2d fait appel aux mêmes données d’entrée que le 2a, mais on y présente une analyse plus élaborée pour démontrer les mesures d'atténuation de niveau d'intégration des objets (ou des biens) dans le modèle. Cela peut être utile pour améliorer la précision d’un modèle lorsque deux biens présentent une fonction comparable, qu’ils font appel à la même fonction de vulnérabilité, alors qu’un d’eux est doté d’un mécanisme pour réduire son exposition (comme une soupape antiretour). De même, cette fonctionnalité peut être utilisée pour étudier les avantages de l’introduction de PLPM avec une analyse comparative.

6.5.1. Créer le modèle
======================

Suivez les étapes du didacticiel 2a ‘Bâtir le modèle’ à l’exception de l'étape ‘Inventory’ que nous modifierons pour appliquer les quatre nouveaux champs à la couche du vecteur d'inventaire (‘finv’) en configurant l’onglet ‘Inventory’ de la manière décrite ci-dessous avant de **cliquer sur ‘Construct finv’**:

.. image:: /_static/tutorials_6_5_1_img_1.jpg

Cela devrait entraîner la création d’une nouvelle couche avec un préfixe ‘finv’ dans le canevas de votre carte. En explorant le tableau des attributs de cette couche (F6), on devrait voir apparaître quatre nouveaux champs qu’on a créés et dans lesquels on a inscrit les valeurs indiquées. Ces champs sont utilisés par le module ‘Impacts (L2)’ pour modifier l'exposition passée à chaque fonction de vulnérabilité d’objet, sans compter qu’ils sont décrits dans la :ref:`Section5.2.2 <Section5.2.2>`. Terminer la création de l'inventaire en s’assurant que l'option ‘Apply Mitigations’ est cochée, que la couche du vecteur d'inventaire nouvellement créée est sélectionnée et que le reste de l’onglet est configuré de la manière décrite ci-dessous (comme dans le didacticiel 2a). **Cliquer sur ‘Store’.**

.. image:: /_static/tutorials_6_5_1_img_2.jpg

Effectuer les étapes ‘Hazard Sampler’, ‘Event Variables’, ‘DTM Sampler’ et ‘Validation’ de la manière décrite dans le didacticiel 2a.


6.5.2. Exécuter le modèle
====================

Ouvrez le dialogue ‘Model’ |runimage| et configurez votre séance de manière comparable au didacticiel 2a.

**Impacts and Risk** (Impacts et risque)

Allez à l’onglet ‘Impacts (L2)’ et assurez-vous que TOUTES les options ‘Run Controls’ sont cochées. **Cliquez ensuite sur ‘Run dmg2’**. Vous devriez voir les mêmes types d’extrants que dans le didacticiel 2a, mais avec d’autres qui nous aideront à comprendre l’influence des paramètres d'atténuation, incluant le tracé de la boîte présenté ci-dessous :

.. image:: /_static/tutorials_6_5_2_img_1.jpg

Cela montre les résumés de données pour les quatre trames d'événement, les valeurs d’impact totales (en rouge) et certaines informations sur les modèles clés.

Pour comprendre l'effet des paramètres d'atténuation, ouvrez le fichier de commande, modifiez le paramètre ‘apply_miti’ à ‘False’, modifiez le paramètre ‘name’ à ‘tut2d_noMiti’, le paramètre ‘color’ à ‘red’, et sauvegardez-le sous un nom différent. Sur l’onglet 'Setup’ pointez ce nouveau fichier de commande et remplacez ‘Run Tag’ par ‘noMiti’. Revenez ensuite à l’onglet ‘Impacts (L2)’ et **cliquez de nouveau sur ‘Run dmg2’.** Vous devriez voir une autre boîte apparaître dans votre répertoire de travail:

.. image:: /_static/tutorials_6_5_2_img_2.jpg

Remarquez que les petits événements (50 ans et 100 ans) ont changé considérablement, mais moins dans le cas des événements plus importants. C’est logique, sachant que nous avons informé CanFlood que les mesures d'atténuation allaient être dépassées à des profondeurs supérieures à 0,2 m (au moyen du paramètre du seuil de profondeur supérieur). Nous pouvons étudier plus longuement ce modèle de comportement en ouvrant (l’influence des fonctions d'atténuation sur les profondeurs ne se reflète pas dans cet extrant) un des extrants ‘depths\_’, qui devrait ressembler à celui ci-dessous (les valeurs sous le seuil supérieur sont surlignées en rouge pour les rendre plus évidentes):

.. image:: /_static/tutorials_6_5_2_img_3.jpg

De même, l’onglet ‘dmg2_smry’ spreadsheet ‘_smry’ pour la fonction d'atténuation présente le changement dans les valeurs d’impact totales (par événement) calculées à chaque étape du module ‘Impacts (L2)’ (des barres et des flèches ont été ajoutées pour des raisons de clarté):

.. image:: /_static/tutorials_6_5_2_img_4.jpg

Cela montre les impacts totaux réalisés par les courbes brutes, suivis de l’algorithme ‘scalilnt (‘fX_scale’), de l’algorithme ‘capping’ (‘fX+cap’), suivis de l’algorithme qui a mis en application le seuil inférieur (‘mi_Lthresh’), l’échelle d'atténuation (‘mi_iScale’), l’addition des valeurs d'atténuation (‘mi_iVal’), ainsi que le résultat final (identique à la rangée précédente). Cette progression démontre que l'algorithme ‘capping’ a eu une profonde influence sur les résultats et que l’addition des valeurs d'atténuation (‘mi_iVal’) a eu une influence négligeable.

6.5.3. Afficher les résultats
=======================

L’outil de résultats ‘Compare’ peut être utilisé pour montrer l’influence sur la courbe de risque et sur le risque total :

.. image:: /_static/tutorials_6_5_3_img_1.jpg

***************************************
6,6. Didacticiel 2e: Analyse des coûts-avantages
***************************************

Ce didacticiel fait la démonstration des outils d'analyse des coûts-avantages (ACA) de CanFlood pour soutenir la version de  base de cette analyse pour les interventions lors du risque d'inondation, comme les mesures d'atténuation qu’on a examinées dans le didacticiel précédent. Avant de poursuivre avec ce didacticiel, les utilisateurs devraient avoir terminé et disposer des résultats du didacticiel 2a (ou encore, il est possible d’utiliser le ‘tut2d_noMiti’ du didacticiel 2d) et 2d :

  • *CanFlood_tut2a.txt*: fichier de commande du didacticiel 2a avec le fichier des résultats totaux valides (‘r_ttl’) et le chemin des fichiers;
  • *CanFlood_tut2d.txt*: fichier de commande du didacticiel 2d avec le chemin des fichiers des résultats totaux valides (‘r_ttl’).

Commencez en ouvrant la boîte d'outils ‘Results’ et allez ensuite sur l’onglet ‘Setup’ pour la configurer au moyen du fichier de commande du didacticiel 2d. Nous allons maintenant créer un tracé d’essai pour nous assurer que nos fichiers de commande sont valides. Assurez-vous que le paramètre ‘impactfmt_str’ est réglé à  ‘,.0f’ (sans apostrophe) dans le fichier de commande du didacticiel 2d. Allez maintenant à l’onglet ‘Compare/Combine’, inscrivez les deux fichiers de commande, cochez une des options ‘Plot Controls’ et cliquez ensuite sur ‘Compare’. Un tracé identique à celui créé à la fin du didacticiel 2d devrait avoir été créé. Notez que les dommages annualisés estimés (EAD) du didacticiel 2d sont de ~57,000. Il s’agit du risque annuel résiduel d'inondation pour ces biens, après l'intervention des PLPM.

**Terminer le cahier d'exercices de l'analyse des coûts-avantages**

Allez à l’onglet ‘BCA’. Assurez-vous que le chemin du fichier de commande du didacticiel 2d apparaît au haut de la fenêtre. Cliquez ensuite sur ‘Copy BCA Template’. Vous devriez voir un nouveau paramètre ‘cba_xls’ réglé dans le fichier de commande et votre fenêtre ‘BCA’ devrait ressembler à celle qu’on peut voir ci-dessous.

.. image:: /_static/tutorials_6_6_img_1.jpg

Cliquez maintenant sur ‘Open’ pour éditer le cahier d'exercices de l'ACA. Vous devriez voir sur l’onglet ‘smry’ l'information du didacticiel 2d, dont principalement les EAD de 57 000 $ calculés à partir de cette option. Remplissez les autres cellules d’intrants sur l’onglet ‘smry’ en précisant les EAD du didacticiel 2a et en utilisant un taux d’escompte de 4 % tel qu'indiqué ci-dessous :

.. image:: /_static/tutorials_6_6_img_2.jpg

Allez maintenant à l’onglet ‘data’ dans le cahier d'exercices pour inscrire les données des avantages-coûts que présentent les mesures d'atténuation présentées dans le didacticiel 2d. Pour ce didacticiel, présumons que nous avons établi les paramètres suivants pour cette intervention :

  • L’installation des PLPM prendra 2 ans au coût de 1 million de dollars par année et assurera une protection pendant 100 ans;
  • L’entretien coûtera 1 000 $ par année dès la fin des travaux de construction et il en sera ainsi tout au long du cycle de vie de 100 ans de l'intervention;
  • Les avantages relatifs et les coûts d'entretien resteront inchangés dans le temps.

Les deux rangées d’EAD sur l’onglet ‘data’ devraient se remplir automatiquement à partir des valeurs inscrites sur l’onglet ‘smry’, mais pour respecter les hypothèses évoquées ci-dessus, nous devons ajuster certaines de ces valeurs de la manière indiquée pour les six premières années de l’onglet ‘data’ :

.. image:: /_static/tutorials_6_6_img_3.jpg

Remarquez que la première année des EAD ‘baseline’ et ‘option’ sont vides, ce qui signifie qu’on n’a encore tiré aucun avantage. Cependant, la deuxième année nous montre que la moitié des avantages seront réalisés. Les coûts d'entretien de 1 000 $ par année devraient s’étendre sur les 100 années (c'est-à-dire qu’on doit les copier/coller dans toutes les cellules vers la droite – ce qu’on ne voit pas).

Une fois l’onglet ‘data’ terminé, un rapport des A/C de 1,18 devrait apparaître sur l’onglet ‘smry’ (si vous obtenez un rapport A/C de 1,9, assurez-vous que les coûts d'entretien de 1 000 $ sont inscrits pour chacune des années du cycle de vie). Sauvegardez et fermez ce chiffrier.

**Plot Financials** (Tracé des données financières)

Pour résumer et analyser davantage les données saisies sur la feuille de travail de l'ACA (assurez-vous de la sauvegarder!), revenez à la fenêtre ‘BCA’ de CanFlood, sélectionnez ‘Future Values’ et cliquez sur ‘Plot Financials’. Le tracé qu’on voit ci-dessous devrait apparaître :

.. image:: /_static/tutorials_6_6_img_4.jpg

Cela démontre les valeurs relatives des avantages et des coûts cumulatifs dans le temps (sans actualisation). Remarquez que les coûts d'installation élevés dépassent les avantages au départ. Après environ 25 ans, cependant, les avantages de cette option sont supérieurs aux coûts (année de remboursement). Remarquez également qu’avec les valeurs futures, le tracé montre des avantages cumulatifs d’environ 10 millions de dollars après 100 ans. D’ici là, nous habiterons peut-être tous dans des vaisseaux spatiaux... De sorte qu’il est mieux de ne pas accorder trop d'importance à ces avantages exagérés des coûts d'atténuation des inondations.

Modifiez le bouton radio à ‘Present Values’ et cliquez de nouveau sur ‘Plot Financials’. Vous devriez voir apparaître un tracé ressemblant à ce qui suit :

.. image:: /_static/tutorials_6_6_img_5.jpg

Remarquez que les paramètres ‘B/C ratio’ et ‘pay-back year’ sont restés inchangés, mais le tracé montre maintenant que les coûts et les avantages diminuent avec le temps, ce qui reflète l'application du taux d’actualisation.

Pour mieux comprendre le taux d'actualisation, retournez à la feuille de travail, remplacez le taux d'actualisation par 8 %, sauvegardez la feuille de travail et, dans la fenêtre CanFlood, cliquez de nouveau sur ‘Plot Financials’ :

.. image:: /_static/tutorials_6_6_img_6.jpg

Remarquez que le paramètre ‘payback year’ est resté inchangé, mais que la taille relative des zones positive (verte) et négative (rouge) ont changé et que le paramètre ‘B/C ratio’ est devenu inférieur à 1. Cela reflète l'actualisation plus prononcée des avantages futurs en raison d’un taux d'actualisation plus élevé, soit 8 %. Autrement dit, d’ici à ce que les futurs résidents de la zone d'étude tirent des avantages considérables des PLPM, les intervenants actuels souhaiteront avoir dépensé l’argent ou quelque chose d’autre.



**************************************************************
6,8. Didacticiel 4a: Risque (L1) avec pourcentage d’inondation (Polygones)
**************************************************************

Ce didacticiel démontre une analyse du risque des biens de type polygone lorsque le paramètre d’impact est un pourcentage d'inondation plutôt qu’une profondeur. Cela peut être utile pour une modélisation grossière du risque ou pour des biens, comme des champs agricoles dont il est possible de calculer la perte de manière raisonnable à partir du pourcentage du bien qui est inondé.

Chargez les couches de données suivantes à partir du dossier ‘tutorials\4\data\’ :

  • *haz_rast*: les trames d'événement à risque avec prédictions de valeur WSL pour la zone d'étude présentent quatre probabilités.

      o *haz_0050_tut4.tif*

      o *haz_0100_tut4.tif*

      o *haz_0200_tut4.tif*

      o *haz_1000_tut4.tif*

  • *dtm_cT2.tif*: Couche DTM (et le fichier .dlr de définition de la couche stylisée correspondante)

  • *finv_tut4a_polygons.gpkg*: couche spatiale de l'inventaire des biens d'inondation (’finv’)

  • |ss| *finv_tut4b_lines.gpkg*: |se| (utilisé dans le didacticiel 4b)

Déplacez la couche d'inventaire du polygone (‘finv’) sur le dessus, appliquez le style ‘fill transparent blue’ de CanFlood (disponible dans l'ensemble de styles CanFlood qu’on décrit dans :ref:`Section5.4.4 <Section5.4.4>` (Plugins > CanFlood > Add Styles)), et votre projet devrait ressembler à ceci (Assurez-vous de charger les couches ‘.qlr’ stylisées à la place des couches brutes) :

.. image:: /_static/tutorials_6_8_img_1.jpg

6.8.1. Créer le modèle
======================

**Setup** (Configuration)

Lancez la trousse d'outils ‘Build’ de CanFlood et allez à l’onglet 'Setup’. Réglez le champ ‘Precision’ à ‘6’ (ce qui est important pour l'analyse du pourcentage d'inondation, qui fonctionne par petites fractions) et terminez ensuite la configuration type de la manière décrite dans le didacticiel 1a.

**Inventory** (Inventaire)

Allez à l’onglet ‘Inventory’. Assurez-vous que le paramètre ‘Elevation type’ est réglé à ‘datum’ (le pourcentage d'inondation du risque (L1) ne peut utiliser les élévations des biens; par conséquent, cette variable d’intrant est redondante. Lorsque as_inun=True, le modèle CanFlood s’attend habituellement à voir une colonne ‘elv’ comportant uniquement des zéros). **Cliquez ensuite sur ‘Store’.**

**Hazard Sampler** (Échantillonneur de risque)

Allez à l'outil ‘Hazard Sampler’. Chargez ensuite les quatre trames de danger dans la fenêtre de dialogue, cochez ‘Box plots’ et, sous la configuration d'exposition, sélectionnez ‘Area-Thresholld’ en tant que type, réglez le paramètre ‘Depth Threshold’ à 0,5 et sélectionnez la couche DTM tel qu'indiqué :

.. image:: /_static/tutorials_6_8_1_img_1.jpg

**Click ‘Sample Rasters’**. (Cliquez sur ‘Sample Rasters’) Allez au fichier des données d'exposition (‘expositions’) créé dans votre répertoire de travail. Vous devriez voir apparaître un tableau ressemblant à celui-ci :

.. image:: /_static/tutorials_6_8_1_img_2.jpg

Ces valeurs représentent le pourcentage calculé de chaque polygone présentant une inondation supérieure au seuil de profondeur indiqué (0,5 m). Les tracés de boîte qui ont été créés présentent des données sous forme graphique :

.. image:: /_static/tutorials_6_8_1_img_3.jpg

**Event Variables and Validation** (Variables et validation des événements)

Exécutez les outils ‘Event Variables’ et ‘Validation’ comme on le demande dans le didacticiel 1a.

6.8.2. Exécuter le modèle
====================

Ouvrez le dialogue ‘Model’ |runimage| et suivez les étapes présentées dans le didacticiel 1a pour configurer cette exécution du modèle. Allez à l'outil ‘Risk (L1)’, cochez les cases indiquées et cliquez sur ‘Run risk1’:

.. image:: /_static/tutorials_6_8_2_img_1.jpg

L’ensemble de fichiers de résultats qu’on décrit ci-dessous devrait avoir été créé.

6.8.3. Afficher les résultats
=======================

Allez à votre répertoire de travail. Vous devriez maintenant voir que les fichiers de résultats suivants ont été créés :

  • *risk1_run1_tut4_passet.csv*: résultats par bien
  • *risk1_run1_tut4_ttl.csv*
  • *tut4a run1 AEP-Impacts plot for 6 events.svg*
  • *tut4a run1 Impacts-ARI plot for 6 events.svg*

Ouvrez le fichier de données des résultats par bien (‘passet’). Il devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_8_3_img_1.jpg

Les premières colonnes sans index représentent simplement les pourcentages d'inondation (provenant du fichier de données ‘expos’) multipliés par l’attribut d’échelle du bien (provenant du fichier de données ‘finv’). La dernière colonne ‘ead’ représente la valeur attendue de ces quatre colonnes.

Pour les visualiser, ouvrez la boîte d'outils ‘Results’ et configurez l’onglet ‘Setup’ en sélectionnant le fichier de commande. Allez à l’onglet ‘Join Geo’ et configurez-le de la manière décrite ci-dessous :

.. image:: /_static/tutorials_6_8_3_img_2.jpg

Cliquez sur **‘Join’**. Vous devriez voir une nouvelle couche vectorielle de polygone chargée dans votre canevas avec un style gradué rouge et des étiquettes appliquées aux résultats EAD qui ont été calculés au cours de l'étape précédente : 

.. image:: /_static/tutorials_6_8_3_img_3.jpg

***********************************************************
6,9. Didacticiel 4b: Risque (L1) avec pourcentage d’inondation (Lignes)
***********************************************************

À l’instar du didacticiel 4a, celui-ci démontre une analyse du risque où le paramètre d’impact est le pourcentage d'inondation, mais avec des géométries en ligne plutôt que des polygones. Cette façon de faire peut être utile pour analyser le risque d'inondation de biens linéaires, comme les routes.

Chargez les mêmes couches de données du dossier  ‘tutorials\4\data\’, en ajoutant :

  • *finv_tut4b_lines.gpkg*

Suivez toutes les étapes décrites dans le didacticiel 4a, mais avec cette nouvelle couche d'inventaire de bien (‘finv’).

Les résultats pour chaque bien devraient ressembler à ce qui suit :

.. image:: /_static/tutorials_6_9_img_1.jpg

Les premières colonnes ‘impact’ sans index représentent les événements dangereux, alors que les valeurs montrent le pourcentage d'inondation de chaque segment multiplié par sa valeur ‘f0_scale’. Cela pourrait représenter les mètres inondés (au-dessus du seuil de profondeur de 0,5 m) par segment, si la valeur ‘f0_scale’ représente la longueur du segment (comme c’est le cas avec l'inventaire du didacticiel). Ou encore, la valeur ‘f0_scale’ pourrait être réglée à ‘1.0’ pour toutes les caractéristiques, de sorte que les valeurs reflèteraient simplement le % d'inondation de chaque segment (reflétant ainsi l’extrant de l'outil d’échantillonneur de risque), alors que la dernière colonne calculerait le pourcentage annuel attendu d'inondation du segment.

************************************************
6,10. Didacticiel 5a: Risque (L1) de l’INRP et de GAR15
************************************************

Ce didacticiel démontre la façon de créer un modèle de ‘Risk (L1)’ CanFlood à partir de deux sources sur le Web :

  • Inventaire national des rejets de polluants (INRP) <https://www.canada.ca/en/services/environment/pollution-waste-management/national-pollutant-release-inventory.html>`__; et
  • `L’évaluation globale du risque d'inondation GAR15 Atlas <https://preview.grid.unep.ch/index.php?preview=home&lang=eng>`__ (Voir Rudari and Silvestro (2015) pour connaître les détails du modèle de risque d'inondation GAR15).

Pour en apprendre davantage sur ces ensembles de données, voir :ref:`Appendix A <appendix_a>`.

Ce didacticiel porte sur les données présentant des CRS disparates, de sorte que les utilisateurs devraient connaître la manière propre à QGIS de traiter le projet et le CRS de couche qu’on aborde ici <https://docs.qgis.org/3.10/en/docs/user_manual/working_with_projections/working_with_projections.html>`__.

6.10.1. Charger les données dans le projet
============================

Commencez en réglant `le CRS de vos projets QGIS à ‘EPSG:3978’ (Project > Properties > CRS > select ‘EPSG:3978’) (Tout dépendant des réglages de votre profil, le CRS du projet peut être réglé automatiquement par la première couche chargée). Vous êtes maintenant prêt à télécharger et ensuite à ajouter la couche de données au didacticiel 5 :

  • *tut5_aoi_3978.gpkg*: Polygone de la ZI pour le didacticiel.

Réglez le style de couche de la ZI de manière à ‘remplir le transparent rouge’ pour vous permettre de voir au travers du polygone. Avant que la création de l'inventaire ne puisse commencer, nous devons ajouter les données brutes de l’INRP et de GAR15 dans le projet de QGIS. Alors qu’il existe plusieurs options permettant d’accéder à ces données et de les importer, ce didacticiel démontrera la façon d’utiliser la caractéristique ‘Add Connections’ |addConnectionsImage| de CanFlood (:ref:`Section5.4.1 <Section5.4.1>`) pour ajouter premièrement une connexion au profil et télécharger ensuite les couches désirées.

**Connect to Web-Data** (Connexion aux données Web)

Commencez en développant ‘Browser Panel’ dans QGIS (ctrl + 2) et en cliquant ensuite sur ‘Refresh’ sur le panneau. Le résultat devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_10_1_img_1.jpg

On peut voir toutes les connexions à l'intérieur de votre profil QGIS.

Exécutez ensuite ‘Add Connections’ |addConnectionsImage| (Plugins > CanFlood) pour exécuter un script qui tentera d’ajouter un ensemble de connexions additionnelles à votre profil. Les messages de votre journal devraient ressembler à ce qui suit :

.. image:: /_static/tutorials_6_10_1_img_2.jpg

Cela décrit chacune des connexions que CanFlood a ajoutées à votre profil. Pour vérifier, revenez à l'option ‘Browser Panel’. Vous devriez voir les connexions suivantes (sous chaque type de connexion) :

  • UNISDR_GAR15_GlobalRiskAssessment (WCS)
  • ECCC_NationalPollutantReleaseInventory_NPRI (ArcGIS Feature Service)

Remarquez que ces connexions resteront dans votre profil en vue des prochaines séances QGIS, ce qui signifie que l'outil ‘Add Connections’ |addConnectionsImage| devrait être nécessaire une seule fois par profil (les nouvelles installations de QGIS devraient mener automatiquement au même répertoire du profil (Settings > User Profiles > Open Active Profile Folder) et, par conséquent, acheminer vos renseignements de connexion précédents).

**Download NPRI Data** (Télécharger les données de l’INRP)

Maintenant que les connexions ont été ajoutées à votre profil, vous êtes prêt à télécharger les couches. Pour limiter la demande de données, assurez-vous que votre canevas de carte correspond approximativement aux étendues de la ZI (appuyez sur Ctrl+Maj+F pour faire un zoom sur les étendues du projet). Ouvrez maintenant la fonction ‘Data Source Manager’ de QGIS (Ctrl + L) et sélectionnez ‘ArcGIS Feature Server’. Sélectionnez ‘ECCC_NationalPollutantReleaseInventory_NPRI’ dans le menu déroulant sous ‘Server Connections’. **Cliquez sur ‘Connect’** pour afficher les couches qui sont disponibles au serveur. Sélectionnez la couche 3 ‘Reported releases to surface water for 2019’, cochez ‘Only request features…’, et**cliquez ensuite sur ‘Add’** pour ajouter des couches au projet comme on peut le voir ci-dessous :

.. image:: /_static/tutorials_6_10_1_img_3.jpg

Vous devriez maintenant voir une couche de points vectoriels ajoutée à votre projet avec de l'information sur chaque installation signalée à l’INRP (dans l'affichage de votre canevas). Notez que le CRS de cette couche est EPSG:3978 (faites un clic droit sur la couche dans le panneau ‘Layers’ > Properties > Information > CRS), ce qui devrait correspondre à votre projet QGIS et à la ZI.

**Téléchargez les données GAR15**

Utilisez une méthode comparable pour procéder au téléchargement (tout dépendant de votre connexion Internet, ce processus peut être lent. On recommande de régler le paramètre ‘Cache’=’Prefer cache’ pour limiter les transferts de données additionnels et pour désactiver les couches ou pour neutraliser le rendu une fois chargé dans le projet) les couches suivantes de ‘UNISDR_GAR15_GlobalRiskAssessment’ sous l’onglet ‘WCS’ tel qu'indiqué ci-dessous :

  • GAR2015:flood_hazard_200_yrp
  • GAR2015:flood_hazard_100_yrp
  • GAR2015:flood_hazard_25_yrp
  • GAR2015:flood_hazard_500_yrp
  • GAR2015:flood_hazard_1000_yrp

.. image:: /_static/tutorials_6_10_1_img_4.jpg

Vous devrez charger une couche à la fois et le message ‘Select Transformation’ pourrait apparaître (vous pouvez sélectionner en toute sécurité la transformation de votre choix ou fermer le dialogue. Ces transformations peuvent être affichées seulement. Nous reviendrons à la transformation des données sur notre CRS ci-dessous). Après avoir terminé, votre canevas devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_10_1_img_5.jpg

6.10.2. Créer le modèle
=======================

Cette section concerne la façon de créer un modèle Risk (L1) à partir des données d’INRP et GAR15 qu’on a téléchargées. Pour connaître le reste du processus de modélisation Risk (L1), voir la Section6.1_.

**Setup** (Configuration)

Suivez les instructions dans la Section6.1.2_ *Setup*; mais assurez-vous d’avoir sélectionné ‘tut5_aoi_3978’ sous ‘Project AOI’, ainsi que ‘Load session results…’.

.. image:: /_static/tutorials_6_10_2_img_1.jpg

**Construct and Store Inventory** (Créer et enregistrer un inventaire)

Allez à l’onglet ‘Inventory’. Pour convertir les données d’INRP téléchargées dans une couche d'inventaire L1 que CanFlood reconnaîtra, nous devrons ajouter les champs et les valeurs ‘elv’ et ‘scale’. Pour cette simple analyse, nous tenons pour acquis que chaque bien présente une hauteur de vulnérabilité égale à zéro (c'est-à-dire que toute profondeur d'inondation positive entraîne une exposition). Cette hypothèse est réalisée dans CanFlood en réglant ‘felv’= ‘datum’ et chaque ‘f0_elv’=0 (et en utilisant les trames de profondeur plutôt que les trames WSL). À partir du menu déroulant de la couche vectorielle, sélectionnez la couche d’INRP et assurez-vous que les champs ‘nestID’, ‘scale’ et ‘elv’ correspondent à ce qu’on voit ci-dessous. Enfin, **cliquez sur ‘Construct finv’** pour créer la nouvelle couche d'inventaire. Pour générer le fichier csv de l'inventaire des biens (‘finv’), assurez-vous que cette nouvelle couche a été sélectionnée dans le menu déroulant ‘Inventory Vector Layer’. Configurez maintenant les paramètres ‘felv’ et ‘cid’ tel qu'indiqué ci-dessous et **cliquez ensuite sur ‘Store’:**

.. image:: /_static/tutorials_6_10_2_img_2.jpg

**Hazard Sampler** (Échantillonneur de risque)

Vous êtes maintenant prêt à échantillonner les couches de risque GAR15 avec votre nouvel inventaire de l’INRP. Contrairement aux couches de risque qu’on a utilisées dans les didacticiels précédents, les couches de risque GAR15 présentent les données de *profondeur* (plutôt que WSL) en *centimètres* (plutôt qu’en mètres) dans un système de coordonnées autre que celui de votre projet. De plus, l’étendue de ces couches de risque est bien plus grande que ce dont on a besoin dans le cadre de notre projet; et parce qu’il s’agit de couches Web, plusieurs des outils de traitement de QGIS ne fonctionneront pas. Par conséquent, nous devons utiliser les quatre outils ‘Raster Preparation’ qu’on décrit dans :ref:`Table5-2 <Table5-2>` avant de passer à l'option ‘Hazard Sampler’.

Allez à l’onglet ‘Hazard Sampler’, assurez-vous que les cinq couches GAR2015 sont énumérées dans la fenêtre et cliquez ensuite sur ‘Sample’. Vous devriez obtenir un message d’erreur vous indiquant que le CRS de la couche ne correspond pas à celui du projet. Pour résoudre la situation, cliquez sur le bouton ‘Raster Prep’ et configurez les poignées de préparation de trame de la manière décrite. **Cliquez ensuite sur ‘Prep’** et enfin sur ‘OK’:

.. image:: /_static/tutorials_6_10_2_img_3.jpg

Vous devriez voir cinq nouvelles trames chargées dans votre canevas (avec le suffixe ‘prepd’). Ces couches devraient présenter des pixels tournés, être fixées à la zone, présenter des profondeurs raisonnables (en mètres) et le même CRS que le projet (dans certains cas, QGIS peut ne pas reconnaître le CRS attribué à ces nouvelles trames, ce qu’indique un « ? » apparaissant à la droite de la couche sur le panneau de la couche. Dans un tel cas, vous devrez définir la projection en allant à l’option ’Properties’ de la couche et, sous ‘Source’, régler le système de coordonnées de manière à ce qu’il corresponde à celui du projet (EPSG: 3978)). De plus, chacune des trames devrait être sauvegardée dans votre répertoire de travail. Ce nouvel ensemble de couches de risque devrait répondre aux attentes de l'échantillonneur de risque, vous permettant ainsi de procéder à la création d’un modèle L1 de la manière décrite dans la Section6.1_.

.. _Section6.11:

****************************************
6,11. Didacticiel 6a: o Polygone de bris de digue :
****************************************

Le didacticiel démontre la façon de générer des polygones de bris à partir des renseignements sur une digue type au moyen de l'outil ‘Dike Fragility Mapper’ de CanFlood (:ref:`Section5.4.1 <Section5.4.1>`). Avant de suivre ce didacticiel, les utilisateurs devraient connaître les types de données des événements à risque qu’on décrit dans :ref:`Section4.2 <Section4.2>` (en particulier les polygones de défaillance) dont on a besoin pour les modèles de risque (L1) et (L2) qui présentent une certaine défaillance. Commencez en téléchargeant les données du didacticiel à partir du dossier `tutorials\6 <https://github.com/IBIGroupCanWest/CanFlood/tree/master/tutorials/6>`__ et téléchargez-les dans un nouveau projet QGIS :

    • trames des événements WSL à risque (sans défaillance)

        o *0010_noFail.tif*

        o *0050_noFail.tif*

        o *0200_noFail.tif*

        o *1000_noFail.tif*

    • *dike_influence_zones.gpkg*: Couche de zone d’influence de segment de digue présentant deux caractéristiques polygonales, chacune correspondant à la zone d’influence de certains segments de digue;
    • *dikes.gpkg*: Couche multiligne d’alignement de digue
    • *dtm.tif*: Modèle de terrain numérique (importer ‘dtm.qlr’ pour obtenir la version stylée);
    • *dike_fragility_20210201.xls*: Bibliothèque des fonctions de fragilité de dique.

Voir :ref:`Section4.5 <Section4.5>` pour une description de ces ensembles de données. Assurez-vous que le CRS de votre projet est réglé à ‘EPSG:3005’. Après avoir chargé les couches GIS, le canevas de votre carte ressemble à celui qu’on peut voir ci-dessous :

.. image:: /_static/tutorials_6_11_img_1.jpg

Pour rendre cet espace de travail plus convivial, assurez-vous que les couches ‘dikes’ et ‘dike_influence_zones’ se trouvent en haut sur le panneau des couches. Appliquez maintenant les styles CanFlood (chargez ces styles sur votre profil au moyen de l'outil Plugins>CanFlood>Add Styles décrit dans la  :ref:`Section5.4.4 <Section5.4.4>`) à chacune de ces couches:

  • *dikes*: ‘flèche noire’
  • *dike_influence_zones*: ‘remplissage rouge transparent’

Le style étroit est utile, puisque nous devrons connaître la directionnalité de la couche de digue pour informer l'outil du côté de la digue qu’on doit échantillonner. Nous sommes maintenant prêts à ouvrir le dialogue ‘Dike Fragility Mapper’:

.. image:: /_static/tutorials_6_11_img_2.jpg

Configurez votre dialogue de manière comparable à ce qu’on peut voir ci-dessous en utilisant vos propres répertoires (assurez-vous que le paramètre ‘dikeID’ est réglé à ‘ID’):

.. image:: /_static/tutorials_6_11_img_3.jpg

6.11.1. Calculer l'exposition des digues
===============================

Cette étape calculera les valeurs d'exposition, ou le franc-bord, de chaque segment de digue. Allez à l’onglet ‘Dike Exposure’, cliquez sur ‘Refresh’ et configurez-le ensuite de la manière décrite décrite ci-dessous en prenant soin de sélectionner la couche DTM dans le menu déroulant, mais non dans la fenêtre de sélection:

.. image:: /_static/tutorials_6_11_1_img_1.jpg

Cliquez sur **‘Get Exposure’**. Vous devriez voir 10 couches se charger sous le groupe ‘CanFlood.Dikes’:

  • *tut6_dike_dikes*: couche de digues traitées
  • couches des points de défaillance (pour chaque événement)

      o *0010_noFail_breach_1_pts*

      o *0050_noFail_breach_3_pts*

      o *0200_noFail_breach_16_pts (voir ci-dessous * |diamondimage| *)*

      o *1000_noFail_breach_50_pts*

  • *tut6_tut6_dike_dikes_transects*: couche de transects (voir ci-dessous |lineimage|)

  • couches des points d'exposition aux transects

      o *tut6_dike_dikes_0010_noFail_expo*

      o *tut6_dike_dikes_0050_noFail_expo*

      o *tut6_dike_dikes_0200_noFail_expo (voir ci-dessous * |dotimage| *)*

      o *tut6_dike_dikes_1000_noFail_expo*

On explique ces types de couches dans la Section6.11_, alors que celles qui concernent la série de 200 ans sont affichées ci-dessous. On peut voir la longueur de 40 m de la digue présentée à titre d’exemple et la longueur de transect de 200 m que nous avons indiquée dans la boîte de dialogue dans l’espacement et la longueur des transects présentés ci-dessous :

.. image:: /_static/tutorials_6_11_1_img_2.jpg

En son centre, cet outil échantillonne la trame WSL à l’extrémité de chaque transect et le DTM à la tête. Il compare ensuite ces valeurs pour calculer le franc-bord. Cela nous porte à croire que l'utilisation doit indiquer le côté approprié du transect, la longueur donnée à titre d’exemple et la longueur du transect basée sur la configuration de la digue et sur l'inondation pour obtenir un calcul précis du franc-bord.

Pour visualiser les valeurs de franc-bord calculées, appliquez l’option ‘Single Labels’ pour les valeurs ‘sid’ sur la couche des digues traitées. Allez ensuite à votre répertoire de travail et ouvrez le fichier d’image *‘tut6 dike 43-1 profiles.svg’*. Celui-ci devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_11_1_img_3.jpg

Il s’agit du tracé de profil de la digue 43, segment 1 (sid=4301) montrant l’élévation de crête calculée et le WSL pour les quatre trames d'événement (échantillonnées avec chaque transect). Notez que ce tracé porte à croire que le franc-bord de 50 ans doit se situer aux alentours de -0,2 m (voyez le cercle rouge ci-dessus). Ouvrez maintenant le fichier ‘tut6_dExpo_7_3.csv’ dans le répertoire de travail. Il s’agit de l'ensemble de données du segment de digue (‘dexpo’ que nous utiliserons dans l'étape suivante pour calculer les probabilités de défaillance.  Notez que la valeur du franc-bord du segment-événement en question est de -0,2m comme on s’y attendait :

.. image:: /_static/tutorials_6_11_1_img_4.jpg

6.11.2. Calculer la vulnérabilité des digues
====================================

Cette étape fera appel aux valeurs de franc-bord que nous avons calculées précédemment et aux courbes de fragilité fournies par l'utilisateur pour calculer la probabilité de bris de chaque segment. Passez à l’onglet ‘Dike Vulnerability’. Vous devriez voir le chemin de fichiers menant aux résultats d'exposition ci-dessus apparaître automatiquement dans le champ ‘dexpo_fp’. Sélectionnez maintenant la bibliothèque des courbes de fragilité, soit le fichier ‘dike_fragility_20210201.xls’ qui accompagne les données du didacticiel. Les noms d’onglet de ce cahier d’exercices correspondent au champ ‘f0_dtag’ sur la couche des digues, qui informe CanFlood sur la courbe qu’il doit appliquer à quel segment. Choisissez ‘None’ pour les corrections à l'effet de longueur. Votre dialogue devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_11_2_img_1.jpg

Cliquez maintenant sur ‘Calc Fragility’ pour générer le tableau des données de probabilité de défaillance (‘pfail’).

6.11.3. Rejoindre les zones
=====================

Au cours de cette dernière étape, nous allons combiner les problèmes de défaillance calculés précédemment aux zones d’influence fournies par l'utilisateur pour chaque segment en fonction des liens fournis sur la couche des digues. Allez à l’onglet ‘Join Areas’. Vous devriez voir le chemin de fichiers de données ‘pfail’ dans le champ correspondant; sinon, allez à ce fichier. Si vous avez exécuté l'outil ‘Dike Exposure’ avec succès au cours de cette séance, vous devriez voir que la première colonne des couches de trame a été sélectionnée; si tel n’est pas le cas, sélectionnez les quatre trames WSL manuellement dans la première colonne. Pour la deuxième colonne, sélectionnez la couche du polygone ‘dike_influence_zone’ dans le premier menu déroulant. Cliquez ensuite sur ‘Fill Down’ pour remplir les autres menus déroulants. Après avoir terminé, votre dialogue devrait ressembler à ce qui suit :

.. image:: /_static/tutorials_6_11_3_img_1.jpg

Cliquez sur **‘Map pFail’**. Vous devriez voir quatre couches de polygone chargées sur votre canevas, soit une pour chaque événement. Déplacez ces couches vers le haut dans la liste des couches afin qu’elles apparaissent au haut des trames. La version de 200 ans est présentée ci-dessous :

.. image:: /_static/tutorials_6_11_3_img_2.jpg

Les couches de résultats sont stylisées automatiquement en tant que polygones de défaillance, alors qu’ils présentent le nom de la trame d'événement, le segment de la digue source (‘sid’) et la probabilité de défaillance de chaque caractéristique. Remarquez que la version de 200 ans renferme trois caractéristiques polygonales qui se chevauchent et qui correspondent aux trois segments présentant une défaillance, et ce, malgré que la couche ‘dike_influznce’zones’ présente deux caractéristiques. Ce mappage des polygones par rapport aux segments de digue est placé sur la couche des digues dans le champ ‘Influence Area ID’ indiqué sur l’onglet ‘Setup’ (‘ifzID’ dans ce cas). Il est donc possible de préciser les liens 1:1 ou many:many segment-polygon, permettant ainsi à l'utilisateur de mapper la probabilité de chaque brèche ou les groupes de segments pour appliquer les probabilités calculées à un cercle de digue plus grand. Voir :ref:`Section5.4.1 <Section5.4.1>` pour en apprendre davantage au sujet de cet outil.

.. _Section6.12:

*************************************************
6,12. Didacticiel 7a: Échantillonnage de géométries complexes
*************************************************

Ce didacticiel démontre *l'échantillonnage des valeurs* à partir de statistiques d'échantillonnage indiquées *pour chaque bien*. Cela peut être utile lorsque vous souhaitez procéder à un échantillonnage en faisant appel à des statistiques hétérogènes à l'intérieur d’un inventaire unique (par exemple, élévation du sol ‘Max’ pour certains édifices et élévation ‘Min’ pour d’autres). Commencez en téléchargeant les données du didacticiel du dossier `tutorials 7 <https://github.com/NRCan/CanFlood/tree/master/tutorials/7>`__ pour le charger dans un nouveau projet QGIS :

  • *haz_rast*: les trames d'événement à risque avec prédictions de valeur WSL pour la zone d'étude présentent quatre probabilités.

      o *haz_0050_tut4.tif*

      o *haz_0100_tut4.tif*

      o *haz_0200_tut4.tif*

      o *haz_1000_tut4.tif*

  • *dtm_cT2.tif*: Couche DTM (et le fichier .dlr de définition de la couche stylisée correspondante)

  • *finv_tut7_polys.gpkg*: couche spatiale d'inventaire des biens d'inondation (’finv’) (et fichier .qlr correspondant de définition stylisée des couches)

6.12.1. Créer le modèle
=======================

**Setup** (Configuration)

Complétez la configuration type de la manière décrite dans le didacticiel 1a. 

**Hazard Sampler** (Échantillonneur de risque)

Allez à l'outil ‘Hazard Sampler’, cochez les quatre trames de risque, réglez le paramètre ‘Type’ à ‘values’, réglez le paramètre ‘Stat. Type’ à ‘Per-Asset’. Sélectionnez ensuite le champ ‘sample_stat’ pour que CanFlood utilise les statistiques d'échantillonnage de ce champ. Vérifiez si votre dialogue ressemble à celui qu’on voit ci-dessous et **cliquez sur ‘Sample’**.

.. image:: /_static/tutorials_6_12_img_1.JPG

Effectuez le reste du processus de création en exécutant les outils ‘Event Variables’, ‘DTM Sampler’ et ‘Validation’ de la manière décrite dans le didacticiel 1a.

6.12.2. Exécuter le modèle
=====================

Ouvrez le dialogue ‘Model’ et suivez les étapes du didacticiel 1a pour configurer l'exécution de ce modèle.  Exécutez ensuite le modèle ‘Risk (L1) pour générer les fichiers suivants :

	• risk1_tut7a_passet.csv: valeur attendue d'inondation par bien; 
	• risk1_tut7a_ttl.csv: résultats totaux, valeur attendue de l'inondation totale par événement; 
	• tut7a.run1 Impact-ARI plot on 6 events.svg: tracé des résultats totaux. 

Pour comprendre et visualiser l’effet d’un réglage des statistiques d'échantillonnage du risque à ‘Per-Asset’, vous pouvez essayer de recréer et d’exécuter le même modèle en réglant le type de statistiques de risque à ‘Global’ et l'ensemble de statistiques à ‘Mean’ pour comparer ensuite les résultats.

6.12.3. Afficher les résultats
====================
Pour visualiser la différence entre ces deux configurations de modèle, ouvrez la trousse d'outils ‘Results’ et sélectionnez un répertoire de travail, ainsi que le fichier de commande ‘Per-Asset’ comme étant le fichier de commande principal sur l’onglet ‘Setup’. Avant de générer des fichiers de comparaison, configurez le style de tracé en ouvrant le même fichier de commande principal et en modifiant les paramètres ‘[plotting]’ suivants : 

    • ‘color = red’ 
    • ‘linestyle = solid’ 
    • ‘impactfmt_str = ,.0f’ 

Pour générer un tracé de comparaison de ces deux scénarios, allez à l’onglet ‘Compare/Combine’, sélectionnez l’option ‘Control File’ pour les deux configurations de modèle (par bien et de façon globale) générées au cours de l'étape précédente, assurez-vous que l'option ‘Control Files’ a été sélectionnée sous ‘Comparison Controls’ et **cliquez ensuite sur ‘Compare’**.  Vos résultats devraient ressembler à ce qui suit :

.. image:: /_static/tutorials_6_12_img_2.JPG

.. |visualimage| image:: /_static/add_connections_image.jpg
   :align: middle
   :width: 22

.. |buildimage| image:: /_static/build_image.jpg
   :align: middle
   :width: 22

.. |runimage| image:: /_static/run_image.jpg
   :align: middle
   :width: 22

.. |visualimage| image:: /_static/visual_image.jpg
   :align: middle
   :width: 22

.. |diamondimage| image:: /_static/red_diamond_image.jpg
   :align: middle
   :width: 22

.. |lineimage| image:: /_static/horizontal_line_image.jpg
   :align: middle
   :width: 22

.. |dotimage| image:: /_static/green_dot_image.jpg
   :align: middle
   :width: 22

.. |ss| raw:: html

    <strike>

.. |se| raw:: html

    </strike>
    
.. _Section6.13:

***************************************
6,13. Didacticiel 8a: Analyse de sensibilité
***************************************

Ce didacticiel démontre le flux des travaux de *l'analyse de sensibilité* (:ref:`Section5.4.5 <Section5.4.5>`). Celui-ci peut être utile pour quantifier la sensibilité de votre modèle en fonction de chaque paramètre et fichier de données.

Commencez en téléchargeant les données du didacticiel du dossier `tutorials 8 <https://github.com/NRCan/CanFlood/tree/master/tutorials/8>`__ pour les télécharger dans un nouveau projet QGIS :
 
  • *haz_rast*: les trames d'événement à risque avec prédictions de valeur WSL pour la zone d'étude présentent quatre probabilités.

      o *haz_0050_tut8.tif*

      o *haz_0100_tut8.tif*

      o *haz_0200_tut8.tif*

      o *haz_1000_tut8.tif*

  • *dtm_tut8.tif*: Couche DTM (et le fichier .dlr de définition de la couche stylisée correspondante)

  • *finv_tut8.gpkg*: couche spatiale d'inventaire des biens d'inondation (’finv’) (et fichier .qlr correspondant de définition stylisée des couches)
  
  • *CanFlood_tut8.txt*: fichier de commande de modèle principal
  
  
6.13.1. Préparer l'analyse
==========================

Lancez le dialogue *Sensitivity Analysis* |targetImage| à partir du menu Plugins>CanFlood. Allez au menu *Setup*, sélectionnez votre répertoire de travail, réglez les chemins de fichiers à ‘relative’. Précisez ensuite votre fichier de commande de modèle principal et réglez 'Model Level' = 'L2’ tel qu'indiqué ci-dessous :

.. image:: /_static/tutorials_6_13_img_1.JPG

**Click Load** (Cliquez sur ‘Load’) pour remplir l’onglet *Compile*.

.. |targetImage| image:: /_static/target.png
   :align: middle
   :width: 22
   
   
6.13.2. Configurer et compiler la suite de modèles
=============================================

Allez à l’onglet *Compile*. Les valeurs de ‘base’ qui apparaissent sur la première rangée du fichier de commande devraient être apparues automatiquement et une reproduction sur la deuxième rangée.

.. image:: /_static/tutorials_6_13_img_2.JPG

Ajoutez maintenant trois autres modèles candidats en **cliquant sur le bouton ‘Add’ à trois reprises**. Remarquez que les noms de modèle ont été générés automatiquement, mais les autres champs sont identiques au modèle de base. Nous allons maintenant modifier ou ‘perturber’ un paramètre ou un fichier de données pour chaque candidat afin de compiler la suite d'analyse de sensibilité.

Pour la première perturbation, **remplacez simplement la valeur rtail ‘cand01’ à 0,1**. Pour la deuxième perturbation, **modifiez le paramètre 'curve_deviation' sur 'cand02' à 'lo'** afin qu’il corresponde à la valeur des dommages à la profondeur moindre qui est enregistrée dans le fichier curves.xls. Nous allons configurer les deux autres perturbations à l'étape suivante. 

Pour nous permettre de différencier les tracés, nous générons (voir ci-dessous), **cliquez sur 'Randomize Colors'**. 

.. image:: /_static/tutorials_6_13_img_3.JPG

Assurez-vous que l'option ‘Copy all candidate data files’ est sélectionnée afin que le compilateur donne à chaque candidat ses propres fichiers de données, plutôt que de voir chaque point retourner aux fichiers de données du modèle principal. Enfin, **cliquez sur 'Compile Candidates'**.  Vous verrez maintenant quatre nouveaux dossiers, soit un pour chaque modèle candidat, dans votre répertoire de travail.


6.13.3. Manipuler les fichiers de données
============================

Sur l’onglet *DataFiles*, sélectionnez 'cand03' et 'finv' pour intégrer le fichier de données correspondant dans le chemin du fichier de données. **Cliquez sur 'Load'** pour ajouter ce fichier de données à votre projet.

.. image:: /_static/tutorials_6_13_img_4.JPG

Nous allons maintenant soustraire 0,5 de f0_elvs. **Cliquer sur 'Open Attribute Table'** (ou sur le bouton correspondant sur la barre d'outils QGIS ou appuyer sur 'F6') pour ouvrir la fenêtre des tables d’attributs. Notez mentalement les valeurs de 'f0_elv'. Ouvrez maintenant le *Calculateur de terrain* (Ctrl + I). Cochez 'Update Existing Field' et sélectionnez 'f0_elv' dans la boîte combo. Sélectionnez la fonction d’expression 'finv_elv_add' dans le menu 'CanFlood' au centre et complétez l’expression affichée :

.. image:: /_static/tutorials_6_13_img_5.JPG

**Cliquez sur 'OK'** pour apporter les changements aux valeurs de champ. Examinez les valeurs de 'f0_elv' dans la table des attributs. Ces valeurs devraient être de 0,5 inférieures à ce qu’elles étaient auparavant (c'est-à-dire 0,5 de moins que le modèle de base). 

De retour sur l’onglet 'DataFiles', **cliquez sur 'Save Datafile'** pour écraser l’ancien fichier .csv avec les modifications. 

Pour notre perturbation finale, nous allons soustraire 0,5 m des élévations du terrain (‘gels’). Sélectionnez 'cand04' et 'gels' et **cliquez sur 'Load'** pour charger ce fichier de données. Procédez de la manière décrite ci-dessus pour configurer le *Calculateur de terrain* et saisir la formule présentée ci-dessous :

.. image:: /_static/tutorials_6_13_img_6.JPG

**Cliquez sur 'OK'** sur le *Calculateur de terrain* pour mettre les valeurs à jour. **Cliquez sur 'Save Datafile'** pour écrire des changements dans le fichier .csv.

6.13.4. Exécuter la suite.
=====================

Sur l’onglet *Run*, on devrait voir le modèle de base et les quatre fichiers de commande du nouveau modèle candidat qui sont indiqués :

.. image:: /_static/tutorials_6_13_img_7.JPG

**Cliquez sur Run** pour exécuter en vrac des modèles L2 CanFlood.



6.13.5. Analyser les résultats
===========================

Sur l’onglet *Analysis*, on devrait voir les résultats de l'exécution, le fichier .pickle chargé, les valeurs sommaires, ainsi que le tableau sommaire rempli :

.. image:: /_static/tutorials_6_13_img_8.JPG

**Cliquez sur 'Plot Risk Curves'** afin d’obtenir les courbes de risque de comparaison pour cette suite :


.. image:: /_static/tutorials_6_13_img_9.svg

À partir de ce schéma, on peut voir clairement l’influence du paramètre ‘rtail’ sur la courbe de risque (et le paramètre annualisé). La baisse des élévations du terrain et des élévations de l'étage principal a produit des résultats comparables comme on s’y attendait.
