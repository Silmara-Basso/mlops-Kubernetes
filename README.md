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
git push
````

### Ao fazer o push para o repositório remoto (no GitHub), o GitHub Actions executará automaticamente o mesmo fluxo para validar, treinar o modelo e gerar uma nova imagem Docker, deixando tudo pronto para o deploy.


### Se quiser, execute os comandos abaixo para atualizar a imagem em tempo real no cluster Kubernetes local:
1) No app.py em st.sidebar.write acrescente "testando atualização v1.1 agora"
Atualize a app e recrie a imagem
````
docker build -t qc-app:v1.1 .
````
2) Atualize a imagem no cluster Minikube:
````
minikube image load qc-app:v1.1
````
3) Atualize seu Deployment YAML com a nova versão da imagem
Edite k8s/deployment.yaml e modifique esta linha:
````
image: qc-app:v1.1
````
4) Aplique novamente o Deployment Kubernetes:
````
kubectl apply -f k8s/deployment.yaml
````
5) Confirme se a atualização foi bem-sucedida:
```
kubectl get pods
````


### Use os comandos abaixo para desativar o ambiente virtual o ambiente (opcional):
````
deactivate
````

![App](/images/app.png)


