# predictionio_bits
install a runnable Apache predictionio

Steps:
i)Download apache spark with prebuilt binaries "http://spark.apache.org/downloads.html".
e.g.  spark-2.0.1-bin-hadoop2.7.tgz and extract it.
ii)set SPARK_HOME in /etc/environment to the location where you downloaded your spark.
iii)Make sure you have mysql and java se 7 installed.
iv)Install Apache PredictionIO from source code.
 a)tar zxvf apache-predictionio-0.10.0-incubating.tar.gz
 b)cd apache-predictionio-0.10.0-incubating
 c)./make-distribution.sh
v)Go to conf/pio-env.sh
  a)chage SPARK_HOME to your SPARK_HOME path.
  b)change jdbc driver to point to mysql-jdbc.jar.
  c)comment everything related to data-source (like HDFS,postgre,Hbase)except mysql.
  d)set jdbc url,mysql username,mysql password,mysql database name.
vi)Also set PIO_HOME in /etc/environment to the location of Apache PredictionIO
vii)Start event server and mysql server
 a)you can make changes to pio-start-all or use "pio eventserver" and start mysql server.

viii)Type pio status to check if everything started properly.
viii)Download any template engine e.g MyRecoomendationEngine
 a)pio template get apache/incubator-predictionio-template-recommender MyRecommendation
 b)cd MyRecommendation
ix)Generate an AppID and Access key.
  a)pio app new MyfirstRecommendationApp.
  b)Note the access key generated.
x)Download some sample data and import to EventServer.
 a)Install Python SDK
     pip install predictionio
 b)Run the following commands in MyRecommendation directory which contains Engine.json
   1)curl https://raw.githubusercontent.com/apache/spark/master/data/mllib/sample_movielens_data.txt --create-dirs -o data/sample_movielens_data.txt
   2)python data/import_eventserver.py --access_key $ACCESS_KEY 
xi)In Engine.json set appName to your AppName.
xii)Build your engine.
 a)pio build --verbose
  1)If it fails go to build.sbt of MyRecommendation and make changes in libraryDependencies.
      1.1)"io.prediction" => "org.apache.predictionio"
      1.2)"core" => "org.apachecore.io"
   
