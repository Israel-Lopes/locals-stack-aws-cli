# Como utilizar SNS

O que e SNS ?

O SNS (Simple Notification Service) é um serviço de mensagens e notificações totalmente 
gerenciado pela AWS (Amazon Web Services). Ele permite que você envie mensagens para 
diferentes tipos de destinatários, como endpoints de e-mail, SMS, aplicativos móveis, 
serviços da AWS, entre outros.

O SNS é um serviço flexível e escalável que permite criar tópicos de notificação e 
inscrever-se neles. Os tópicos representam canais de comunicação para enviar mensagens, e as 
inscrições definem os destinos onde as mensagens serão entregues.

Quando uma mensagem é publicada em um tópico do SNS, ela é entregue a todos os destinos 
(inscrições) associados a esse tópico. Isso facilita a implementação de comunicações 
assíncronas entre componentes de um sistema distribuído.

O SNS pode ser utilizado em diversos casos, tais como envio de notificações, alertas, 
atualizações em tempo real, publicação de eventos, entre outros, tornando-se uma peça 
fundamental para a criação de arquiteturas e sistemas resilientes na nuvem.

Vamos aprender a criar nosso. Execute o comando:

``aws sns create-topic --name MyTopic``

Esse comando foi para criarmos nosso primeiro topico chamado ``MyTopic``.

Agora vamos criar nossa inscrição:

``aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic --protocol email --notification-endpoint my-email@example.com``

A inscrição no topico e um meio de configurar um end pointer para o topico, ou seja, voce
esta configurando para qual destino as mensagem no topico que chegarem serao enviadas.

No caso eu configurei para as mensagem serem enviadas para o email ficticio.

Podemos configurar para os seguites destinos:

- E-mail: As notificações são enviadas por e-mail para o endereço de e-mail especificado.
- SMS: As notificações são enviadas como mensagens de texto para o número de telefone celular especificado.
- HTTP/HTTPS: As notificações são enviadas como solicitações HTTP POST para o URL especificado.
- Lambda: As notificações são enviadas para uma função AWS Lambda especificada.
- Aplicativo móvel: As notificações são enviadas para um aplicativo móvel registrado por meio de serviços como Amazon Pinpoint, Apple Push Notification Service (APNS) ou Firebase Cloud Messaging (FCM).

Agora vamos enviar a mensagem para topico encaminha para seu destino:

``aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic --message "Hello, world!"``

Lembre-se de por a zona correta em seu comando, aqui eu coloquei e apenas um exemplo.
