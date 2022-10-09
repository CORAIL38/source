===============================================
Comment contribuer à la Documentation 
===============================================

Où travailler pour ajouter de la documentation?
-----------------------------------------------

:file:`F:/ZZ. ECHANGE/PYTHON/SANDBOX/Doc/`

Dans ce répertoire se trouvent 2 sous répertoires :
    - **source** : tous les fichiers source de la documentation dans une hiérarchie de répertoires
    - **build** : tous les fichiers constituants la documentation consultable avec Edge ou Chrome en partant de index.html

.. note::

    Dans la suite de ce document, tous les fichiers et tous les répertoires sont spécifiés relativement à ce path: :file:`F:/ZZ. ECHANGE/PYTHON/SANDBOX/Doc/source/`


Quels outils doit on utiliser?
------------------------------

Les fichiers source permettant d'élaborer la documentation avec ``Sphinx`` sont des fichiers de texte placés dans le répertoire **source**. 
N'importe quel editeur de texte doit donc convenir. ``VSC`` fait particulièrement l'affaire dans la mesure ou les roles/directives/options de ``Sphinx`` ou de ``reStructuredText``  sont spécialement affichées
(Voir le fichier source :file:`intro.rst`). Ces fichiers sont ensuite processés par le programme ``sphinx-build`` pour générer la documentation en html dans le répertoire **build**.

Comment ajouter une documentation?
----------------------------------

Pour ajouter une documentation dans une section existante comme "Produits dans SANDBOX" (resp. "Packages dans LIB") , il suffit de créer un fichier rst dans le répertoire **sandbox** (resp. **LIB**).
qui se trouve dans le répertoire **source**. Par exemple: ARMORLUX.rst

.. image:: /intro/assets/images/explorer.png
    :align: center

|br|

.. note::

    Pour savoir exactement où travailler pour ajouter/modifier de la documentation, il faut explorer le fichier **index.rst** qui se trouve à la racine de la documentation **source** et repérer les fichiers déclarés sans extension rst. Il faut ensuite explorer les fichiers rst correspondants.

Exemple: ajouter le produit ARMORLUX de SANDBOX.

Dans le répertoire source/sandbox, ajouter le fichier ARMORLUX.rst et dans ce fichier, ajouter les premières lignes qui constituent le titre et qui apparaitront dans la Table des Matières.

.. code-block:: python

  ========
  ARMORLUX
  =========

Pour vérifier que tout est correct, ouvrez un terminal dans le répertoire DOC et construisez la documentation en tapant la commande:

.. code-block::


  sphinx-build -b html source build

    Sphinx v5.1.1 en cours d'exécution
    chargement des traductions [fr]... fait
    Chargement de l'environnement pickled... fait
    Construction en cours [mo] : cibles périmées pour les fichiers po 0
    Construction [html] : cibles périmées pour les fichiers sources 1
    Mise à jour de l'environnement : 0 ajouté(s), 1 modifié(s), 0 supprimé(s)
    Lecture des sources... [100%] intro
    Recherche des fichiers périmés... aucun résultat
    Environnement de sérialisation... fait
    Vérification de la cohérence... *source\quickstart\venv.rst:96: WARNING: La citation [gayerie] n'est pas référencée* fait
    Document en préparation... fait
    Écriture... [100%] intro
    Génération des index... genindex fait
    Écriture des pages additionnelles... search fait
    Copie des images... [100%] quickstart/assets/images/pytest/palette_0.png
    Copie des fichiers statiques... fait
    Copie des fichiers complémentaires... fait
    Export de l'index de recherche en French (code: fr)... fait
    Export de l'inventaire des objets... fait
    La compilation a réussi, 1 avertissement.

.. note::

    - Avant d'aller plus loin, il est très important de vérifier les informations fournies par la commande. 
    - L'outil fonctionne de manière incrémentale. Les erreurs ne sont visibles qu'au premier traitement. 
    - Si on relance la commande sans faire de modification, les erreurs et les warnings n'apparaissent plus. 
    - Ils ne réapparaissent que si le fichier est à nouveau traité à l'occasion d'une modification. 
    - Les explications sont en général assez claires et la ou les lignes en cause bien identifiées.

Après avoir corriger toutes les erreurs, ouvrir le fichier index.html qui se trouve dans build.

ARMORLUX doit maintenant apparaitre dans la Table des Matières.

Exemples d'utilisation des fonctionnalités de ``Sphinx``
--------------------------------------------------------

roles:
    - doc: https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html   
    - exemple:  venv.rst - ligne 40

    \:pep\:`8`  --> :pep:`8`
        
directives avec options:
    doc: https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html
    code-block: exemple:  sqlalchemy.rst - ligne 58
    image: exemple: pytest.rst - ligne 17
    note: ce fichier ligne 21

.. note::
    - Les directives sont précédées et suivies d'une ligne blanche. 
    - Les directives commencent par .. en début de ligne et finissent par :: à la fin du nom de la directive.
    - Les options :<option>: sont listées sur les lignes suivantes (pas de ligne blanche entre la directive et ses options) avec une indentation identique. un espace, par exemple. 
    - Les paramètres des options peuvent être donnés à la suite de l'option ou à la ligne suivante mais il doivent être indentés par rapport à l'option.
    - il doit y avoir une ligne blanche entre les options et le contenu de la directive.
    - Le contenu de la directive peut avoir la même indentation que les options ou être indenté par rapport aux options.

.. code-block:: python
 :linenos:
 :emphasize-lines:
  3,5    

  .. code-block:: python
                :linenos:
                :emphasize-lines: 3,5
  ou
                :emphasize-lines: 
                    3,5

                import os
                x=3

.. code-block::        

            .. image:: /quickstart/assets/images/pytest/palette_0.png
                :align: center

.. image:: /quickstart/assets/images/pytest/palette_0.png
    :align: center

|br|

substitution:
    exemple: ce fichier ligne 51

.. note::
    - Ne pas oublier les lignes blanches avant et après la substitution, dans ce cas précis, pour permettre la reconnaissance du sous-titre. 

.. code-block::        

            ...
            xyz |br|abc
            ...

            .. |br| raw:: html

                <br /> 	

Titre et sous Titre




        

Documentation
-------------

https://docutils.sourceforge.io/docs/user/rst/quickref.html fait un tour rapide de reStructuredText.

https://www.sphinx-doc.org/fr/master/# est la documentation de ``Sphinx``.

https://sphinx-rtd-theme.readthedocs.io/en/stable/ est la documentation du theme utilisé.


.. |br| raw:: html

  <br /> 	