# Helm Charts

Este repositório contém [Helm charts](https://helm.sh/) para facilitar o deploy e gerenciamento de aplicações no Kubernetes.

## Estrutura do Projeto

- Cada diretório representa um chart Helm independente.
- Os charts podem ser utilizados para instalar aplicações, serviços ou componentes de infraestrutura.


## Como Usar

Os charts deste repositório são publicados automaticamente no GitHub Pages utilizando o [helm/chart-releaser-action@v1.6.0](https://github.com/helm/chart-releaser-action). Siga os passos abaixo para utilizar os charts publicados:

Inclua o chart como dependência em seu projeto GitOps:
   
   1. **Exemplo de Chart.yaml:**
   ```
    apiVersion: v2
    name: modulo
    description: Chart para publicação do módulo
    type: application
    version: 0.0.1
    dependencies:
    - name: <cloud-comp|cloud-comp-job>
      version: <informe-a-versao-do-chart>
      repository: https://albatis.github.io/helm-charts/
   ```
    
   2. **Exemplo de values.yaml do cloud-comp:**
   ```
    name: webapp
    login: ldapuser
    image: albatis/imagem:latest
    containerPort: 8000
    servicePort: 8080
    nodePort: 30000
    resources:
    cpu: 1m
    memory: 2Gi
    volumes:
    - name: volume1-pv
      storageClassName: default-storage-class
      size: 1Gi
      path: /mnt
      hostPath: /host/path/data/
      accessMode: ReadWriteMany
      ignore: true
      ignorePVC: true
    livenessProbe:
    httpGet:
        path: /healthz
        port: 8000
    readinessProbe:
    httpGet:
        path: /healthz
        port: 8000
    startupProbe:
    httpGet:
        path: /healthz
        port: 8000
   ```

3. **Exemplo de values.yaml do cloud-comp-job:**
   ```
    cloud-comp-job:
      name: job1
      login: ldapuser
      image: albatis/name-image:latest
      resources:
        cpu: 1m
        memory: 500Mi
      environments:
        ENVVAR1: "VALUE1"
        ENVVAR2: "VALUE2"
      commandInitContainer: "sleep 30;"
      volumes:
      - name: volume2-pv
        storageClassName: default-storage-class
        size: 1Gi
        path: /mnt
        hostPath: /host/path/
        accessMode: ReadWriteMany
        ignore: true
        ignorePVC: true
   ```

## Variáveis de Configuração

Abaixo estão as variáveis que serão preenchidas no arquivo values.yaml do chart:


### Variáveis do Chart `cloud-comp`

| Variável           | Tipo     | Obrigatório | Descrição                                                                                   |
|--------------------|----------|-------------|---------------------------------------------------------------------------------------------|
| `name`             | string   | Sim         | Nome base utilizado para nomear recursos (Deployment, ConfigMap, etc).                      |
| `replicas`         | int      | Não         | Número de réplicas do Deployment. Padrão: 1.                                                |
| `image`            | string   | Não         | Imagem do container a ser utilizada. Padrão: `nginx:latest`.                                |
| `containerPort`    | int      | Sim         | Porta exposta pelo container.                                                               |
| `servicePort`      | int      | Não         | Porta exposta pelo serviço Kubernetes (Service).                                            |
| `nodePort`         | int      | Não         | Porta NodePort para expor o serviço externamente.                                           |
| `resources.cpu`    | string   | Não         | Limite e requisição de CPU para o container. Exemplo: `"250m"`.                             |
| `resources.memory` | string   | Não         | Limite e requisição de memória para o container. Exemplo: `"256Mi"`.                        |
| `login`            | string   | Sim (se usar volumes) | Nome do usuário, usado para nomear volumes/persistentVolumeClaims.                |
| `volumes`          | lista    | Não         | Lista de volumes a serem montados.                                                          |
| `environments`     | map      | Não         | Mapa de pares chave/valor que serão inseridos como dados no ConfigMap e como envs no pod.   |
| `livenessProbe`    | objeto   | Não         | Configuração da livenessProbe do container. Exemplo: `httpGet`, `initialDelaySeconds`, etc. |
| `readinessProbe`   | objeto   | Não         | Configuração da readinessProbe do container.                                                |
| `startupProbe`     | objeto   | Não         | Configuração da startupProbe do container.                                                  |

---

### Variáveis do Chart `cloud-comp-job`

| Variável           | Tipo     | Obrigatório | Descrição                                                                                   |
|--------------------|----------|-------------|---------------------------------------------------------------------------------------------|
| `name`             | string   | Sim         | Nome base utilizado para nomear recursos (Job, ConfigMap, etc).                             |
| `image`            | string   | Não         | Imagem do container a ser utilizada. Padrão: `nginx:latest`.                                |
| `containerPort`    | int      | Não         | Porta exposta pelo container (se aplicável).                                                |
| `resources.cpu`    | string   | Não         | Limite e requisição de CPU para o container. Exemplo: `"250m"`.                             |
| `resources.memory` | string   | Não         | Limite e requisição de memória para o container. Exemplo: `"256Mi"`.                        |
| `login`            | string   | Não         | Nome do usuário, usado para nomear volumes/persistentVolumeClaims.                          |
| `volumes`          | lista    | Não         | Lista de volumes a serem montados. Cada item deve conter `name` (string) e `path` (string). |
| `environments`     | map      | Não         | Mapa de pares chave/valor que serão inseridos como dados no ConfigMap e como envs no pod.   |
| `command`          | lista    | Não         | Comando de entrada do Job. Exemplo: `["python", "script.py"]`                               |
---

#### Detalhamento das propriedades de `volumes`

| Propriedade         | Tipo     | Obrigatório | Descrição                                                                                   |
|---------------------|----------|-------------|---------------------------------------------------------------------------------------------|
| `name`              | string   | Sim         | Nome do volume.                                                                             |
| `storageClassName`  | string   | Não         | Nome da StorageClass a ser utilizada para o PVC.                                            |
| `size`              | string   | Não         | Tamanho do volume (ex: `1Gi`).                                                              |
| `path`              | string   | Sim         | Caminho onde o volume será montado no container.                                            |
| `hostPath`          | string   | Não         | Caminho no host para montar como volume (usado para volumes do tipo hostPath).              |
| `accessMode`        | string   | Não         | Modo de acesso do PVC (ex: `ReadWriteOnce`, `ReadWriteMany`).                               |
| `ignore`            | bool     | Não         | Se verdadeiro, ignora a criação do PersistentVolume no template.                            |
| `ignorePVC`         | bool     | Não         | Se verdadeiro, ignora a criação do PersistentVolumeClaim para este volume.                  |
