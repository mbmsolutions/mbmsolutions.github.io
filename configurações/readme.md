# Configurações

Todos os arquivos de configurações estão localizados dentro de `@config/` e seguem as seguintes configurações:

### App

| Propriedade | Tipo     | Exemplo                               | Descrição              |
| ----------- | -------- | ------------------------------------- | ---------------------- |
| env         | `string` | `DEVELOPMENT/PRODUCTION`              | Ambiente do projeto    |
| token       | `string` | `hash-de-quantos-digitos-você-quiser` | Hash para autenticação |

### Database

| Propriedade      | Tipo     | Exemplo           | Descrição                                            |
| ---------------- | -------- | ----------------- | ---------------------------------------------------- |
| host             | `string` | `localhost`       | Host do banco de dados                               |
| database         | `string` | `meu-banco`       | Nome do banco de dados                               |
| user             | `string` | `postgres`        | Usuário para o banco de dados                        |
| password         | `string` | `minhasenha123`   | Senha do banco de dados                              |
| application_name | `string` | `Minha aplicação` | Nome da aplicação que será exibida no banco de dados |
| port             | `number` | `5432`            | Porta da conexão do banco de dados                   |

### Redis

| Propriedade | Tipo     | Exemplo         | Descrição                 |
| ----------- | -------- | --------------- | ------------------------- |
| host        | `string` | `localhost`     | Host do Redis             |
| port        | `number` | `5432`          | Porta da conexão do Redis |
| user        | `string` | `postgres`      | Usuário para o Redis      |
| password    | `string` | `minhasenha123` | Senha do Redis            |

### SMTP

| Propriedade | Tipo        | Exemplo                                     | Descrição                |
| ----------- | ----------- | ------------------------------------------- | ------------------------ |
| host        | `string`    | `localhost`                                 | Host do SMTP             |
| port        | `number`    | `5432`                                      | Porta da conexão do SMTP |
| secure      | `boolean`   | `false`                                     | Utilizar HTTPS           |
| auth        | `SMTP Auth` | [`reference`](/configurações/?id=smtp-auth) | Autenticação             |

###### SMTP Auth

| Propriedade | Tipo     | Exemplo         | Descrição     |
| ----------- | -------- | --------------- | ------------- |
| user        | `string` | `admin`         | Login do SMTP |
| pass        | `string` | `senha@secreta` | Senha do SMTP |
