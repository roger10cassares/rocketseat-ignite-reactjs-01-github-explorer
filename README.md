## 1 - Introduction

At first, initialize the repository creating the package.json file

yarn init -y
yarn add react

node_modules -> Present in every projects js. This is the application (sub)dependencies of the js application

yarn add react-dom -> react for web. DOM is the tree of elements of the html browser in the application. DOM is like the html converted to javascript

## 2 - Creating the Project Structure

src -> Every created code in the application
public -> public files/assets. index.html, favicons, robot.txt (which pages to be indexed or ignored in the application) and any file that needs to be directly accessed from the external environment.

public/index.html: html5


## 3 - Configure the Babel

Babel is a tool to convert the js code to a manner the every browser could understand the all code.

Javascript is an very updatable language and meny React functionalities, for example, the writing of HTML inside the js code, that the brownser do not compreend yet. So Babel convert the code to the Browsers undestand these.

https://babeljs.io/


At new js language, there is the nullage collecing operator, that verify if the object props is null before access a subprops of that object.

const user = {
    name: ´Roger´
}
console.log (user.adress?.street)

yarn add @babel/core @babel/cli @babel/preset-env -D

-D is the development dependency. It is not used to production but it converts all the code to the browser before runs the production.

Then, create a babel.config.js at the top level directory

module.exports = {
  presets: [
    '@babel/preset-env'
  ]
}

    "@babel/core" is the core of babel library. "99%" of babel functionalities are here.
    "@babel/cli" is to execute babel throw the command line interface (yarn babel -h)
    "@babel/preset-env" is an extension of the babel that identify the enviroment that the application is being executed to convert the code of the better possible way. If the environment is node (backend) or the browser (frontend)


Create and index.js in src/

const user = {
    name: ´Roger´
}
console.log (user.adress?.street)

Than, the use of babel-cli converts the indexjs syntax to the brownser undestandlable sintax

yarn babel src/index.js --out-file dist/bundle.js

bundle is a convention name from the files converted by the babel. The developer should never modify the dist/ files because it is an auto conversion and the files are auto generated.

Now, inside the babel, we need to configure the structure to work with React.

Modify src/index.js to :

import React from 'react';

export function App() {
  return <h1>Hello World!</h1>
}

Now, when trying converting the above code with babel, the babel could not undestanding the HTML from inside the js file. THis happens because it is a particularity of the React. The Babel's presets for react needs to be installed:

SyntaxError: /home/roger/Softwares/Rocketseat/Ignite/Reactjs/01-github-explorer/src/index.js: Support for the experimental syntax 'jsx' isn't currently enabled (4:10):

  2 |
  3 | function App() {
> 4 |   return <h1>Hello World!</h1>
    |          ^
  5 | }



The solving is to add @babel/preset-react (https://git.io/JfeDR) to the 'presets' section of your Babel config to enable transformation.
If you want to leave it as-is, add @babel/plugin-syntax-jsx (https://git.io/vb4yA) to the 'plugins' section to enable parsing.

yarn add @babel/preset-react -D

From now, each HTML line inside the React code is converted to a createElement line in the dist/bundle.js file to transform the functions inside of the js to the browser understand the code and execute in the correctly way.

Here the structure of the babel is ready to work with the react!

The .js extension can be transformed to a .jsx extendion at the index.js, because .jsx is the correct nomenclature when working with html inside the js. This was introduced by the React. Basically it changes anythings but it is a good practice.

yarn babel src/index.jsx --out-file dist/bundle.js

Please note the bundle.js not turns into .jsx file because this extension not existis in the browser


## 4 - Configure the Webpack

In most cases used along side with Babel

In the js file, it is commom to import anothers js files.

But in the js we may be able to import any other fyle type as json, csv, pre-processores as sass, png.

The webpack estipulates the configurations called as loaders that will teach to the application how each type of the files have to be treated. So webpack will interpret this files and converted them to a browser understandible ones.
sass -> css
jpg/png -> jpg/png

Install webpack

yarn add webpack webpack-cli -D

Create the webpack file config in the top level directory of the project and export and configuration object.

webpack.config.js:

module.exports = {
  entry: 'src/index.jsx',
}

entry is the main initial file of the application.
webpack runs inside the nodejs server. So as nodejs just understand js, the importing format using require, all import must be with require.

Output is the file generated by the Webpack

The path library standardizes the path to Linux ('/') and Windows ('\') in just one format with 
path.resolve(__dirname, 'src', 'index.jsx')

So, using path library:

const path = require('path');

module.exports = {
  entry: path.resolve(__dirname, 'src', 'index.jsx'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  module: {
    rules: [
      {
        test: /\.jsx$/,
        exclude: /node_modules/,
        use: babel-loader
      }
    ]
  }
}

extensions: by default, webpack will read js files, but we are using jsx files also. So in the resolve we must describe which extensions the webpack shall reads.

module: how the application should behavior when we are importing each type of file.
Explicit how to lead with js files, css files 

Create an array of rules with and object for each type of files:
The first type is the js or jsx files.

THen writes a regular expression to know if the file is or is not a js or jsx file

This file must ends with .jsx (/\.jsx$/). The reverse \ (escape) means that we wanto to verify the . as file extension, not any character as .

Exclude all files from node_modules, because the files from inside the node_modules may be ready to the browser correctly interpret, because each library must provide the own build file to the browser reads.

So, when import a .jsx file from inside the node_modules, we do want that the webpack makes the conversion of this files, because it is from the library responsability.

use: integrate the babel with the webpack

yarn add babel-loader -D

When webpack trying to import the .jsx file, we need to convert the .jsx file using the babel from a menner that the browser nderstand


Testing the webpack:

Create an src/App.jsx file:

function App() {
  return <h1>Hello World!</h1>
}

In /src/index.js:

import { App } from './App';

Here in import statement, the file extension do not need to be passed, because as we passed this within the webpack config, it shall resolves automatically.


Execute webpack in the application:
yarn webpack

This command generated a new human unreadable file.

All the import we have done inside the index.js (entryfile), react and App.
The bundle.js do not have the requires lines anymore.
Thruly, the code of react and the code of App were all inside just single one file.
There is all the needy code to execute the application.
THis is the code the shall be understandble for the browser.

With babel and webpack pre configured we are able to use the react!


## React structure

React creates all the applicatin interface with javascript
In index.html there is only one element inside the body

<div id="root"></div>

All React app should be built inside that <div>

Inside index.jsx,

import render from react-dom, than pass that functipn to render the stuffs
render(WHAT TO RENDER, INSIDE WHICH ELEMENT I WOULD TO RENDER THAT INFO)

By default, we need to import the react in every file/component that is using the jsx syntax
import { React } from 'react';
In the React >17 it is not necessary anymore!



index.html 
render(<h1>Test</h1>, document.getElementById('root'))
<script src="dist/bundle.js"></script>

getElementById is a js fnction to search in screen for an element with that Id. IN THIS CASE, 'root'.

So, render with insert an h1 element with string Test inside the div with id root.

Compiple Webpack again to see the modifications

yarn webpack

Open the file at the browser


Remove import { React } from 'react'; from index.jsx, and inside the babel.config.js, then pass a config to the '@babel/preset-react'. 

To pass a paramenter, open with [] and pass and configuration object at the second position of this array.

['@babel/preset-react', {
  runtime: 'automatic'
}]

Save and re execute the webpack

yarn webpack

Now, even withou React importation, all must works fine!

Now, in the place of <h1> in index.jsx, pass the <App/> component.

render(<App />, document.getElementById('root'))


Save and webpack it again!

yarn webpack

App is not an htl element, so it no appears inside the root div!


We can execute the webpack in proction and in the development mode.

In webpacl.config.js, pass the parameter option called 
mode: 'development'

Save and webpack it again!

yarn webpack

It not minified the code, but it great to develop! It makes the compiling process faster



## Serving an static html

THere is a webpack pluging that injects the webpack compied files in the index.html, to we do not need to reference the /dist/bundle.js everytime

remove
<script src="../dist/bundle.js"></script>
from index.html,

Go to webpack.config.js and install 

yarn add html-webpack-plugin -D

import it in the webpack config file

const HtmlWebpackPlugin = require('html-webpack-plugin');

and pass the new option in the module.exports object:
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, 'public', 'index.html')
    })
  ],

Save and webpack it again!

yarn webpack

Now, inside the /dist, a file index.html was created with the script loading entry!

<script defer src="bundle.js"></script></head>

THis is the really file that will be readable from the browser!


## Webpack Dev Server

Use Dev Server from Webpack

Install with:

yarn add webpack-dev-server -D

This automatize the files from objerving the changes of the src directory. Every time the changes are identified, it creates a new /dist/bundle.js!

Inside the webpack.config.js:

  devServer: {
    contentBase: path.resolve(__dirname, 'public')
  },

From inside the contentBase, we need to pass the directory where the static content of our application is located.

yarn webpack serve

Access http://localhost:8080/


So here we have the application executing the application as a webserver with a hot reload generating a new bundle as we change any files from inside the src/ directory!!!



## Using source maps

THis is a way to visualize the final code of the application even all the code is scrambled in the bundle.js


Imagine an error in some React component in the src code.

Configure webpack.config.js

  devtool: 'eval-source-map',

After that, when error occurs or we want to know the code without buuncle in the console.log, we configure this lib, and it helps us to understand and debug the errors!


## Dev and Production Environments

Create a webpack config for dev and another for the the production

ENV variables configure variables according with the application env.

Configure webpack.config.js

NODE_ENV -> verify if the env is dev our prod and needs to be created for us.

const isDevelopment = process.env.NODE_ENV !== 'production';
module.exports = {
  mode: isDevelopment ? 'development' : 'production',
  devtool: isDevelopment ? 'eval-source-map' : 'source-map',
  ...

  source-map is a little slow yhan the eval because it generates a more detailed level

Create NODE_ENV in Unix (Linux/Mac)

NODE_ENV=production yarn webpack
 Window is not the same, so, install:

 yarn add cross-env -D

in package.json -> create a option called "scripts

When the script is being executed from the package.json, there is no need to write yarn or npm in the scripts.

"scripts": {
  "dev": "webpack serve",
  "build": "cross-env NODE_ENV=production webpack"
},

Now, run yarn dev to developmet env
run yarn build to production env


## Importing CSS files

Create src/styles/global.css

In all the Applications:

Remove the marging, padding. Set box-sizing to border-box

In the body , change the font to the Arial (Linux Windows), Helvetica (Mac) ou sans-serif

global.css

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font: Arial, Helvetica, sans-serif;
  background: #333;
  color: #FFF;
}


Remind the in React, Vue, and frontend libraries, all css files and images files of the applcation, we import from inside the js file (js or jsx).

in App.jsx

import './styles/global.css'

But it gives an error because ir cannot compreend the impor css files. Css file is not a js file nor sintax!

So, install two css loaders and inside the webpack we need to setup a new rule

Must be both to undestand css files.
yarn add style-loader css-loader -D 

    rules: [
      {
        test: /\.jsx$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      },
      {
        test: /\.css$/,
        exclude: /node_modules/,
        use: ['style-loader', 'css-loader']
      }



yarn dev


Console.log -> Element -> Style -> THere is!

THis loaders takes the css of the application and apply in the page as needed



## Using SASS

Sass is a pre-processor for css.

Chaining, bright/escurecer, 

Install

yarn add sass-loader -D

THis is a loader for webpack undertand sass files inside the js

IN webpack:

CHANGE
      {
        test: /\.css$/,
        exclude: /node_modules/,
        use: ['style-loader', 'css-loader']
      }

TO
      {
        test: /\.scss$/,
        exclude: /node_modules/,
        use: ['style-loader', 'css-loader', 'sass-loader']
      } 

.sass is a css without {}
.scss is a css with {}

Change global.css to scss in file and in App.jsx

INstall sass

yarn add node-sass -D in dev mode, because in prod it should be converted to css by the webpack

Re execute the yarn dev


Encadeameto. Style all h1 from inside the <body>

body {
  font: Arial, Helvetica, sans-serif;
  background: #333;
  color: #FFF;

  h1 {
    font-size: 80px;
  }
}


## First React Component

Components are like HTML tags in html. It has own stylings, props and structure

Components is a way to organize and modularize the application.


IN React, all is a component.

COmponent is a fuction that starts with a Firt as captal letter and returns a HTML

Always ONE COMPONENT BY FILE! Never more than one. THis a convention


Create a new file at src/components/RepositoryList.jsx

export function RepositoryList() {
  return (

  );
}

ul -> list nor in order

IN react, the transition between html and js is natural

We can interpolate the js variables inside the jsx html code with interpolation using {var}



## Props in React
 3 more important components: Components, Props, State

Props in React works like the atributes works in HTML tags.
They are info/variables that you can pass to a component to it works in a differente manner.
So we change the behavior of a component based in a information that is passing over its props.

Create RepositoryItem.jsx in /src/components

The props concept consists in you send an information from the Parent`s component (RepositoryList, outside of RepositoryItem). So, RepositoryList can send info to the RepositoryItem.

In the React child component creation, we can access the props by passing the props as the arguments of the Functional COmponent.

If the props is empty, we can write a default name in the Child component React FIle:
      <strong>{props.repository ?? 'Default'}</strong>

      ?? is like || but not consider that 0 is a invalid value

IMportant: We can pass the props as text with "", or we can pass values encapsulating it with { } and pass all info using an object as props!!


IN RepositoryItems :       <strong>{props.repository.name ?? 'Default'}</strong>

But for the others Components, the props .name is undefined!

We could pass the props for all components or verify if it is null as optional with 
props.repository?.name ?? 'Default'}.

For now, just pass the props for all!

Resuming, props in a manner to pass info from the parents components to the CHilds. 


## State of a Component

Create a ficticio Components Counter.jsx

RENDER IS THE ACT OF A COMPONENT BE EXIBIDO IN TELA, EM REACT

When we have two or more components one bellow the other, need to be an component around them.

<div> can be used, but also the Fragment <> </> is a better choice because it does the job without creating a new <div> in the html elements.

By default, the React do not monitore the variables to check if its variables are modified,
to render the changed value in tela. THis is not permormatic.

So in React, we have an concept called State.

The are variables thar React will monitor and on every change, it will modify and render the content again.

THis is not every variables, but just thatdescripted using State property. useState is the REACT HOOK for that!

The useState not only returns the new variable changed value. THis returns an array with 2 things. Using the atributing via destructuring:

const [counter, setCounter] = useState(0);

Obs: const -> contant
let -> let it change

The setCounter option gives the capability to modify the value of counter.

So, states in react is a form to have changeable variables that when they alter in the applciation, it should reflect in the interface modifying.

Any other type of variables we use in js, it should alter only in the js and not in the html interface of the page.


## Imutability in React


The Imitability behavioer previwes that a variable should never be its value modified. It always should receive a new value.


When we say that a variable is immutable, we say that we cannot change directly change the value of this var, but we will give a new value to that var.

```js
//Mutability

users = ['user1', 'user2', 'user3']
users.push('user4')
// adds user 4 in the end of the array
```
In tha way we are changing an info the memory. We access the memory and adding a new value inside the array

So, in that moment, we are making a mutation in the original value of the var users

In the imutabilidade paradigma, we cannot do that
If we want to add a new indormation at the end of the array, we have to create a new array like newUsers copyning the infromation we have already had inside users with the spread opeator, for example (...users) and add at the end, the new value of the array.


```js
//Imutability

users = ['user1', 'user2', 'user3']
newUsers=[...users, 'user4']
// adds user 4 in the end of the array
```
So, imutability always creates a new space in the memory creating the new information than modifying an existent variable that is already saved at the memory.

THis should provides a ber=tter permance in the functional programming.

It makes much easier to React understand the new existent informations inside a var when it can appont to a new space in the meory than stary comparing with the var we alread had stored in the memory.

setCounter not modifies the counter, but is sets a new value for counter.

Everytime setCOunter is called we are creating a new counter var. For thar we pass counter +1 and not 'recursive' counter ++ or counter += 1 


Resposta de Danilo Nunes em https://app.rocketseat.com.br/h/forum/react-js/d9c78174-56a2-4f82-b2ab-ef948c514be9

"Então, a imutabilidade tem uma série de vantagens para nos ajudar a manter o código e evitar cometer erros que não esperavamos ao alterar valores que não são primitivos no javascript, como arrays, funções e objetos em geral. Ou seja, teremos um código mais fácil de prever, testar, ler e com menor número de bugs.

De fato, seu pensamento está correto e o processo de criar um novo espaço na memória para alocar esse novo valor, na maioria dos casos e implementações, não será mais perfomático do que simplesmente fazer uma alteração.

Porém, de modo geral, o ganho de perfomance é quase que insignificante, fazendo todos os benefícios de trabalhar com imutabilidade pesarem mais na balança.

Agora falando sobre imutabilidade dentro do próprio react é importante entender que o framework foi pensando para funcionar dessa forma, então, ele já está bem otimizado para isso e precisamos trabalhar com imutabilidade para que ele funcione corretamente.

Ou seja, para o react é menos custoso entender que duas referências de objetos são diferentes e, por isso, houve uma alteração e ele deve renderizar um componente, do que comparar valores.

Quando o Diego fala sobre garantir uma melhor performance ao usar a imutabilidade no react ele tá falando dentro desse contexto onde o react foi criado e está preparado para fazer as renderizações e reações esperando uma nova variável na memória do que comparações. O vuejs, que é outro framework, faz a criação de interface trabalhando com mutabilidade.

Espero ter ajudado e explicado melhor."



## Fast Refresh in the Webpack

It lets that the dev env be more fluid

When we are using the webpack dev server, it reset the application everytime the application code is modified, but all variables in the code are reseted. An example is the count var. It turn to its initial states 0 when we modify the code.

It resets all the states and all we have changed in the application
An example is reset all products in the shop car when we change something


The fast refresh in React is a way to alter the code from our app and save this code and have this code reflected in the browser and keeping the state of the components.

Lets configure it from the zero

yarn add -D @pmmmwh/react-refresh-webpack-plugin react-refresh

in webpack config

```js
const ReactRefreshWebpackPLugin = require('@pmmmwh/react-refresh-webpack-plugin')
...
  plugins: [
    isDevelopment && new ReactRefreshWebpackPLugin(),
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, 'public', 'index.html'),
    })
  ],

```
ON that way if we are not in the dev mode, the if sentence should be false and false is not a plugin
&& is a ternary that apply just the true condition to the if sentence

So, to not apply the false when not in dev mode, the .filter(Boolean) will filter all things that not be true, removing the undesrible false.
```js
const ReactRefreshWebpackPLugin = require('@pmmmwh/react-refresh-webpack-plugin')
...

  devServer: {
      hot: true,
  ...

  plugins: [
    isDevelopment && new ReactRefreshWebpackPLugin(),
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, 'public', 'index.html'),
    })
  ].filter(Boolean),

  ...

  module: {
    rules: [
      {
        test: /\.jsx$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            plugins: [
              isDevelopment && require.resolve('babel-refresh/babel')
            ].filter(Boolean)
          }

```

THis update turns the code simple very faster using the fast refresh!



# HTTP CALLS

## Stylyind if the list

Reorganizing the files, delete the COunter.jsx, removing teh counter from App.jsx

Create the file src/styles/repositories.scss and impoer it in the RepositoryList.jsx

IN Sass, & references the own element

li {
  & + li {
    // Configurations for all li that before it has a li also.
  }
}


## Using useEffect

API of the github:

api.github.com/repositories 
api.github.com/users/roger10cassares 


Always we will start a list, we start weith an empty Array
useState([])

Why the repositories are inside a var in the state?
Because as we should call an external resource from the app to retrieve the data, is takes a little longer dependining on API. Gihub ~200-300ms to respond.

So, when the components be render at the first time, the data fro the github wont be finesehd to load. So, as respositorie var shall be empty at the very first moment, beacuse the API dis not ends to load and after that fill the repositories list, always that a var have this value changing (onClick or API call), we always have to have this var in the state because it will acuse a change in the layout HTML of the App..

O Hook useEffect, basically, serves to dispatch a function when something occurs in the APp.

If a var changed, we have to emit something informing some API that something on that var has changed. Or  disparar some function on the system.

useEffect receives two parameters: which function to execute and when executed that function when the var is changed

useEffect(() => {}, [])

The secont parameter is called array of dependencies.
if void [], the useEffect function will be executed once as the component is mount in the screen.
Because as the array of dependencies is void, there is nothing that if changes, shall execute that function again.


VERY WARNING! 
Never keep the useEffect without that second params. The useEffect shall stay in loop forever!
Also we cannot update a variable inside the useEffect that is our monitoring variable! 


Fazer um fetch para a api, quando o fetch devolver essa resposta (.then), vamos convert⁻la para json, e quando a resposta para json terminar de ser convertida, teremos os dados do nosso repositorios. Quando tovermos os dados, salvamos esse json para a variavel repositories com o setRepositories(data)

Everytime a var in React changes, the React renders again it component. It is like reload the application with the repository list again. 

If we set a console.log(repositories) after useEffect function, it gives an empty array at the console.log, because the first render there was no information from the feth api. But after the fetch, it shown the repositories value with the correct one! 

Any code that we put in component or inside the function of a component that is not in the useEffect or an useState, it will be executed again everytime that that component be rendered in the DOM. 

Every state change in that component or in the component props, it should render the component again!



## Listing the repositories
Fetch the info using API and change the props to the child from Parent dinnamically

When we have an info stored in a array that we want to show some different things for each props from Parent, we can reurn it in the js or jsx file in functional component by passing { } between any returning  components in React.

From that manner we can insert the js in the HTML.

The js function map shall percorrer os repositórios e para cada um of repositories we have something different.


We can passing the { } as the function body and make a return, of repository item:
```js
{repositories.map(repository => { 
  return <RepositoryItem repository={repository}/>
})}
```
Or we can pass ( ) around and ommit the return, puttin the <RepositoryItem /> directly
```js
{repositories.map(repository => ()
  <RepositoryItem repository={repository}/>
))}
```
When the return is small (one line for examke)  we do not have to put ( ) or { }:
```js
{repositories.map(repository => <RepositoryItem repository={repository}/>)}
```

But that way, since application is work, there is an error in console:

Warning: Each child in a list should have a unique "key" prop.

It happens always we have a map in the html

Everytime we make a ma in the html, jsx com o react, we need to provide to the React, throw a prop that the own react creates, called key. The key should obeserver which information that is unique in each repository.

For example, we cannot have a repository with repeated name. So key={repository.name}. We pass repository.name prop from the api to map as the map key!

This is an internal prop of the React that helps itself to se localizar in the case of the the array of repositories changes which repository to keep or remove or alter from screen.

ALWAYS THAT WE ARE USING MAP, WE NEED TO USE AT THE VERY FIRST PROPS OF THE COMPOINENT THAT RETURNS THE MAP, THE KEY PROPERTY!!! 

So from that manner we work with state inside react and make an iteration inside an array to return an HTML to each position of an Array.


# Using Typescript

## Typescript fundamentals

When working with many people in the team we don not know what the code does and what it returns and which paramenters it receives and go on.


Typescript is a superset (a conjunto of fncionalites that we add above a language).

So the language is js and we add a mount of funcionalites above, that is the Typescript

Example:

```ts
function showWelcomeMessage(user) {
  return `Welcome ${user.name}, uour email is ${user.email}, your city is ${user.city}, your state is ${user.state}`
}
```

With the dinammically type, we do not could know what are the fields in the user object to really use, since any type was defined for it yet. It could be user.city, user.address.city or whatever. So

When we make a change in the code, or use a variable or call a funtion, we do not konw the format we are heving there inside.

The user parameter will acceps anything! THis is dinamically typing!

Typescript allows we add formats and types for what we are waiting of the application arguments or the format we are waiting to return of a function. We
For example, when we have user, we can define a type for the user. The type we always define with capital letter.


```ts

type User = {  // interfaces instead of tyoe is more to heranças para herdar contratos.
  name: string
  email: string
  adress: {
    city: string
    state?: string  // Optional 
  }
}
 
function showWelcomeMessage(user: User) { // Uner needs to follow the format that was defined!
  return `Welcome ${user.name}, uour email is ${user.email}, your city is ${user.city}, your state is ${user.state}`
}

const message = showWelcomeMessage({
  nema: 'Roger',
  email: 'roger',
  address: {
    city:'Sao Paulo',
    state: 'SP'
  }
})
```

Typescript is a static type checking. It chacks the application while we are developing.

IN production, the application will not be executed with typescript, since in prod it will be executed with typescript

Before put the code to prod, is is transpiled to js. Dino can understand the typescript in a direct way.

Typescript add an intelligence wen we are writing the code. 

The vantages is do not let we make mistakes when we are writinh the application and add some addicional inteligence when we are coding our App.

We do not have to TYpe all the var in our app. TYpecript has an TYPE`s inference. It could determine the type of the var in the most of cases.

const message = showWelcomeMessage()

As the return of the function showWelcomeMessage() is a string, so it determines thas messages var is also a string! 


if we pass an variable user inside the showWelcomeMessage(user) and user has the object data, the typescipt also suppose that the user is a string type, because it is the argument of a function that should receive User type, as it is written in its definition!


## TYpescript in the React

TS have an easy compreendable utility that isi taht determines the types the props that a component can receive

repository can have any format or receives any variables without ts.
With ts we can have the erros monitored! 

We can aldo define the types to the state!

Stop application to install some dependencies:

yarn add typescript -D

tsc is a short from to write typescript using yarn cli

INitializes the ts in the application: Create a tsconfig.json
yarn tsc --init 

go to tsconfig.json and modifu the following lines: 

```json
{
  "compilerOptions": {                     
    "lib": ["dom", "dom.iterable", "esnext"],                                   
    "allowJs": true,                             
    "jsx": "react-jsx",                           
    "noEmit": true,                              
    "strict": true,                                 
    "moduleResolution": "node",
    "resolveJsonModule": true,     
    "isolatedModules": true,          
    "allowSyntheticDefaultImports": true,        
    "esModuleInterop": true,                        
    "skipLibCheck": true,                           
    "forceConsistentCasingInFileNames": true        
  },
  "include": ["src"]
}
```

Hotkeys:
Ctrl Shift L

Ctrl Shift P
OPen keyboard shortcuts
Sellect all occurences of find match  

Remove all comments by select any /*, Ctrl Shiftt L, Shift End, Detele


Now, lets configure the typescript inside the webpack

By default, webpack is using babel-loader, but the babel-loader not interpret the ts code, just the js. 

So to babel undestand the ts, we have to add some conf to it.

yarn add @babel/preset-typescript -D

add in te oresets of babel.config.js

@babel/preset-typescript

In webpack.config.js
  entry: path.resolve(__dirname, 'src', 'index.tsx'),

resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx'],
  },

test: /\.(j|t)sx$/


ts do not have components inside it
tsx shall have react components inside it


Change the src/index.jsx to index.tsx

Errror to import "react-dom" in the index.tsx.

When we install ts in the app, and we use 3rd part library as "react-dom", some of these libs do not include the types definition of the typescript. Mean, they do not include all the parts taht ts needs to compreend how that lib works!

THen, when the lib do not include thes definition of types, the def types, in the most of times are created by the community or even maintened in a repository  à parte.
 On we hover the mouse over the errors it show us some tips to install these dependencies. It should works 99% of the times.


 Could not find a declaration file for module 'react-dom'. '/home/roger/Softwares/Rocketseat/Ignite/Reactjs/01-github-explorer/node_modules/react-dom/index.js' implicitly has an 'any' type.
  Try `npm i --save-dev @types/react-dom` if it exists or add a new declaration (.d.ts) file containing `declare module 'react-dom';`ts(7016)

yarn add @types/react-dom -D 
Beacuse typescript will not be executed in production,

Error was fixed!


Install others:

yarn add @types/react -D


## Components with TS

Lets configure all jsx components to work with tsx

App.jsx -> App.tsx

RepositoryItem.jsx -> RepositoryItem.tsx

Parameter 'props' implicitly has an 'any' type.ts(7006)
(parameter) props: any

RepositoryItemProps -> THis is a convention for typing the Props of a React COmponent

We have to type the info used in our app and not all received from the API

Diference between interface e types.

String or string

The format of props must be a format in the RepositoryItemProps fromat.
```ts
interface RepositoryItemProps {
  repository: {
    name: string;
    description: string;
    hrml_url: string;
  }
}

export function RepositoryItem(props: RepositoryItemProps) {
}

```
At this moment, the TS shows us that autocomplete function when we use props.SOMETHING and monitor the correct typing format of current variables avaiables.

From thar moment also, when we deixa de passar uma props from the Components in the Partent, it shows an error from what is missing.


THe error inside RepositoryLIst in name, means that TS yet could not understand what is the repository.name, that name existis inside the repository beacuse state can have the typing but it is not defined yed

Lets define the typagem for the state. State inside the react. Mainly is ans array or object, for example, and not is a string or Number.

Beacuse if we init the state whith a string or number, by inference, the TS should understand the type. 

When start with an array it could not be determined! THe type is never. 

Lets create an interface without props, because here is not for the props, but statte. THe convention is to keep the simple name as possible with just the info will should use.


```ts
interface Repository {
  name: string;
}
```
Lets inform now, that the state is an ARRAY OF REPOSITORIES!

In TS exists a GENERIC FUNCIONALITY! It means that it can be changeable.

For states, it can be a string, number, array of repositories. So we need to pass this property within an <> before the () of useState(). So we can tell which type we can stroe inside this state!


```ts
const [epositories, setRepositories] = useState<Repository>([])
}
```
From the manner above, we said that state should store a single repository.

But as we want to tell that the state should stroe a list of repositories, we need to insert the array signal:


```ts
const [epositories, setRepositories] = useState<Repository[]>([])

```
At this point, the error in repository.name was fixed, because the TS understood the inside repository, the name should e a string. But there is an error in the second props repository={repository}.

Type 'Repository' is missing the following properties from type '{ name: String; description: string; html_url: string; }': description, html_urlts(2739)
RepositoryItem.tsx(2, 3): The expected type comes from property 'repository' which is declared here on type 'IntrinsicAttributes & RepositoryItemProps'
(JSX attribute) RepositoryItemProps.repository: {
    name: String;
    description: string;
    html_url: string;
}

The problem is that in the RepositoryItemProps (RepositoryItem.tsx), the repository receives name, description and htnl_url, and in the Repository (RepositoryList.tsx), the repository just have name.

SO, when we try to pass the repository props with a repository var that it type just have the name (stete), inside the RepositoryItem it should accuse that some informations is missing.

So, we need to compete the informations in RepostoryList.tsx

```ts
interface Repository {
  name: string;
  description: string;
  html_url: string;
}
```


Comentário de Josimar em https://app.rocketseat.com.br/h/forum/react-js/9abbd352-af97-4c66-bc33-c599e133c1e2


Pode ser feito sim, na maioria dos casos os devs criam uma pasta na raiz do projeto chamada types ou @types e cria um arquivo index.ts, lá vc pode criar todas as suas interfaces que se repetem em mais de uma tela e antes do nome interface vc precisa colocar o export, dessa forma:

```ts
export interface RepositoryProps {
    name: string;
    description: string;
    html_url: string;
  }
```
e nas paginas que vc precisa era so fazer o import assim:
```ts
  import { RepositoryProps } from '../../types';
```
assim vai funcionar perfeitamente tambem.. Só uma dica, faz isso apenas com interfaces que vc ver que são identicas nas paginas, caso uma receba campos não obrigatorios, porem na outra todos são obrigatorios, é melhor e ncessario que vc crie duas interfaces distintas, atendendo cada página.

Espero ter ajudado, até mais!




# Finalizando a Aplicação

## Using teh React DevTools

ReactDevTools is an best extensions for chrome to develop with React! It also advertise that the page is using React!

https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=pt-BR

Inspect Element -> Dev Tools -> Elements
We can see the complete HTML generated at the final, but we cannot see the React COMPONENTS.Just the HTML that the components generates.

Clincking in COmponents,

We now are able to see the Tree of the Components React in a page!

Now we can see all the components that are used in the application!
App
--RepositoruList
----RepositoryItems, states strored in that component, all the infos! Copy the informations, the props of each Component, for example the props from RepositoryItem! THis is awesome flexibiliuy and debug.


We also can search for a COmponent

We can click in a Component and seed the generateed Dom HTML FOR IT!We can log and access the code of that component! As we are using the sourcemap of webpack, the informations are all well organized.

The profiler tols allow we get observe, monitor and lead with the renders of the COmponents.

Forexample, we can click and make a reload, that is a profile of our application. It annotate all the componens exibidos em tela ou o temo that they load , why they were loaded, even more! 

It tells the applciation had 2 commits -> two renderization steps. Why?
Because the first renderization is to load without repositories API data. 3ms
The second commit, we obtain the RepositoryList  and all the RepositoryItem 10ms

We shall see this beyound to understand and comprhen performance problems!

## FInalizando o módulo.

Embasado na prática.

  Set structure of a React Project from the really zero.
  Bbel, TS, Webpack, CSS LOADES, SASS LOADER! Real World!!!

  Performance issues, bundle sizinhg, configu Babel, Webpack, TS

  Webpack dev server, source maps, dev/prod in webpack, scc e sass in webpack , Components, Props, states, imutabilidade.

  IMutabil is one of the more used cncepts in the functional programming.

  Styleyn the application.

  Hook froom useEffetc. Side effects --> Execute Something when something happens,

  List Repositorrios, fetch github API, save the state, map to the HTML render each repository isolated. Property key from React. Each time we make a map, the first elelement inside the map needs to have a key props to the react reaches in a easy manner to the time to remove, add, change.

  Confi TS in proj., use.tsx , hot do the tipagem das props of components, props of the state of the component, and about React Dev Tools, to measure the performance of the application and debug it! 

  THe props components received, which state the component is receiving tn some moment, and discover why a component have rendered.





# Desafio: Conceitos do React

# Como utilizar um repositório do GitHub como template?

Para realizar cada um dos desafios técnicos, você precisará criar um repositório na sua conta do GitHub a partir do template do desafio que será disponibilizado na plataforma do Ignite junto com a descrição do que deve ser feito.

1. Com o link do template aberto no navegador, clique no botão verde "Use this template":



2.  Após clicar no botão, você será redirecionado para uma nova página onde você deve escolher o nome do repositório que você irá criar a partir do template. Dê um nome ao repositório, certifique-se que está marcado como público e clique em "Create repository from template".

É importante que o repositório esteja público para que a plataforma consiga fazer a correção do seu desafio ao ser submetido.


Após isso você será redirecionado para a página do repositório que acabou de criar.

3.  Para clonar o repositório, clique no botão "Code" e irá aparecer um menu. Copie a URL que aparece logo abaixo do botão.

4.  No seu terminal, navegue até a sua pasta de preferência e rode o comando git clone {URL_DO_REPOSITORIO}, isso irá baixar todos os arquivos para a sua máquina. Lembre-se que você precisa do Git instalado em sua máquina, caso ainda não tenha:


5.  Com seu repositório baixado na sua máquina, execute o comando `yarn` na pasta do projeto clonado para instalar todas as dependências.

6.  Agora é só você fazer todas as alterações que você precisa para completar o desafio seguindo a descrição dada e enviar o repositório atualizado para o GitHub para o repositório que você acabou de criar utilizando o template.







# O que são os testes automatizados?

Para te ajudar a saber se você está no caminho certo, no projeto que você já clonou deixamos alguns testes automatizados.

Os testes sempre vão ficar dentro de uma pasta chamada `__tests__` dentro da pasta `src`.

Dentro da pasta de testes, para cada arquivo testado na sua aplicação, existe um arquivo com o mesmo nome, com a extensão `.spec.js`.

Para começar a utilizar os testes, execute o comando `yarn test` no seu terminal, e ele irá te retornar o resultado dos testes.

Isso deve te retornar vários erros logo após clonar o projeto, como esse:



Esse erro, por exemplo, significa que o teste esperava que a função `mockNext` fosse chamada pelo menos uma vez mas ela não foi chamada nenhuma vez. 

Esse teste se refere a um dos desafio da trilha de Node.js mas não se preocupe caso você esteja na trilha de ReactJS. Os testes usarão esse mesmo padrão.

Nem sempre você precisa usar apenas os testes para saber se tudo está funcionando, você pode sempre executar a aplicação e testar utilizando o Insomnia (ou Postman) no caso de APIs ou o navegador no caso de aplicações web.

Esses testes serão os mesmos testes que irão corrigir o seu desafio e darão sua nota, então recomendamos que os siga a risca e tenha certeza que todos passem para receber nota máxima!

# Como interpretar os erros nos testes?

Agora que você já sabe como rodar os testes, você também deve entender como interpretá-los. Vamos começar analisando a seguinte imagem:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b593c301-01f0-4a1e-8efb-d16d813caae7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b593c301-01f0-4a1e-8efb-d16d813caae7/Untitled.png)

Logo acima da imagem, temos em vermelho um título que específica qual teste deu errado. Nesse caso o teste que falhou é o teste `should not be able to find a non existing user by username in header`, do módulo `checksExistsUserAccount`.

Para entender o que houve de errado, podemos olhar exatamente esse trecho:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78157391-f2bc-463f-b1fb-2be7125ef7f6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78157391-f2bc-463f-b1fb-2be7125ef7f6/Untitled.png)

Vendo isso, podemos inferir que o teste esperava que a função `mockResponse.status` fosse chamada com o valor `404` como parâmetro mas a função não foi chamada nenhuma vez.

Para resolver problemas assim, de acordo com o seu desafio basta ir até o arquivo/função onde o erro está acontecendo e cumprir com o requisito do desafio (no caso acima, bastava retornar o `response.status` passando `404` por parâmetro).

Se você está um pouco confuso sobre onde deve mexer para passar em algum teste, olhe o nome do teste e confira na descrição do desafio. Lá terá um detalhamento maior do que e como deve ser feito 🚀


















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































