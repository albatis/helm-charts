# helm-charts
Esse projeto foi feito baseado na documentação disponível em https://helm.sh/docs/howto/chart_releaser_action/

## Uso do Helm-Chart cloud-comp

Para usar esse Helm-Chart crie um repositório com os arquivos abaixo:

Chart.yaml
```
apiVersion: v2
name: modulo
description: Chart para publicação do módulo
type: application
version: 0.0.1
dependencies:
  - name: cloud-comp
    version: 0.0.1
    repository: https://albatis.github.io/helm-charts/
```

values.yaml
```
cloud-comp:
    nome: primeiro-deploy
```
