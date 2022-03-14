# Pastas
A Estrutura de pastas se mantem o mais fiel possivel a [Laravel](https://laravel.com/) com a adição da pasta `database` para models e para o frontend o projeto se mantem na estrutura do [NextJS](https://nextjs.org/) a partir da pasta `src`, sendo possivel pelo projeto todo, utilizar `aliases` para as respectivas pastas, exemplo:

`@bootstrap/database` resultará em `./bootstrap/database.ts`

`@app/http/controllers/AuthController` resultará em `./app/http/controllers/AuthController.ts`

!> É **obrigatório** que o projeto siga a seguinte estrutura de pastas:
```yaml
└── raiz        
    ├── app     
    ├── bootstrap
    ├── config
    │   ├── app.yaml
    │   ├── database.yaml
    │   ├── redis.yaml
    │   └── smtp.yaml
    ├── database
    ├── library
    ├── public
    ├── resources
    ├── routes
    │   └── web.ts
    ├── src
    └── storage    

```

### app
Nessa pasta é onde os `controllers`, `middlewares` e `classes sem definição expecifica` devem ser mantidos, assim facilitando encontrar os arquivos e vinculando-os a rota automaticamente sem a necessidade de passar caminhos e importações explicitas.

### bootstrap
Arquivos que serão iniciados assim que o projeto tambem for iniciado, exportando e configurando variaveis globais que não devem-se repetir durante o projeto, assim evitando multiplas conexões desnecessarias com os bancos.

### database
Arquivos como `models`, `schemas`, `modelInterfaces` e [`dfms`](/banco-de-dados/?id=dfm39s) onde serão utilizados atraves de `controllers`, `middlewares` e qualquer outro tipo de arquivo que exige a conexão com o banco de dados, seja o banco do cliente ou interno.

### library
Essa pasta é a `raiz` do projeto, onde as bibliotecas customizadas, tratativas de frontend e backend, controles e roteadores customizados para o express, validações automaticas e construção de schemas/interfaces automaticas e mais, basicamente seria a **"nossa"** versão da `node_modules`.

### public
Caminho onde arquivos que **SEMPRE SERÃO ACESSIVEIS AO USUÁRIO** deverá ser mantido, arquivos gerados pelo sistema que ficarão disponiveis ao usuario e pasta de imagens e recursos que o [NextJS](https://nextjs.org/) fará utilização.

### resources
Arquivos para utilização interna, como templates de relatórios/impressões, `JSON's` que foram escritos manualmente que tambem serão utilizados pelo backend, não é aconselhado servir nenhum arquivo desta pasta ao frontend sem tratativa do backend primeiro.

### routes
Atualmente essa pasta contem apenas `web.ts` onde são as rotas que o sistema servirá no backend (pode se utilizar para o frontend, mas não é recomendado).

### src
Arquivos para utilização do [NextJS](https://nextjs.org/) e [React](https://pt-br.reactjs.org/).


### storage
Pasta não possui utilização atualmente, mas será uma nova versão da pasta `public` onde pode se manter arquivos tanto para o backend, como para o frontend.
