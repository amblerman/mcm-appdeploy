
# Chicago Public Market K8 - Project
# Application promotion model with multiple clusters

This project looks at  the application deployment and promotion model within IBM Multi Cluster Manager (MCM).
This project explores the how Applications, Channels, Subscriptions and Deployables can be used to manage and promote appications across multiple clusters.

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
![alt text] (screenshots/application-model.png "Application Model Diagram")


## Acknowledgments

* Developed by Andy Moynahan, Paul Lucas, Dave Wakeman and Christina Churchill
* Thank you to our K8s enablement leaders!

