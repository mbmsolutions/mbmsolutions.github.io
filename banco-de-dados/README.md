# Banco de dados
O banco de dados é mantido em `PostgreSQL`, `Redis` e `MongoDB` sendo utilizado o SQL para controlar transações entre framework web e sistema

### Tabelas
A estrutura das tabelas internas é feita da seguinte forma:

Utilizar apenas nomes em ingles para as tabelas e colunas, com excessões de dados nacionais, exemplo CNPJ e CPF.

Os nomes das tabelas **DEVEM** ser utilizadas com o nome no plural, exemplo:
- users
- roles
- clients

Os nomes das colunas devem possuir o prefixo da tabela, sendo o nome da tabela no singular, exemplo:
- user_id
- user_name

Colunas que são `foreign_key` devem possuir o seguinte formato: `${NOME_DA_TABELA}_fk_${NOME DA FK}_id`, exemplo:
- user_fk_role_id (usuários com a referencia em funções)
- user_fk_client_id (usuários com referencia em clientes)

Caso o relacionamento seja feito em uma coluna externa de outro banco, por exemplo a coluna "cod_empresa" muito utilizada no próprio MBM Solutions, a coluna deverá seguir o formato `${NOME_DA_TABELA}_external_ref` sendo preenchida com um JSON equivalendo as tabelas e as colunas referentes. por exemplo:
- Caso **parameters** possua uma configuração por empresa, existirá a coluna `parameters_external_ref` do tipo `jsonb` com o JSON: 
```json
{
    "empresa": {
        "cod_empresa": "0001"
    }
}
```

Alem disso a utilização de JSON/JSONB é comum, pois assim dados como multiplas chaves de relacionamentos ou dados que realmente são do tipo JSON não precisam ser convertidos explicitamente em todos os lugares do sistema.

### Relacionamentos
A framework fornece o helper `@library/helpers/RelationBuilder` que utiliza apenas 4 parametros para montar o relacionamento em uma só linha, exemplo de formato:
```ts
Relation(User, Client, "user_fk_client_id", "HasOneRelation"),
```

### Models
As models do sistema, utilizam uma extensão customizada do [Objection.JS](https://vincit.github.io/objection.js/), sendo encontrada em `@library/database/CatModel` a model utilizada internamente e em `@library/database/LibModel` a model utilizada para o sistema MBM Business, sendo as duas utilizadas com o conceito da programação ser feita nelas e nem sempre nos controllers, assim facilita bastante a "customização" de ações para coisas muito expecificas, por exemplo, após a criação de um usuário, o sistema deve enviar um email para o usuário, isso será adicionado na função `afterInsert` da model `User` sem necessidade de ser adicionado ao controller.

#### Propriedades

###### [orderBy *static*](/banco-de-dados/models/propriedades/orderBy.md)

###### appends *static*
O parametro faz que com a lista de strings de `relacionamentos` seja automaticamente utilizada nas consultas, exemplo:
```ts
static appends = ["client","role"];
```
irá automaticamente executar os seguintes relacionamentos:
```ts
static relationMappings = {
    client: Relation(User, Client, "user_fk_client_id", "HasOneRelation"),
    role: Relation(User, Role, "user_fk_role_id", "HasOneRelation"),
};
```
`appends: string[]`

#### Métodos

###### beforeInsert *static* 
Será executado antes do `insert` de uma query.

###### beforeUpdate *static* 
Será executado antes do `update` de uma query.

###### beforeSave *static* 
Será executando antes tanto do `insert` quanto do `update` sendo por padrão a validação e trativas de dados.

###### $beforeInsert *async* :id=p-before-insert
Metodo assincrono que será executado antes do `insert` de uma query.

###### $beforeUpdate *async* :id=p-before-update
Metodo assincrono que será executado antes do `update` de uma query.

###### getJsonSchema *static* 
Retorna o `schema` utilizado na propria model

### Schemas
O schema poderá ser construido em um arquivo com o caminho `@library/database/schemas/*/NOME.json` onde `NOME` deverá ser o mesmo da model ou o schema pode ser criado como um parametro na propria model com o nome de `jsonSchema` sendo um parametro *estatico*

###### uiSchema
O `uiSchema` foi criado para possibilitar a customização de formulários do lado do cliente com informações que não são válidas para o `jsonSchema` e como foi criado totalmente do zero, não existe um padrão a ser seguido, o recomendado é observar os parametros que podem ser utilizados pelo frontend, segue-se um exemplo

```ts
static uiSchema = {
    defaults: {
        "ui:cols": 6,
    },
    user_fk_client_id: {
        'ui:order': 1,
        "ui:cols": 6
    },
    user_password: {
        "grid:widget": "password",
        "ui:help":"Deixe em branco para não alterar a senha"
    },
    user_id: {
        "ui:widget": "hidden",
    },
    user_type: {
        'ui:order': 0,
        "ui:cols": 6,
        "ui:enum": {
            "admin": "Administrador",
            "client": "Cliente",
            "colaborator": "Colaborador"
        }
    }
};
```

### DFM's
Ainda está em desenvolvimento, mas a ideia é utilizar os arquivos `.dfm` do Delphi 7 e conseguir mapear eles em um `jsonSchema` onde ja vira com os campos basicos de um cadastro ou tela.
