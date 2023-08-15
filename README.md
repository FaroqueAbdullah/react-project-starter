We will see, How to set up a React Application step by step. We will try to use some best practices and tools to make our applications more robust and development friendly.

Topics

- [Create React App](#create-react-app)
- [Build Analyzer](#build-analyzer)
- [Engine Locking](#engine-locking)
- [ESLint and Prettier](#eslint-and-prettier)
- [Husky](#husky)

## Create React App

Create React App (CRA) is a command-line tool that helps you create a React application quickly and easily. It does this by setting up all the necessary dependencies and configurations for you, so you can focus on writing React code.

We will use typescript for our codebase.

We can use npx

```
npx create-react-app project-name --template typescript
```

But, We will try to use Yarn for our project.

```
yarn create react-app client project-name --template typescript
```

CRA uses several popular tools under the hood, Webpack, Babel, Jest and many more, CRA also includes several features that make it easy to develop React applications, such as:

Hot reloading: This feature allows you to see your changes to the React code immediately without having to restart the application.

Code splitting: This feature allows you to load only the parts of your React application that are needed by the user, which can improve performance.

Production build: CRA can create a production build of your React application that is optimized for performance and security.

## Build Analyzer

We'll install the dependencies we need to analyze the project.

craco: Create React App Configuration Override, an easy and comprehensible configuration layer for create-react-app.

webpack-bundle-analyzer: It will create an interactive treemap visualization of the contents of all your bundles.

```
yarn add @craco/craco webpack-bundle-analyzer -D
```

Now, Add craco.config.js file and configure webpack-bundle-analyzer.

```
const BundleAnalyzerPlugin = require("webpack-bundle-analyzer").BundleAnalyzerPlugin;

module.exports = function () {
  return {
    webpack: {
      plugins: [new BundleAnalyzerPlugin({ analyzerMode: "server" })],
    },
  };
};
```

craco build is as same as npm run build but it will inject any webpack configuration that are overridden by the craco.

We need to add a new script in our package.json file to start analyze our craco build file.

```
{
  ...
  "scripts": {
    ...
    "analyze": "craco build"
  }
}
```

Now, We can analyze our applications by running

```
yarn run analyze
```

## Engine Locking

We will write two file .nvmrc and .npmrc to maintain and force developers to use selected node versions and selected package managers. For node it will be Node version 16 and greater, for package manager it will be yarn

.nvmrc

```
v16
```

.npmrc

```
engine-strict=true
```

also, We will updated our package.json file to configure our engine

package.json

```
{
  ...
  "engines": {
    "node": ">=16.0.0",
    "yarn": ">=1.22.0",
    "npm": "please-use-yarn"
  },
}
```

Where we are declaring our node version should be greater than 16 and we are bound to use yarn.

## ESLint and Prettier

we will add eslint to our dev dependencies

```
yarn add eslint --dev
```

Initiate eslint to our project

```
yarn run eslint --init
```

Update package.json file to add lint commad

```
{
  ...
  "scripts": {
    ...
    "lint": "eslint ./src"
  },
}
```

Now we can run

yarn lint

Now, let's add prettier

```
yarn add eslint-config-prettier eslint-plugin-prettier prettier --dev
```

Now update .eslintrc

```
{
  ...
  extends: [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:prettier/recommended",
  ],
}
```

now we will add prettier ignore file

.prettierignore

```
.yarn
.next
build
node_modules
```

we will also add .prettierrc file to config the prettier file

```
{
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true
}
```

Now update package.json to run prettire

```
{
  ...
  "scripts": {
    ...
    "prettier": "prettier --write ."
  },
}
```

We can run prettier

```
yarn prettier
```

## Husky

First install husky

```
yarn add husky -D
```

Now initiate husky

```
yarn husky install
```

update package.jason to prepare husky in every npm install

```
{
  ...
  "scripts": {
    ...
    "prepare": "husky install"
  },
}
```

Now we can add pre-commit and pre-push whatever configuration we need

```
yarn husky add .husky/pre-commit "yarn lint"

yarn husky add .husky/pre-push "yarn build"
```
