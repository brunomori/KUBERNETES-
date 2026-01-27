# ☸️ Kubernetes — Guia de Sobrevivência (SRE Jr)

---

## 📌 Índice

* [O que é Kubernetes](#-o-que-é-kubernetes)
* [Principais Componentes](#-principais-componentes)
* [Arquitetura Básica](#-arquitetura-básica)
* [kubectl — Comandos Essenciais](#-kubectl--comandos-essenciais)
* [Pods](#-pods)
* [Deployments](#-deployments)
* [Services (Portas e Acesso)](#-services-portas-e-acesso)
* [ConfigMap e Secret](#-configmap-e-secret)
* [Logs e Debug](#-logs-e-debug)
* [Escalabilidade](#-escalabilidade)
* [Ciclo de Vida de uma Aplicação](#-ciclo-de-vida-de-uma-aplicação)
* [Mini Lab Mental (Entrevista)](#-mini-lab-mental-entrevista)

---

## 🧠 O que é Kubernetes

Kubernetes (K8s) é um **orquestrador de containers**.
Ele gerencia:

* Deploy
* Escala
* Comunicação
* Recuperação automática (self-healing)

👉 Docker roda containers. **Kubernetes gerencia containers em produção**.

---

## 🧩 Principais Componentes

* **Cluster** → conjunto de máquinas
* **Node** → máquina (VM ou física)
* **Pod** → menor unidade (1 ou mais containers)
* **Deployment** → controla Pods
* **Service** → expõe Pods
* **ConfigMap / Secret** → configuração

---

## 🏗️ Arquitetura Básica

* **Control Plane** (decide)

  * API Server
  * Scheduler
  * Controller Manager

* **Worker Node** (executa)

  * kubelet
  * kube-proxy
  * container runtime

---

## 🧰 kubectl — Comandos Essenciais

```bash
kubectl get nodes
kubectl get pods
kubectl get deployments
kubectl get services
```

```bash
kubectl describe pod <pod>

Principais informações importantes

Name / Namespace → Identificação do Pod

Status → Estado atual (Running, Pending, CrashLoopBackOff, etc)

Node → Node onde o Pod está rodando

IP → IP interno do Pod no cluster

Controlled By → Quem gerencia o Pod (normalmente um ReplicaSet de um Deployment)

Image → Imagem Docker usada pelo container

Ready → Indica se o container está pronto para receber tráfego

Restart Count → Quantas vezes o container reiniciou

Labels → Usadas por Services e Deployments para seleção

Conditions → Saúde geral do Pod (tudo True = OK)

Events → Logs de erro e problemas do Kubernetes

kubectl delete pod <pod>
```

---

## 📦 Pods

Pod = **container + contexto**

```bash
kubectl get pods
```

Criar Pod simples:

```bash
kubectl run nginx --image=nginx
```

⚠️ Em produção, **não se usa Pod direto**, e sim Deployment.

---

## 🚀 Deployments

Deployment controla:

* Réplicas
* Atualizações
* Rollback

```bash
kubectl create deployment web --image=nginx
```

```bash
kubectl get deployments
kubectl get pods
```

---

## 🌐 Services (Portas e Acesso)

Service conecta **rede → Pods**.

Tipos principais:

* ClusterIP (interno)
* NodePort
* LoadBalancer

Expor Deployment:

```bash
kubectl expose deployment web --type=NodePort --port=80
```

```bash
kubectl get services
```

---

## 🔐 ConfigMap e Secret

Configuração desacoplada da aplicação.

ConfigMap:

```bash
kubectl create configmap app-config --from-literal=ENV=prod
```

Secret:

```bash
kubectl create secret generic app-secret --from-literal=PASSWORD=123
```

---

## 📜 Logs e Debug

Logs de Pod:

```bash
kubectl logs <pod>
```

Logs contínuos:

```bash
kubectl logs -f <pod>
```

Entrar no container:

```bash
kubectl exec -it <pod> -- sh
```

---

## 📈 Escalabilidade

Escalar manualmente:

```bash
kubectl scale deployment web --replicas=3
```

Ver estado:

```bash
kubectl get pods
```

---

## 🔄 Ciclo de Vida de uma Aplicação

1. Criar Deployment
2. Kubernetes cria Pods
3. Service expõe Pods
4. Se Pod cair → outro sobe
5. Scale ajusta carga

👉 **Self-healing automático**

---

## 🧪 Mini Lab Mental (Entrevista)

Pergunta comum:

> O que acontece se um Pod morrer?

Resposta esperada:

* Deployment detecta
* Novo Pod é criado
* Service redireciona tráfego
* Usuário não percebe

---

## 🎯 Visão SRE Jr

Você **não precisa decorar tudo**, mas precisa:

* Entender fluxo
* Saber debugar
* Saber onde olhar logs
* Entender escala e falhas

👉 Isso já cobre **80% do dia a dia SRE Jr**.

---

## 🆚 Docker vs Kubernetes (Visão Prática)

```text
Docker           → Executa containers
Docker Compose   → Orquestra containers em uma máquina
Kubernetes       → Orquestra containers em várias máquinas (cluster)
```

---

## 🚨 Problemas Comuns em Produção (SRE Reality Check)

```text
CrashLoopBackOff        → Aplicação quebrando ao iniciar
ImagePullBackOff       → Erro ao baixar imagem
Pod Running mas app off→ Porta errada ou app não escutando
Service sem resposta   → Selector errado ou Pod não pronto
```

---

## 🛠️ Fluxo Rápido de Troubleshooting

```bash
# Ver estado geral
kubectl get pods

# Ver detalhes do pod
kubectl describe pod <pod>

# Ver logs
kubectl logs <pod>

# Entrar no container
kubectl exec -it <pod> -- sh
```

---

## 📌 Dicas de Sobrevivência (Dia a Dia SRE Jr)

```text
- Sempre comece com kubectl get
- Logs antes de entrar no container
- Service quebrado quase sempre é selector
- Pod reiniciando = olhar logs
- Deployment não sobe = imagem ou env
```
