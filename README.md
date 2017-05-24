# fh-openshift-templates

## MBaaS Overview 

The MBaaS (Mobile Backend As A Service) for the RHMAP self managed deployment on OpenShift is made up of 2 different templates

- 1 Node persistent template - used for POC and non production type environments
- 3 Node persistent template - used for production type environments

The 1 node template creates a single mongodb persistent pod and one replica of each of the components that make up the mbaas, i.e fh-mbaas, fh-messaging, fh-metrics, fh-statsd. The nagios pod is used to monitor the resource usage.

As mentioned the 3 node mbaas is strongly recommended for production type environments, it makes use of 3 mongodb pods that get deployed on labeled nodes (this is to ensure that the mongodb pods are not deployed on the same nodes), and 3 replicas of fh-mbaas, fh-messaging and fh-metrics with fh-statsd and nagios set for only one replica.

A simple example on labeling the nodes

```bash

oc label node mbaas-1 type=mbaas
oc label node mbaas-2 type=mbaas
oc label node mbaas-3 type=mbaas

oc label node mbaas-1 mbaas_id=mbaas1
oc label node mbaas-2 mbaas_id=mbaas2
oc label node mbaas-3 mbaas_id=mbaas3

```

### 1 Node persistent MBaaS

RHMAP 1-Node MBaaS for OpenShift will require the following resources outlined in the table below at a minimum:

- **Prerequisites**
    - OpenShift project created 
    - A 25G PV (persistent volume) needs to be available
    - Acces to this repo (i.e clone the repository)



	|  Description                               | Parameter name               | Default value           | Fail/warning/recommended             |
	|--------------------------------------------|------------------------------|-------------------------|--------------------------------------|
	| Min number of CPUs                         | `min_required_vCPUS`         | 2                       | fail only in strict_mode             |
	| Min system memory per node (in MB)         | `required_mem_mb_threshold`  | 7000                    | fail only in strict_mode             |
	| Min total free memory of all nodes (in KB) | `warning_kb_value`           | 4000000                 | warning                              |
	| Number of PVs with 50 GB storage           | `required_50_pv`             | 0                       | warning                              |
	| Number of PVs with 25 GB storage           | `required_25_pv`             | 1                       | warning                              |
	| Number of PVs with  5 GB storage           | `required_5_pv`              | 0                       | warning                              |
	| Number of PVs with  1 GB storage           | `required_1_pv`              | 1                       | warning                              |



- **To deploy the MBaaS on OpenShift execute the following command**

  ```bash
  
  cd /directory-to-fh-openshift-templates
  oc new-app -f fh-mbaas-template-1node.json 

  --> Deploying template "1node-mbaas/fh-mbaas" for "fh-mbaas-template-1node.json" to project 1node-mbaas

     fh-mbaas
     ---------
     Red Hat Mobile Backend as a Service template


     * With parameters:
        * FH_CLUSTER=development
        * FH_MBAASID=default
        * MONGODB_FHMBAAS_USER=mbaas-user
        * MONGODB_FHMBAAS_PASSWORD=jiFgyfMQ86mwFNTlJOiD3GV4SqYmhwsAGppy7Ai4 # generated
        * FHMBAAS_KEY=GYLAB3TsNCeJNxN004sPksCcmXvxsHG8mYYjloeb # generated
        * MONGODB_KEYFILE_VALUE=D5KvFaVKIjTFH85dNO3V4IHr6V4UuoY3RmKIjsEj3emfYHTb6QW21J8RucQdO0FKMHAkbM5UFxwccpdw5DWXASwkAHRBgg1VT8RBHh1Uolv1aRbUUA3lTnKW3qkWoyboJhk7fYw5eWPDtrWSHNBTJjq2qY4tbbKVuDnl7VLehka1p1YTvxJxWQy6OeyECqmsff18Pg8OybqFdPaQXDtku36St0oHYNe4Kuferowt3KcOtebOEWioSVJ5IkRBa66 # generated
        * MONGODB_ADMIN_PASSWORD=F213Gk2KdRMxXqKawo2Q4VWKcw8NS11tDqXDV1pW # generated
        * MONGODB_FORM_PASSWORD=ttvN2BVpRfoBtt7w0dMhq47WYQYIlqwpQXgfnp5K # generated
        * MONGODB_FHREPORTING_USER=reporting-user
        * MONGODB_FHREPORTING_PASSWORD=KFUimKrUBVEHqc2Se7tMfMP5vb0k6xMaGmkG41qG # generated
        * FH_MBAAS_IMAGE=docker.io/feedhenry/fh-mbaas
        * FH_MBAAS_IMAGE_VERSION=5.4.4-1333
        * FH_MESSAGING_IMAGE=docker.io/feedhenry/fh-messaging
        * FH_MESSAGING_IMAGE_VERSION=3.0.7-488
        * FH_METRICS_IMAGE=docker.io/feedhenry/fh-metrics
        * FH_METRICS_IMAGE_VERSION=3.0.6-488
        * FH_STATSD_IMAGE=docker.io/feedhenry/fh-statsd
        * FH_STATSD_IMAGE_VERSION=2.0.4-127
        * MONGODB_IMAGE=docker.io/rhmap/mongodb
        * MONGODB_IMAGE_VERSION=centos-3.2-44
        * FH_MESSAGING_API_KEY=eNdhKQesCSFBHdlrPO3E7TTgAe1SnuAYXuaty1aQ # generated
        * FH_METRICS_API_KEY=8Srnf70QicBJkpU7noFGw7rEHHenSJo3TvvbW1Q7 # generated
        * MBAAS_ROUTER_DNS=
        * ENDPOINT_COUNT=1
        * FH_MBAAS_REPLICAS=1
        * FH_MESSAGING_REPLICAS=1
        * FH_METRICS_REPLICAS=1
        * FH_STATSD_API_KEY=su4oWH378DvNuBD2thytes0iNtBK1InSxfNWB83s # generated
        * FH_DEFAULT_LOG_LEVEL=info
        * MONGODB_PVC_SIZE=25Gi
        * SMTP_SERVER=localhost
        * SMTP_USERNAME=username
        * SMTP_PASSWORD=password
        * SMTP_TLS=auto
        * SMTP_FROM_ADDRESS=admin@example.com
        * RHMAP_ADMIN_EMAIL=root@localhost
        * NAGIOS_USER=nagiosadmin
        * NAGIOS_PASSWORD=BWCdcyNGdc # generated
        * RHMAP_ROUTER_DNS=localhost
        * RHMAP_HOSTGROUPS=mbaas
        * NAGIOS_IMAGE=docker.io/rhmap/nagios4
        * NAGIOS_IMAGE_VERSION=4.0.8-146
        * Proxy Url=

  --> Creating resources ...
    configmap "fh-mbaas-info" created
    configmap "node-proxy" created
    service "fh-mbaas-service" created
    service "fh-messaging-service" created
    service "fh-metrics-service" created
    service "fh-statsd-service" created
    service "mongodb-1" created
    service "nagios" created
    serviceaccount "nagios" created
    rolebinding "nagiosadmin" created
    persistentvolumeclaim "nagios-claim-1" created
    deploymentconfig "fh-mbaas" created
    deploymentconfig "fh-messaging" created
    deploymentconfig "fh-metrics" created
    deploymentconfig "fh-statsd" created
    deploymentconfig "nagios" created
    persistentvolumeclaim "mongodb-claim-1" created
    deploymentconfig "mongodb-1" created
    route "mbaas" created
    route "nagios" created
  --> Success
    Run 'oc status' to view your app.
  

  ``` 
- **Monitor the deploy on OpenShift**

  ```bash
  
  oc get pods
  NAME                   READY     STATUS    RESTARTS   AGE
  fh-mbaas-1-vrzfm       1/1       Running   4          7m
  fh-messaging-1-brldn   1/1       Running   3          6m
  fh-metrics-1-bxnwk     1/1       Running   4          7m
  fh-statsd-1-wx4ck      1/1       Running   0          6m
  mongodb-1-1-ff6vp      1/1       Running   0          5m
  nagios-1-704pp         1/1       Running   0          6m
  

  ```

- **Check Nagios for all health endpoints**

    - In the web console navigate to Applications -> Pods
    - Click on the nagios-x-xxx link
    - Navigate to the Environments tab
    - Copy the nagios password
    - Navigate to Applications route
    - Click on the nagios route
    - Enter the credentials (user is nagiosadmin) and use the copied password



### 3 Node persistent MBaaS

RHMAP 3-Node MBaaS for OpenShift will require the following resources outlined in the table below at a minimum:

- **Prerequisites**
    - You must have at least 3 hosts in the inventory under `mbaas` group.
    - The 3 nodes need to be labeled `mbaas_id=mbaas-1,mbaas_id=mbaas-2 and mbaas-id=mbaas-3` respectively - refer to the OpenShift documentation for labeling nodes
    - 3 25G PV (persistent volumes) created and available
    - Installation of RHMAP 3-Node MBaaS in production will require the following resources outlined in the table below at a minimum:
    - Acces to this repo (i.e clone the repository)



	|  Description                               | Parameter name               | Default value           | Fail/warning/recommended             |
	|--------------------------------------------|------------------------------|-------------------------|--------------------------------------|
	| Min number of CPUs                         | `min_required_vCPUS`         | 2                       | fail only in strict_mode             |
	| Min system memory per node (in MB)         | `required_mem_mb_threshold`  | 7000                    | fail only in strict_mode             |
	| Min total free memory of all nodes (in KB) | `warning_kb_value`           | 4000000                 | warning                              |
	| Number of PVs with 50 GB storage           | `required_50_pv`             | 3                       | warning                              |
	| Number of PVs with 25 GB storage           | `required_25_pv`             | 0                       | warning                              |
	| Number of PVs with  5 GB storage           | `required_5_pv`              | 0                       | warning                              |
	| Number of PVs with  1 GB storage           | `required_1_pv`              | 1                       | warning                              |



- **To deploy the 3 node MBaaS on OpenShift execute the following command**

  ```bash
  
  cd /directory-to-fh-openshift-templates
  oc new-app -f fh-mbaas-template-3node.json 

  --> Deploying template "3node-mbaas/fh-mbaas" for "fh-mbaas-template-3node.json" to project 3node-mbaas

     fh-mbaas
     ---------
     Red Hat Mobile Backend as a Service template


     * With parameters:
        * FH_CLUSTER=development
        * FH_MBAASID=FqYufgceAyicKNO46fvG # generated
        * MONGODB_FHMBAAS_USER=mbaas-user
        * MONGODB_FHMBAAS_PASSWORD=6XlNBYoJFsokECGVSs3VC5FR7GJY5OWRGQodavrc # generated
        * FHMBAAS_KEY=u6KXyxxs6lvR3NodYBL8rUeFvqFBWXC1r6pA5ma0 # generated
        * MONGODB_KEYFILE_VALUE=oMPoeDk6u4I3qKGJ5k75KeKkpSaU7INAfnw6RYdsBInpxh5DBk5iLAAOxbyfi1ittjvhFqA7mHel1AOQOQF117NMRCC2crkJ1niMjmE2dFs6ARkW4XkfBTsP8P22XTsaow3YUQpBdaGa4j80oSrcxSGwbalhIsQa4xw6gXJ3hYXymoDba4dsQ20AxQ27dx5VHBHot5eoUgux7J4VRiIB46KSp3XUDs2HvuIJgKDFqwDda0LNjwm75WuOgbQsGjk # generated
        * MONGODB_ADMIN_PASSWORD=YHO7mNwiLa1ONP3YEDlHW5yDTT5EDqlsCr7Pqsb3 # generated
        * MONGODB_FORM_PASSWORD=CUgucQaoUSmcEDXdK6icysLmgYTXXC30LqBSNQsy # generated
        * MONGODB_FHREPORTING_USER=reporting-user
        * MONGODB_FHREPORTING_PASSWORD=vo5ouB4YaL17CvxGsBOff7h8oUuxwd000So3J0jV # generated
        * FH_MBAAS_IMAGE=docker.io/feedhenry/fh-mbaas
        * FH_MBAAS_IMAGE_VERSION=5.4.4-1333
        * FH_MESSAGING_IMAGE=docker.io/feedhenry/fh-messaging
        * FH_MESSAGING_IMAGE_VERSION=3.0.7-488
        * FH_METRICS_IMAGE=docker.io/feedhenry/fh-metrics
        * FH_METRICS_IMAGE_VERSION=3.0.6-488
        * FH_STATSD_IMAGE=docker.io/feedhenry/fh-statsd
        * FH_STATSD_IMAGE_VERSION=2.0.4-127
        * MONGODB_IMAGE=docker.io/rhmap/mongodb
        * MONGODB_IMAGE_VERSION=centos-3.2-44
        * FH_MESSAGING_API_KEY=XCFFYjtD2dLq7ubdcYrHcNbEJHvEne5ObCsDNw2o # generated
        * FH_METRICS_API_KEY=nvXsqga1BLUGclvqf05b7qDNNUjweAcW3GTIFHSL # generated
        * MBAAS_ROUTER_DNS=
        * ENDPOINT_COUNT=3
        * FH_MBAAS_REPLICAS=3
        * FH_MESSAGING_REPLICAS=3
        * FH_METRICS_REPLICAS=3
        * FH_STATSD_API_KEY=Xr3JT8fxKPcQt5k45v3pQ8CV7ReQPYJBPPNeRNxK # generated
        * FH_DEFAULT_LOG_LEVEL=info
        * MONGODB_PVC_SIZE=50Gi
        * SMTP_SERVER=localhost
        * SMTP_USERNAME=username
        * SMTP_PASSWORD=password
        * SMTP_TLS=auto
        * SMTP_FROM_ADDRESS=admin@example.com
        * RHMAP_ADMIN_EMAIL=root@localhost
        * NAGIOS_USER=nagiosadmin
        * NAGIOS_PASSWORD=CKoKo67M5q # generated
        * RHMAP_ROUTER_DNS=localhost
        * RHMAP_HOSTGROUPS=mbaas
        * NAGIOS_IMAGE=docker.io/rhmap/nagios4
        * NAGIOS_IMAGE_VERSION=4.0.8-146
        * MONGODB_REPLICA_NAME=rs0
        * Proxy Url=

  --> Creating resources ...
    configmap "fh-mbaas-info" created
    configmap "node-proxy" created
    service "fh-mbaas-service" created
    service "fh-messaging-service" created
    service "fh-metrics-service" created
    service "fh-statsd-service" created
    service "mongodb-1" created
    service "mongodb-2" created
    service "mongodb-3" created
    service "nagios" created
    serviceaccount "nagios" created
    rolebinding "nagiosadmin" created
    persistentvolumeclaim "nagios-claim-1" created
    deploymentconfig "fh-mbaas" created
    deploymentconfig "fh-messaging" created
    deploymentconfig "fh-metrics" created
    deploymentconfig "fh-statsd" created
    deploymentconfig "nagios" created
    persistentvolumeclaim "mongodb-claim-1" created
    persistentvolumeclaim "mongodb-claim-2" created
    persistentvolumeclaim "mongodb-claim-3" created
    deploymentconfig "mongodb-1" created
    deploymentconfig "mongodb-2" created
    deploymentconfig "mongodb-3" created
    pod "mongodb-initiator" created
    route "mbaas" created
    route "nagios" created
  --> Success
    Run 'oc status' to view your app.
    
    ```

- **Monitor the deploy on OpenShift**

  ```bash
  
  oc get pods
  NAME                   READY     STATUS    RESTARTS   AGE
  fh-mbaas-3-7pc82       1/1       Running   1          12m
  fh-mbaas-3-7wfrb       1/1       Running   1          12m
  fh-mbaas-3-qzqf2       1/1       Running   1          12m
  fh-messaging-1-2f7v3   1/1       Running   3          31m
  fh-messaging-1-t1wc7   1/1       Running   0          21m
  fh-messaging-1-vx319   1/1       Running   0          21m
  fh-metrics-1-2l9vw     1/1       Running   3          31m
  fh-metrics-1-9xgb7     1/1       Running   0          21m
  fh-metrics-1-rjtk0     1/1       Running   0          21m
  fh-statsd-1-dtmsn      1/1       Running   0          31m
  mongodb-1-1-jtz12      1/1       Running   0          33m
  mongodb-2-1-hzkjr      1/1       Running   0          31m
  mongodb-3-1-mpm43      1/1       Running   0          21m
  nagios-1-pk9sm         1/1       Running   0          21m 

  ```

- **Check Nagios for all health endpoints**

    - In the web console navigate to Applications -> Pods
    - Click on the nagios-x-xxx link
    - Navigate to the Environments tab
    - Copy the nagios password
    - Navigate to Applications route
    - Click on the nagios route
    - Enter the credentials (user is nagiosadmin) and use the copied password


### Troubleshooting
- Problems usually encountered are to do with docker images (check access and credentials) and PV (persistent volumes)
