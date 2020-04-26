# Securing cluster using Kerberos

## Pre requisite

## Kerberos Wizard
* On ambari console navigate to Admin &rarr; Kerberos

<p align="left">
  <img src="../_images/1-kerb.png">
</p>

* Select exiting MIT KDC and tick on all the prerequisites

<p align="left">
  <img src="../_images/2-kerb.png">
</p>

* Provide the KDC server containers details as follows make sure remove tick box in `Manage Kerberos client krb5.conf`

<p align="left">
  <img src="../_images/3-kerb.png">
</p>

* Click next to proceed
<p align="left">
  <img src="../_images/4-kerb.png">
</p>

* By now we have got kerberos client configure and tested on all the nodes. Confirm the configuraion and click next to proceed.
<p align="left">
  <img src="../_images/5-kerb.png">
</p>

* Onece all the services stopped click next to proceed
<p align="left">
  <img src="../_images/6-kerb.png">
</p>

* Click next to kerbrise the cluster
  
<p align="left">
  <img src="../_images/7-kerb.png">
</p>

* Wait for all the services started
  
<p align="left">
  <img src="../_images/8-kerb.png">
</p>

* Once all the services stared click complete
  
<p align="left">
  <img src="../_images/9-kerb.png">
</p>

* Once the completion the Kerberos configuration will look as follow
  
<p align="left">
  <img src="../_images/10-kerb.png">
</p>


## Creating User

*  Connect to the `kdcserver` using following command.
  
````
  $ kubectl exec -it kdcserver-0 bash
````
## Generating Keytabs

Post kerberisation we need create users and create keytabs in in KDC and use them to access the Hadoop cluster.

* Login to KDC server
```
$ kubectl exec -it kdcserver-0 bash
```
* Use [addUser.sh](https://github.com/hokstack/hok-helm/tree/master/scripts/addUser.sh)
* Update array to generate user
* This will create users in KDC and HDFS.


## Accessing Secure Cluster
* Once users are created share their `.keytab` file with them
* To generate the ke using keytab use following command.

```
$ kinit -kt username.keytab
```

* To list the keys use following command.

```
$ klist
```

* Once you have valid key entry use access hadoop cluster.
  
