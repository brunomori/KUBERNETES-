# ☸️ Kubernetes – Essencial para Analista SRE Jr

Este documento segue **o mesmo estilo de formatação do README de Docker**, usando `###`, `####` e blocos `bash` para manter padrão visual e organização no GitHub.

---

## 📦 Instalação

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
```

```bash
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

```bash
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

```bash
sudo apt update
sudo apt install -y kubectl
```

---

## 🧱 Conceitos Básicos

### Cluster

* Conjunto de máquinas que executam containers

### Node

* Máquina (VM ou física) dentro do cluster

### Pod

* Menor unidade do Kubernetes
* Pode conter 1 ou mais containers

### Deployment

* Gerencia réplicas e atualizações de Pods

### Service

* Expõe Pods para rede interna ou externa

---

## 🚀 Comandos Essenciais

### Verificar cluster

```bash
kubectl cluster-info
```

### Listar nodes

```bash
kubectl get nodes
```

### Listar pods

```bash
kubectl get pods
```

### Listar pods em todos os namespaces

```bash
kubectl get pods -A
```

---

## 📄 Trabalhando com YAML

### Exemplo de Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
```

### Criar recurso

```bash
kubectl apply -f pod.yaml
```

### Remover recurso

```bash
kubectl delete -f pod.yaml
```

---

## 📦 Deployment

### Exemplo de Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
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
          image: nginx:latest
```

### Criar deployment

```bash
kubectl apply -f deployment.yaml
```

---

## 🌐 Service

### Exemplo NodePort

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

---

## 📊 Logs e Debug

### Logs de um Pod

```bash
kubectl logs nginx-pod
```

### Logs em tempo real

```bash
kubectl logs -f nginx-pod
```

### Acessar container

```bash
kubectl exec -it nginx-pod -- sh
```

---

## 🧹 Limpeza

### Deletar pod

```bash
kubectl delete pod nginx-pod
```

### Deletar tudo do namespace atual

```bash
kubectl delete all --all
```

---

## 🎯 O que isso cobre para SRE Jr

✔ Conceitos fundamentais
✔ Leitura de YAML
✔ Deploy de aplicações
✔ Debug básico
✔ Logs
✔ Networking simples

👉 Com isso + Docker + Linux, você **tem a base esperada para Analista SRE Jr**.
