GIT dans VSC
=========================================================


À partir de l’adresse <https://git-scm.com/book/fr/v2/D%C3%A9marrage-rapide-Param%C3%A9trage-%C3%A0-la-premi%C3%A8re-utilisation-de-Git> 

Mise sous Git d'un projet :

Ouvrir la directory du projet avec la commande "Open Folder"

.. image:: /quickstart/assets/images/git/open_folder.png
 :align: center	

Ce projet contient sa propre arborescence de fichier.

Une fois le projet ouvert, initialisation de Git en cliquant sur "Initialize Repository"

.. image:: /quickstart/assets/images/git/initialize_projet.png
 :align: center	
	
2 paramètres de Git doivent être initialisés une fois pour toute pour un utilisateur.
Ceci peut être fait dans un terminal standard ou dans un terminal ouvert dans VSC.

Ouvrir un terminal dans VSC avec la commande "New Terminal" :

.. image:: /quickstart/assets/images/git/new_terminal.png
 :align: center	


Dans le terminal utiliser la commande git config pour passer en revue les settings de Git puis positionner les 2 variables : user.name et user.email

.. code-block:: python
	
	git config --list --show-origin

	git config --global user.name "Christian Dupillier"

	config --list --show-origin
		file:C:/Users/c.dupillier/.gitconfig    user.name=Christian Dupillier

	git config --global user.email cdupillier@offset5.fr
		file:C:/Users/c.dupillier/.gitconfig    user.name=Christian Dupillier
		file:C:/Users/c.dupillier/.gitconfig    user.email=cdupillier@offset5.fr
		
A partir de là, il est possible de gérer simplement le versionnage du projet en utilisant les commandes ``stage`` et ``commit`` sur le(s) fichier(s) du projet.

Au préalable, installer l'extension <<Git Graph>> 

.. image:: /quickstart/assets/images/git/git_graph.png
 :align: center	
	
Revenir dans "Source Control" dans la barre d'activité (Activity Bar)
	
.. image:: /quickstart/assets/images/git/source_control.png
 :align: center		
	
Le fichier main.py n'est pas encore géré par Git : U (pour Unmanaged)

Cliquer sur main.py : 

.. image:: /quickstart/assets/images/git/widgets.png
 :align: center		


Passer en revue les informations disponibles sur chaque widget.

Cliquer sur Stage : +

main.py passe de Changes à Staged Changes.

Cliquer sur Commit (check) après avoir renseigné le champ Message. Par exemple: First commit.

Cliquer sur l'icone de Git Graph :

.. image:: /quickstart/assets/images/git/graph1.png
 :align: center	

La branche master qui est la branche initialisée par défaut a un premier état archivé.

On peut ensuite ajouter des fichiers et des directories et faire les mêmes opérations de stage et de commit pour archiver de nouveaux etats du projet.

On obtient ainsi un graph avec plusieurs états successifs, chacun d'entre eux étant accessible par la commande check-out.

Il est conseillé de faire régulièrement des commits lorsqu'une tache est terminée. Correction, ajout de fonctionnalité,...

A partir de chaque état marérialisé par une bulle, il est possible de dériver une ou plusieurs branches. Si les développements sur la branche sont satisfaisant, on peut alors les merger sur la branche principale master. Puis on peut détruire la branche une fois qu'elle a été mergée.

Exemple :
Une branche SQLAlchemy est créé à partir de l'état "Ajout de fichiers utiles".
Les développements avancent alternativement sur master et SQLAlchemy, master étant la branche "principale".

.. image:: /quickstart/assets/images/git/branch.git.png
 :align: center	

Lorsqu'on est satisfait par les résultat de SQLAlchemy et qu'on souhaite les intégrer dans master, on fait un merge qui vérifie qu'il n'y a pas de conflit.
Puis les développements se poursuivent.

On obtient alors un graph comme le suivant :

.. image:: /quickstart/assets/images/git/merge.png
 :align: center	

Ce qu'on peut faire seul, on peut aussi le faire à plusieurs et se synchroniser sur un dépot référence appellé remote.

On fait alors des push sur le projet remote pour déposer ses propres développements et les rendre accessibles aux autres.
Et on fait des pull afin de récupérer les développements des autres.

Les stratégies de développement possibles sont infinies suivant le nombre de développeurs et les rôles assignés à chacun.


Pour travailler sur un projet sous git dans le même système de fichier on peut cloner le projet pour travailler dessus dans son propre environnement de travail.

.. code-block:: powershell

	Clone
	git clone --local --no-hardlinks 'F:\ZZ. ECHANGE\PYTHON\SANDBOX\CDU' c:\Users\c.dupillier\Documents\myCDU
	Cloning into 'c:\Users\c.dupillier\Documents\CDU2'...
	done.

Si on continue des développements sur myCDU et qu'on déclare CDU comme remote, on obtient le graph suivant.

.. image:: /quickstart/assets/images/git/clone.png
 :align: center	

On peut ensuite pusher ces développement sur remote à condition que d'autres développements ne soient apparus sur CDU.

Exemple : master sur CDU avance.

.. image:: /quickstart/assets/images/git/remote.png
 :align: center	

Avant de pouvoir faire un push de la branche master de myCDU sur remote, il faut d'abord intégrer les nouveaux développements qui sont apparus sur la branche master remote.

On fait donc, à partir de myCDU, un pull. Dans ce cas, le pull se passe sans encombre et le merge est fait.

.. image:: /quickstart/assets/images/git/pull.png
 :align: center	

Ensuite, on peut faire un push pour synchroniser la branch master de remote et y envoyer nos développement.
Après le push, origin/master est bien au même niveau que master.

.. image:: /quickstart/assets/images/git/push.png
 :align: center	


Il est conseillé de faire régulièrement des pulls ou des fetch+rebase, pour rester sycnhronisé avec le remote. Cela permet d'être compatible avec les nouveaux développements et cela simplifie les push. Bien sû, cela nécessite parfois la résolution de conflits au moment du merge.



.. note::

		Pour éviter les problèmes de push sur le remote qui ne marche pas, explorer la création du remote avec l'option --bare. 

.. error::
		
		remote: error: refusing to update checked out branch: refs/heads/master


.. code-block::

	git init --bare