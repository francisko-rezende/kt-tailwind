# KT tailwind

- Iniciar o projeto com Next
- Configurar prettier

```bash
yarn add --dev eslint-config-prettier
```

- Adicionar o prettier às configurações do eslint pra evitar conflitos

```json
//.eslintrc.json
{
  "extends": ["next", "prettier"]
}
```
