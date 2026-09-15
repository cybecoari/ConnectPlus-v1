# ConnectPlus V3 — Guia de instalação

Este guia explica como colocar o painel em uma hospedagem com cPanel, configurar o banco de dados e entrar no painel.

## 1. Requisitos

- Hospedagem com cPanel.
- PHP 8.x (recomendado usar a mesma versão configurada na hospedagem; PHP 8.1+ é recomendado).
- MySQL/MariaDB.
- phpMyAdmin.
- Um domínio, subdomínio ou uma pasta. O painel foi preparado para detectar automaticamente a URL onde estiver instalado.

## 2. Criar o banco de dados no cPanel

No cPanel:

1. Abra **MySQL Databases**.
2. Crie um banco de dados.
3. Crie um usuário MySQL.
4. Adicione o usuário ao banco.
5. Marque **ALL PRIVILEGES**.
6. Anote o nome completo do banco, usuário e senha.

> Em muitas hospedagens o cPanel acrescenta automaticamente o prefixo da conta ao nome do banco e do usuário.

## 3. Importar o banco

1. Abra **phpMyAdmin**.
2. Selecione o banco criado.
3. Clique em **Importar**.
4. Selecione o arquivo `banco.sql` deste pacote.
5. Execute a importação.

Se a hospedagem limitar o tamanho do arquivo, use o recurso de importação disponível no próprio cPanel ou importe o SQL em partes.

## 4. Configurar o acesso ao banco

Abra o arquivo:

`config.php`

Localize:

```php
define('DB_HOST', 'localhost');
define('DB_NAME', 'SEU_BANCO');
define('DB_USER', 'SEU_USUARIO');
define('DB_PASS', 'SUA_SENHA');
```

Preencha com os dados do banco criado no cPanel.

**Não coloque a senha do banco em páginas públicas ou envie esse arquivo para terceiros.**

## 5. Enviar o painel para a hospedagem

### Usando um subdomínio

Exemplo:

`https://painel.seudominio.com.br`

1. Crie o subdomínio no cPanel.
2. Confira qual é a pasta/document root desse subdomínio.
3. Abra o **Gerenciador de Arquivos**.
4. Envie o conteúdo do ZIP para essa pasta.
5. Extraia o ZIP.
6. O arquivo `admin/login.php` deve ficar dentro da instalação do painel.

O endereço de login será:

`https://painel.seudominio.com.br/admin/login.php`

### Usando uma pasta do domínio

Exemplo:

`https://seudominio.com.br/painel`

Coloque os arquivos dentro da pasta `painel`.

O login será:

`https://seudominio.com.br/painel/admin/login.php`

### Usando um domínio próprio

Se o domínio apontar diretamente para a pasta do painel, o login será:

`https://seudominio.com.br/admin/login.php`

O painel não deve precisar de um domínio fixo no código para funcionar nesses casos.

## 6. Primeiro acesso

O banco incluído possui um usuário administrativo com o nome:

**Usuário:** `admin`

### Senha

A senha original do hash que está no `banco.sql` **não pode ser recuperada lendo o banco**. Por segurança, a senha fica armazenada como hash.

Para garantir um acesso conhecido após importar o banco, faça um reset da senha pelo phpMyAdmin.

No phpMyAdmin, selecione o banco do painel, abra a aba **SQL** e execute:

```sql
UPDATE users
SET password = '$2y$12$r58N2iqYRjnhRldlSMP3k.4ekhGs1sE6MjI9uO1nTwiyq5bpoU1wi',
    role = 'admin',
    status = 'active'
WHERE username = 'admin';
```

Depois disso:

**Usuário:** `admin`  
**Senha:** `Admin@1234`

> Troque essa senha depois do primeiro acesso em **Alterar Senha**. Não mantenha uma senha padrão em um painel publicado na internet.

## 7. Endereço de login

Se o painel estiver no subdomínio:

`https://painel.seudominio.com.br/admin/login.php`

Depois do login, o painel deve permanecer no mesmo domínio/subdomínio:

`https://painel.seudominio.com.br/admin/index.php`

Ele não deve mandar o usuário para a raiz de outro domínio.

## 8. Se aparecer página sem estilo (CSS)

Confira se este arquivo existe:

`assets/css/style.css`

E se a URL estiver apontando para a instalação do painel, e não para a raiz de outro domínio.

Exemplo correto em subdomínio:

`https://painel.seudominio.com.br/assets/css/style.css`

## 9. Se der erro de banco de dados

Confira primeiro no `config.php`:

- `DB_HOST`
- `DB_NAME`
- `DB_USER`
- `DB_PASS`

Depois confirme no cPanel que o usuário MySQL foi adicionado ao banco com **ALL PRIVILEGES**.

## 10. Pastas usadas pelo painel

O painel possui áreas para atualizações, temas, backups e arquivos enviados. Não apague estas pastas:

- `admin/`
- `api/`
- `assets/`
- `backups/`
- `database/`
- `includes/`
- `theme/`
- `update/`

Mantenha também os arquivos `.htaccess` existentes dentro das pastas que já possuem um.

## 11. Após instalar

Faça este teste:

1. Abrir `/admin/login.php`.
2. Entrar com `admin` / `Admin@1234` após executar o reset da senha acima.
3. Abrir **Dashboard**.
4. Abrir **Configurações**.
5. Abrir **Servidores**.
6. Abrir **Redes**.
7. Abrir **Tema**.
8. Abrir **Mensagens**.
9. Abrir **Logs**.
10. Abrir **Usuários**.
11. Testar **Alterar Senha**.
12. Clicar em **Sair**.
13. Confirmar que retorna para `/admin/login.php` no mesmo domínio/subdomínio.

## 12. Segurança

Depois de colocar o painel online:

- Troque a senha `Admin@1234`.
- Use HTTPS/SSL.
- Não publique o conteúdo de `config.php`.
- Não compartilhe o usuário e senha do banco.
- Faça backups do banco antes de alterações importantes.
- Evite deixar arquivos de backup SQL públicos.

## Estrutura principal

```text
ConnectPlus/
├── admin/
│   ├── login.php
│   ├── logout.php
│   ├── index.php
│   ├── tema.php
│   ├── config/
│   ├── servers/
│   ├── networks/
│   ├── users/
│   ├── messages/
│   ├── logs/
│   └── backups/
├── api/
├── assets/
│   ├── css/
│   └── js/
├── backups/
├── database/
├── includes/
├── theme/
├── update/
├── banco.sql
├── config.php
└── index.php
```

**Fim do guia.**

### 5. Login

Acesse:

```text
SEU_ENDERECO/admin/login.php
```

As credenciais do administrador devem ser definidas na instalação. Não existe senha real documentada neste README.

Se precisar criar/redefinir a senha, use um hash PHP:

```php
password_hash('SUA_NOVA_SENHA', PASSWORD_DEFAULT)
```

Grave o hash no campo de senha do usuário administrador, conforme a estrutura da tabela do projeto.

### 6. Segurança

Antes de deixar o repositório público:

- nunca publique senha de banco;
- nunca publique API keys;
- nunca publique tokens;
- nunca publique credenciais de serviços;
- não faça commit de `.env`;
- não faça commit de logs;
- não publique backups do banco;
- altere qualquer senha inicial depois da instalação.

O `.gitignore` já bloqueia `.env`, logs e arquivos de backup comuns.

## Estrutura

```text
admin/
assets/
config/
includes/
index.php
README.md
.env.example
.gitignore
```

## Teste após a instalação

1. Abra `/admin/login.php`.
2. Faça login.
3. Confirme que o dashboard permanece no mesmo domínio/subdomínio.
4. Abra Tema.
5. Teste salvar.
6. Teste Servidores.
7. Teste Redes.
8. Teste Usuários.
9. Clique em Sair.
10. Confirme que volta para `/admin/login.php` no mesmo host.

## Observação

O GitHub é usado para armazenar o **código**. As credenciais reais pertencem à **hospedagem** e não ao repositório.
