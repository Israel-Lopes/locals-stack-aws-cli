# Como utilizar AWS Lambada em command line

O que e AWS Lambda ?

AWS Lambda é um serviço de computação sem servidor oferecido pela Amazon Web Services (AWS).
Ele permite que você execute código de maneira escalável e eficiente, sem precisar 
provisionar ou gerenciar servidores. Com isso podemos garregar codigos em uma função lambda.
Ela pode ser assionada atraves de um mecanismo chamado trigger.

O tipo de trigger pode variar confome a necessidade que melhor se adequa com a situação.

Abaixo irei por alguns tipos de triggers:

1. Eventos do Amazon S3
2. Eventos do Amazon DynamoDB
3. Eventos do Amazon Kinesis
4. Eventos do Amazon SQS
5. API Gateway

Vamos agora criar nossa primeira função lambda. Para isso vamos usar o seguinte comando 
para criar nosso função lambda:

```bash
aws lambda create-function --function-name MyLambdaFunction \
  --runtime java11 \
  --role arn:aws:iam::000000000000:role/paper-test \
  --handler com.example.MyLambdaFunction::handleRequest \
  --code S3Bucket=my-bucket,S3Key=my-java-function.jar
```
Precisamos primeiro ter o **ARN**, **paper**, e **trust-policy.json**, seguimos:

Vamos criar nosso trust-policy.json em nossa maquina local, ele ira definir a politica de
confiança para nosso papel que termos que criar tambem. A politica de confiaça define quais
entidades tem permissão para assumir o papel.

Nosso arquivo ficara o seguinte trust-policy.json:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Agora com arquivo criado, podemos criar nosso papel:

```bash
aws iam create-role --role-name "paper-test" --assume-role-policy-document file://$HOME/trust-policy.json
```

Deve retornar o seguinte:

```json
{
    "Role": {
        "Path": "/",
        "RoleName": "paper-test",
        "RoleId": "AROAQAAAAAAANBXQQUMU3",
        "Arn": "arn:aws:iam::000000000000:role/paper-test",
        "CreateDate": "2023-05-11T20:20:52.968000Z",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "lambda.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        }
    }
}
```
O campo **Arn** que veio no retorno e o que devemos passar no parametro --role para criar a
lambda, inserimos o seguinte valor: ``arn:aws:iam::000000000000:role/paper-test``

Proximo passo e passar o nosso **jar** que sera executado na lambda, uma observação aqui
importante, nosso **jar** devemos ter que subir o jar para o **s3**, nisso devemos
especificar o nome do bucket s3 e nome do jar que la esta. Se voce ainda nao sabe criar
bucket s3, siga va para pagina de home e siga o tutorial de bucket.

Outra coisa importante e que nossa classe java que sera executada ela deve estender a
classe ``com.amazonaws.services.lambda.runtime.RequestHandler``, abaixo irei por um exemplo
da classe e nao esqueça de por o metodo tambem:

```java
import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;

public class MyLambdaFunction implements RequestHandler<String, String> {

    public String handleRequest(String input, Context context) {
        // Lambda function logic
        return "Hello, " + input + "!";
    }
}
```

Agora que preenchemos todos os parametros podemos executar o codigo para criar a lambda:

```bash
aws lambda create-function --function-name MyLambdaFunction \
  --runtime java11 \
  --role arn:aws:iam::000000000000:role/paper-test \
  --handler com.example.MyLambdaFunction::handleRequest \
  --code S3Bucket=my-bucket,S3Key=my-java-function.jar
```

Ao executar o comando caso obtenhamos sucesso, devemos receber a seguinte resposta:

```json
{
    "FunctionName": "MyLambdaFunction",
    "FunctionArn": "arn:aws:lambda:us-west-2:123456789012:function:MyLambdaFunction",
    "Runtime": "java11",
    "Role": "arn:aws:iam::000000000000:role/paper-test",
    "Handler": "com.example.MyHandler::handleRequest",
    "CodeSize": 123456,
    "Description": "",
    "Timeout": 3,
    "MemorySize": 128,
    ...
}
```
1. **--runtime java11:** Estamos especificando que sera executado em java 11.
2. **--role:** Esse parametro e usado para especificar a funçao ao IAM, ou seja estamos 
dando permissao. Nele devemos preencher com o **ARN**.
3. **--handler:** Deve especificar nome da classe e metodo.
4. **--code:** Especifique o bucket onde jar esta e nome do jar.

Agora com tudo terminado basta que o trigger seja assionado para que a função
lambda se inicie.
