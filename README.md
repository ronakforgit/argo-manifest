## **argoCD-geting-started**

ArgoCD  is an easy to use tool that allows development teams to deploy and manage applications without having to learn a lot about Kubernetes, and without needing full access to the Kubernetes system. 

*   We will try to achieve the below  GitOps principles using Argo CD and  Git. 
*   Declarative: A system managed by GitOps must have its desired state expressed declaratively.
*   Versioned and Immutable: The desired state is stored in a way that enforces immutability, and versioning and retains a complete version history.
*   Pulled Automatically: Argo Agents automatically pull the desired state declarations from the source(git repo in our case).  
    Continuously Reconciled: Argo agents will  continuously observe the current system and attempt to apply the desired state.

---

### Prerequisites :

*   Docker
*   kubectl 

### Installation -

*   Installing K3d and creating a  single node local cluster named **mylocalcluster**   :

```plaintext
curl https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
k3d cluster create mylocalcluster
```

*   Install ArgoCd:

Now let's create a good namespace  and apply argoCD  resources inside it

```plaintext
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
watch kubectl get pods -n argocd
```

Once all pods in argocd namespace are in a running state we will need to forward argocd service port to the localhost . Argo CD positions a service called argocd-server on port 443 internally. As port 443 is the default HTTPS port, and we may be running some additional HTTP/HTTPS services, it’s a standard convention to forward those to arbitrarily chosen different ports, like 8080

```plaintext
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Argo places the admin password in a Kubernetes secret named **argocd-initial-admin-secret** while installation. We will need it for login.

```plaintext
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

```plaintext
#OUTPUT
Rfsfsw23424sca
```

Open **http://localhost:8080**   to access argoCD
