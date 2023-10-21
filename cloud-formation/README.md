# Aprenda a utilizar a CloudFormation

O AWS CloudFormation é um serviço da Amazon Web Services (AWS) que permite provisionar e 
gerenciar recursos de forma automatizada e consistente na nuvem. Ele usa arquivos de modelo, 
chamados de templates, para descrever a infraestrutura e os recursos desejados.

Em outras palavras a CloudFormation e uma ferramenta de automação de infra.

O objetivo principal do CloudFormation é permitir a criação e o gerenciamento de pilhas de 
recursos da AWS de maneira programática e escalável. Em vez de provisionar manualmente cada 
recurso, como instâncias EC2, bancos de dados RDS, filas do SQS, entre outros, o CloudFormation
permite que você defina esses recursos em um arquivo de modelo e os implante facilmente em sua
conta da AWS.

Os templates do CloudFormation são escritos em formato JSON ou YAML e descrevem a estrutura 
e as configurações dos recursos a serem criados.

Vamos agora aprender a implementalo.

Primeiro temos que criar nosso arquivo de template, ele quem vai dar as diretrizes de como sera
a nossa infra. Nome do nosso arquivo sera ``cloudformation-template.yaml``

```yaml
---
AWSTemplateFormatVersion: '2010-09-09'
Description: Exemplo de template CloudFormation

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-12345678
      KeyName: my-key-pair
      SecurityGroupIds:
        - sg-12345678
      UserData: |
        #!/bin/bash
        echo "Hello, World!"

  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-bucket
      AccessControl: Private

  MyDynamoDBTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: my-table
      AttributeDefinitions:
        - AttributeName: id
          AttributeType: N
        - AttributeName: name
          AttributeType: S
      KeySchema:
        - AttributeName: id
          KeyType: HASH
      ProvisionedThroughput:
        ReadCapacityUnits: 5
        WriteCapacityUnits: 5

Outputs:
  MyEC2InstanceIP:
    Value: !GetAtt MyEC2Instance.PublicIp
    Description: IP público da instância EC2

  MyS3BucketName:
    Value: !Ref MyS3Bucket
    Description: Nome do bucket S3

  MyDynamoDBTableName:
    Value: !Ref MyDynamoDBTable
    Description: Nome da tabela DynamoDB
```

Este exemplo de arquivo de template cria três recursos: uma instância EC2, um bucket do 
S3 e uma tabela do DynamoDB.

OBS: O arquivo deve conter em seu inicio o seguinte "---", este e um separador que tem a
utilidade de indicar o inicio do documento YAML. Isso garante a interpretação correta do file.

Agora com template criado vamos implantar nossa stack:

```bash
aws cloudformation create-stack \
  --stack-name MyStack \
  --template-body file://$HOME/cloudformation-template.yaml \
  --tags Key=Environment,Value=dev Key=Department,Value=IT
```
No exemplo dado, duas tags estão sendo adicionadas à pilha:
- `Key=Environment,Value=dev`: Essa tag indica que o ambiente da pilha é "dev", ou seja, é uma 
  pilha destinada ao desenvolvimento.
- ``Key=Department,Value=IT``: Essa tag indica que a pilha está associada ao departamento de TI.

A utilização das tags são úteis para fins de categorização, rastreamento de custos, 
gerenciamento de recursos e aplicação de políticas.

Agora podemos listar nossa stack: 

```bash
aws cloudformation describe-stacks
```

Um ponto de atenção, quando criamos nossa stack, automaticamente a CloudFormation provisiona
todos os nossos recursos que estao no arquivo de template, ou seja, tudo sera criado na hora.

Ficamos por aqui.