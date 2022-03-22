# JsonSchemaEditor

O editor possui a função de facilitar a criação de "JsonSchemas" que sejam validos para a criação e validação de formulários dentro do sistema, o projeto é totalmente separado feito em NextJS, a instalação é 100% padrão do próprio NextJS e é necessario apenas configurar o banco de dados que será salvo os dados do formulário.

### Formulários

Os formulários podem ser importados no sistema pela classe `@library/forms/Form` sendo possivel utilizar dois métodos principais da classe, o `getSchemaAsync` onde retorna o ultimo schema do formulário do próprio banco de dados, sendo sempre atualizado, possui o problema de possuir menos performance e o `getSchema` que retorna o schema do formulário salvo em cache ao inicializar o sistema, a desvantagem é que modificações feita no banco de dados, não são diretamente visíveis no formulário, ou seja, não é possivel fazer alterações no formulário sem que o sistema seja reiniciado.

Para o frontend pode ser utilizado o decorator `@library/forms/decorators/withForm` onde o parametro principal deve ser uma $string ou $Array<$string>, exemplo:

```ts
/*
Isso resultará preenchendo o componente de pagina com a propriedade "form" e 
o valor será o formulário que foi importado no banco de dados.
*/
export const getServerSideProps = withForm(["p.001", "p.002"]);
```

Exemplo de utilização dos parametros dentro do componente de página:

```tsx
export default class ParametersPage extends React.Component<any, {}> {
    render() {
        /*
        Resultará em 2 formularios sendo respectivamente p.001 e p.002
        */
        return (
            <>
                {_.map(this.props.forms, (form: any) => {
                    return (
                        <>
                            <h1>{form.title}</h1>
                            <Form schema={form.properties} />
                        </>
                    );
                })}
            </>
        );
    }
}
```

E abaixo segue um exemplo de como é o registro no banco:

| form_id | form_name | form_code | form_schema | form_ui_schema | form_created_at | form_updated_at |
| ------- | --------- | --------- | ----------- | -------------- | --------------- | --------------- |
| 2       | menus     | d.001     | {...schema} | {...uiSchema}  | 2020-05-05      | 2020-05-05      |

Exemplo de schema do formulário

```json
{
    "title": "Menus",
    "icon": "fa-list",
    "type": "object",
    "required": ["menu_title", "menu_slug", "menu_order", "menu_icon"],
    "properties": {
        "menu_id": {
            "type": "number",
            "title": "Código"
        },
        "menu_module": {
            "type": "string",
            "title": "Módulo",
            "enum": ["default", "relatorios"],
            "enumNames": ["Menu", "Relatórios"],
            "default": "default"
        },
        "menu_title": {
            "type": "string",
            "title": "Título"
        },
        "menu_slug": {
            "type": "string",
            "title": "Slug"
        },
        "menu_icon": {
            "type": "string",
            "title": "Ícone"
        },
        "menu_order": {
            "type": "number",
            "title": "Ordem"
        },
        "menu_color": {
            "type": "string",
            "title": "Cor do menu"
        }
    },
    "name": "menus"
}
```
