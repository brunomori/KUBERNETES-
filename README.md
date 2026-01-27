# ☸️ Kubernetes — Guia de Sobrevivência (SRE Jr)

Guia prático de **Kubernetes** com foco em **fundamentos**, **comandos essenciais** e **troubleshooting**, baseado em laboratório local utilizando **Kind**.

> Objetivo: servir como material de estudo, revisão rápida e base para entrevistas de **SRE Jr / DevOps Jr**.

---

## 🔍 Detalhar um Pod

```bash
kubectl describe pod <pod>
```

### Informações importantes no `describe pod`
- **Name / Namespace** → Identificação
- **Status** → `Running`, `Pending`, `CrashLoopBackOff`
- **Node** → Onde o Pod está rodando
- **IP** → IP interno do cluster
- **Controlled By** → `Deployment` / `ReplicaSet`
- **Image** → Imagem Docker utilizada
- **Ready** → Pronto para receber tráfego
- **Restart Count** → Quantidade de reinícios
- **Labels** → Usadas por Services e Deployments
- **Events** → Principal fonte de erro do Kubernetes

### Remover Pod
```bash
kubectl delete pod <pod>
```

---

## 📦 Pods

- Pod = **container + contexto Kubernetes**
- Menor unidade de execução do cluster

### Listar Pods
```bash
kubectl get pods
```

### Criar Pod simples (apenas para testes)
```bash
kubectl run nginx --image=nginx
```

⚠️ Em produção, **não se cria Pod direto** — usa-se **Deployment**.

---

## 🚀 Deployments

- Garante **alta disponibilidade**
- Controla **réplicas**
- Faz **self-healing**

### Criar Deployment
```bash
kubectl create deployment web --image=nginx
```

### Ver estado
```bash
kubectl get deployments
kubectl get pods
```

👉 Se um Pod morrer, o Deployment cria outro automaticamente.

---

## 🌐 Services (Rede e Acesso)

Service conecta **usuários → Pods**.

### Tipos principais
- **ClusterIP** → acesso interno
- **NodePort** → expõe porta do Node
- **LoadBalancer** → cloud providers

### Expor Deployment
```bash
kubectl expose deployment web --type=NodePort --port=80
```

### Listar Services
```bash
kubectl get services
```

---

## 📂 Namespaces

Namespaces organizam e isolam recursos no cluster.

### Listar Pods de todos os namespaces
```bash
kubectl get pods -A
```

### Namespaces comuns
- `default` → aplicações do usuário
- `kube-system` → componentes internos do Kubernetes
- `local-path-storage` → volumes (Kind)

---

## 🔐 ConfigMap e Secret

Configuração desacoplada da aplicação.

### ConfigMap
```bash
kubectl create configmap app-config --from-literal=ENV=prod
```

### Secret
```bash
kubectl create secret generic app-secret --from-literal=PASSWORD=123
```

---

## 📜 Logs e Debug

### Ver logs do Pod
```bash
kubectl logs <pod>
```

### Logs contínuos
```bash
kubectl logs -f <pod>
```

### Entrar no container
```bash
kubectl exec -it <pod> -- sh
```

---

## 📈 Escalabilidade

### Escalar manualmente
```bash
kubectl scale deployment web --replicas=3
```

### Ver resultado
```bash
kubectl get pods
```

---

## 🔄 Ciclo de Vida de uma Aplicação

1. Criar Deployment  
2. Kubernetes cria Pods  
3. Service expõe Pods  
4. Pod falha → outro é criado  
5. Scale ajusta carga  

👉 **Self-healing automático**

---

## 🚨 Troubleshooting Essencial

- `CrashLoopBackOff` → App quebrando ao iniciar
- `ImagePullBackOff` → Erro ao baixar imagem
- Pod `Running`, app fora → Porta errada ou app não escutando
- Service sem resposta → Selector errado

### Fluxo padrão de debug
```bash
kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>
kubectl exec -it <pod> -- sh
```

---

## 🧪 Mini Lab Mental (Entrevista)

**Pergunta:** O que acontece se um Pod morrer?

**Resposta esperada:**
- Deployment detecta
- Novo Pod é criado
- Service redireciona tráfego
- Usuário não percebe

---

## 🖥️ Cluster e Contexto

### Ver nodes do cluster
```bash
kubectl get nodes
```

### Ver contexto atual
```bash
kubectl config current-context
```

---

## 📄 Trabalhando com YAML

### Aplicar manifesto
```bash
kubectl apply -f arquivo.yaml
```

### Deletar recurso via YAML
```bash
kubectl delete -f arquivo.yaml
```

---

## 🎯 Visão SRE Jr

Você não precisa decorar tudo, mas precisa:
- Entender o fluxo
- Saber debugar
- Ler logs
- Entender escala e falhas

👉 Isso cobre a maior parte do dia a dia de um **SRE Jr**.

---

## 🆚 Docker vs Kubernetes

- **Docker** → Executa containers
- **Docker Compose** → Orquestra containers em uma máquina
- **Kubernetes** → Orquestra containers em um cluster


# Kubernetes Local com KIND – Guia de Aprendizado

Este README resume, de forma prática, tudo o que foi aprendido até aqui sobre **Kubernetes local usando KIND (Kubernetes IN Docker)**, com **exemplos reais e comandos**.

---

## 📌 Pré-requisitos

* Docker instalado e funcionando
* kubectl instalado
* Internet **pode estar bloqueada** (ambiente corporativo)

Verificações básicas:

```
docker --version
kubectl version --client
```

---

## 🚀 Criação do Cluster Kubernetes com KIND

Criamos um cluster Kubernetes local rodando dentro de containers Docker.

```
kind create cluster
```

Verificar se o cluster está ativo:

```
kubectl get nodes
```

Saída esperada:

```
kind-control-plane   Ready   control-plane
```

---

## 🧠 Conceito Importante

* **kubectl** é apenas o cliente
* **KIND** é quem cria o cluster
* Kubernetes trabalha com **estado desejado**, não com processos manuais

---

## 📦 Criando um Pod Simples (nginx)

Criamos um Pod manualmente:

```
kubectl run nginx --image=nginx --port=80
```

Verificar o status:

```
kubectl get pods
```

---

## ❌ Problema Comum: ImagePullBackOff

Erro encontrado:

```
ImagePullBackOff
```

Motivo:

* Cluster KIND não consegue acessar o Docker Hub
* Muito comum em redes corporativas

---

## ✅ Solução: Carregar Imagem Manualmente no KIND

### 1️⃣ Baixar imagem localmente

```
docker pull nginx
```

### 2️⃣ Carregar imagem no cluster KIND

```
kind load docker-image nginx
```

### 3️⃣ Criar Pod sem puxar da internet

```
kubectl run nginx \
  --image=nginx:latest \
  --port=80 \
  --image-pull-policy=IfNotPresent
```

---

## 🌐 Acessando o Pod com Port-Forward

```
kubectl port-forward pod/nginx 8080:80
```

Acessar no navegador:

```
http://localhost:8080
```

---

## 🧠 Conceitos Aprendidos Até Aqui

* Pod é a menor unidade do Kubernetes
* Pod sozinho **não é resiliente**
* Port-forward permite acesso local
* `latest` força download da imagem

---

## 🚀 Criando um Deployment (Forma Correta)

Deployment garante:

* Auto-healing
* Escala
* Controle de versão

### Criar Deployment

```
kubectl create deployment nginx-deploy --image=nginx:latest
```

### Ajustar para não puxar imagem da internet

```
kubectl patch deployment nginx-deploy \
  -p '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","imagePullPolicy":"IfNotPresent"}]}}}}'
```

### Escalar para 2 réplicas

```
kubectl scale deployment nginx-deploy --replicas=2
```

Ver Pods:

```
kubectl get pods
```

---

## ♻️ Auto-healing na Prática

Ao deletar um Pod do Deployment:

```
kubectl delete pod nginx-deploy-XXXX
```

O Kubernetes automaticamente cria outro Pod para manter o estado desejado.

Isso prova que:

> Kubernetes não gerencia containers, ele garante estado.

---

## 🧹 Limpeza de Pod Solto (Boa Prática)

Remover Pod criado manualmente:

```
kubectl delete pod nginx
```

Manter apenas Pods gerenciados por Deployment.

---

## 🏁 Conclusão

Até aqui foi possível aprender:

* Criar cluster Kubernetes local
* Entender Pods e Deployments
* Resolver problemas reais de imagem
* Aplicar conceitos usados em ambientes corporativos
* Ver o Kubernetes se auto-recuperar

Este é o **fundamento real** do Kubernetes.

---

📚 Próximos passos sugeridos:

* YAML na prática
* Services (ClusterIP, NodePort)
* ConfigMap e Secrets
* Debug de falhas (CrashLoopBackOff)
* Mini-projeto completo
