# Propriedades

###### orderBy *static*
<!-- Parametro padrão que será utilizado no orderBy de qualquer query.
`orderBy: string[] | OrderByDirection[]` -->

###### [appends *static*](a)
<!-- O parametro faz que com a lista de strings de `relacionamentos` seja automaticamente utilizada nas consultas, exemplo:
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
`appends: string[]` -->