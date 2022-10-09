=====
tests
=====

Choix du framework: **pytest**
******************************

La première fois qu'on veut utiliser **pytest** dans ``VSC``, il faut le spécifier explicitement. 

.. image:: /quickstart/assets/images/pytest/palette_0.png
 :align: center

|br|

Les fois suivantes, il apparait dans les choix.

.. image:: /quickstart/assets/images/pytest/palette.png
 :align: center

|br|

Fichiers et directories:
************************

.. image:: /quickstart/assets/images/pytest/files_and_dirs.png
 :align: center

|br|
__init__.py
============

tests\\__init__.py
----------------------

	Si le fichier __init__.py n'est pas présent dans la directory tests, tests n'est pas détecté en tant que module et l'exploration échoue.
		
.. image:: /quickstart/assets/images/pytest/Testing_ko.png
 :align: center

|br|
	Si le fichier __init__.py est présent dans la directory tests :
		
.. image:: /quickstart/assets/images/pytest/Testing_ok.png
 :align: center			

|br|	

Projet\\__init__.py
----------------------

	Ce fichier n'a pas d'effet sur les tests.
		
__init__.py
----------------------

	Ce fichier ne doit pas exister, sinon, l'explorer a un problème.

.. image:: /quickstart/assets/images/pytest/Testing_ko2.png
 :align: center

|br|

Pour avoir des détails sur le problème de l'explorer il faut laisser la souris sur l'erreur.
	
		
	
Tests
-----

Explorateur
		Pour être détectés par l'explorateur, les fichiers doivent commencer par "test\_" ou finir par "_test".
		Mais les méthodes doivent commencer par "test\_"
	
		
		
		Les erreurs de l'explorateur apparaissent dans l'onglet TESTING :

.. image:: /quickstart/assets/images/pytest/Explorer_problems.png
 :align: center		

|br|

Résultats
		Pour que les tests fonctionnent, on peut ajouter le fichier context.py pour améliorer la lisibilité du code.
		Pour autant, il faut bien importer les procédures dont on a besoin dans context.py et dans le test.
		
		Résultat obtenu via l'interface :
		
.. image:: /quickstart/assets/images/pytest/Tests_run.png
 :align: center
			
::		
		Résultat obtenu en tapant pytest dans le terminal :

.. image:: /quickstart/assets/images/pytest/pytest.png
 :align: center
		
|br|				

Coverage
--------

Pour obetnir la couverture des tests, on peut utiliser le module coverage.

::

	coverage run --source=Projet --branch -m pytest .

		=========== test session starts ===========
		platform win32 -- Python 3.9.7, pytest-7.1.2, pluggy-1.0.0
		rootdir: C:\Users\c.dupillier\Documents\BRIQUES
		collected 6 items                                                                                                                                                                             
		
		tests\Other_test.py .                                         [ 16%]
		tests\test_cmdAnalysis.py .                                   [ 33%]
		tests\test_cmdAnalysis_GS.py ....                             [100%]		
		=========== 6 passed in 21.29s ===========

	coverage report -m

		Name                     Stmts   Miss Branch BrPart  Cover   Missing
		--------------------------------------------------------------
		Projet\GS.py          60     60     16      0       0%   1-131
		Projet\Google.py      98     98     24      0       0%   3-161
		Projet\main.py       130     22     44     11      80%   50-52, 93, 96-98, 120-121, 133-134, 188, 250-252, 265-266, 274-278, 353, 357
		--------------------------------------------------------------
		TOTAL                288    180     84     11    37%
	

Pour obtenir un rapport html: 

.. code-block::

	coverage html

.. image:: /quickstart/assets/images/pytest/coverage_html.png


.. |br| raw:: html

  <br /> 	
