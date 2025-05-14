# Argocd command:
To get credentials:
$ oc extract secret/openshift-gitops-cluster -n openshift-gitops --to=-
admin.password
rA2U8zCjFuYh6D3JaQBGnyxws5KPgSId

Port forward for argocd command:
$ oc port-forward svc/openshift-gitops-server -n openshift-gitops 8080:443

In next tab, login:
	$ argocd login localhost:8080   --username admin   --password XXX   --insecure

To list all running app:
$ argocd app list| grep redhat

To implement an application:
$ git remote -v
origin	https://github.com/YogiSoni-lazy/devhub-apps.git (push)
$ pwd
/home/yogita/test2/devhub-apps/argocd
$ argocd app create -f rhdh.yaml
application 'redhat-devhub' created

Command to display applied app status:
$ argocd app get openshift-gitops/redhat-devhub
Name:               openshift-gitops/redhat-devhub
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          openshift-gitops
URL:                https://openshift-gitops-server-openshift-gitops.apps.cluster-nkmjp.dynamic.redhatworkshops.io/applications/redhat-devhub
Source:
- Repo:             https://github.com/YogiSoni-lazy/devhub-apps.git
  Target:           main
  Path:             rhdh-keycloak
SyncWindow:         Sync Allowed
Sync Policy:        Automated
Sync Status:        Synced to main (fcc99a9)
Health Status:      Healthy

GROUP                 KIND           NAMESPACE         NAME                 STATUS   HEALTH   HOOK  MESSAGE
                      Namespace      openshift-gitops  rhdh-operator        Running  Synced         namespace/rhdh-operator created
operators.coreos.com  Subscription   rhdh-operator     rhdh                 Synced   Healthy        subscription.operators.coreos.com/rhdh created
operators.coreos.com  OperatorGroup  rhdh-operator     rhdh-operator-group  Synced                  operatorgroup.operators.coreos.com/rhdh-operator-group created
                      Namespace                        rhdh-operator        Synced                  


To display only resources:
$ argocd app resources redhat-devhub
GROUP                 KIND           NAMESPACE      NAME                 ORPHANED
                      Namespace                     rhdh-operator        No
operators.coreos.com  OperatorGroup  rhdh-operator  rhdh-operator-group  No
operators.coreos.com  Subscription   rhdh-operator  rhdh                 No

To delete app with resources:
$ argocd app delete redhat-devhub --cascade --yes


# Project Commands

$ argocd app create -f argocd/keycloak.yaml 
$ argocd app create -f argocd/rhdh.yaml 
