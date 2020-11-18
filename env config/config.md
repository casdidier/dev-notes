## configure eslint of vscode

`npm i -D eslint`
`npx eslint --init`

https://github.com/karlhadwen/eslint-prettier-airbnb-react

## configure eslint + prettier of vscode

go to [https://github.com/paulolramos/eslint-prettier-airbnb-react]

## fix auto-import issues on vscode

create a file jsconfig.json at the root

```json
{
  "exclude": ["node_modules"]
}
```
