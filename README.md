## Configuração de um serviço NestJS + utilização da ferramenta [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) para realização de GitOps.

**OBS**: Todo material foi produzido seguindo o vídeo [Deploy Contínuo com GitOps e ArgoCD](https://www.youtube.com/watch?v=63HGUgQXD1w) do time da Full Cycle.

Objetivos: 
 - Configurar repositório com solução simples utilizando Kubernetes (Aplicação NestJS com uma mensagem ao acessar a URL da aplicação).
 - Configurar [Kustomize](https://kustomize.io/) para geração dos manifestos Kubernetes.
 - Configurar CI/CD utilizando o github actions.
   - Build da solução (Criação da imagem + push para Dockerhub)
   - Deploy da solução (Edição do arquivo de Kustomize + push da alteração do Kustomize)
 - Utilização do ArgoCD para GitOps:
   - Configuração da ferarmenta
   - Configuração do repositório Github
   - Validação de modificações e sincronização da solulão

Passos de execução:
- Instalar o [k3d](https://k3d.io/v4.4.8/#installation)
- Criar Cluster Kubernetes localmente: 
    ```shell
    k3d cluster create argocd -p "8000:30000@loadbalancer"
    ```
- Instalação do ArgoCD:
    ```shell
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```
- Acessar UI ArgoCD:
    ```shell
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```
  - Como username coloca **admin** e para obtenção da senha execute o seguinte comando:
      ```shell
      kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
      ```
- Criar namespace:
    ```shell
    kubectl create namespace nestjsapp
    ```