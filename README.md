📘 Guia Kubernetes para SRE Jr

(Em construção — foco total em uso prático no dia a dia)

☸️ Conteúdo 1 — Kubernetes Básico (Sobrevivência)
📌 O que é Kubernetes

Kubernetes (K8s) é um orquestrador de containers.
Ele gerencia deploy, escala, restart e comunicação entre containers automaticamente.

👉 Docker roda container
👉 Kubernetes gerencia vários containers em produção

🧠 Conceitos Fundamentais (ESSENCIAL)
🔹 Cluster

Conjunto de máquinas que rodam Kubernetes.

🔹 Node

Máquina (VM ou física) dentro do cluster.

🔹 Pod

Menor unidade do Kubernetes

Um pod pode ter 1 ou mais containers

Containers do mesmo pod compartilham:

rede

volume

IP

Container < Pod < Node < Cluster

🔹 Deployment

Define como os pods devem rodar

Controla:

quantidade de réplicas

atualização

rollback

🔹 Service

Expõe os pods para acesso interno ou externo

Resolve o problema de IP dinâmico dos pods

🛠️ Conteúdo 2 — kubectl (Ferramenta Principal)
Ver status do cluster
kubectl cluster-info

Ver nodes
kubectl get nodes

📦 Conteúdo 3 — Pods
Listar pods
kubectl get pods


Todos os namespaces:

kubectl get pods -A

Descrever pod (debug)
kubectl describe pod nome-do-pod


👉 Muito usado quando pod não sobe.

Logs de pod
kubectl logs nome-do-pod


Últimas linhas:

kubectl logs --tail=50 nome-do-pod


Tempo real:

kubectl logs -f nome-do-pod


Container específico no pod:

kubectl logs nome-do-pod -c nome-container

🧠 Conteúdo 4 — Exec (igual Docker, mas em Pod)
Entrar no pod (shell)
kubectl exec -it nome-do-pod -- sh


Rodar comando direto:

kubectl exec nome-do-pod -- ls


📌 Observação SRE:

exec → debug rápido

logs → sempre primeiro

🚀 Conteúdo 5 — Deployments
Listar deployments
kubectl get deploy

Criar deployment simples
kubectl create deployment app --image=nginx

Escalar aplicação
kubectl scale deploy app --replicas=3

Atualizar imagem
kubectl set image deploy/app nginx=nginx:latest

🔁 Rollout (muito importante)
Ver status
kubectl rollout status deploy app

Ver histórico
kubectl rollout history deploy app

Rollback
kubectl rollout undo deploy app


👉 Isso cai direto em produção.

🌐 Conteúdo 6 — Services (Portas no Kubernetes)
Listar services
kubectl get svc

Tipos principais

ClusterIP → acesso interno

NodePort → acesso via porta do node

LoadBalancer → cloud (AWS / GCP / Azure)

Expor deployment
kubectl expose deploy app --type=NodePort --port=80

📂 Conteúdo 7 — Namespaces

Listar namespaces:

kubectl get ns


Usar namespace:

kubectl get pods -n meu-namespace


👉 Muito comum erro por estar no namespace errado.

🧹 Conteúdo 8 — Limpeza / Debug Rápido

Deletar pod:

kubectl delete pod nome-do-pod


Deletar deployment:

kubectl delete deploy app

📋 Checklist de Incidente Kubernetes (SRE Jr)
1. kubectl get pods
2. kubectl describe pod
3. kubectl logs --tail
4. kubectl exec (se necessário)
5. kubectl get deploy / svc

📌 Mapeamento Docker → Kubernetes (ajuda muito)
Docker	Kubernetes
container	pod
docker logs	kubectl logs
docker exec	kubectl exec
docker run	deployment
docker-compose	manifests / helm
