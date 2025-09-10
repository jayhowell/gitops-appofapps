# Appplication of Applications for a cluster.
This demo is a demo to pull down multiple applications. You will need the following.
- Openshift 4.18 or greater
- Gitops installed
- (optional) Advanced cluster manager installed on each cluster you want to deploy to.
  
**Tips:**
- install the following to link argocd to your openshift login
```
cat << EOF | oc apply -f -
apiVersion: user.openshift.io/v1
kind: Group
metadata:
  name: cluster-admins
users:
- admin
EOF
```
- In order for Argocd to create namespaces you must give it permissions.
```
oc adm policy add-cluster-role-to-user cluster-admin \
  system:serviceaccount:openshift-gitops:openshift-gitops-argocd-application-controller
```

**Steps for gitops:**
- Go into the Gitops operator
- In the gui Create an application that points to 
  - URL: https://github.com/levenhagen/rocketchat-acm.git
  - Revision: main
  - Path: cluster
  - namespace: openshift-gitops
- or add this Application yaml.
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-of-apps
  namespace: openshift-gitops
spec:
  project: default
  source:
    path: manifests/cluster/
    repoURL: https://github.com/jayhowell/gitops-appofapps.git
    targetRevision: main
  destination:
    namespace: openshift-gitops
    server: https://kubernetes.default.svc
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

**Steps for ACM**
- go into ACM console
- Create a gitops push
- using the following information
  - URL: https://github.com/levenhagen/rocketchat-acm.git
  - Revision: main
  - Path: cluster
  - namespace: openshift-gitops

You will see the application object that points to the other application objects.  Using ACM, you can use a placement to determine what clusters will get these applications.


