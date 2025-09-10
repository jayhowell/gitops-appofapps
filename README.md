# Appplication of Applications for a cluster.
This demo is a demo to pull down multiple applications. You will need the following.
- Openshift 4.18 or greater
- Gitops installed
- (optional) Advanced cluster manager installed on each cluster you want to deploy to.

**Steps for gitops:**
- Go into the Gitops operator
- Create an application that points to 
  - URL: https://github.com/levenhagen/rocketchat-acm.git
  - Revision: main
  - Path: cluster
  - namespace: openshift-gitops

**Steps for ACM**
- go into ACM console
- Create a gitops push
- using the following information
  - URL: https://github.com/levenhagen/rocketchat-acm.git
  - Revision: main
  - Path: cluster
  - namespace: openshift-gitops

You will see the application object that points to the other application objects.  Using ACM, you can use a placement to determine what clusters will get these applications.


