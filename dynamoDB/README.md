# Como utilizar DynamoDB

O que e DynamoDB ?

O Amazon DynamoDB é um serviço de banco de dados NoSQL (não relacional) totalmente gerenciado 
oferecido pela Amazon Web Services (AWS). Ele fornece armazenamento escalável e de alto desempenho
para aplicativos que precisam de acesso rápido e previsível aos dados, independentemente 
do tamanho da carga de trabalho.

Agora vamos aprender a criar nosso dynamoDB!

## Como criar uma tabela

Para criar uma tabela basta fazer o seguinte comando:

```bash
aws dynamodb create-table \
    --table-name Orders \
    --attribute-definitions \
        AttributeName=id,AttributeType=S \
        AttributeName=sid,AttributeType=S \
    --key-schema \
        AttributeName=id,KeyType=HASH \
        AttributeName=sid,KeyType=RANGE \
    --provisioned-throughput \
        ReadCapacityUnits=5,WriteCapacityUnits=5 
```

Agora se listarmos, poderemos ver que a Orders foi criada:

```bash
aws dynamodb list-tables
```

Retorno:

```json
{
    "TableNames": [
        "Orders"
    ]
}
```
Vamos listar a tabela Orders agora:

```bash
aws dynamodb scan --table-name Orders
```

Retorno:

```json
{
    "Items": [],
    "Count": 0,
    "ScannedCount": 0,
    "ConsumedCapacity": null
}
```
Vamos aprender agora a utilizar o dynamo com java. Primeiro temos que criar um arquivo de
configuração chamado ``credentials``, no linux ele deve ser criado no 
diretorio ``~/.aws/credentials`` ou no mac OS.

```yaml
[<profile_name>]
aws_access_key_id = SUA_ACCESS_KEY_ID
aws_secret_access_key = SUA_SECRET_ACCESS_KEY
```
O arquivo pode ter varios profiles configurados, basta separalos pulando uma linha.
Ficando dessa forma:

```yaml
[default]
aws_access_key_id = SUA_ACCESS_KEY_ID_DEFAULT
aws_secret_access_key = SUA_SECRET_ACCESS_KEY_DEFAULT

[profile1]
aws_access_key_id = SUA_ACCESS_KEY_ID_PROFILE1
aws_secret_access_key = SUA_SECRET_ACCESS_KEY_PROFILE1

[profile2]
aws_access_key_id = SUA_ACCESS_KEY_ID_PROFILE2
aws_secret_access_key = SUA_SECRET_ACCESS_KEY_PROFILE2
```
Vamos dar continuidade ao que estavamos fazendo.

Voce deve preencher com suas credenciais e perfil, no meu exemplo o perfil irei por dev.
Ficando da seguinte forma:

```yaml
[dev]
aws_access_key_id = **********
aws_secret_access_key = *********
```
Caso voce nao tenha um profile configurado, podera criar um seguindo o passo:

Abra o arquivo ``~/.aws/config`` com editor de texto, preencha com informaçoes abaixo:
```
[profile dev]
region = us-west-2
```
Voce pode subistituir a regiao de acordo com sua preferencia.

Agora podemos listar nosso profile: 

```bash
aws s3 ls --profile dev
```

Implementando a classe para dynamo:

```java
import software.amazon.awssdk.auth.credentials.*;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.dynamodb.DynamoDbClient;
import software.amazon.awssdk.services.dynamodb.model.*;

public class DynamoDBExample {

    public static void main(String[] args) {

        String profileName = "dev"; // Nome do perfil que você deseja usar

        AwsCredentialsProvider credentialsProvider = ProfileCredentialsProvider.builder()
                .profileName(profileName)
                .build();

        DynamoDbClient dynamoDbClient = DynamoDbClient.builder()
                .region(Region.US_EAST_1) // Especifique a região desejada
                .credentialsProvider(credentialsProvider)
                .build();

        String tableName = "Orders"; // Nome da tabela no DynamoDB

        // Crie uma expressão de consulta
        String expression = "#id = :value";
        String attributeName = "#id";
        String attributeValue = "123";

        QueryRequest queryRequest = QueryRequest.builder()
                .tableName(tableName)
                .keyConditionExpression(expression)
                .expressionAttributeNames(Collections.singletonMap(attributeName, "id"))
                .expressionAttributeValues(Collections.singletonMap(":value", AttributeValue.builder().s(attributeValue).build()))
                .build();

        // Execute a consulta
        QueryResponse queryResponse = dynamoDbClient.query(queryRequest);

        // Processar os resultados da consulta
        List<Map<String, AttributeValue>> items = queryResponse.items();
        for (Map<String, AttributeValue> item : items) {
            // Processar cada item retornado
            // Acesse os atributos do item usando item.get("nomeDoAtributo")
        }

        dynamoDbClient.close();
    }
}
```

Import as dependencias ao **pom.xml**:

```xml
<dependencies>
    <dependency>
        <groupId>software.amazon.awssdk</groupId>
        <artifactId>dynamodb</artifactId>
    </dependency>
    <dependency>
        <groupId>software.amazon.awssdk</groupId>
        <artifactId>credentials</artifactId>
    </dependency>
</dependencies>
```
