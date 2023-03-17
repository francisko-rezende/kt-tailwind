# KT tailwind

## Setup

`

### Next

- Iniciar o projeto com Next

```bash
npx create-next-app@latest
```

- Escolhi as opções padrão durante a instalação

### Prettier

- Instalar prettier

```bash
yarn add --dev --exact prettier
```

- Criar configurações vazias

```bash
echo {} > .prettierrc.json
```

- Esse passo é importante mais pra frente, quando formos configurar o plugin do prettier pro tailwind
- Criar `.prettierignore`

```
# Exemplo da documentação
build
coverage

# Outras pastas que podem entrar
.next/**
public/**
out/**
node_modules/**
.git/**
```

- Instalar o [plugin do prettier pro VSCode](https://github.com/prettier/prettier-vscode)

### Prettier + ESlint no Next

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
