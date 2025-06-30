## Tutorial Completo de Keycloak: Do Básico ao Avançado

O Keycloak é uma poderosa ferramenta de código aberto para gerenciamento de identidade e acesso (IAM), que permite proteger aplicações e serviços com o mínimo de esforço. Este tutorial abrangente guiará você pelos conceitos fundamentais e funcionalidades essenciais do Keycloak, desde a criação de realms e usuários até a configuração de tokens e o processo de logout.

### 1\. Criação de Realms

Um "Realm" no Keycloak é um espaço isolado que gerencia um conjunto de usuários, credenciais, roles e clientes. Essencialmente, cada realm funciona como um domínio de segurança independente.

**Passo a Passo para Criar um Realm:**

1.  **Acesse o Console de Administração:** Faça login no console de administração do seu servidor Keycloak. Geralmente, ele está localizado em `http://localhost:8080/admin/` (ou o endereço do seu servidor).
2.  **Adicionar um Novo Realm:** No canto superior esquerdo, passe o mouse sobre o nome do realm atual (o padrão é "Master") e clique em **"Create Realm"**.
3.  **Defina o Nome do Realm:** No formulário que aparece, insira um nome para o seu novo realm (por exemplo, `meu-app-realm`).
4.  **Crie o Realm:** Clique em **"Create"**.

Você será automaticamente redirecionado para o console de administração do seu novo realm. Todas as configurações subsequentes serão aplicadas a este realm específico.

### 2\. Criação de Usuários

Com o seu realm criado, o próximo passo é adicionar os usuários que poderão se autenticar.

**Passo a Passo para Criar um Usuário:**

1.  **Navegue até a Seção de Usuários:** No menu de navegação à esquerda, certifique-se de que seu novo realm esteja selecionado e clique em **"Users"**.
2.  **Adicionar um Novo Usuário:** Na página de usuários, clique em **"Create new user"**.
3.  **Preencha as Informações do Usuário:**
      * **Username:** (Obrigatório) O nome de usuário para login.
      * **Email:** O endereço de e-mail do usuário.
      * **First Name:** O primeiro nome do usuário.
      * **Last Name:** O sobrenome do usuário.
4.  **Crie o Usuário:** Clique em **"Create"**.

Após criar o usuário, você será levado para a página de gerenciamento deste usuário.

**Definindo uma Senha:**

1.  Acesse a aba **"Credentials"** na página do usuário.
2.  No campo **"Set password"**, digite uma senha para o usuário.
3.  Confirme a senha.
4.  Por padrão, a opção **"Temporary"** está ativada, o que exigirá que o usuário altere a senha no primeiro login. Desmarque se não desejar esse comportamento.
5.  Clique em **"Set password"**.

### 3\. Criação de "Roles" (Papéis)

As "Roles" representam as permissões e os níveis de acesso dentro de suas aplicações. Existem dois tipos de roles no Keycloak:

  * **Realm Roles:** Papéis globais que se aplicam a todos os clientes dentro de um realm.
  * **Client Roles:** Papéis específicos de um determinado cliente (aplicação).

**Passo a Passo para Criar uma Realm Role:**

1.  **Acesse as Roles do Realm:** No menu à esquerda, clique em **"Realm roles"**.
2.  **Adicionar uma Nova Role:** Clique em **"Create role"**.
3.  **Defina o Nome da Role:** Insira um nome para a role (por exemplo, `admin` ou `usuario-comum`).
4.  **Salve a Role:** Clique em **"Save"**.

### 4\. Atribuição de Roles a Usuários

Uma vez que as roles são criadas, você precisa atribuí-las aos usuários para conceder as permissões correspondentes.

**Passo a Passo para Atribuir uma Role:**

1.  **Selecione o Usuário:** Vá para a seção **"Users"** e selecione o usuário ao qual deseja atribuir a role.
2.  **Aba de Mapeamento de Roles:** Acesse a aba **"Role mapping"**.
3.  **Atribuir a Role:** Na seção **"Available roles"**, você verá as roles que criou. Selecione a role desejada e clique em **"Assign role"**.

A role agora aparecerá na lista de **"Assigned roles"** para aquele usuário.

### 5\. Criação de Clientes

Um "Cliente" no Keycloak representa uma aplicação ou serviço que será protegido.

**Passo a Passo para Criar um Cliente:**

1.  **Acesse a Seção de Clientes:** No menu à esquerda, clique em **"Clients"**.
2.  **Adicionar um Novo Cliente:** Clique em **"Create client"**.
3.  **Configurações Iniciais do Cliente:**
      * **Client ID:** (Obrigatório) Um identificador único para sua aplicação (por exemplo, `minha-app-web`).
      * **Name:** Um nome amigável para o cliente.
      * **Client authentication:** Ative esta opção para clientes que precisam trocar um código de autorização por um token de acesso (confidenciais).
4.  **Configurações de Login:**
      * **Valid redirect URIs:** (Essencial) Insira as URLs para as quais o Keycloak pode redirecionar o usuário após um login bem-sucedido. Por exemplo, `http://localhost:3000/*`.
5.  **Salve o Cliente:** Clique em **"Save"**.

### 6\. Escolha de Algoritmos de Assinatura de Tokens

O Keycloak utiliza JSON Web Tokens (JWT) para proteger a comunicação. A assinatura digital desses tokens garante sua autenticidade e integridade.

**Algoritmos Comuns:**

  * **RS256 (RSA com SHA-256):** Um algoritmo de assinatura assimétrico (usa um par de chaves pública/privada). É o padrão e altamente recomendado. O Keycloak assina o token com a chave privada, e as aplicações o validam com a chave pública.
  * **RS512 (RSA com SHA-512):** Similar ao RS256, mas com um hash mais forte. Oferece um nível de segurança maior, com um custo computacional ligeiramente mais alto.

**Onde Configurar:**

1.  Acesse a seção **"Realm settings"** no menu à esquerda.
2.  Vá para a aba **"Tokens"**.
3.  No campo **"Default signature algorithm"**, você pode escolher o algoritmo desejado. Para a maioria dos casos, **RS256** é uma escolha segura e eficiente.

### 7\. Tipos de Tokens

O Keycloak opera com três tipos principais de tokens, conforme o padrão OpenID Connect (OIDC):

  * **Access Token (Token de Acesso):** É um JWT que contém informações sobre o usuário e suas permissões (roles). É enviado no cabeçalho de cada requisição para as APIs protegidas, permitindo que a aplicação verifique se o usuário tem autorização para acessar o recurso solicitado. Geralmente, possui um tempo de vida curto (alguns minutos).

  * **ID Token (Token de Identidade):** Também um JWT, seu propósito principal é fornecer informações sobre a identidade do usuário autenticado para a aplicação cliente (por exemplo, nome, e-mail). Ele prova que o usuário foi autenticado com sucesso.

  * **Refresh Token (Token de Atualização):** É um token de longa duração usado para obter novos Access Tokens e ID Tokens sem que o usuário precise fazer login novamente. Quando o Access Token expira, a aplicação pode usar o Refresh Token para solicitar novos tokens ao Keycloak.

### 8\. Logout

O processo de logout no Keycloak invalida a sessão do usuário tanto no Keycloak quanto nas aplicações.

**Como Funciona o Logout:**

O logout é iniciado redirecionando o usuário para o endpoint de logout do Keycloak. A URL geralmente segue este formato:

```
http://<seu-servidor-keycloak>/realms/<nome-do-seu-realm>/protocol/openid-connect/logout
```

**Parâmetros Importantes:**

  * **`redirect_uri`**: Após o logout ser processado pelo Keycloak, ele redirecionará o usuário de volta para a URL especificada neste parâmetro. É crucial que esta URL esteja registrada na lista de **"Valid redirect URIs"** do seu cliente.
  * **`id_token_hint`**: Fornecer o ID Token do usuário como uma "dica" pode melhorar a experiência de logout, especialmente em cenários de Single Sign-On (SSO).

**Exemplo de URL de Logout:**

```
http://localhost:8080/realms/meu-app-realm/protocol/openid-connect/logout?redirect_uri=http%3A%2F%2Flocalhost%3A3000%2Flogin
```

Este tutorial cobriu os pilares fundamentais para começar a usar o Keycloak. Com esses conceitos, você está apto a configurar um ambiente de autenticação e autorização robusto para suas aplicações. Explore o console de administração para descobrir funcionalidades mais avançadas, como federação de identidades, fluxos de autenticação personalizados e políticas de autorização detalhadas.


---------------------
### Apoio
https://medium.com/@douglasaleixomendes/keycloak-integrando-com-a-api-tutorial-ebef9ba510a9
