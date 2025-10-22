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
