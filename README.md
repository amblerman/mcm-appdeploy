
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



Applicaions: https://www.ibm.com/support/knowledgecenter/SSFC4F_1.1.0/mcm/applications/app_resources.html
Applications can be thought of as a top level reourse type that will reference all the components that make up your container based application. Since applications in a containerized world may be made up of multiple services running in containers, you can use this application resource do define your application as a whole. The application resource defines what pieces make up the application by listing what type of other resources can be included in the appication. For example, this project uses Subcriptions to capture what deployable objects are included in the application


Subscriptions:
 

## Acknowledgments

* Developed by Andy Moynahan, Paul Lucas, Dave Wakeman and Christina Churchill
* Thank you to our K8s enablement leaders!
