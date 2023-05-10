# Como utilizar o IAM

O que e IAM ?

O IAM (Identity and Access Management) é um serviço da AWS que permite gerenciar o acesso aos 
recursos e serviços da AWS de forma segura. Ele fornece controle granular sobre permissões e 
políticas de acesso para usuários, grupos, funções e serviços dentro da sua conta da AWS.

O IAM permite que você crie e gerencie identidades (usuários, grupos e funções) e atribua 
permissões específicas a essas identidades. Alguns dos principais recursos e 
funcionalidades do IAM incluem:

1. Gerenciamento de usuários: Você pode criar usuários IAM e fornecer credenciais 
de login para eles. Cada usuário terá um conjunto de credenciais exclusivas 
para autenticar-se na conta da AWS.

2. Grupos: Você pode organizar usuários em grupos lógicos e atribuir permissões em nível de grupo. 
Isso simplifica a administração de permissões, pois você pode adicionar ou remover usuários de 
um grupo e as permissões serão aplicadas automaticamente.

3. Funções do IAM: As funções do IAM permitem que você conceda permissões temporárias a serviços e 
aplicativos da AWS. Isso é útil quando você deseja permitir que um serviço acesse 
outros recursos em sua conta em seu nome, sem precisar compartilhar credenciais de acesso. 

4. Políticas de IAM: As políticas do IAM são documentos JSON que definem as permissões para 
usuários, grupos e funções. Você pode criar políticas personalizadas para atender aos requisitos 
específicos de acesso e atribuir essas políticas às identidades.

5. Integração com outros serviços: O IAM se integra a vários serviços da AWS, permitindo que você 
controle o acesso a recursos específicos nesses serviços. Por exemplo, você pode definir políticas 
de acesso para buckets do S3, instâncias do EC2, filas do SQS e muitos outros.


