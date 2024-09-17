To deploy the Kubernetes Dashboard using YAML manifests, follow these steps:

### 1. **Download Kubernetes Dashboard YAML**
Kubernetes provides an official YAML manifest that you can apply directly to your cluster to install the Dashboard. You can download and apply it using the following command:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

This YAML file contains all the necessary resources (e.g., Deployment, Service, Roles, etc.) required to deploy the Kubernetes Dashboard.

### 2. **Accessing the Dashboard**
Once the dashboard is deployed, you can access it through a `kubectl proxy` or by exposing the service externally.

To create a proxy and access the dashboard locally, use:

```bash
kubectl proxy
```

Then, open this URL in your web browser:

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

### 3. **Create Admin User for the Dashboard**
By default, you may not have enough permissions to access all the cluster resources via the Dashboard. To create an admin user, you can use the following YAML manifest:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

Apply this YAML:

```bash
kubectl apply -f admin-user.yaml
```

### 4. **Obtain the Bearer Token for Login**
To log in to the Dashboard, you need a bearer token. You can retrieve it using the following command:

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

This command will generate a token, which you can copy and use to log in to the Dashboard UI.

### 5. **Secure Access**
For production clusters, it is recommended to set up proper authentication and SSL encryption for the Dashboard. You can use ingress controllers, RBAC policies, and OIDC for secure access.

This process allows you to deploy and manage the Kubernetes Dashboard using YAML configuration.