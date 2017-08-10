Introduction
^^^^^^^^^^^^

The goal of this tutorial is to help you install Apache Spark and have a working jupyter environment with the following kernels/features:
 - Python 3.5 with access to a local Apache Spark cluster through pyspark or findspark module
 - Scala 2.11 with access to a local Apache Spark cluster
 - Apache Toree with scala 
 
At the end of the tutorial you will have python 3.5, scala 2.11 and Apache Spark 2.1.1 installed on an Ubuntu system.

Install Spark
^^^^^^^^^^^^^

Steps to install Scala
----------------------

Since the primary language for Apache Spark is Scala we first need to install it. 

.. code:: bash

    sudo apt-get install scala

This also has the advantage of installing java 8.

To check which version of scala has been installed on your system :

.. code:: bash

    (spark)prompt:~$ scala

And you should see something close to :

.. code:: bash

   Welcome to Scala version 2.11.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_101).
   Type in expressions to have them evaluated.
   Type :help for more information.

   scala> 

To exit scala REPL simply type **:q**

Installation of Apache Spark
----------------------------

Go to `Apache Spark download page <http://spark.apache.org/downloads.html>`_

Select Spark 2.1.1 Prebuilt with hadoop 2.7 and later and download spark to your ./Downloads directory

Open a terminal

.. code:: bash

    (spark)prompt:~$ cd Downloads
    (spark)prompt:~/Downloads$ mv spark-2.1.1-bin-hadoop2.7.tgz ~/pyenv/spark/apachespark.tgz
    (spark)prompt:~/Downloads$ cd ../pyenv/apachespark.tgz
    (spark)prompt:~/pyenv/spark$ tar xvf apachespark.tgz

If everything installed smoothly, you should be able to start a spark shell : 

.. code:: bash

    (park):~/pyenv/spark$ ./spark-2.1.1-bin-hadoop2.7/bin/spark-shell

And you should see something close to :

.. code:: bash

    Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties Setting default log level to "WARN".
    To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
    17/08/09 20:17:08 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
    17/08/09 20:17:08 WARN Utils: Your hostname, goldentom-MS-7850 resolves to a loopback address: 127.0.1.1; using 192.168.1.11 instead (on interface enp2s0)
    17/08/09 20:17:08 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
    17/08/09 20:17:25 WARN ObjectStore: Version information not found in metastore. hive.metastore.schema.verification is not enabled so recording the schema version 1.2.0
    17/08/09 20:17:26 WARN ObjectStore: Failed to get database default, returning NoSuchObjectException
    17/08/09 20:17:28 WARN ObjectStore: Failed to get database global_temp, returning NoSuchObjectException
    Spark context Web UI available at http://192.168.1.11:4040
    Spark context available as 'sc' (master = local[*], app id = local-1502302629768).
    Spark session available as 'spark'.
    Welcome to
          ____              __
         / __/__  ___ _____/ /__
        _\ \/ _ \/ _ `/ __/  '_/
       /___/ .__/\_,_/_/ /_/\_\   version 2.2.0
          /_/
         
    Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_101)
    Type in expressions to have them evaluated.
    Type :help for more information.

    scala> 


If you need to exit the shell type **:q** and hit enter like in the scala REPL.


Install jupyter notebook
^^^^^^^^^^^^^^^^^^^^^^^^

The first step is to create a working virtual environment for python 3.5.
This is particularly usefull to avoid messing up the python libraries shipped with your linux distribution.

To do so open a terminal and type the following commands

.. code:: bash

    prompt:~$ sudo apt-get update
    prompt:~$ sudo apt-get install virtualenv
    
Create a virtualenv with python 3.5 to install jupyter:

.. code:: bash

    prompt:~$ virtualenv -p /usr/bin/python3.5 pyenv/spark

This installs python3.5 along with setuptools, pip and wheel
Activate the environment

.. code:: bash

    prompt:~$ source pyenv/spark/bin/activate
    (spark)prompt:~$ 
    
The prompt should be preceded by (spark). Now to check things are in good order, issue: 

.. code:: bash

    (spark)prompt:~$ which python
    /home/$username/pyenv/spark/bin/python

It's now time to install jupyter

.. code:: bash

    (spark)prompt:~$ pip install jupyter
    (spark)prompt:~$ jupyter notebook

This should launch your favorite browser and open the jupyter notebook home page.

Clicking on new will show a menu with only python 3 for notebooks

Quit your browser, go back to the terminal and type Ctrl-C to stop jupyter.



Install Apache Toree
^^^^^^^^^^^^^^^^^^^^

Introduction
------------

You must be wondering why the hell did we install jupyter?

The answer is that spark shell is certainly very good for command line users but notebooks are a lot better for interactively playing with datasets.

This is where `Apache Toree <https://github.com/apache/incubator-toree>`_ comes into play.

`Apache Toree <https://github.com/apache/incubator-toree>`_ uses the IPython protocol to provide `Apache Spark <http://spark.apache.org>`_ users with a notebook application capable of interactively communicate with a Spark cluster in Scala. Sounds great ? 

But wait there is more! Apache Toree has crazy lines and cells magics you can check in their `magic tutorial <https://github.com/apache/incubator-toree/blob/master/etc/examples/notebooks/magic-tutorial.ipynb>`_. 

This adds terrific features like being able to change language from one cell to the other: **%%SparkR** brings you to R language and SparkR environment then type **%%PySpark** and you are using Python. 

Apache Toree Installation
-------------------------

At the time of writing, installing Toree using pip would install Apache Toree version 0.1.x which unfortunately uses scala 2.10 when our Spark installation uses Scala 2.11.

Therefore we need to install Apache Toree from the dev snapshots they provide.

Open a terminal and source spark python virtualenv and issue

.. code:: bash
    
    (spark)prompt:~/$ pip install https://dist.apache.org/repos/dist/dev/incubator/toree/0.2.0/snapshots/dev1/toree-pip/toree-0.2.0.dev1.tar.gz
    (spark)prompt:~/$  jupyter toree install --user --spark_home=~/pyenv/spark/spark-2.1.1-bin-hadoop2.7 

The --user option makes jupyter install Apache Toree kernel at user level. Without this option the install requires root priviledges, which means using the linux' distribtion python's libraries, something we don't want.

You can now launch the notebook and make sure the new kernel *Apache Toree - Scala* is available. 

In jupyter home page click new and select Apache Toree - Scala. A new notebook opens.
Select the first cell and type

.. code:: scala

   println("Hello World!")

You may experience syntax highlighting issues. Refreshing the notebook with F5 should highlight the syntax correctly.

So we now have a way to interact with Apache Spark clusters in Scala/Python/R

  
Spark in Scala notebooks - an alternative to Toree
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`Jupyter-Scala <https://github.com/jupyter-scala/jupyter-scala>`_ is a Scala kernel for jupyter wrote by `Alexandre Archambault <https://github.com/alexarchambault>`_ 

Spark support is provided by importing Apache Spark libraries on the fly. So the first time you import things may take some time but after that all works like a breeze.

Obviously Jupyter-Scala is limited to Scala and does not offer the spaecial features provided by Apache Toree.


Scala kernel in Jupyter
-----------------------

The kernel for scala notebook can be found `here <https://github.com/jupyter-scala/jupyter-scala>`_

Open a terminal and activate *spark* virtualenv

.. code:: bash

    (spark):$ cd pyenv/spark
    (spark):~/pyenv/spark$ git clone --recursive https://github.com/jupyter-scala/jupyter-scala.git
    (spark):~/pyenv/spark$ cd jupyter-scala
    (spark):~/pyenv/spark/jupyter-scala$ ./jupyter-scala

This will download lots of stuff from maven repos. 

You can now launch jupyter notebook and select scala kernel in the New menu. 


Make Apache Spark available in Scala notebooks
----------------------------------------------
    
If you want to use jupyter-scala to communicate with your cluster you have to download the spark and hadoop source files !!

To do so open jupyter notebook and create a new scala notebook.

In the first cell type : (this is directly inspired by jupyter-scala readme file on github)

.. code:: scala
 
    import $exclude.`org.slf4j:slf4j-log4j12`, $ivy.`org.slf4j:slf4j-nop:1.7.21`
    import $profile.`hadoop-2.7`
    import $ivy.`org.apache.spark::spark-sql:2.1.1`
    import $ivy.`org.apache.hadoop:hadoop-aws:2.7.4`
    import $ivy.`org.jupyter-scala::spark0.4.2`


Depending on your network connection this may take some time but now that we have a local copy the import statements will be a lot faster.

Now to start a spark session type this in the following cell : 

.. code:: scala
   
    import org.apache.spark._
    import org.apache.spark.sql._
    import jupyter.spark.session._

    val sparkSession = JupyterSparkSession.builder() 
      .jupyter()
      .config("spark.master", "local")
      .appName("notebook")
      .getOrCreate()

    val sc = sparkSession.sparkContext


the *.config* part let's you specify the sort of cluster you want to connect to.


Using Apache Spark in Python notebooks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Scala is the native language to use Spark but lot's for data scientist would rather use python to communicate with the cluster.

Here we have 2 options to do this insive Jupyter notebooks.

1st option - make pyspark open jupyter
--------------------------------------

update your .bashrc with the following lines

.. code:: bash

    export SPARK_HOME=/type/your/spark/directory
    export PATH=$SPARK_HOME/bin:$PATH
    export PYSPARK_DRIVER_PYTHON=jupyter
    export PYSPARK_DRIVER_PYTHON_OPTS='notebook'

Now activate your spark virtualenv and type pypsark at the command prompt. A jupyter notebook should open in your favorite web browser.

2nd option - use findspark python module
----------------------------------------

As an alternative to pyspark you can install *findspark* python module. This option still requires you to export SPARK_HOME.

.. code:: bash

    source ~/pyenv/spark/bin/activate
    (spark):$ pip install findspark
    (spark):$ jupyter notebook

Choose a python 3 notebook and type the following command in the first cell:

.. code:: python

    import findspark
    findspark.init() # Find spark using SPARK_HOME and link pyspark python module
    import pyspark
    # Get the spark context
    sc = pyspark.SparkContext(appName='Pi')
    # Do not forget to stop the SPARKCONTEXT at the end of the notebook
    sc.stop()


