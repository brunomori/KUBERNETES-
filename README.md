📘 Guia Kubernetes para SRE

📑 Sumário

Kubernetes Básico → Introdução à plataforma, conceitos fundamentais (Cluster, Node, Pod, Control Plane, kubectl).

Pods → Menor unidade do Kubernetes, encapsula containers. YAML básico e comandos para criar, listar e remover.

Deployments → Controlam réplicas de Pods e atualizações. Exemplo de Deployment com escalabilidade e rollout.

Services → Exposição de Pods. Tipos: ClusterIP (interno), NodePort (externo), LoadBalancer (balanceador).

ConfigMaps & Secrets → Armazenamento de configurações e dados sensíveis. Exemplos de YAML para uso em aplicações.

Volumes & Persistent Volumes → Persistência de dados com PVC (PersistentVolumeClaim).

Namespaces → Organização de recursos em ambientes separados (dev, prod, etc.).

kubectl Comandos Essenciais → Tabela de comandos para criar, listar, escalar, logs e execuções.

Helm → Gerenciador de pacotes para Kubernetes. Comandos básicos para instalar, listar e remover aplicações.

🐳 Conteúdo 1 — Kubernetes Básico (Sobrevivência)

📌 O que é KubernetesKubernetes é uma plataforma de orquestração de containers que automatiza a implantação, o gerenciamento e a escalabilidade de aplicações em clusters.

🧠 Conceitos Fundamentais

Cluster → conjunto de máquinas (nós) que executam aplicações.

Node → máquina física ou virtual que roda os containers.

Pod → menor unidade do Kubernetes, encapsula um ou mais containers.

Control Plane → gerencia o estado desejado do cluster.

kubectl → CLI para interagir com o cluster.

📦 Instalação do kubectl (Ubuntu/Debian)

sudo apt update
sudo apt install -y kubectl

🧱 Conteúdo 2 — Pods

📄 Pod básico

apiVersion: v1
kind: Pod
metadata:
  name: meu-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80

📌 Comandos úteis

kubectl get pods
kubectl describe pod meu-pod
kubectl delete pod meu-pod

▶️ Conteúdo 3 — Deployments

📄 Deployment com réplicas

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80

📌 Comandos

kubectl apply -f deployment.yaml
kubectl get deployments
kubectl scale deployment nginx-deployment --replicas=5
kubectl rollout status deployment nginx-deployment

🌐 Conteúdo 4 — Services

📄 Service NodePort

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort

📌 Tipos de Service

ClusterIP → acesso interno.

NodePort → expõe porta em cada nó.

LoadBalancer → integra com balanceadores externos.

📂 Conteúdo 5 — ConfigMaps & Secrets

📄 ConfigMap

apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: production

📄 Secret

apiVersion: v1
kind: Secret
metadata:
  name: db-secret
stringData:
  DB_PASSWORD: exemplo123

📂 Conteúdo 6 — Volumes & Persistent Volumes

📄 PersistentVolumeClaim

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-demo
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

📂 Conteúdo 7 — Namespaces

📌 Organização de recursos

kubectl get namespaces
kubectl create namespace dev
kubectl apply -f pod.yaml -n dev

🧹 Conteúdo 8 — kubectl Comandos Essenciais

Ação

Comando Exemplo

Listar Pods

kubectl get pods

Ver detalhes

kubectl describe pod meu-pod

Criar recurso

kubectl apply -f arquivo.yaml

Deletar recurso

kubectl delete -f arquivo.yaml

Logs

kubectl logs meu-pod

Executar comando

kubectl exec -it meu-pod -- bash

Escalar Deployment

kubectl scale deployment app --replicas=3

📦 Conteúdo 9 — Helm (Gerenciador de Pacotes)

📌 Comandos básicos

helm repo add stable https://charts.helm.sh/stable
helm install minha-app stable/nginx
helm list
helm uninstall minha-app

🧠 Objetivo: servir como cola rápida e base sólida para SRE Jr / DevOps em Kubernetes.
