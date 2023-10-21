# Como utilizar CodeBuild

O que e CodeBuild ?

O AWS CodeBuild é um serviço de compilação gerenciado pela AWS que compila, testa e empacota 
seu código automaticamente em ambientes escaláveis e eficientes. Ele oferece um ambiente de 
compilação totalmente gerenciado, eliminando a necessidade de provisionar e gerenciar 
seus próprios servidores de compilação.

Para utilizar o AWS CodeBuild, é necessário definir um projeto de compilação, que especifica 
as configurações, como ambiente de compilação, localização do código-fonte, fases de 
compilação e ações de pós-compilação. O projeto de compilação é então usado para iniciar e 
executar as compilações. O CodeBuild é integrado com serviços como o AWS CodeCommit e o 
AWS CodePipeline para facilitar a configuração de fluxos de trabalho de CI/CD completos.

Listar os projetos de compilação que existem:

```bash
aws codebuild list-projects
```

Para criar um novo projeto:

```bash
aws codebuild create-project \
  --name MyBuildProject \
  --source "type=CODECOMMIT,location=https://git-codecommit.us-east-1.amazonaws.com/v1/repos/MyRepository,sourceVersion=branch-name" \
  --environment "type=LINUX_CONTAINER,image=aws/codebuild/standard:4.0" \
  --service-role MyServiceRole \
  --artifacts "type=NO_ARTIFACTS"
```
Inicia compilaçao de um projeto existente:

```bash
aws codebuild start-build --project-name MyBuildProject
```

Antes de executar o comando ***start-build***, você precisa ter configurado a branch desejada no
projeto do CodeBuild. Isso pode ser feito ao criar ou atualizar o projeto utilizando o comando 
``aws codebuild create-project`` ou ``aws codebuild update-project``, especificando a branch no 
parâmetro ***--source*** ou ***--source-version***.

Por exemplo, se você deseja iniciar uma compilação na branch "develop" do seu projeto 
"MyBuildProject", certifique-se de que o projeto tenha sido configurado corretamente com a 
branch correta. Em seguida, você pode iniciar a compilação usando o comando:

```bash
aws codebuild start-build --project-name MyBuildProject
```

O CodeBuild usará as configurações do projeto para determinar a branch e iniciar a 
compilação correspondente.

