===================================
SQLAlchemy dans GraphisoftReader.py
===================================

.. |br| raw:: html

  <br /> 

Durant l'execution de la méthode ``loadData`` de la classe ``GraphisoftReader``, un **WARNING** est émis : 

.. warning::

 
  **GraphisoftReader >** __init__ : success |br|
  **GraphisoftReader >** setSQLStatementByString : success |br|
  *c:\Users\c.dupillier\Documents\BRIQUES\.venv\lib\site-packages\pandas\io\sql.py:761:* **UserWarning:** *pandas only support SQLAlchemy connectable(engine/connection) or database string URI or sqlite3 DBAPI2 connection other DBAPI2 objects are not tested, please consider using* SQLAlchemy |br|
  warnings.warn(|br|
  **GraphisoftReader >** loadData : success

Ce message est provoqué par le code suivant dans la méthode ``loadData``:

.. code-block:: python
 :linenos:
 :emphasize-lines: 2

  conn = pyodbc.connect(self.CONN_STR)
  sql_query = pandas.read_sql_query(self.sql_statement_str,conn)
  conn.close()

La méthode ``read_sql_query`` du module ``pandas`` produit ce warning à cause de la nature de l'argument ``conn``.

Pour suivre la première suggestion du warning et utiliser ``SQLAlchemy``, le code est modifié de la façon suivante:

.. code-block:: python

  def loadData():
   ...
   from sqlalchemy.engine import URL
   connection_url = URL.create("mssql+pyodbc", query={"odbc_connect": self.CONN_STR})

   from sqlalchemy import create_engine
   self.engine = create_engine(connection_url)
   sql_query = pandas.read_sql_query(self.sql_statement_str,self.engine)
   ...


De plus, d'après la doc de ``SQLAlchemy`` :

.. note::

  The typical usage of ``create_engine()`` is once per particular database URL, held globally for the lifetime of a single application process. 
	A single Engine manages many individual DBAPI connections on behalf of the process and is intended to be called upon in a concurrent fashion. 
	The Engine is not synonymous to the DBAPI connect function, which represents just one connection resource - the Engine is most efficient when created just once at the module level of an application, not per-object or per-function call.

Le code est donc modifié en déplaçant les imports en tête du fichier et ``engine`` est déclaré en variable de classe, ce qui la rend commune à toutes les instances de la classe.
Enfin le module ``pyodbc`` n'est plus importé.

.. code-block:: python
 :linenos:
 :emphasize-lines: 2,6,7,12,13

 import pandas
 #import pyodbc 
 import codecs
 import time
 import datetime
 from sqlalchemy.engine import URL
 from sqlalchemy import create_engine

 class GraphisoftReader:

  CONN_STR = 'DRIVER={SQL Server};SERVER=SRV3-OFFSET5\GRAPHISOFT;DATABASE=OFFSET5;UID=PYTHON;PWD=T:4Q:3ncMq3~/'
  connection_url = URL.create("mssql+pyodbc",query={"odbc_connect": CONN_STR})	
  engine = create_engine(connection_url)

Il ne reste alors plus que l'instruction de lecture de la query sql dans le code de ``loadData``. ::

 def loadData():
  ...
  sql_query = pandas.read_sql_query(self.sql_statement_str,self.engine)
  ...