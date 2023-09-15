# ArgoCD

Argo CD is a Kubernetes controller, responsible for continuously monitoring all running applications and comparing their live state to the desired state specified in the Git repository. It identifies deployed applications with a live state that deviates from the desired state as OutOfSync. Argo CD reports the deviations and provides visualizations to help developers manually or automatically sync the live state with the desired state. <br/>
Argo CD can automatically apply any change to the desired state in the Git repository to the target environment, ensuring the applications remain in sync.<br/><br/>
ArgoCD has an internal **repository service**. This service caches the Git repository locally and stores the application manifests. The repository server generates Kubernetes manifests and returns them based on inputs such as the repository URL, application path, revisions (i.e., commits, tags, branches), and any template-specific settings (i.e., Helm values, Ksonnet environments, parameters).<br/><br/>
ArgoCD has an **Application Controller**. This controller is a Kubernetes controller that continuously monitors applications, comparing the target state specified in the Git repository with the current state of each application. The applications controller identifies when an application is OutOfSync and can implement corrections where specified. It invokes hooks defined by the user for application lifecycle events such as PreSync, Sync, and PostSync.<br/><br/>
ArgoCD has an **API server**. It is reponsible for letting you interact with Argo CD via the CLI or web UI.

## ArgoCD Installation

## References