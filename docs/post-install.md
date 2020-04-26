# Post Installation


## Accessing Hadoop Cluster


#### Accessing from cli  `edgenode`

To connect get the edgenode shell follow the below process.

* Run below command on the cluster where you have `kubectl` access.
  
```
  $ kubectl exec -it edgenode-0 bash
  [root@edgenode-0 /]# 
```
* Change user to hdfs user, its just for initial later you can create own user and access from it.
  
```
  [root@edgenode-0 /]# su - hdfs
  -bash-4.1$ 
```
* list hdfs files you will see default directories created by the cluster during the installation.
  
```
  -bash-4.1$ hdfs dfs -ls /
  Found 10 items
  drwxrwxrwx   - yarn   hadoop          0 2020-04-25 16:27 /app-logs
  drwxr-xr-x   - hdfs   hdfs            0 2020-04-25 16:39 /apps
  drwxr-xr-x   - yarn   hadoop          0 2020-04-25 16:27 /ats
  drwxr-xr-x   - hdfs   hdfs            0 2020-04-25 16:28 /hdp
  drwxr-xr-x   - mapred hdfs            0 2020-04-25 16:27 /mapred
  drwxrwxrwx   - mapred hadoop          0 2020-04-25 16:42 /mr-history
  drwxrwxrwx   - spark  hadoop          0 2020-04-25 16:55 /spark-history
  drwxrwxrwx   - spark  hadoop          0 2020-04-25 16:55 /spark2-history
  drwxrwxrwx   - hdfs   hdfs            0 2020-04-25 16:43 /tmp
  drwxr-xr-x   - hdfs   hdfs            0 2020-04-25 16:40 /user

```

#### Accessing from GUI portals  `ambari` `nodemanager` etc.

To access the cluster UI portals you need to configure your web browser, you can use any browser prefable `Firefox` as nowaddays `Chrome` is managed by orgnisation.

* Get the any kubernetes node IP address but
```
  $ kubectl get nodes -o wide
  NAME  STATUS   ROLES    AGE    VERSION  INTERNAL-IP      EXTERNAL-IP   
  node1 Ready    <none>   111m   v1.14.9  192.168.43.151   3.16.51.175   
  node2 Ready    <none>   111m   v1.14.9  192.168.89.231   3.23.97.43 
  bash-3.2$ 

```
* Open firefox and go to Preferences.
* Look for Network Settings.
* Add the details as shown below.

<p align="left">
  <img width="500" height="500" src="../_images/sock5-settings.png">
</p>

## Accessing Hive Metastore

 * Connet to `edgenode`
```
  $ kubectl exec -it edgenode-0 bash
  [root@edgenode-0 /]# 
```
 * Change user to `hive`
```
  [root@edgenode-0 /]# su - hive
  -bash-4.1$ 
```
 * Execute `hive command`
```
  -bash-4.1$ hive
  log4j:WARN No such property [maxFileSize] in org.apache.log4j.DailyRollingFileAppender.

  Logging initialized using configuration in file:/etc/hive/2.6.4.0-91/0/hive-log4j.properties
  hive> show databases;
  OK
  default
  Time taken: 1.799 seconds, Fetched: 1 row(s)
  hive> 
```

## Running Sample Spark Job

#### To run sample spark job to test the cluster follow the below process.

* Connect to the `egdenode`
```
  $ kubectl exec -it edgenode-0 bash
  [root@edgenode-0 /]# 
```
* Change user to `spark`
```
  [root@edgenode-0 /]# su - spark
  -bash-4.1$ 
```
* Natigate to spark client folder
```
-bash-4.1$ cd /usr/hdp/current/spark2-client
-bash-4.1$ 
```
* Submit the spark job.

```
-bash-4.1$ ./bin/spark-submit --class org.apache.spark.examples.SparkPi \
    --master yarn-client \
    --num-executors 1 \
    --driver-memory 512m \
    --executor-memory 512m \
    --executor-cores 1 \
    examples/jars/spark-examples*.jar 10

```
* Job output
```
20/04/25 17:37:48 INFO DAGScheduler: Job 0 finished: reduce at SparkPi.scala:38, took 1.688762 s
Pi is roughly 3.1426031426031424

```

## Loading test data in Hive

* Login to your datanode, create a Hadoop home directory for "admin", and put some sample logfiles into it

```
  [root@edgenode-0 /]# su - hive
  -bash-4.1$ 
  -bash-4.1$ su hdfs
  -bash-4.1$ hdfs dfs -mkdir /user/admin
  -bash-4.1$ hdfs dfs -chown admin /user/admin
```

* Run the following Hive queries to create raw and derived wordcount tables from the logfiles:
```
  -bash-4.1$ hive
  hive> create table loglines (line string);
  hive> load data local inpath '/var/log/ambari-agent/' overwrite into loglines;
  hive> create table words as
  select word, count(*) as count
  from (
    select
      explode(split(line, " ")) as word
    from loglines
  ) a
  group by word;
```
* More examples can be found here [Running Apache Spark Application](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.0/running-spark-applications/content/running_sample_spark_2_x_applications.html)