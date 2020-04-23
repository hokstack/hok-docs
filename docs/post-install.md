# Post Installation


## Accessing Hadoop Cluster


#### Accessing from cli  `edgenode`

To connect get the edgenode shell follow the below process.

* Run below command on the cluster where you have `kubectl` access.
  
```
  $ kubectl exec -it edgenode-0 bash
```
* Change user to hdfs user, its just for initial later you can create own user and access from it.
  
```
  $ su - hdfs
```
* list hdfs files you will see default directories created by the cluster during the installation.
  
```
  $ hdfs dfs -ls
```

#### Accessing from GUI portals  `ambari` `nodemanager` etc.

To access the cluster UI portals you need to configure your web browser, you can use any browser prefable `Firefox` as nowaddays `Chrome` is managed by orgnisation.

* Get the any kubernetes node IP address but
```
  $ kubeclt get nodes -o wide
```
* Open firefox and go to Preferences.
* Look for Network Settings.
* Add the details as shown below.


## Accessing Hive Metastore

 * Connet to `edgenode`
 * Change user to `hdfs`
 * Execute `hive command`

## Running Sample Spark Job

#### To run sample spark job to test the cluster follow the below process.

* Connect to the `egdenode`
* Change user to `spark`
* Natigate to spark client folder
* Submit the spark job.


## Loading test data in Hive

Login to your datanode, create a Hadoop home directory for "admin", and put some sample logfiles into it

```
localhost:randy$ docker exec -it compose_master0.dev_1 bash
[root@dn0 /]# su hdfs
bash-4.2$ hdfs dfs -mkdir /user/admin
bash-4.2$ hdfs dfs -chown admin /user/admin
```

Run the following Hive queries to create raw and derived wordcount tables from the logfiles:
```
[root@dn0 /]# hive
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