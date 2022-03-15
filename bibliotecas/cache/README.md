# Cache
A biblioteca de cache foi feita com a utilização do Redis com o intuito de facilitar e otimizar as utilização de rotas, dados e respostas.

### Redis
Dentro das bibliotecas localizado em `@library/cache/Redis` é possivel utilizar a classe que executa metodos e salva em respectivas chaves.

###### Qual a utilidade ?
A classe pode conter dados retornados de uma query do banco de dados, por exemplo, caso exista menus laterais no frontend e o usuário precisa consultar todos os menus, todas as vezes que entra em qualquer tela, com o cache, pode se evitar executar a mesma query para cada clique do usuário, assim aumentando a performance do banco de dados.
Exemplo:
```ts
//Verifica se ja existe o usuário com X token no banco do redis, caso não tenha executar a query
const user: any = await Redis.getCache("x-token:user:" + token, async () => {
    /*
    O retorno da query será automaticamente 
    salvo na chave "x-token:user:Y" sendo Y a token requisitada
    */
    return await User.query().findOne({
        user_token: token,
    });
});
```

###### Quando devo utilizar ?
A classe não deve ser utilizada em todos os tipos de consulta pois precisa-se lembrar que os dados do banco podem ser modificados a qualquer momento! Assim poderá causar diferentes dados visuais para cada usuário acessando o sistema, mas para tratar isso, pode-se ser feito a limpeza de algumas chaves de cache caso ela seja modificada

### Decorators
A função dos [decorators](https://www.typescriptlang.org/docs/handbook/decorators.html) são para diminuir o código escrito e mesmo assim manter a funcionalidade, por exemplo:

```ts
/*
A resposta da requisição será mantida por 10 minutos

Levando em consideração que a chave será automaticamente gerada
com os parametros da requisição, pois nem todas as 
requisições possuem o mesmo parametro
*/
@withRedisCache(10)
public async schema(req: Request, res: Response) {
    ...
    return {
        status: 200,
        message: "Schema localizado!"
    }
}
```