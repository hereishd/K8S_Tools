# ArgoCD

Argo CD is a Kubernetes controller, responsible for continuously monitoring all running applications and comparing their live state to the desired state specified in the Git repository. It identifies deployed applications with a live state that deviates from the desired state as OutOfSync. Argo CD reports the deviations and provides visualizations to help developers manually or automatically sync the live state with the desired state. <br/>
Argo CD can automatically apply any change to the desired state in the Git repository to the target environment, ensuring the applications remain in sync.<br/><br/>
ArgoCD has an internal **repository service**. This service caches the Git repository locally and stores the application manifests. The repository server generates Kubernetes manifests and returns them based on inputs such as the repository URL, application path, revisions (i.e., commits, tags, branches), and any template-specific settings (i.e., Helm values, Ksonnet environments, parameters).<br/><br/>
ArgoCD has an **Application Controller**. This controller is a Kubernetes controller that continuously monitors applications, comparing the target state specified in the Git repository with the current state of each application. The applications controller identifies when an application is OutOfSync and can implement corrections where specified. It invokes hooks defined by the user for application lifecycle events such as PreSync, Sync, and PostSync.<br/><br/>
ArgoCD has an **API server**. It is reponsible for letting you interact with Argo CD via the CLI or web UI.<br/><br/>
There are multiple ways to install ArgoCD: by using the manifest files, by using helm chars or by using an operator. Here, I will be using the manifest files to make the installation. 

## ArgoCD Installation with manifest (part 1)
* We first create a namespace 
```
$ kubectl create ns argocd
```
* We then apply the manifest from argocd
```
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
This manifest creates all the necessary resources and components for/of ArgoCD.
* Now wait for all the ArgoCD pods to be running. 
Check and wait till all is running
```
$ kubectl get pods -n argocd -w
```
## Explaining the components
I will just take a small moment here to explain the use of each pod that have been created.
![ArgoCD_Pods](img/argo_components.png)
## References
* [The Official ArgoCD docs](https://argo-cd.readthedocs.io/en/stable/)