# helm-charts
helm-charts

=======
# Helm Charts

Este repositório contém charts Helm para facilitar o deploy e gerenciamento de aplicações no Kubernetes.

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
    
   2. **Exemplo de values.yaml:**
   ```
    cloud-comp-job:
      name: job-rules
      login: alexandrevieira
    ...
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
| `resources.cpu`    | string   | Não         | Limite e requisição de CPU para o container. Exemplo: `"250m"`.                             |
| `resources.memory` | string   | Não         | Limite e requisição de memória para o container. Exemplo: `"256Mi"`.                        |
| `login`            | string   | Sim (se usar volumes) | Nome do usuário, usado para nomear volumes/persistentVolumeClaims.                  |
| `volumes`          | lista    | Não         | Lista de volumes a serem montados. Cada item deve conter `name` (string) e `path` (string). |
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