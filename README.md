# Simulando AWS CLI com LocalStack

Dependencias:

 - AWS CLI
 - Docker
 - Python 2.7.x ou o Python 3.4
 - python3-pip
 - python3-venv

Para simularmos ambiente da aws de forma local e muito simples, mais primeiro  temos
que instalar o ***AWS CLI*** sigamos abaixo:

Instale o AWS CLI usando um método apropriado para o seu sistema operacional.

![download aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-configure-quickstart-install

Como meu sistema e Linux irei seguir outros passos a seguinte:

Caso nao tenha o python na maquina basta seguir

``sudo apt-get update && sudo apt-get install python3 && sudo apt install python3-pip``

Agora instale o AWS CLI usando ambiente virtual.

Certifique-se de ter o pacote python3-venv instalado executando o seguinte comando:

``sudo apt install python3-venv``

Crie um novo diretório onde você deseja criar o ambiente virtual. Por exemplo:

``mkdir myenv``

Entre no diretorio ``cd myenv``

Crie um ambiente virtual usando o seguinte comando: ``python3 -m venv venv``

Ative o ambiente virtual executando o comando, sempre que o você quiser rodar AWS CLI, tera
que rodar esse comando do ambiente virtual:

``source venv/bin/activate``

depois basta instalar awscli de forma isolada para que nao corra risco de 
conflitos de dependencias.

``pip install awscli``


Lembre-se de que, sempre que precisar usar a AWS CLI, você primeiro precisará ativar o 
ambiente virtual usando o comando ``source venv/bin/activate``.

Após a instalação ser concluída, você pode verificar se a AWS CLI foi instalada corretamente 
digitando o seguinte comando:

``aws --version``

1. Instale o Docker em seu ambiente local, se ainda não estiver instalado.
2. Baixe e instale o LocalStack usando o Docker Hub. Por exemplo, você pode 
executar o seguinte comando no terminal:

``docker run --rm -it -p 4566:4566 -p 4571:4571 localstack/localstack``

Isso iniciará o contêiner LocalStack e exporá as portas 4566 e 4571,
que são usadas para acessar os serviços simulados.

Agora, você pode usar o AWS CLI ou SDKs para se conectar ao LocalStack e interagir 
com os serviços simulados. Certifique-se de configurar a região para 
"us-east-1" e o endpoint para "http://localhost:4566" ao usar o AWS CLI ou SDKs.

Por exemplo, para listar os buckets do Amazon S3 usando o AWS CLI:

``aws --endpoint-url=http://localhost:4566 s3 ls``

Esse comando ira dar erro pois antes temos que configurar aws-cli em nossa maquina. Sigamos ao
proximo passo.

## Configuração do AWS CLI

Agora temos que configurar aws-cli antes de utilizalo.

Execute o comando ``aws configure``

Ele solicitará as seguintes informações:

 - AWS Access Key ID: Insira o ID da chave de acesso da sua conta da AWS.
 - AWS Secret Access Key: Insira a chave de acesso secreta correspondente à chave de acesso 
    ID fornecida anteriormente.
 - Default region name: Insira a região da AWS que deseja usar (por exemplo, 
    "us-east-1" ou "eu-west-1").
 - Default output format: Escolha um formato de saída preferido, como "json", "text" ou "table".
 
 Após inserir todas as informações necessárias, as credenciais serão configuradas e
 armazenadas em um arquivo de configuração no diretório do AWS CLI.

Agora você pode começar a usar a LocalStack para interagir com os serviços da AWS.
Por exemplo, você pode listar os buckets do Amazon S3 usando o seguinte comando:

Como seria com aws-cli: ``aws s3 ls``

Com local-stack: ``aws --endpoint-url=http://localhost:4566 s3 ls``

Isso retornará uma lista dos buckets disponíveis na sua conta da AWS.

Cada serviço da AWS tem seus próprios comandos e opções específicas que podem
ser exploradas na documentação oficial da AWS CLI.

