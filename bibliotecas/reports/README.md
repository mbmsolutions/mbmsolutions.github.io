# Reports

Classes onde a ideia é a manipulação de documentos, relatórios e arquivos onde o cliente possui mais contato, exemplo emails, impressões, notas fiscais, etc.

### `@library/reports/email` :id=email

Utilizada para disparos de email's com o templating de [handlebars](https://handlebarsjs.com/) ou HTML puro, exemplo de utilização:


```ts
new Email()
    .from("willian.carminatti@mbmsolutions.com.br")
    .to("a@a.com")
    .subject("Email de teste")
    .render("password-recovery", { recoveryKey: 'sua-chave-é-essa' })
    .send();
```

?> A função **render**, utiliza a biblioteca de `view` referenciada abaixo

### `@library/reports/view` :id=view
Classe utilizada para extensão de [handlebars](https://handlebarsjs.com/) para funções customizadas e facilidades para criação dos mesmo, exemplo de utilização:

```ts
View.render("password-recovery", { recoveryKey: 'sua-chave-é-essa' })
```
