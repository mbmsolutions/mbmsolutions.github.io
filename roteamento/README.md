# Roteamento

Atualmente todos os arquivos e configurações de rota, vem do arquivo `@library/routing/RouteHandler` onde permite a utilização de todos os metodos de requisições como, `GET/POST/UPDATE...` e outros.

### Basico

Para adicionar uma nova rota, deve ser implementado no arquivo `@routes/web` sendo adicionado a qualquer ordem, por exemplo uma rota do tipo `GET` para a rota `/`:

```ts
router.get("/", "Api/HomeController@index");
```

Onde o caminho `/` irá executar a função `index` do arquivo `@routes/web/Api/HomeController.ts`

### Middlewares

Middlewares são executadas sempre no meio tempo de uma requisição, atualmente só podem ser adicionar em `groups` mencionado acima, para utiliza-lá, é necessario coloca-la em `@app/http/middlewares/` com o nome desejado (em CamelCase) , exemplo:

!> É **obrigatório** adicionar o método assincrono chamado `handle` à middleware!

```ts
export default class AdminMiddleware {
    public async handle(req: Request, res: Response, next: NextFunction) {
        //Verifica se o usuário autenticado é um admin
        return req.input("user")?.user_type == "admin" ? true : false;
    }
}
```

é possivel retornar os seguintes tipos de parametros:

###### boolean **true ou false**

Caso `true`, proximo metodo será executado sem problemas, caso `false`, será interrompido e retornará

```json
{
    "status": 401,
    "message": "Unauthorized."
}
```

###### object

Caso retorne um objeto, será enviado a resposta exatamente igual ao objeto criado pelo usuário.

### Groups

Tambem é possivel utilizar a função `group` onde a ideia é permitir o agrupamento de rotas e utilização de middlewares sem a necessidade de repetição ou duplicação de código, exemplo:

exemplo sem `middleware`:

```ts
router.group("/auth", () => {
    router.get("/me", "Api/AuthController@me");
    router.post("/login", "Api/AuthController@login");
    router.post("/forgot-password", "Api/AuthController@forgotPassword");
    router.post("/recovery-password", "Api/AuthController@recoveryPassword");
});
```

exemplo com `middleware`:

```ts
router.group("/:model", ["AuthenticatedMiddleware"], () => {
    router.all("/list", "Mbm/ApiController@list");
    router.all("/select", "Mbm/ApiController@select");
    router.get("/:id", "Mbm/ApiController@get");
});
```

A ordem dos parametros são

-   Caminho `string`
-   Middlewares ou Callback `string[] | Function`

### Request & Response

A requisição e a resposta existem bibliotecas customizadas localizadas em `@library/routing/Request` e `@library/routing/Response`, atualmente não existe mapeamento de todas as funções, porem podem ser visualizadas com o proprio _autocomplete_ do `TypeScript`

### Extensão NextJS

É possivel extender ações padrões da framework utilizando o `getServerSideProps` do [NextJS](https://nextjs.org/), como por exemplo, adicionando uma **permissão** automatica dentro de uma pagina do frontend é feito da seguinte maneira:

```tsx
export const getServerSideProps = async (context: any) => Permissions.set(context).withModules(["interno"]).withPermission(["{{model}}.update"]).handle();
```

**TODO:** Adicionar mais informações de como utilizar `name`, `title`, `appends` e `register`

Caso deseje utilizar multiplas extensões para o NextJS, basta utilizar como `wrapper` a extensão `withAllProps`, exemplo:

```tsx
export const getServerSideProps = withAllProps([withForm(["p.001"]), withForm(["p.002"])]);
```

### Cron

O sistema de roteamento do backend possibilita a utilização do **Cron** para rotinas que devem ser executadas em determinado intervalo de tempo, exemplo:

```ts
router.cron("*/5 * * * * *", "Mbm/Wms/OrderController@buscaNovosXmlsNfe");
```
