==========================================
Python Virtual Environment (venv) dans VSC
==========================================

Si on souhaite personnaliser son environnement de travail dans VSC en utilisant des modules python, il parait souhaitable d'utiliser `venv` afin de ne personnaliser que son propre environnement.

Les modules externes qui sont utilisés à l'intérieur du code développé peuvent/doivent être installés en central en commun accord avec les autres développeurs et l'IT qui peut se charger de l'installation de ces modules. Pour autant, il faudra les réinstaller dans le `venv` car le `venv` ne contient que l'installation d'un python standard correspondant au python central.

Exemple de modules à réinstaller dans le `venv` :

	- gspread	4.0.1
	- pandas 	1.4.2
	- oauth2client	4.1.3
	
Une liste exhaustive des modules/packages utilisés par un code est disponible avec la commande : 

.. code-block:: python

 pip freeze

On peut envoyer cette liste dans un fichier en utilisant les redirections.
Cette liste contient tous les modules : les nouveaux, ceux qui ont été mis à jour et ceux qui sont d'origine.

.. code-block:: python

 pip freeze > requirements_central.txt

On pourra ensuite utiliser ce fichier pour aligner le `venv` sur le central.

Par contre, la liste des modules utilisés pour personnaliser l'environnement de développement est individuelle. De plus, l' installation de ces modules peut "polluer" l'installation centrale, voire créer des incompatibilités ou nécessiter des maj de modules préexistants.
Dans la mesure où ces modules ne sont pas nécessaires à la bonne marche du code, il parait souhaitable d'en laisser la gestion à chacun, quitte à proposer ce qui est nécessaire pour faciliter le setup d'un nouveau projet. Par exemple, en proposant un fichier requirements_perso.txt

On peut nuancer cette approche si il y a consensus entre tous les développeurs sur l'utilisation par tous d'un ou plusieurs modules qui pourraient être installés en central.

Exemples de modules dédiés au développement :

	- black     # Code format ( :pep:`8`)
	- coverage  # Test coverage
	
	- flake8	# Installation issue
	- isort	# import sort
	- mccabe	# Code complexity
	- pylint	# Linter (:pep:`20`)
	- pytest 	# framework de test
	- tox	# Test automation
	
* Python Enhancement Proposals

La mise en service d'un `venv` dans VSC est assez simple. [VSC]_

Dans VSC, à partir d'un terminal du projet:

.. code-block::

	python -m venv .venv

Command Palette : Select Interpreter


		
L'interpreter sélectionné apparait dans la barre de status


	
Tous les terminaux ouverts avant cette sélection fonctionnent encore sur l'installation centrale.
Mais à partir de là, tous les terminaux nouvellement ouverts dans VSC fonctionneront dans ce `venv` ainsi que les différentes opérations comme l'éxécution du code, le debug, l'éxécution des tests, etc… Sachant que chacune de ces opérations peut être elle-même configurée différemment.

De plus, à chaque instant il est possible de changer d'interpreteur via la commande de la palette ou en cliquant sur l'interpreter dans la barre de status.

Le fichier requirements.txt généré avec pip dans l'environnement central peut-être utilisé pour aligner le `venv` sur le central.

Dans un nouveau terminal, on peut exécuter : 

.. code-block::
	
	pip install -r requirements_central.txt

Si on execute un freeze : 

.. code-block::
	
	pip freeze > requirements_venv.txt

les 2 fichiers sont identiques.

Documentation:
--------------

.. [VSC]

	https://code.visualstudio.com/docs/python/environments extrait de la documentation VSC
	
.. [gayerie]

	https://gayerie.dev/docs/python/python3/pip_venv.html: remarques pertinentes extraites de https://gayerie.dev/

