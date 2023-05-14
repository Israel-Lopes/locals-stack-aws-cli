# Aprenda SQS da AWS

O que e SQS ?

O Amazon Simple Queue Service (SQS) é um serviço de mensagens totalmente gerenciado fornecido 
pela Amazon Web Services (AWS). Ele oferece uma maneira de desacoplar e desacelerar os 
componentes de um sistema distribuído, permitindo que eles se comuniquem e troquem informações 
de forma assíncrona.

O SQS é baseado em filas de mensagens, onde os produtores de mensagens enviam mensagens para 
uma fila e os consumidores as recebem e processam. Isso permite que os sistemas se comuniquem 
de maneira assíncrona, onde o produtor e o consumidor não precisam estar em execução 
simultaneamente. O SQS garante que as mensagens sejam armazenadas de forma durável e que
sejam entregues pelo menos uma vez a um consumidor.

Vamos a pratica para configurar nosso SQS. Primerio temos que criar uma fila para isso
siga o comando abaixo:

``aws sqs create-queue --queue-name MyQueue``

Na opcao ***--queue-name*** devemos especifica o nome da nossa fila, no caso coloquei nome
para teste.

Agora vamos enviar mensagem para fila:

``aws sqs send-message --queue-url <URL_da_Fila> --message-body "Hello, world!"``

Para pegarmos a url da file que criamos, podemos utilizar o comando:

``aws sqs get-queue-url --queue-name MyQueue``

O comando ira retornar:
```
{
    "QueueUrl": "https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue"
}
```
Agora podemos enviar mensagem para fila:

``aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue --message-body "Hello, world!"``


