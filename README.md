# ConnectPlus V1

Painel ConnectPlus V1 em PHP.

## Instalação segura

Este repositório é preparado para ficar no GitHub sem publicar a senha real do banco.

### 1. GitHub

Envie o conteúdo do projeto para o repositório:

`ConnectPlus-v1`

O arquivo `.env.example` é apenas um modelo. **Nunca coloque a senha real do banco no GitHub.**

### 2. Hospedagem/cPanel

Baixe o projeto do GitHub ou envie os arquivos para a hospedagem.

Crie o banco MySQL/MariaDB e o usuário no cPanel e conceda as permissões necessárias.

Configure os dados do banco **somente no servidor**:

```text
DB_HOST=localhost
DB_NAME=nome_real_do_banco
DB_USER=usuario_real
DB_PASS=senha_real
```

Se o projeto usar um arquivo PHP de configuração em vez de `.env`, preencha os valores diretamente nesse arquivo **no servidor**, sem fazer commit dessas credenciais.

### 3. Domínio, subdomínio ou subpasta

O painel deve funcionar em:

```text
https://painel.seudominio.com.br/admin/login.php
https://seudominio.com.br/painel/admin/login.php
https://outrodominio.com/admin/login.php
```

Não fixe `cybercoari.com.br` no código.

Os redirecionamentos internos devem preservar o host e o caminho onde o painel foi instalado.

### 4. Banco de dados

Importe o SQL fornecido pelo projeto no banco criado.

Depois confira se as tabelas necessárias foram criadas antes de tentar o login.

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
