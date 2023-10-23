[<<___VOLTAR](../README.md)

# Utilizando KMS

O que e KMS ?

KMS significa Key Management Service e é um serviço de gerenciamento de chaves oferecido pela 
AWS (Amazon Web Services). O KMS é usado para criar e controlar o acesso a chaves de 
criptografia que são usadas para proteger os dados armazenados e transmitidos pela AWS.

O KMS permite que você crie chaves mestras para criptografar e descriptografar dados, bem 
como chaves de dados para criptografar dados específicos, como chaves de criptografia de 
volume do Amazon EBS (Elastic Block Store) ou chaves de criptografia de objetos do 
Amazon S3 (Simple Storage Service).

Os principais recursos e usos do KMS são:

1. Gerenciamento centralizado de chaves
2. Controle de acesso granular
3. Integração com outros serviços AWS
4. Auditoria e monitoramento

O KMS é usado para ajudar a proteger dados sensíveis, garantindo que as chaves de criptografia 
sejam gerenciadas de forma segura. 

Vamos agora a alguns exemplo.

Criar uma chave mestra (CMK):

```bash
aws kms create-key
```

Criptografar um dado usando uma chave mestra:

```bash
aws kms encrypt --key-id <key-id> --plaintext <data-to-encrypt>
```

Descriptografar um dado usando uma chave mestra:

```bash
aws kms decrypt --ciphertext-blob <encrypted-data> --key-id <key-id>
```

Listar as chaves mestras disponíveis:

```bash
aws kms list-keys
```

Rotacionar uma chave mestra:

```bash
aws kms enable-key-rotation --key-id <key-id>
```

[<<___VOLTAR](../README.md)