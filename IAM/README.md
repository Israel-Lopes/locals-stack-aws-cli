# Como utilizar o IAM

O que e IAM ?

O IAM (Identity and Access Management) é um serviço da AWS que permite gerenciar o acesso aos 
recursos e serviços da AWS de forma segura. Ele fornece controle granular sobre permissões e 
políticas de acesso para usuários, grupos, funções e serviços dentro da sua conta da AWS.

O IAM permite que você crie e gerencie identidades (usuários, grupos e funções) e atribua 
permissões específicas a essas identidades. Alguns dos principais recursos e 
funcionalidades do IAM incluem:

1. Gerenciamento de usuários: Você pode criar usuários IAM e fornecer credenciais 
de login para eles. Cada usuário terá um conjunto de credenciais exclusivas 
para autenticar-se na conta da AWS.

2. Grupos: Você pode organizar usuários em grupos lógicos e atribuir permissões em nível de grupo. 
Isso simplifica a administração de permissões, pois você pode adicionar ou remover usuários de 
um grupo e as permissões serão aplicadas automaticamente.

3. Funções do IAM: As funções do IAM permitem que você conceda permissões temporárias a serviços e 
aplicativos da AWS. Isso é útil quando você deseja permitir que um serviço acesse 
outros recursos em sua conta em seu nome, sem precisar compartilhar credenciais de acesso. 

4. Políticas de IAM: As políticas do IAM são documentos JSON que definem as permissões para 
usuários, grupos e funções. Você pode criar políticas personalizadas para atender aos requisitos 
específicos de acesso e atribuir essas políticas às identidades.

5. Integração com outros serviços: O IAM se integra a vários serviços da AWS, permitindo que você 
controle o acesso a recursos específicos nesses serviços. Por exemplo, você pode definir políticas 
de acesso para buckets do S3, instâncias do EC2, filas do SQS e muitos outros.

## Criando usuario no IAM

Agora vamos aprender a criar nosso primeiro usuario no IAM, basta seguir o comando abaixo:

``aws iam create-user --user-name <nome-do-usuario>``

Podemos agora listar nosso usuario: ``aws iam list-users``

Depois do usuario criado, agora podemos conceder acessos a ele. Segue exemplo:

``aws iam attach-user-policy --user-name <nome-do-usuario> --policy-arn arn:aws:iam::aws:policy/MinhaPolitica``

Substitua ``<nome-do-usuario>`` pelo nome do usuário que você criou e 
``arn:aws:iam::aws:policy/MinhaPolitica`` pelo ARN (Amazon Resource Name) da 
política que deseja associar.

Abaixo segue uma lista de politicas mais comuns para por:

 - AmazonS3FullAccess: Concede acesso completo aos serviços de armazenamento do Amazon S3.
 - AmazonEC2FullAccess: Concede acesso completo aos serviços do Amazon EC2(Elastic Compute Cloud).
 - AmazonDynamoDBFullAccess: Concede acesso completo aos serviços do Amazon DynamoDB.
 - AmazonRDSFullAccess: Concede acesso completo aos serviços do Amazon RDS (Relational Database Service).
 - AmazonSQSFullAccess: Concede acesso completo aos serviços do Amazon SQS (Simple Queue Service).
 - AmazonSNSFullAccess: Concede acesso completo aos serviços do Amazon SNS (Simple Notification Service).
 - AWSLambdaFullAccess: Concede acesso completo aos serviços do AWS Lambda.
 - AWSReadOnlyAccess: Concede acesso somente leitura a uma ampla variedade de serviços da AWS.
 
 Caso queira ver mais politicas acesse o link [politicas](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies)
Proceguindo, quero dar acesso ao usuario Luis para ter acesso ao Dynamo, nesse caso:

``aws iam attach-user-policy --user-name Luis --policy-arn arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess``

Agora vamos litar os acessos do nosso usuario Luis:

``aws iam list-attached-user-policies --user-name Luis``

Ele ira retornar o seguinte:

```
{
    "AttachedPolicies": [
        {
            "PolicyName": "AmazonDynamoDBFullAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess"
        }
    ]
}
```
Quero remover o acesso que eu avia dado ao usuario, como faz agora?
Basta seguir o exemplo, vamos remover o acesso que acabos de dar ao Luis:

``aws iam detach-user-policy --user-name Luis --policy-arn arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess``

Listando os acesso do usuario, ele deve retornar:

```
{
    "AttachedPolicies": []
}
```

Dessa forma, acabamos de desanexar o acesso ***AmazonDynamoDBFullAccess*** ao usuario Luis.
Seguimos em frente agora.

Agora vamos deletar o usuario Luis:

``aws iam delete-user --user-name Luis``

## Finish

Aprendemos aqui a criar usuarios, setar politicas de acessos, removelas e deletar usuarios.
Existem mais funcionalidades que o IAM pode oferecer, porem apresentei apenas as basicas.
