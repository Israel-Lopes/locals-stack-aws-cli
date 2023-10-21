# Aprenda sorbe CodePipeline

 O que e CodePipeline ?

 O AWS CodePipeline é um serviço de automação de lançamento contínuo que ajuda a automatizar 
 e coordenar os processos de compilação, teste e implantação de aplicativos. 
 Ele permite criar pipelines de entrega contínua que ajudam a liberar software rapidamente, 
 de forma confiável e com baixo risco.

 Etapas do CodePipeline:

 1. **Etapa de origem (Source):** Nesta etapa, você especifica a origem do código-fonte, como um 
 repositório do AWS CodeCommit, GitHub ou Amazon S3.
 2. **Etapa de compilação (Build):** Nesta etapa, você realiza a compilação do código-fonte, 
 executando tarefas como compilação, empacotamento e criação de artefatos.
 3. **Etapa de teste (Test):** Nesta etapa, você pode executar testes automatizados para validar a 
 qualidade do código e a funcionalidade do aplicativo.
 4. **Etapa de implantação (Deploy):** Nesta etapa, você pode implantar o aplicativo em um 
 ambiente de produção ou de teste usando serviços como AWS Elastic Beanstalk, Amazon EC2, 
 AWS Lambda, entre outros.
 5. **Etapa de ação (Action):** Além das etapas mencionadas acima, você pode adicionar etapas 
 personalizadas que executam ações específicas necessárias para o seu fluxo de trabalho, como 
 notificações por e-mail, integração com ferramentas de terceiros, entre outros.

 Agora vamos criar nossa primeira pipeline:

```bash
aws codepipeline create-pipeline --pipeline-definition  \
    file://pipeline-definition.json \ 
    --cli-input-json file://pipeline-config.json
```
Atualizar pipeline:

```bash
aws codepipeline update-pipeline    \ 
    --pipeline-name MyPipeline  \
    --pipeline-definition file://pipeline-definition.json
```

Obter informacoes sobre status de execução do pipeline:

```bash
aws codepipeline get-pipeline-state --name MyPipeline
```
Listar a pipeline:

```bash
aws codepipeline list-pipelines
```

