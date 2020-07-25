# Projeto com React Typescript

- Para iniciar o projeto execute no terminal:

```bash
create-react-app NOME_DO_PROJETO --template=typescript
```

- Acesse a pasta do projeto

- Vamos eliminar alguns arquivos em src:

  - App.css
  - App.test.tsx
  - index.css
  - logo.svg
  - serviceWorker.ts

- Vamos eliminar alguns arquivos em public:

  - favicon.ico
  - logo192.png
  - logo512.png
  - manifest.json

- No arquivo index.html iremos remover os comentários e algumas tags:

```html
<link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
<meta
  name="description"
  content="Web site created using create-react-app"
/>
<link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />

<link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
```

---

## Editor Config

- Instale o Editor Config no vscode: [](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

- Na raiz do projeto clique com o botão direito do mouse e selecione:

```
Generate .editorconfig
```

- Será gerado um arquivo e insira o código:

```yml
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
```

---

## ESLint

- No arquivo `package.json` remover:

```json
"eslintConfig": {
  "extends": "react-app"
},
```

- Instalar a dependencia:

```bash
yarn add eslint@6.8.0 -D
```

- Iniciar a configuração do eslint:

```bash
yarn eslint --init
```

- Responder as perguntas:

1 - `How would you like to use ESLint?`
  - `To check syntax, find problems, and enforce code style`

2 - `What type of modules does your project use?`
  - `JavaScript modules (import/export)`
3 - `Which framework does your project use?`
  - `React`
4 - `Does your project use TypeScript?`
  - `y`
5 - `Where does your code run?`
  - `Broser` (apenas manter esse selecionado)
6 - `How would you like to define a style for your project?`
  - `Use a popular style guide`
7 - `Which style guide do you want to follow?`
  - `Airbnb: https://github.com/airbnb/javascript`
8 - `What format do you want your config file to be in?`
  - `JSON`
9 - `Would you like to install them now with npm?`
  - `n`

- Um pouco acima da pergunta 9 ele irá exibir as dependencias que deverão ser instaladas nesse caso

```bash
eslint-plugin-react@^7.20.0 @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint@^5.16.0 || ^6.8.0 || ^7.2.0 eslint-plugin-import@^2.21.2 eslint-plugin-jsx-a11y@^6.3.0 eslint-plugin-react-hooks@^4 || ^3 || ^2.3.0 || ^1.7.0 @typescript-eslint/parser@latest
```


E como respondemos `No` vamos ter que adicionar manualmente as dependências, para isso basta copiar os pacotes listados acima da pergunta e adicionar com `yarn add` apenas removendo o `eslint@^5.16.0 || ^6.8.0` pois já temos o **ESLint** instalado, e remover a versão `1.7.0` do trecho `eslint-plugin-react-hooks@^2.5.0 || ^1.7.0` o comando final vai ficar:

```bash
yarn add eslint-plugin-react@^7.20.0 @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint-plugin-import@^2.21.2 eslint-plugin-jsx-a11y@^6.3.0 eslint-plugin-react-hooks@^2.3.0 @typescript-eslint/parser@latest -D
```

- Criar o arquivo `.eslintignore`:

```yml
/*.js
**/*.js
node_modules
build
/src/react-app-env.d.ts
```

- O arquivo `.eslintrc.json` deverá ficar assim:

```js
{
  "env": {
      "browser": true,
      "es6": true
  },
  "extends": [
      "plugin:react/recommended",
      "airbnb",
      "plugin:@typescript-eslint/recommended"
  ],
  "globals": {
      "Atomics": "readonly",
      "SharedArrayBuffer": "readonly"
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
      "ecmaFeatures": {
          "jsx": true
      },
      "ecmaVersion": 2018,
      "sourceType": "module"
  },
  "plugins": [
      "react",
      "react-hooks",
      "@typescript-eslint"
  ],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "camelcase": "off",
    "react/jsx-filename-extension": [1, { "extensions": [".tsx", ".ts"] }],
    "import/prefer-default-export": "off",
    "@typescript-eslint/explicit-function-return-type": ["error", {"allowExpressions": true}],
    "import/extensions": [
      "error",
      "ignorePackages",
      {
        "ts": "never",
        "tsx": "never"
      }
    ]
  },
  "settings": {
    "import/resolver": {
      "typescript": {},
      "babel-plugin-root-import": {
        "rootPathSuffix": "src"
      }
    }
  }
}

```

## Prettier

- Instale a dependencia:

```bash
yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
```

- Criar o arquivo `prettier.config.js`

```js
module.exports = {
  singleQuote: true,
  trailingComma: 'all',
	arrowParens: 'avoid',
}
```

- Adicionar a lib:

```bash
yarn add eslint-import-resolver-typescript -D
```

---

## Root Import ReactJS

Para importar arquivos muitas vezes é necessário atravesar várias e várias pastas como no caso de `../../../pasta/subpasta` para evitar isso podemos utilizar a dependencia do babel root import.

- Para isso precisamos instalar o seguinte:

```bash
yarn add customize-cra react-app-rewired -D
```

- Instalar também o seguinte:

```bash
yarn add babel-plugin-root-import -D
```

- Criar o arquivo na raiz do projeto chamado `config-overrides.js`:

```js
const { addBabelPlugin, override } = require('customize-cra');

module.exports = override(
  addBabelPlugin([
    'babel-plugin-root-import', // Plugin
    // Abaixo configurações do plugin
    {
      rootPathSuffix: 'src', // Pasta onde está a maioria do meu código
    },
  ])
);
```

- Esse arquivo é carregado automáticamente pelo `react-app-rewired`

- Ajustar no `package.json` dentro do bloco `scripts`:

```json
"scripts": {
  "start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
  "eject": "react-scripts eject"
},
```

- Nos arquivos podemos utilizar o seguinte na importação `~/pasta_dentro_do_src/`

- Reiniciar a aplicação utilizando:

```bash
yarn start
```

- Até agora tudo certo o React consegue compreender quando utilizamos essa sintaxe, porém o `eslint` se perdeu, para resolver isso instalamos a seguinte dependência:

```bash
yarn add eslint-import-resolver-babel-plugin-root-import -D
```

- No arquivo `.eslintrc.js` adicionamos o seguinte no final do arquivo:

```js
settings: {
  "import/resolver": {
    "babel-plugin-root-import": {
      rootPathSuffix: "src"
    },
  },
},
```

- Agora o `eslint` ok, porém precisamos informar ao vscode do novo padrão, crie o arquivo `tsconfig.paths.json`


```json
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "~/*": ["*"]
    }
  }
}
```

- Por fim no arquivo `tsconfig.json` adicione:

```json
"extends": "./tsconfig.paths.json"
```

---

## Criando rotas da aplicação

- Para iniciar vamos instalar a lib:

```bash
yarn add react-router-dom
```

- E instale a definições de tipos:

```bash
yarn add @types/react-router-dom -D
```

- Criar o arquivo: `src/routes/index.tsx`

- Vamos criar também as paginas

- Primeiro vamos criar o Dashboard, `src/pages/Dashboard/index.tsx`

- E a página `src/pages/Repository/index.tsx`

- Vamos chamar as rotas em `src/App.tsx`

- Nesse componente adicionamos o `BrowserRouter`, mais informações em [React Router Dom](https://reactrouter.com/web/guides/quick-start)

- Algo importante está em `src/routes/index.tsx`, adicionamos a propriedade `exact` na primeiro rota, pois a verificação é por inclusão ou seja verifica se a rota inclui a path informado, e como todas as rotas inclui o `/`, ele irá entrar na primeira, dessa forma para mudarmos esse comportamento para que ele verifique se a primeira é exatamente `/` adicionamos a propriedade `exact`

- O Componente `Switch` garante que apenas uma rota seja exibida e não todas de uma vez só


---

## Styled Components

- Inicialmente instale o component:

```bash
yarn add styled-components
```

- Instale também a definição de tipos:

```bash
yarn add @types/styled-components
```

- Precisamos ter instalado a extensão `vscode-styled-components` no VSCode para uma sintaxe highlight do css no template literals do js

- Precisamos escrever no formato de `Template Literals`

- Criamos o arquivo `src/pages/Dashboard/styles.ts`

---

### Style global

- Criar o arquivo `src/styles/globals.ts`

- Importar ele no `src/App.tsx`


---

### Clareando / Escurecendo as cores

- Para isso instale a dependencia:

```bash
yarn add polished
```

---

### Icons

- Instale a pacote de icones:

```bash
yarn add react-icons
```

---

## API axios

- Instale a api do axios:

```bash
yarn add axios
```

- Criar o arquivo `src/services/api.ts`

- Importar esse arquivo em `src/pages/Dashboard/index.tsx`


---

## Utilizar props no styled components

- Para utilizar props no styled components precisamos utilizar interface do typescript, um exemplo temos no `src/pages/Dashboard/styles.ts`:


```ts
import styled, { css } from 'styled-components';

interface FormProps {
  hasError: boolean;
}

export const Form = styled.form<FormProps>`

  input {
    flex: 1;
    height: 70px;
    padding: 0 24px;
    border: 0;
    border-radius: 5px 0 0 5px;
    color: #3a3a3a;
    border: solid 2px #fff;
    border-right: 0;

    ${(props) => props.hasError && css`border-color: #c53030;`}

    &::placeholder {
      color: #a8a8b3;
    }
  }
`;
```

- Transformar valor em Boolean:
  ```ts
    Boolean(valor);
  ```
  Ou
  ```ts
    !!valor
  ```

---

## Salvar dados no localStorage

- Para obter os dados do localstorage como não é uma informação assincrona podemos utilizar essa chamada direto no useState:

```ts
const [repositories, setRepositories] = useState<Repository[]>(() => {
    const storageRepositories = localStorage.getItem('@GithubExplore:repositories');
    if (storageRepositories) {
      return JSON.parse(storageRepositories);
    }
    return [];
  });
```

- E para definir informações no local storage podemos realizar isso no `useEffect` para toda vez que o estado repositories alterar:

```ts
useEffect(() => {
    localStorage.setItem('@GithubExplore:repositories', JSON.stringify(repositories));
  }, [repositories]);
```


---

## Navegar para outra página

- Para navegar de uma página para outra no react utilizamos o `Link` do `react-router-dom`:

```tsx
<Link key={repository.full_name} to={`/repositories/${repository.full_name}`} >Link</Link>
```

- Uma dica no `src/routes/index.tsx`:

```tsx
<Route path="/repositories/:repository+" component={Repository} />
```

- O sinal de mais ali no final indica que é tudo o mais que vier a mais depois de :repository, como no caso se a rota for `/repositories/autor/repositorio` então essa parte `autor/repositorio` será recebida pelo `:repository+`


- Para receber esse paramentro `:repository+` na tela seguinte utilizamos o seguinte:

```tsx
// importar o useRouteMatch
import { useRouteMatch } from 'react-router-dom';

interface RepositoryParams {
  repository: string;
}

const Repository: React.FC = () => {
  // adicionamos a tipagem como generics
  const { params } = useRouteMatch<RepositoryParams>();

  return (
    <h1>
      Repository
      {params.repository}
    </h1>)
}
```

## Retornar duas chamadas api ao mesmo tempo

1 - Forma, utilizando o then:

```ts
  api.get(`repos/${params.repository}`).then((response) => {
    console.log(response);
  });

  api.get(`repos/${params.repository}/issues`).then((response) => {
    console.log(response);
  });

```

2 - Forma utilizando Promise.all():

```ts
const [repository, issues] = await Promise.all([
  api.get(`repos/${params.repository}`),
  api.get(`repos/${params.repository}/issues`),
]);
```

- Existe uma function da Promise que pode ser bem util que é o race, no caso de precisar buscar várias requisições o promise irá executar todas ao mesmo tempo e a que retornar primeiro é aceita as demais serão descartada...
