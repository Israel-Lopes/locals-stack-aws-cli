# Aprendendo Cloud Watch

O que e Cloud Watch ?

O Amazon CloudWatch é um serviço de monitoramento e observabilidade fornecido pela Amazon Web 
Services (AWS). Ele permite coletar e monitorar métricas, logs e eventos de diversos recursos e 
serviços da AWS, fornecendo insights sobre o desempenho, a disponibilidade e a 
utilização dos recursos.

Com o CloudWatch, é possível criar painéis de controle personalizados para visualizar as métricas 
e os logs em um único lugar, definir alarmes para ser notificado quando uma métrica ultrapassar um
limite configurado e criar regras para acionar ações automáticas, como o dimensionamento 
automático de recursos.

Vamos agora aprender a criar um alarme com Cloud Watch.

Para criar o alarme basta executar:

```
aws cloudwatch put-metric-alarm \
  --alarm-name "MyEC2Alarm" \
  --alarm-description "Alarm for the number of running EC2 instances" \
  --metric-name "CPUUtilization" \
  --namespace "AWS/EC2" \
  --statistic "Average" \
  --period 300 \
  --threshold 80 \
  --comparison-operator "GreaterThanOrEqualToThreshold" \
  --dimensions "Name=InstanceId,Value=i-1234567890abcdef0" \
  --evaluation-periods 1 \
  --alarm-actions "arn:aws:sns:us-east-1:123456789012:MyTopic" \
  --ok-actions "arn:aws:sns:us-east-1:123456789012:MyTopic"
```

## Criando paineis

Crie o arquivo ``dashboad.json`` e insira o codigo abaixo:

```
{
  "widgets": [
    {
      "type": "metric",
      "x": 0,
      "y": 0,
      "width": 12,
      "height": 6,
      "properties": {
        "metrics": [
          [ "AWS/EC2", "CPUUtilization", "InstanceId", "i-1234567890abcdef0", { "period": 300, "stat": "Average" } ]
        ],
        "view": "timeSeries",
        "region": "us-east-1",
        "title": "EC2 CPU Utilization"
      }
    }
  ]
}
```
Agora use o comando:

``aws cloudwatch put-dashboard --dashboard-name MyDashboard --dashboard-body file://dashboard.json``

Neste exemplo, o comando cria ou atualiza um painel de controle chamado "MyDashboard" com um único
widget de gráfico que exibe a utilização da CPU de uma instância EC2 específica.

Você pode personalizar o arquivo JSON ou YAML para adicionar mais gráficos, métricas e 
propriedades de exibição conforme necessário.

