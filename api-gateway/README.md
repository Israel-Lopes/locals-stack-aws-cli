# Aprendendo a utilizar API Gateway

O que e API Gateway ?

O AWS API Gateway é um serviço gerenciado pela Amazon Web Services (AWS) que permite criar, 
publicar, monitorar e proteger APIs (Interfaces de Programação de Aplicativos) 
para aplicativos web e móveis.

Ele atua como um ponto de entrada para as APIs, fornecendo recursos para controlar o tráfego, 
a segurança e a transformação de dados.

Dando continuidade vamos criar um arquivo de json de configuração que se chamara:

``api-config.jon``

Nele tera o conteudo:

```
{
  "name": "MyAPI",
  "description": "Minha API Gateway",
  "protocolType": "REST",
  "routeSelectionExpression": "$request.method $request.path",
  "apiKeySource": "HEADER",
  "disableExecuteApiEndpoint": false
}
```
Agora basta executar o comando:

``aws apigatewayv2 create-api --cli-input-json file://api-config.json``

Isso ira criar a API Gateway com base nas configuracoes descritas no arquivo. Depois disso,
devemos adicionar as rotas a API Gateway:

```
{
  "routes": [
    {
      "routeKey": "GET /resource",
      "target": "integrations/<INTEGRATION_ID>/routes/GET_resource"
    },
    {
      "routeKey": "POST /resource",
      "target": "integrations/<INTEGRATION_ID>/routes/POST_resource"
    }
  ]
}
```
Lembre-se de subistituir <INTEGRATION_ID> pelos IDs reais de integração que voce configurou
para cada rota.

Ficando ela dessa forma:

```
{
  "routes": [
    {
      "routeKey": "GET /users",
      "target": "integrations/abc123/routes/GET_users"
    },
    {
      "routeKey": "POST /users",
      "target": "integrations/def456/routes/POST_users"
    },
    {
      "routeKey": "GET /products",
      "target": "integrations/ghi789/routes/GET_products"
    },
    {
      "routeKey": "PUT /products/{id}",
      "target": "integrations/jkl012/routes/PUT_products"
    }
  ]
}
```
1. routeKey:  Especifica o método HTTP e o caminho da rota. Por exemplo, "GET /resource" indica 
que essa rota será acionada quando uma solicitação GET for feita para o caminho "/resource".
2. target:  Indica o destino da rota, ou seja, para onde a solicitação será encaminhada.


Depois de tudo preenchido, devemos executar o comando para subir as configuracoes:

``aws apigatewayv2 import-api --api-id <API_ID> --basepath <BASE_PATH> --body file://routes-config.json``

Substitua <API_ID> pelo ID da API Gateway que você obteve na etapa anterior e <BASE_PATH> pelo 
caminho de base da API (por exemplo, /v1).

Agora temos uma API Gateway criada e configurada com as rotas.
