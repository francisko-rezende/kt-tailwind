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

### Storybook

- Setar storybook com Next

```bash
npx storybook@next init
```

- Instalei também o plugin do storybook pro eslint

### Tailwind

#### Instalar tailwind no next

```bash
yarn add -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

- Configurar o _path_ para os arquivos no `tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx}",
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",

    // Or if using `src` directory:
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

- Adicionar as diretivas (comandos especiais pra que as classes do tailwind sejam aplicadas ao html) ao `globals.css`

```css
/* globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

- Agora já deve estar funcionando!

### Configurar VSCode com tailwind

- **Instalar a** [**extensão de IntelliSense**](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)

  - Mão na roda, ajuda com:
    - Autcomplete
    - Linting pra css e html
    - Preview quando rolar hover
    - Syntax highlighting

- **Instalar também o** [**plugin do prettier pra tailwind**](https://github.com/tailwindlabs/prettier-plugin-tailwindcss)
  - Importante pra manter a ordem de classes => precedência no css
    - classes que vem depois no têm prioridade independente da ordem em que as classes aparecem no HTML
    - plugin coloca as classes na ordem em que elas entram no css, evitando dor de cabeça
    - [fonte](https://www.youtube.com/watch?v=QBajvZaWLXs)

```bash
yarn add -D prettier prettier-plugin-tailwindcss
# Nem precisava de instalar o `prettier` porque já estava no projeto mas deixei pra manter igual á doc do plugin
```

- Apontar a localização do `tailwind.config.js` pra que classes customizadas também sejam ordenadas. O _path_ é relativo à localização do arquivo de configurações do prettier.

```js
// prettier.config.js
module.exports = {
  tailwindConfig: "./tailwind.config.js",
};
```

Ou então:

```json
// prettierrc.json
{
  "tailwindConfig": "./tailwind.config.js"
}
```

**Na doc do plugin eles mostram da primeira forma**

### Storybook com tailwind

- Disponibilizar o tailwind pras stories

```js
// .storybook/preview.js

import "../src/styles/globals.css";
```

## Mão na massa

### Botão

```tsx
import type { Meta, StoryObj } from "@storybook/react";
import { KtButton } from "./KtButton";

const meta: Meta<typeof KtButton> = {
  title: "KT/Button",
  component: KtButton,
  tags: ["autodocs"],
};

export default meta;
type Story = StoryObj<typeof KtButton>;

export const Primary: Story = {};
```

```tsx
type KtButtonProps = {
  label: string;
};

export const KtButton = ({ label = "KtButton" }: KtButtonProps) => {
  return <button>{label}</button>;
};
```

- Funcionamento básico do tailwind
- Hover
- Responsividade (mobile first)
  - container
- dark mode
- customização
  - inline: #e00f63
  - tema
- possíveis problemas
  - loooooooooooooooooooooooooooooooooongo
    - extensão `inline fold`
  - repetição de código

```css
@layer components {
  .btn-primary {
    @apply w-full rounded-full bg-blue-500 py-4 px-16 text-xl transition-colors hover:bg-blue-600 active:bg-blue-700 dark:bg-rose-500 dark:hover:bg-rose-600 dark:active:bg-rose-700 md:w-auto;
  }
}
```

### Lista

```tsx
import Image from "next/image";
import molejao from "../../../public/molejao.jpeg";

type UlProps = {
  children: React.ReactNode;
};

const Ul = ({ children }: UlProps) => {
  return (
    <div className="relative grid h-60 w-full place-content-center lg:h-80 lg:max-w-2xl">
      <div className="absolute inset-0">
        <Image src={molejao} alt="Molejao" fill />
      </div>

      <ul className="rounded-md bg-white bg-opacity-25 p-6 backdrop-blur-sm backdrop-filter">
        {children}
      </ul>
    </div>
  );
};

export const List = () => {
  return (
    <Ul>
      <li className="text-lg font-black text-slate-800">Pera 🍐</li>
      <li className="text-lg font-black text-slate-800">Uva 🍇</li>
      <li className="text-lg font-black text-slate-800">Maçã 🍎</li>
      <li className="text-lg font-black text-slate-800">Salada Mista 💋</li>
    </Ul>
  );
};
```

- repetição de código e como lidar com isso
  - ferramentas do próprio editor
  - map
  - componentização
  - apply, porém [a documentação recomenda evitar](https://tailwindcss.com/docs/reusing-styles#avoiding-premature-abstraction)

### Variantes

#### Setup

- Instalar CVA

```bash
yarn add class-variance-authority
```

- Convém usar também a lib `tailwind-merge` pra evitar conflitos de classe

```bash
yarn add tailwind-merge
```

- Depois de instalada temos que envolver o objeto com as variantes do nosso componente com a função `twMerge`

```tsx
<button
  className={twMerge(buttonStyles({ variant, size, className }))} //aqui
  {...props}
>
  {children}
</button>
```

- integrar com o intellisense

```json
//settings.json do vscode mesmo
{
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"]
  ]
}
```

- criar componente com a lib passa por três passos

1.  criar as variantes usando a api da lib

```ts
import { cva } from "class-variance-authority";
const buttonStyles = cva(
  [
    "classes base", // classes base, se aplicam a todas as variantes
  ],
  {
    variants: {
      // variantes do componente
      variante1: {
        opcao1: ["classe1"],
        opcao2: ["classe2", "classe3"],
      },
      variante2: {
        opcao1: ["classe4", "classe5"],
        opcao2: ["classe6"],
      },
      varianteBoleana: { true: "classe10" },
    },
    compoundVariants: [
      {
        variante1: "opcao1",
        variante2: "opcao2",
        class: ["classe7", "classe8"],
      },
    ],
    defaultVariants: {
      variante1: "opcao1",
      variante2: "opcao1",
      varianteBooleana: false,
    },
  }
);
```

2. criar o tipo do componente

```ts
import { type VariantProps } from "class-variance-authority"; // a lib fornece esse type helper pra que possamos pegar a tipagem do objeto que acabamos de ver

export interface KtButtonVariantsProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonStyles> {
  children: React.ReactNode; // adicionei children mais pra aparecer lá no storybook
}
```

1. criar o componente em si

```tsx
import { twMerge } from "tailwind-merge";

export const KtButtonVariants = ({
  className,
  variant,
  size,
  children,
  upperCase,
  ...props
}: KtButtonVariantsProps) => (
  <button
    className={twMerge(buttonStyles({ variant, size, className, upperCase }))} // passamos um objeto com as props pro objeto com as variantes que criamos
    {...props}
  >
    {children}
  </button>
);
```

> > > > > > > 7140587 (feat: finish initial version of notes and track important asset)
