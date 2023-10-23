[<<___VOLTAR](../README.md)

# Aprenda a utilizar CodeDeploy

O AWS CodeDeploy é um serviço de implantação totalmente gerenciado que facilita a implantação 
de aplicativos de forma automatizada em diferentes ambientes, como instâncias do Amazon EC2, 
serviços do ECS (Elastic Container Service), servidores locais ou até mesmo em instâncias 
do Lambda.

Iremos agora criar nossa implantação.

Crie um grupo de implantação: Um grupo de implantação é uma coleção lógica de instâncias do 
Amazon EC2 nas quais você deseja implantar seu aplicativo. Use o seguinte comando para criar 
um grupo de implantação:

```bash
aws deploy create-deployment-group  \
    --application-name <nome-da-aplicacao>  \
    --deployment-group-name <nome-do-grupo-de-implantacao>  \
    --service-role-arn <ARN-do-papel>   \ 
    --ec2-tag-filters Key=<nome-da-chave>,Type=<tipo-da-chave>,Value=<valor-da-chave>
```

Crie uma revisão de implantação: A revisão de implantação contém o arquivo de implantação do 
seu aplicativo e outras informações relevantes. Use o seguinte comando para criar uma 
revisão de implantação:

```bash
aws deploy create-deployment --application-name \
    <nome-da-aplicacao> \
    --deployment-group-name <nome-do-grupo-de-implantacao> \
    --s3-location bucket=<nome-do-bucket>,  \
        bundleType=<tipo-do-pacote>, \
        key=<caminho-para-o-arquivo-de-implantacao>
```

Inicie a implantação: Use o seguinte comando para iniciar a implantação do seu aplicativo:

```bash
aws deploy create-deployment \
    --application-name <nome-da-aplicacao> \
    --deployment-group-name <nome-do-grupo-de-implantacao> \
    --s3-location bucket=<nome-do-bucket>, \
    bundleType=<tipo-do-pacote>, \
    key=<caminho-para-o-arquivo-de-implantacao>
```

[<<___VOLTAR](../README.md)