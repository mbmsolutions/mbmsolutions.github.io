# Decorators
A framework possuir alguns [decorators](https://www.typescriptlang.org/docs/handbook/decorators.html) que podem ser utilizados para facilitar ou otimizar o código sem a necessidade de repetição

###### withRedisCache(timeout: number) :id=cache
Adiciona cache sobre redis no metodo utilizado, o ideal é utilizar somente sobre funções de `controllers`, exemplo:

```ts
export default HomeController {

    @withRedisCache(60) //Essa pagina manterá o cache por 60 segundos do valor aleatorio
    public async homePage(req: Request, res: Response, next: NextFunction) {
        let valorAleatorio = Math.random();
        return `Olá mundo, seu numero é ${valorAleatorio}!`
    }
}
```

###### withParameter(parameterName :string, parameterType :ITypes = "any", parameterDefault :any = null) :id=withParameter
A utilidade desse decorator é para obrigar parametros em métodos de rotas, exemplo:

```ts
/*
Com a requisição, será obrigatorio existir o parametro ID, seja ele na Query,Body ou Params.
Caso a regra não seja cumprida, irá retornar automaticamente o erro 400 apontando o parametro
*/
@withParameter("id", "number")
public async fetch(req: Request, res: Response, { id }: any) {
    /*
    O parametro exigido acima, será recebido automaticamente como o terceiro parametro da função
    em forma de objeto.
    */
    const model = this.getModelByRequest();
    let queryResult: any = [];
    ...
    return {
        status: 200,
        result: queryResult
    }
}
```

!> :construction: esta página ainda está em desenvolvimento!
