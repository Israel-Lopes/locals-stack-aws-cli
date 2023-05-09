
# Como utilizar bucket s3 com Localstack

O que Bucket s3?

Bucket s3 (Amazon Simple Storage Service) e um container de armazenamento na nuvem. Uma boa
analogia que podemos fazer e que o bucket s3 seria um diretorio em ambiente linux em que voce
pode criar os diretorios que seriam o bucket, e dentro do diretorios os arquivos de dados que
voce queira armazenar. Nao precisa de muita atenção para perceber que os comandos do bucket sao
semelhante aos comandos de listar e deletar no linux.

Agora seguimos com o tutorial.

<br />

Aqui vamos aprender:

 - criar bucket
 - subir arquivos ao bucket
 - listar buckets e arquivos
 - deletar arquivos do bucket

Para facilitar o aprendizado e fica semelhante ao serviço da amazon, vamos criar uma alias.

Dentro dou seu .bashrc ou outro de preferencia, cole o seguinet codigo.

``alias aws="aws --endpoint-url=http://localhost:4566"``

Nao se esqueça tambem de carregar o arquivo: ``source ~/.bashrc``

Dessa forma estamos sobrescrevendo o comando da aws para com o do localstack. Agora ao inves de
fazermos:

``aws --endpoint-url=http://localhost:4566 s3 mb s3://my-bucket-to-test``

Agora sera dessa forma: ``aws s3 mb s3://my-bucket-to-test``, assim acamos de criar nosso bucket s3.

Se você quiser pode subistituir o nome "my-bucket-to-test" para nome do bucket que queira.

Ao criar bucket ele retornara a mensagem "make_bucket: my-bucket-to-test" na saida do prompt.
Aqui vamos exemplificar algumas formas de subir arquivos para bucket s3.

## Upload do bucket

Agora vamos isnerir massa de dados no bucket, dessa formas ele ira subir todos arquivos que
estao dentro do diretorio especificado.

``aws s3 sync /home/oem/Workspace/local-stack-aws/bucket-s3/ s3://my-bucket-to-test/``

Ao fazer o upload para bucket, o arquivo sera armazenado com mesma estrutura de diretorios. o meu
ficou da seguinte forma:

``aws s3 sync /home/oem/Workspace/local-stack-aws/bucket-s3/product_data.json s3://my-bucket-to-test/``

Caso queira subir apenas um arquivo, podemos fazer assim:

``aws s3 cp /home/oem/Workspace/local-stack-aws/bucket-s3/product_data.json s3://my-bucket-to-test/``


Agora que subimos ele vamos baixalo:

## Download do bucket

``aws s3 cp s3://my-bucket-to-test/product_data.json - | cat``

Esse comando assima baixa o arquivo product_data e faz cat na saida dele, retornando o objeto.

## Deletando arquivo do bucket

Primeiro vamos listar nosso bucket:

`aws s3 ls s3://my-bucket-to-test/`


Agora que ja sabemos qual deletar, vamos processeguir:

``aws s3 rm s3://my-bucket-to-test/product_data.json``

Ao executar o comando ele retorna a seguinte mensagem:

``delete: s3://my-bucket-to-test/product_data.json``

Acabamos de dizer que queremos deletar o arquivo product_data.json do nosso bucket. Se listarmos o
conteudo de nosso bucket novamente, iremos ver que ele nao existe mais.
