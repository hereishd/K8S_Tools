# ArgoCD

Argo CD is a Kubernetes controller, responsible for continuously monitoring all running applications and comparing their live state to the desired state specified in the Git repository. It identifies deployed applications with a live state that deviates from the desired state as OutOfSync. Argo CD reports the deviations and provides visualizations to help developers manually or automatically sync the live state with the desired state. <br/>
Argo CD can automatically apply any change to the desired state in the Git repository to the target environment, ensuring the applications remain in sync.<br/><br/>
ArgoCD has an internal **repository service**. This service caches the Git repository locally and stores the application manifests. The repository server generates Kubernetes manifests and returns them based on inputs such as the repository URL, application path, revisions (i.e., commits, tags, branches), and any template-specific settings (i.e., Helm values, Ksonnet environments, parameters).<br/><br/>
ArgoCD has an **Application Controller**. This controller is a Kubernetes controller that continuously monitors applications, comparing the target state specified in the Git repository with the current state of each application. The applications controller identifies when an application is OutOfSync and can implement corrections where specified. It invokes hooks defined by the user for application lifecycle events such as PreSync, Sync, and PostSync.<br/><br/>
ArgoCD has an **API server**. It is reponsible for letting you interact with Argo CD via the CLI or web UI.<br/><br/>
There are multiple ways to install ArgoCD: by using the manifest files, by using helm chars or by using an operator. Here, I will be using the manifest files to make the installation. 

## ArgoCD Installation 
For this part, I chose to make the documentation in 2 different parts. On for the sake of simplicity and understanding the concepts on minikube, and another one my cluster on virutalbox. Here are my links:
* [Installing ArgoCD on minikube and practice](https://github.com/hereishd/K8S_Tools/tree/main/ArgoCD/with_minikube) 
* [Installing and customizing ArgoCD on my VirtualBox VM Cluster](https://github.com/hereishd/K8S_Tools/tree/main/ArgoCD/with_virtualbox)

## Explaining the components
I will just take a small moment here to explain the use for each pod that has been created.
![ArgoCD_Pods](img/argo_components.png)<br/>

* **argocd-application-controller-0**: This is the core of ArgoCD. It continuously monitors applications, comparing the target state specified in the Git repository with the current state of our Cluster.
* **argocd-applicationset-controller-...**: Is basically used to generate applications. *(we will go deeper in explaining applications in terms of ArgoCD further on)*
* **argocd-dex-server-...**: Used for the SSO. In case you want to set OIDC or any Auth related configurations with your Identity provider.
* **argocd-notification-controller-...**: This is a new feature. It continuously monitors ArgoCD applications and provide a way to notify users about important changes in the application state.
* **argocd-redis-...**: Responsible of caching the states.
* **argocd-repo-server-...**: Used to communicate with the version control system that we are using to store ou manifests. (ArgoCD only supports GIT based version control systems).
* **argocd-server-...**: Basically, this is the API server of ArgoCD. It is reponsible for letting you interact with Argo CD via the CLI or web UI. (takes all the requests from the users).

## Configuer the service as NodePort
For the simplicity of this exemple, I am not going to use my Ingress Controller and simply configure the argocd-server as type NodePort to access the UI. 
```
$ kubectl edit svc argocd-server -n argocd
```
Then edit the type ClusterIP to NodePort.<br/>
You can now access the UI via the nodeport.<br/><br/>
For the UI credentials the username will be admin. To know the password simply copy the password's secret value from the secret file
```
$ kubectl describe secret argocd-initial-admin-secret -n argocd
```
and decode it
```
$ echo <seret-password-value | base64 --decode
```
You can now access the ArgoCD panel.

## Practical example
It is now time to try ArgoCD.<br/>
For this, ArgoCD itself maintains repositorys with example apps to allow us to practice with ArgoCD. Here is the url: [https://github.com/argoproj/argocd-example-apps](https://github.com/argoproj/argocd-example-apps).<br/>
I will use the guestbook app to start off with. In this repo there is a simple deployment yaml and service yaml
## Ingress Configuration
Since I am using the NGINX Ingress Controller in my cluster, I will configure it with ArgoCD
## References
* [The Official ArgoCD docs](https://argo-cd.readthedocs.io/en/stable/)