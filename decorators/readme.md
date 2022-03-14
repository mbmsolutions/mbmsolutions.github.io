# Decorators
A framework possuir alguns [decorators](https://www.typescriptlang.org/docs/handbook/decorators.html) que podem ser utilizados para facilitar ou otimizar o código sem a necessidade de repetição

###### ``withRedisCache(timeout: number)``
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

!> :construction: esta página ainda está em desenvolvimento!