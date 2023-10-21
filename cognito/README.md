# Utilizando Congnito

O que e Cognito ?

O Amazon Cognito é um serviço da AWS que oferece recursos para autenticação, autorização e 
gerenciamento de usuários em aplicativos da web e móveis. Ele simplifica o processo de criação
de recursos de autenticação seguros, permitindo que você adicione facilmente recursos como 
login por e-mail/senha, login social, autenticação multifator, gerenciamento de usuários, 
entre outros, aos seus aplicativos.

O Amazon Cognito fornece três principais componentes:

- **User Pools**: É um serviço completo de diretório de usuários que permite que você crie e 
gerencie facilmente um banco de dados de usuários para autenticação. Ele permite que você 
defina fluxos de login personalizados, capture atributos personalizados do usuário, implemente 
autenticação multifator e muito mais.
- **Identity Pools**: É um serviço de autenticação federada que permite que seus usuários 
façam login em seu aplicativo usando provedores de identidade populares, como Amazon, Facebook,
Google, Apple, entre outros. Ele fornece tokens de acesso temporários para acesso seguro aos 
recursos da AWS.
- **Cognito Sync**: É um serviço de sincronização de dados que permite que os usuários 
sincronizem seus dados em dispositivos diferentes. Ele permite que você salve dados de usuário,
como preferências, configurações e progresso do aplicativo, de forma segura e sincronizada em 
vários dispositivos.

O Amazon Cognito é amplamente utilizado em aplicativos da web e móveis que precisam de 
recursos de autenticação seguros e gerenciamento de usuários. 

Criar um grupo em um pool de usuários:

```bash
aws cognito-idp create-group --user-pool-id <user-pool-id> --group-name <group-name>
```

Criar um usuário em um pool de usuários:

```bash
aws cognito-idp admin-create-user --user-pool-id <user-pool-id> --username <username>
```

Adicionar um usuário a um grupo em um pool de usuários:

```bash
aws cognito-idp admin-add-user-to-group --user-pool-id <user-pool-id> --username <username> --group-name <group-name>
```

Autenticar um usuário em um pool de usuários:

```bash
aws cognito-idp initiate-auth --client-id <client-id> --auth-flow USER_PASSWORD_AUTH --auth-parameters USERNAME=<username>,PASSWORD=<password>
```

Obter informações sobre um usuário em um pool de usuários:

```bash
aws cognito-idp admin-get-user --user-pool-id <user-pool-id> --username <username>
```