# mlops-Kubernetes
Lab de Otimização e Escalabilidade Para Pipeline de Cl/CD com Kubernetes


# Versionamento e Controle de Dados em Pipelines CI/CD com Github Actions e Kubernetes

- Crie um ambiente virtual:
````
python3 -m venv qcvenv
source qcvenv/bin/activate
pyenv local 3.12
pip install pip
pip install -r requirements.txt 
````
- Garanta que o Docker Desktop esteja instalado

- Instale o Kubectl na sua máquina host
https://kubernetes.io/pt-br/docs/tasks/tools/
````
brew install kubectl
````
Valide a instalação checando a versão com o comando:
````
kubectl version --client
````

- Instale o minikube na sua máquina host.
https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fbinary+download
````
brew install minikube
````
````
brew unlink minikube
brew link minikube
````
````
minikube version
````
- Inicia o cluster Kubernetes local com Minikube
````
minikube start
````

- Treine e salve a versão inicial do modelo
````
python treinamento/treina_modelo.py
````

- Construa a imagem Docker localmente após treinar o modelo
````
docker build -t qc-app:latest .
````

- Carregue a imagem Docker para o Minikube
````
minikube image load qc-app:latest
````

- Aplica o manifesto YAML para criar o deployment Kubernetes
````
kubectl apply -f k8s/deployment.yaml 
````

- Aplica o manifesto YAML para criar o serviço Kubernetes
````
kubectl apply -f k8s/service.yaml 
````

- Verifique os pods criados
````
kubectl get pods
````

- Acesse a app via web
````
minikube service qc-app-service
````

- Execute os comandos abaixo para atualizar o repositório Git:
````
git add .
git commit -m ":tada: Commit full"
gi push
````

# Execute os comandos abaixo para atualizar a imagem em tempo real no cluster Kubernetes:

# Carrega a nova imagem no Minikube
minikube image load qc-app:latest

# Atualiza o deployment no Kubernetes para usar a imagem nova
kubectl set image deployment/qc-app-deployment qc-app-container=qc-app:latest

# Esses passos acima devem ser executados sempre que houver mudança no código de treinamento do modelo e então o Pipeline CI/CD será executado.

# A partir daqui os passos são:

# 1- Modificar ou atualizar script de modelo, app ou dados.
# 2- Commit e envio das alterações (git add, git commit e git push ou act push).
# 3- Executar act push para testar localmente se o fluxo de CI/CD (treinamento, build da imagem Docker e validação Kubernetes) funciona corretamente.

# Depois de garantir que tudo funciona corretamente no ambiente local com o Act, ao fazer o push para o repositório remoto (no GitHub, por exemplo), o GitHub Actions executará automaticamente o mesmo fluxo para validar, treinar o modelo e gerar uma nova imagem Docker, deixando tudo pronto para o deploy.

# Use os comandos abaixo para desativar o ambiente virtual e remover o ambiente (opcional):

### Use os comandos abaixo para desativar o ambiente virtual o ambiente (opcional):
````
deactivate
````

![App](/images/app.png)


