# Remnawave Subscription Page Helm Chart

This Helm chart simplifies the deployment of the [Remnawave Subscription Page](https://github.com/remnawave/subscription-page) on Kubernetes (optimized for **k3s**).

The Subscription Page allows your users to view their connection details, download configuration files, and see their data usage in a beautiful, responsive web interface.

## Prerequisites

-   A running Kubernetes cluster (k3s, k8s, etc.)
-   Helm 3.x installed
-   An existing [Remnawave Panel](https://github.com/remnawave/panel) installation
-   An API Token generated from the Remnawave Dashboard (`Settings` -> `API Tokens`)

## Installation

1.  **Clone the repository (or copy the chart files):**
    ```bash
    git clone https://github.com/TheroDev-Corp/remnaware-subscription-page.git
    cd remnaware-subscription-page
    ```

2.  **Configure `.env` in Remnawave Panel:**
    Ensure your Remnawave Panel's `.env` file has the `SUB_PUBLIC_DOMAIN` set to the domain you plan to use for this subscription page.
    ```bash
    SUB_PUBLIC_DOMAIN=subscription.yourdomain.com
    ```
    *Don't forget to restart your Panel after changing this.*

3.  **Install the chart:**
    ```bash
    helm upgrade --install remnawave-subscription-page . \
      --namespace remnawave --create-namespace \
      --set app.panelUrl="https://panel.yourdomain.com" \
      --set app.apiToken="YOUR_REMNAWAVE_API_TOKEN" \
      --set ingress.enabled=true \
      --set ingress.hosts[0].host="subscription.yourdomain.com"
    ```

## Configuration

The following table lists the configurable parameters of the Remnawave Subscription Page chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Image repository | `remnawave/subscription-page` |
| `image.tag` | Image tag | `latest` |
| `service.type` | Kubernetes Service type | `ClusterIP` |
| `service.port` | Internal service port | `3010` |
| `app.panelUrl` | URL of your Remnawave Panel | `https://panel.example.com` |
| `app.apiToken` | API Token from Remnawave Dashboard | `""` |
| `app.existingSecret` | Use an existing secret for variables | `""` |
| `app.apiTokenKey` | Key for the API token in secret | `apiToken` |
| `ingress.enabled` | Enable ingress controller resource | `false` |
| `ingress.className` | Ingress class name (e.g., `traefik`) | `""` |
| `ingress.hosts[0].host` | Hostname for the subscription page | `subscription.example.com` |

> [!TIP]
> You can also use `CUSTOM_SUB_PREFIX` in `values.yaml` if you want to serve the page on a specific sub-path (e.g., `yourdomain.com/sub`).

## Real IP Support (Traefik/k3s)

If you are using Traefik (default in k3s) and want to pass the real client IP to the application, you can use the `Middleware` provided in your infrastructure.

Example annotation for `values.yaml`:
```yaml
ingress:
  annotations:
    traefik.ingress.kubernetes.io/router.middlewares: "kube-system-remnaware-real-ip@kubernetescrd"
```

## Credits

-   [Remnawave Team](https://github.com/remnawave) for the amazing software.
-   [Documentation](https://docs.rw/docs/install/subscription-page/separate-server)

## License

This project is licensed under the MIT License.
