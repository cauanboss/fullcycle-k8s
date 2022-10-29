# Create cluster
kind create cluster --config=k8s/kind.yaml --name fullcycle

# Add to monitoring cluster with context
kubectl cluster-info --context kind-fullcycle

# Start and update version with relicaSet
Para atualizar é preciso deletar o pod e recrialo

# Rollback version deployment
kubectl rollout history deployment goserver
kubectl rollout undo deployment goserver
kubectl rollout undo deployment goserver --revision-to=<number>

# Port forward
kubectl port-forward service/goserver 8000:80

# Service
Porta acessada de fora
port: 8000
Redirect para o serviço
targetPort: 80

# Proxy
kubectl proxy --port=8080
Após comando executado acesse via navegador e veja o conteudo das APIs kubernetes

# Variable de ambiente
Dentro do deployment tem a opçõa env

# Config map Variavel de ambiente
Uma nova configuração yaml com o kind do tipo ConfigMap

# Create volume end attach a files
Para isso é necessário utilizar o configMap. E dentro do deployment é configurado a criação de volumes para transformar o configMap em arquivo

# Create secret in ENV
Para isso é preciso criar um arquivo yaml do tipo (Kind) Secret

# Health checks
dentro do deploy usando a opção readinessProbe
dentro do deploy usando a opção livenessProbe
Problema ao usar os readinessProbe e livenessProbe em conjunto
Para fazer funcionar tem que usar o "initialDelaySeconds" para que um não restarte o outro fazendo nunca iniciar 

# Health check
Evitar problemas de inicialização ou reinicialização entre os redminess e liveness.
Utilize a configuração startupProbe (Feito para resolver o problema de sincronia entre os dois)

# Auto Scaling HPA usando as METRICS-SERVER
fazer o download do metrics-server
https://github.com/kubernetes-sigs/metrics-server
e depois faça o wget no component e depois faça o kubectl apply no component

# Usando a configuração Recursos (resource)
A opção resource você define o minimo para rodar um pod e o máximo

# Usando HPA HorizontalPodAutoscaler
Este tipo (Kind) no Kubernetes serve para fazer o auto scaling

# Ferramenta para testar performace e fazer o HPA funcionar (fortio, k6)
Nome da ferrarmenta é a fortio e o comando usado é:
kubectl run -it fortio --rm --image=istio/fortio -- load -t 120m -qps 800 http://goserver-service/healthz

# Volume perssistente usando PersistentVolumeClaim
Caso precise salvar dados persistentes pode-se criar um volume persistente usando a
configuração do k8s que é o Kind PersistentVolumeClaim 
-- Ler sobre as opções do accessModes
kubectl get storageclass

# Como fazer o bind do volume criado ?
Dentro do deployment na marte de volumes é adicionado o vinculo com o PersistentVolumeClaim 
Depois adicionar em volumeMounts dentro do deployment

# Stateless & Stateful
Stateless não salva histórico para uma possivel re-analise do processo, e caso de um erro na chamada do quer que seja, tera que re-executa-lo.
Stateful salva históprico e pode re-analisar os processos.

# Usando StatefulSet
Quando utiliza o StatefulSet no Kind do k8s ele consegue ordenar uma criação caso precise que o primeiro seja o master e o restante se comuniquem. E quando diminui replicas ele diminui do fim para o primeiro