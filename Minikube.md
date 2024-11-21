
# Configuração do Ambiente do Desafio

## Descrição do Ambiente

Neste desafio, utilizei o **Minikube** como cluster Kubernetes local para facilitar o desenvolvimento, teste e validação da solução. A configuração foi realizada no Windows com suporte ao Docker como driver.

---

## Passo a Passo para Configuração do Ambiente

### Pré requisitos
- SSH
- Git 
- Docker
- Choco

Para Instalar o Choco, basta executar o comando de instalação como administrador no PowerShell:
```bash
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### 1. Instalação do Minikube

Baixe e instale o Minikube com o comando abaixo:

```bash
choco install minikube -y
```
---

### 2. Configuração do Driver do Minikube

O Minikube foi configurado para usar o driver Docker:

```bash
minikube start --driver=docker --cpus=2 --memory=3800 --disk-size=50g
```

Isso garante que o cluster seja executado localmente utilizando containers Docker.

Limitei a utilização de memória e CPU, pois não tenho muitos recursos disponíveis, mas fique à vontade para alocar os recursos que achar necessário.

---

### 3. Verificar o Status do Cluster

Após a inicialização, execute o seguinte comando para verificar o status do cluster:

```bash
minikube status
```

---

### 4. Instalação do `kubectl`

O `kubectl` foi usado para gerenciar o cluster. Para instalá-lo:

```bash
choco install kubernetes-cli -y
```

Certifique-se de que ele esteja configurado corretamente:

```bash
kubectl version --client
```

---

### 5. Acessar o Dashboard do Minikube (Opcional)

Para monitorar o cluster visualmente, utilize:

```bash
minikube dashboard
```

---

### 6. Configurar Acesso ao Cluster

O comando abaixo foi usado para configurar o `kubectl` para usar o contexto do Minikube:

```bash
kubectl config use-context minikube
```

---

### 7. Configurar Permissões Iniciais (Opcional)

Foi criada uma namespace específica chamada `sre-challenge` para isolar os recursos do desafio:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: sre-challenge
```

Para aplicar essa configuração:

```bash
kubectl apply -f namespace.yaml
```

### 8. Para Acessar a aplicação:

O Minikube usando o driver Docker. O comportamento esperado é que o NodePort funcione apenas dentro do cluster Kubernetes ou através de ferramentas como o minikube service ou kubectl port-forward, então utilize o comando abaixo para testar a aplicação.

``` bash
kubectl port-forward -n sre-challenge service/sre-challenge-app 30000:8080
```
