[<<___VOLTAR](../README.md)

# Trabalhando com EC2 em linha de comando

O que e EC2 ?

EC2 (Elastic Compute Cloud) é um serviço de computação em nuvem da Amazon Web Services (AWS) que
permite criar e gerenciar instâncias de máquinas virtuais na nuvem. As instâncias EC2 fornecem 
recursos computacionais escaláveis e flexíveis, permitindo executar aplicativos e cargas 
de trabalho em um ambiente virtualizado.

Sigamos a diante agora.

Para subir nosso ambiente simulado da aws faça:

```bash
sudo docker run --rm -it -p 4566:4566 -p 4571:4571 localstack/localstack
```

Vamos criar nosso primeiro ***EC2*** em CLI. Para iniciarmos seguimos etapas 
descritas na linha abaixo:

Para criar o EC2:

```bash
aws ec2 run-instances --image-id <ami-id> --instance-type <instance-type> --key-name <key-pair-name> --security-group-ids <security-group-id> --subnet-id <subnet-id> --count <number-of-instances>
```

Para executar o comando de criação do EC2, precisamos fornecer detalhes como tipo de imagem
IAM, numero de instancias, grupo de segurança, chaves de acesso e outras opçoes.

``--image-id``: O ID da imagem da Amazon Machine Image (AMI) que será usada para criar a 
instância EC2. A AMI é a base para a instância e define o sistema operacional e outros 
softwares pré-instalados.
``--instance-type``: O tipo de instância EC2 que você deseja criar. O tipo de instância 
determina os recursos computacionais, como CPU, memória e capacidade de armazenamento, 
disponíveis para a instância.
``--key-name``: O nome do par de chaves (key pair) que você deseja associar à instância EC2. 
Isso permite que você faça login na instância usando SSH (Secure Shell).
``--security-group-ids``: O ID do grupo de segurança (security group) ao qual você deseja 
associar a instância EC2. O grupo de segurança define as regras de tráfego permitido 
para a instância.
``--subnet-id``: O ID da subnet (sub-rede) na qual você deseja lançar a instância EC2. A subnet
determina a rede na qual a instância será colocada e pode afetar a conectividade e a disponibi
-lidade da instância.
``--count``: O número de instâncias EC2 que você deseja criar. Especifica quantas instâncias 
idênticas serão criadas.

Vamos entao precisar de especificar essas informaçoes, sigamos abaixo: 

Para podemos escolher nossa imagem para EC2, podemos usar o seguinte comando para listar 
as imagens disponiveis na AWS: ``aws ec2 describe-images``, observe que ele ira retornar um
obejto json muito grande com detalhes da arquitetura da imagem.

Exemplo da saida do comando:

```json
},
        {
            "Architecture": "x86_64",
            "CreationDate": "2023-05-11T09:57:08.000Z",
            "ImageId": "ami-fbc1c684",
            "ImageLocation": "None",
            "ImageType": "machine",
            "Public": true,
            "KernelId": "None",
            "OwnerId": "591542846629",
            "RamdiskId": "ari-1a2b3c4d",
            "State": "available",
            "BlockDeviceMappings": [
                {
                    "DeviceName": "/dev/xvda",
                    "Ebs": {
                        "DeleteOnTermination": false,
                        "SnapshotId": "snap-00e5f6c8",
                        "VolumeSize": 15,
                        "VolumeType": "standard"
                    }
                }
            ],
            "Description": "Amazon Linux AMI 2018.03.b x86_64 ECS HVM GP2",
            "Hypervisor": "xen",
            "ImageOwnerAlias": "amazon",
            "Name": "amzn-ami-2018.03.b-amazon-ecs-optimized",
            "RootDeviceName": "/dev/xvda",
            "RootDeviceType": "ebs",
            "Tags": [],
            "VirtualizationType": "hvm"
        },
```
Agora temos que criar nosso grupo de seguraça, pois e ele quem vai determinar regras de
entrada e saida. Devemos informar nome do grupo que queremos criar e a descrição dele:

```bash
aws ec2 create-security-group --group-name MyGroupTest --description "group description"
```

Executando o comando ele retorna algo assim:

```json
{
    "GroupId": "sg-0937433e1dbd16c91",
    "Tags": []
}
```
Quarde o GroupId pois ele sera util para criar o ***EC2***. Se você ja tem um Security-group
ja criado, pode listar eles ta seguinte maneira: ``aws ec2 describe-security-groups``.
Dessa forma sera listado todos os grupos que voce tem.

Podemos tambem aplicar filtros caso voce saiba o nome do group, nesse caso basta por nome
do group que acamos de criar:

```bash
aws ec2 describe-security-groups --filters "Name=group-name,Values=MyGroupTest"
```

Sigamos em frente.

Vamaos pegar o ***subnet-id***, para isso podemos listalos com seguinte:

```bash
aws ec2 describe-subnets
``````

Podemos aplicar filtros se quisermos tambem:

```bash
aws ec2 describe-subnets --filters "Name=subnet-id,Values=<subnet-id>"
```

Eu irei utilizar como exemplo o id "subnet-b8a4b5ff".

Vamos pegar ***instance-type***, basta fazer o comando para listar:

```bash
aws ec2 describe-instance-types
```

No retorno o que nos interessa e o campo ***InstanceType***, ele contem o nome da instancia.

Por ultimo vamos pegar a ***key-name***, que e o nome da chave de acesso(key pair) que voce
usara para se conectar a instancia ***EC2***. Essa chave de acesso e um par de chaves publicas
ou privadas que sao utilizadas para autenticar o acesso a instancia ***EC2***.

Caso voce ja tenha uma, podemos listala da seguinte maneira:

```bash
aws ec2 describe-key-pairs
```

No meu caso nao tenho ainda, nisso terei que criar uma, segue o passo:

```bash
aws ec2 create-key-pair --key-name MyKeyPairTest --output text > MyKeyPairTest.pem
```

Isso ira criar uma nova chave de acesso chamada MyKeyPairTest e salvara a chave privada no
no arquivo MyKeyPairTest.pem, apenas certifique-se de salvala em um local seguro.

Agora que ja coletamos todas as informaçoes necessarias, podemos criar ***EC2***, o meu
ficou da seguinte maneira:

```bash
aws ec2 run-instances \
  --image-id "ami-fbc1c684" \
  --instance-type "d3.2xlarge" \
  --key-name "MyKeyPairTest" \
  --security-group-ids "sg-0937433e1dbd16c91" \
  --subnet-id "subnet-b8a4b5ff" \
  --count 2
```
Agora basta executar o comando acima para criar nosso primeiro ***EC2***. Proximo passo agora
e iniciarmos nosso instancia ***EC2***, basta seguir o exemplo abaixo:

```bash
aws ec2 start-instances --instance-ids <instance-id>
```

Ficando da seguinte maneira: 

```bash
aws ec2 start-instances --instance-ids "i-304d1f1933d722ab6"
```

Esse e o retorno de sucesso que queremos ter:

```json
{
    "StartingInstances": [
        {
            "CurrentState": {
                "Code": 0,
                "Name": "pending"
            },
            "InstanceId": "i-304d1f1933d722ab6",
            "PreviousState": {
                "Code": 16,
                "Name": "running"
            }
        }
    ]
}
```
Finalizamos aqui nosso aprendizado sobre EC2.

[<<___VOLTAR](../README.md)