
# Chicago Public Market K8 - Project
# Application promotion model with multiple clusters

This project looks at  the application deployment and promotion model within IBM Multi Cluster Manager (MCM).
This project takes a look at how the Applications, Channels, Subscriptions and Deployables can be used to manage and promote appications across multiple clusters.

The resources defined below are done so in the context of a ficticious application called "acmeapp".
All the resources use "acme" in their names, namespaces, and other metadata as a way to identify the resource belonging to the acme application.
The applicaiton is completely ficticous and really is just using a standard nginx container as the deployable asset. 
The purpose of this exercise is to illustrate the application model in MCM as opposed to any functionality of the deployed application. Any application could be used for this pupose.

# Technology used in this project

* IBM Multi Cloud Manager: https://www.ibm.com/cloud/multicloud-manager
* RedHat OKD: https://www.okd.io/

For this project, a MCM hub cluster was used that managed two OKD clusters. https://www.okd.io/

The MCM cluster served solely as a management hub cluster. The two OKD clusters servered as the potential deployment targets.
For the purposes of this project, one of the OKD clusters was considered a Dev cluster and the other a Production level cluster.

# Concepts

The application deployment model employed by MCM is that of using a combination of Applications, Subscriptions, Channels and Deployable objects to manage
the deployment of applications. The below provides a brief overview of these objects.

An overview of the application model itself can be found here: https://www.ibm.com/support/knowledgecenter/SSFC4F_1.1.0/mcm/applications/app_lifecycle.html


Applications: 
Applications are used to group together the different components that make up the overall application.
The Application definition uses selector matching in its yaml defintion to select what components will be included as part of the application.

Deployables:
Deployables are used as a wrapper to the Kubernetes or Helm artifacts that make up the assets that get deployed to the target clusters. 
The Deployable resource allows the MCM hub cluster to be aware of the the artifacts that neeed to be deployed and allows the deployment to occur to one or more target clusters once a proper match is made as to which cluster or clusters will be the actual target.

Channel:
Channels are used to point to the source of the actual deployable artifacts. Channels can be one of three type: Namespace, Helm or ObjectStore.
In all cases the type of the channel indicates where it will look for possible artifacts that can be used for deployment.

This project uses a namespace channel. This means that any deployables in a namespace that the channel knows about could be possible candidates for deployment.
The Channel can also use a gate defined by annotations to further filter out which deployable resources the channel will identify.

Subscriptions:
Subscriptions (Subscription.app.ibm.com) are Kubernetes resources that serve as sets of definitions for identifying deployables within channels by using annotations, labels, and versions. Subscription resources reside on the managed cluster and can point to a channel or storage location for identifying new and updated deployables, such as Helm releases or Kubernetes resources for deployment. 
Subscriptions also can define placement rules as to where the deployables can be placed or deployed.
Once a subscription identifies the proper deployables that match the given criteria, they can be deployed to the target clusters.
 
The relationship of these objects is illustrated in the following diagram:

![alt text](screenshots/application-model.png "MCM Application Model Diagram")

# How the Applicaiton model in MCM works.
In this project, two clusters, an Application, two channels, two subscriptions and one deployable were created.
This was the minimum number of resources needed to illustrate how the application model in Multi Cloud Manager works and be able to deploy to more than one cluster..

Clusters:
The two target clusters were OKD clusters and were both managed by the MCM hub cluster. The configuration of the two target cluster was irrelevant to this exercise. 
The clusters were identified both by name and purpose.

Cluster Name:      Purpose:
lancel             development cluster
kevan              production cluster

This was a simple example where we will be choosing the placement of our deployments by clustername.
More advanced cluster selection techniques are possible through the use placement rules.  
Placement rules are discussed here: https://www.ibm.com/support/knowledgecenter/SSFC4F_1.1.0/mcm/applications/managing_placement_rules.html


Application:
The application is defined through the file named acmeapp.yaml.
```
apiVersion: app.k8s.io/v1beta1
kind: Application
metadata:
  name: acme-app
  namespace: acmeproj
  labels: 
    app: acme-app
spec:
  componentKinds:
  - group: app.ibm.com
    kind: Subscription
  selector:
    matchLabels:
      release: acme101
status: {}

```

In the above example, the application will be composed any subscription resource that happens to have the label of "acme101".
This allows MCM to identify which resources could make up the application.


Subcription. 
The development and production subscription are defined in the dev-sub.yaml and prod-sub.yaml files respectively. The only significant differences in the definitions will be the package filter 
annotationed used to identify potential resoruces and the target cluster for the placement.

Below the development subscription is shown.  

```
apiVersion: app.ibm.com/v1alpha1
kind: Subscription
metadata:
  name: acmeproj-dev-sub
  namespace: acmeproj
  labels:
    release: acme101
    app: acme-app
spec:
  channel: acmeproj/acmeproj-dev
  packageFilter: 
    annotations:
      dev-ready: approved
  placement:
    clusters:
    - name: lancelcluster

```

In the above example, we see that the subscription will be looking at the acmeproj/acmeproj-dev channel. This is of the form &ltnamespace&gt/&ltchannel name&gt.
This subscription will also be looking for packages (deployables) that carry the annotation of dev-ready = approved. This is an indication that the given deployable is 
approved for moving to a development cluster.

Finally, the placement rules used is merely a simple rule by targeting a particular cluster by name. In this case 'lancelcluster' is our development cluster.




                           

## Acknowledgments

* Developed by Andy Moynahan, Paul Lucas, Dave Wakeman and Christina Churchill
* Thank you to our K8s enablement leaders!

