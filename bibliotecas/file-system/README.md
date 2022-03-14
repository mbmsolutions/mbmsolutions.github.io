# FileSystem
As classes de FileSystem permite manipular, criar, excluir e ler arquivos com mais facilidade, assim sendo mais seguro a movimentação de arquivos apenas por dentro do projeto.

### `@library/fileSystem/config` :id=config
Utilizada para ler dados de [configurações](/configurações/) do projeto, exemplo de utilização:

```ts
const isDevelopment = Config.get("app.env") !== "PRODUCTION";
```
