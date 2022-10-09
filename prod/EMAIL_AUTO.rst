==========
EMAIL_AUTO
==========

.. contents:: Table des Matières

COMMANDES_BLOQUEE_COMMERCIAUX
=============================		
	
Ce programme permet d'envoyer un mail récapitulatif à tous les commerciaux qui sont en charge d'une ou plusieurs commandes bloquées
Son objectif est de prévenir les commandes bloquées.

______Architecture______
-------------------------- 

EMAIL_AUTO ::

	├─COMMANDES_BLOQUEE_COMMERCIAUX
	    |
	    ├── Main.py	#FICHIER PRINCIPAL DU PROGRAMME QUI PERMET LE LANCEMENT DE CELUI-CI
	    └── Logs	#REPERTOIRE CONTENANT TOUS LES LOGS
	│
	├─Sample
	   ├── graphisoft_tools.py  #CONTIENT TOUTES LES METHODES COMMUNIQUANTS AVEC GRAPHISOFT
	   ├── offset5_tools.py	    #CONTIENT TOUTES LES METHODES POUR ENVOYER LES MAILS ET ECRIRE LES LOGS.
	   ├── Email.py             #CONTIENT LES MÉTHODES POUR COMPILLER LES MAILS AVANT DE LES ENVOYER.
	   ├── SQL.py               #CONTIENT LA FONTION POUR EXECUTER UNE REQUETE SQL.
	   ├── Template.py          #PERMET LA LECTURE DES TEMPLATES ET LE REMPLACEMENT DES BALISES DANS LE TEMPLATE.
	   └── *.sql                # toutes les requêtes sql dont le programme à besoins pour fonctionner.
	|
	├─Tests            #REPERTOIRE CONTENANT TOUTS LES FICHIERS TESTS
	├─Email_Templates  #REPERTOIRE CONTENANT TOUTS LES TEMPLATES DE MAILS HTML

______Explication des méthodes______
------------------------------------
offset5_tools.py:
	printLogCreateFiles(message)
		Permet de créer un log journalier dans le fichier log

	printLogEnd(message)
		Permet d'ajouter du contenu à la fin du log journalier précédemment crée

	sendHTMLEmail(subject_text, message_text, from_text, to_text)
		Permet d'envoyer un mail via ``SendInBlue``

SQL.py:
	SQLrequest(db, query)
		Connecte à une base de donnée, retourne le concur, une row contenant le connector en position 0 et le cursor en position 1

Template.py:
	TempReplaceBody(row, html_body)
		Permet de remplacer les balises du template par des informations venant d'une row[]

Email.py:
	createMail(row, concur)
		Compile un mail pour chaques commerciaux


______Fonctionnement du programme______
---------------------------------------

- Le programme commence par aller chercher les informations des commandes bloquées dans graphisoft (cf:SQL)
- il fait ensuite une lecture du template de mail,
- ensuite, tant qu'il y a les lignes dans la réponse de la requète, il remplace les balises dans le template afin de compléter les informations du mail.
- Pour finir, le programme va remplacer la bannière avant d'envoyer finalement le mail à la comptabilité.


______SQL______
--------------------------

.. code-block:: sql

    SELECT TOP (100) 
    PERCENT RIGHT(CMD.Numero, 10) AS CMD_NUMERO, 
    CMD.Reference AS CMD_REFERENCE, 
    SOC.RaiSocTri AS SOC_RAISOCTRI, 
    COMM.Nom + ' ' + COMM.Prenom AS COMM_NOM_PRENOM, 
    COMM_EMAIL.Mail AS COMM_EMAIL, 
    CMD.QteComm AS CMD_QTE, 
    CMD.DteDoss AS CMD_DTEDOSS, 
    CMD.DteBat AS CMD_DTEBAT, 
    CMD.DteLivPrev AS CMD_DTELIVPREV, 
    CMD.DteReceptElem AS CMD_DTERECEPTELEM, 
    SOC.SIRET AS SOC_SIRET, 
    SOCUTIL.Compte AS SOC_COMPTE, 
    CMD.PrxVteReel AS CMD_PRXVTEREEL, 
    COMM.Mobile AS COMM_TEL 

    FROM [Offset5].[dbo].[COMMANDES] AS CMD 

    LEFT OUTER JOIN  [Offset5].[dbo].[SOCIETES] AS SOC ON SOC.ID = CMD.ID_SOCIETE 
    LEFT OUTER JOIN  [Offset5].[dbo].[SOCIETES_SOCUTIL] AS SOCUTIL ON SOC.ID = SOCUTIL.ID_SOCIETE 
    LEFT OUTER JOIN  [Offset5].[dbo].[PERSONNES] AS COMM ON COMM.ID = CMD.ID_COMMERCIAL 
    LEFT OUTER JOIN  [Offset5].[dbo].[PERSONNES_MAIL] AS COMM_EMAIL ON COMM_EMAIL.ID_PERSONNE = CMD.ID_COMMERCIAL 

    WHERE (SOCUTIL.NouvBL = 0) -- ou la commande est bloquée
    AND (CMD.DteLivPrev > GETDATE()) 
    AND (CMD.DteDoss > DATEADD(day, - 720, GETDATE())) 
    AND (CMD.DteLivPrev< DATEADD(day, 720, GETDATE())) 
    AND CMD.DteLivPrev < GETDATE() + 7 

    ORDER BY CMD_DTELIVPREV



COMMANDES_BLOQUEE_COMPTA
=============================

Ce programme permet d'envoyer un mail récapitulatif a la comptabilité contenant une liste des commandes bloquées.
Son objectif est de prévenir les commandes bloquées.

______Architecture______
--------------------------

EMAIL_AUTO ::

	├─COMMANDES_BLOQUEE_COMPTA
	    |
	    ├── Main.py	#FICHIER PRINCIPAL DU PROGRAMME QUI PERMET LE LANCEMENT DE CELUI-CI
	    └── Logs	#REPERTOIRE CONTENANT TOUS LES LOGS
	│
	├─Sample
	   ├── graphisoft_tools.py  #CONTIENT TOUTES LES METHODES COMMUNIQUANTS AVEC GRAPHISOFT
	   ├── offset5_tools.py	    #CONTIENT TOUTES LES METHODES POUR ENVOYER LES MAILS ET ECRIRE LES LOGS.
	   ├── Email.py             #CONTIENT LES MÉTHODES POUR COMPILLER LES MAILS AVANT DE LES ENVOYER.
	   ├── SQL.py               #CONTIENT LA FONTION POUR EXECUTER UNE REQUETE SQL.
	   ├── Template.py          #PERMET LA LECTURE DES TEMPLATES ET LE REMPLACEMENT DES BALISES DANS LE TEMPLATE.
	   └── *.sql                # toutes les requêtes sql dont le programme à besoins pour fonctionner.
	|
	├─Tests            #REPERTOIRE CONTENANT TOUTS LES FICHIERS TESTS
	├─Email_Templates  #REPERTOIRE CONTENANT TOUTS LES TEMPLATES DE MAILS HTML

______Explication des méthodes______
----------------------------------------------------

offset5_tools.py:
	printLogCreateFiles(message)
		Permet de créer un log journalier dans le fichier log

	printLogEnd(message)
		Permet d'ajouter du contenu à la fin du log journalier précédemment créé.

	sendHTMLEmail(subject_text, message_text, from_text, to_text)
		Permet d'envoyer un mail via ``SendInBlue``

SQL.py:
	SQLrequest(db, query)
		Connecte à une base de donnée, retourne le concur, une row contenant le connector en position 0 et le cursor en position 1

Template.py:
	TempReplaceBody(row, html_body)
		Permet de remplacer les balises du template par des informations venant d'une row[]

Email.py:
	createMail(row, concur)
		Compile un mail pour chaques commerciaux

______Fonctionnement du programme______
----------------------------------------------------

- Le programme commence par aller chercher les informations des commandes bloqués dans graphisoft (cf:SQL)
- il fait ensuite une lecture du template de mail,
- ensuite, tant qu'il y a les lignes dans la réponse de al requéte, il remplace les balises dans le template affin de compléter les informations du mail.
- Pour finir, le programme va remplacer la banniére avant d'envoyer finalement le mail é la comptabilité.

______SQL______
--------------------------


.. code-block:: sql

    SELECT TOP (100) 
    PERCENT RIGHT(CMD.Numero, 10) AS CMD_NUMERO, 
    CMD.Reference AS CMD_REFERENCE, 
    SOC.RaiSocTri AS SOC_RAISOCTRI, 
    COMM.Nom + ' ' + COMM.Prenom AS COMM_NOM_PRENOM, 
    COMM_EMAIL.Mail AS COMM_EMAIL, 
    CMD.QteComm AS CMD_QTE, 
    CMD.DteDoss AS CMD_DTEDOSS, 
    CMD.DteBat AS CMD_DTEBAT, 
    CMD.DteLivPrev AS CMD_DTELIVPREV, 
    CMD.DteReceptElem AS CMD_DTERECEPTELEM, 
    SOC.SIRET AS SOC_SIRET, 
    SOCUTIL.Compte AS SOC_COMPTE, 
    CMD.PrxVteReel AS CMD_PRXVTEREEL, 
    COMM.Mobile AS COMM_TEL 

    FROM [Offset5].[dbo].[COMMANDES] AS CMD 

    LEFT OUTER JOIN  [Offset5].[dbo].[SOCIETES] AS SOC ON SOC.ID = CMD.ID_SOCIETE 
    LEFT OUTER JOIN  [Offset5].[dbo].[SOCIETES_SOCUTIL] AS SOCUTIL ON SOC.ID = SOCUTIL.ID_SOCIETE 
    LEFT OUTER JOIN  [Offset5].[dbo].[PERSONNES] AS COMM ON COMM.ID = CMD.ID_COMMERCIAL 
    LEFT OUTER JOIN  [Offset5].[dbo].[PERSONNES_MAIL] AS COMM_EMAIL ON COMM_EMAIL.ID_PERSONNE = CMD.ID_COMMERCIAL 

    WHERE (SOCUTIL.NouvBL = 0) -- ou la commande est bloqué
    AND (CMD.DteLivPrev > GETDATE()) 
    AND (CMD.DteDoss > DATEADD(day, - 720, GETDATE())) 
    AND (CMD.DteLivPrev< DATEADD(day, 720, GETDATE())) 
    AND CMD.DteLivPrev < GETDATE() + 7 

    ORDER BY CMD_DTELIVPREV

VERIF_DESTINATION_DEVIS
=============================

Ce programme permet d'envoyer un mail aux deviseurs qui n'ont pas correctement renseigné l'adresse de livraison d'une commande pour qu'ils corrigent l'adresse.

______Architecture______
--------------------------

EMAIL_AUTO ::

	├─VERIF_DESTINATION_DEVIS
	    |
	    ├── Main.py	#FICHIER PRINCIPAL DU PROGRAMME QUI PERMET LE LANCEMENT DE CELUI-CI
	    └── Logs	#REPERTOIRE CONTENANT TOUS LES LOGS
	│
	├─Sample
	   ├── graphisoft_tools.py  #CONTIENT TOUTES LES METHODES COMMUNIQUANTS AVEC GRAPHISOFT
	   ├── offset5_tools.py	    #CONTIENT TOUTES LES METHODES POUR ENVOYER LES MAILS ET ECRIRE LES LOGS.
	   ├── Email.py             #CONTIENT LES MÉTHODES POUR COMPILLER LES MAILS AVANT DE LES ENVOYER.
	   ├── SQL.py               #CONTIENT LA FONTION POUR EXECUTER UNE REQUETE SQL.
	   ├── Template.py          #PERMET LA LECTURE DES TEMPLATES ET LE REMPLACEMENT DES BALISES DANS LE TEMPLATE.
	   └── *.sql                # toutes les requêtes sql dont le programme à besoins pour fonctionner.
	|
	├─Tests            #REPERTOIRE CONTENANT TOUTS LES FICHIERS TESTS
	├─Email_Templates  #REPERTOIRE CONTENANT TOUTS LES TEMPLATES DE MAILS HTML

______Explication des méthodes______
----------------------------------------------------
offset5_tools.py:
	printLogCreateFiles(message)
		Permet de créer un log journalier dans le fichier log

	printLogEnd(message)
		Permet d'ajouter du contenu é la fin du log journalier précédemment crée

	sendHTMLEmail(subject_text, message_text, from_text, to_text)
		Permet d'envoyer un mail via ``SendInBlue``

SQL.py:
	SQLrequest(db, query)
		Connecte à une base de donnée, retourne le concur, une row contenant le connector en position 0 et le cursor en position 1

Template.py:
	TempReplaceBody(row, html_body)
		Permet de remplacer les balises du template par des informations venant d'une row[]

Email.py:
	createMail(row, concur)
		Compile un mail pour chaques commerciaux

______Fonctionnement du programme______
----------------------------------------------------

- Le programme commence par aller chercher les informations des commandes bloqués dans graphisoft (cf:SQL)
- il fait ensuite une lecture du template de mail,
- ensuite, tant qu'il y a les lignes dans la réponse de al requéte, il remplace les balises dans le template affin de compléter les informations du mail.
- Pour finir, le programme va remplacer la banniére avant d'envoyer finalement le mail é la comptabilité.

______SQL______
--------------------------

.. code-block:: sql

    SELECT TOP (100) 
    PERCENT RIGHT(CMD.Numero, 10) AS CMD_NUMERO, 
    CMD.Reference AS CMD_REFERENCE, 
    SOC.RaiSocTri AS SOC_RAISOCTRI, 
    COMM.Nom + ' ' + COMM.Prenom AS COMM_NOM_PRENOM, 
    COMM_EMAIL.Mail AS COMM_EMAIL, 
    CMD.QteComm AS CMD_QTE, 
    CMD.DteDoss AS CMD_DTEDOSS, 
    CMD.DteBat AS CMD_DTEBAT, 
    CMD.DteLivPrev AS CMD_DTELIVPREV, 
    CMD.DteReceptElem AS CMD_DTERECEPTELEM, 
    SOC.SIRET AS SOC_SIRET, 
    SOCUTIL.Compte AS SOC_COMPTE, 
    CMD.PrxVteReel AS CMD_PRXVTEREEL, 
    COMM.Mobile AS COMM_TEL 

    FROM [Offset5].[dbo].[COMMANDES] AS CMD 

    LEFT OUTER JOIN  [Offset5].[dbo].[SOCIETES] AS SOC ON SOC.ID = CMD.ID_SOCIETE 
    LEFT OUTER JOIN  [Offset5].[dbo].[SOCIETES_SOCUTIL] AS SOCUTIL ON SOC.ID = SOCUTIL.ID_SOCIETE 
    LEFT OUTER JOIN  [Offset5].[dbo].[PERSONNES] AS COMM ON COMM.ID = CMD.ID_COMMERCIAL 
    LEFT OUTER JOIN  [Offset5].[dbo].[PERSONNES_MAIL] AS COMM_EMAIL ON COMM_EMAIL.ID_PERSONNE = CMD.ID_COMMERCIAL 

    WHERE (SOCUTIL.NouvBL = 0) -- ou la commande est bloqué
    AND (CMD.DteLivPrev > GETDATE()) 
    AND (CMD.DteDoss > DATEADD(day, - 720, GETDATE())) 
    AND (CMD.DteLivPrev< DATEADD(day, 720, GETDATE())) 
    AND CMD.DteLivPrev < GETDATE() + 7 

    ORDER BY CMD_DTELIVPREV

