# Re-Magma

This project is meant to help intermediate/advanced JS developers to get into the React library alongside the Redux pattern and library by building a small single page application that access a public API and shows the provided information to the user.

## Pre-requisites
Previous **JS** experience, not too much but at least basic understanding of the **ES6** syntax.

**Node JS**, basic understanding of exports and requires and **npm** install.

you can get it by in MacOS by running
```brew install node```

And in Ubuntu
```sudo apt-get install nodejs npm``` 

## 01 Base application

For this workshop we will use the ```create react app``` library that allows us to rapidly initialize a **React** application with pre-configured *development server*, *webpack*, *babel*, *jest testing* and *production build* generation.

### Create react app

You can install the **[library](https://github.com/facebook/create-react-app)** and initialize a new React application in 2 different ways.

The first one documented in the page by March 2, 2018 needs the **[npx](https://github.com/zkat/npx)** library which we can obtain by running

```npm install -g npx```

Then just generate the new app

```npx create-react-app <app name>```

The second way is by installing the ```create react app``` package globally with npm

```npm install -g create react app```

Then just generate the new app

```create-react-app <app name>```

And then navigate into our new React app

```cd <app name>```

To see the basic generated app in action just run

```npm start```

## 02 Dependencies

We will need the next libraries

Name | Description
----- | -----------
react | A declarative, efficient, and flexible JavaScript library for building user interfaces
react-dom | This package serves as the entry point of the DOM-related rendering paths
react-redux | Official React bindings for Redux
react-scripts | Shiped with create react app
redux | Redux is a predictable state container for JavaScript apps
redux-thunk | Thunk middleware for Redux
axios | Promise based HTTP client for the browser and node.js
immutable | Immutable collections for JavaScript

### Install

```npm install --save redux react-redux redux-thunk axios immutable```

## 03 Redux configuration

Redux is composed by three basic concepts:

### The Store
Is where our app state will be allocated, the store will keep track of the state and supply it to our application when require it.

```src/redux/store.js```
```javascript
import { createStore, applyMiddleware } from 'redux'
import { combineReducers } from 'redux'
import thunk from 'redux-thunk'
import reducer from './reducer'

export default createStore(reducer, applyMiddleware(thunk))
```

### Reducer/s
The reducer is in charge of change the state after and actions is triggered.

```src/redux/reducer.js```
```javascript
import Immutable from 'immutable'
import * as constants from './constants'

export default function reducer(state = Immutable.Map(), action) {
  switch (action.type) {
    default:
      return state
  }
}
```

### Actions
The actions will be call to trigger events, the actions supply the reducer with an action type and an action payload.

```src/redux/actions.js```
```javascript
import * as constants from './constants'

export function() {
    return {
        type: constants.ACION_NAME
    }
}
```

---

More concepts

### Constants
Optionally you can use a set of STRING constants to represent action type names.

```src/redux/contants.js```
```javascript
export const ACTION_NAME = 'ACTION_NAME'
```


### Provider

This React component wraps our app and supplies the inner components with the store object.

```javascript
import { Provider } from 'react-redux'
import store from './redux/store'

<Provider store={store}>
   ...Rest of compoenents
</Provider>
```

---

At the end you will end up with the next files hierarchy

```
- src
 |_ redux
   |_ actions.js
   |_ constants.js
   |_ reducer.js
   |_ store.js 
```

## 04 React components

React creates a virtual DOM in memory which at the end will be rendered to the browser DOM as common DOM children tags, but while they are in memory will also have a reacty plus, by having a life cycle, properties and state.

### Basic HTML components

React can easily create components that will render common HTML tags into the browser DOM, they don't need any special configuration and can have any attribute their analogue HTML can have, except for the ```class``` attribute in React this should be written as ```className```.

JSX example div:
```javascript
<div className="juts-a-div">Content</div>
```

This will render as:
```html
<div class="just-a-div">Content</div>
```

Let's take a look to our current App.js file.
```javascript
import React, { Component } from 'react';
import { Provider } from 'react-redux'
import store from './redux/store'
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <div className="App">
          <header className="App-header">
            <img src={logo} className="App-logo" alt="logo" />
            <h1 className="App-title">Welcome to React</h1>
          </header>
          <p className="App-intro">
            To get started, edit <code>src/App.js</code> and save to reload.
          </p>
        </div>
      </Provider>
    );
  }
}

export default App;
```

This will render as
```html
<div class="App">
  <header class="App-header">
    <img src="/static/media/logo.5d5d9eef.svg" class="App-logo" alt="logo">
    <h1 class="App-title">Welcome to React</h1>
  </header>
  <p class="App-intro">
    To get started, edit <code>src/App.js</code> and save to reload.
  </p>
</div>
```

As you can see the ```<Provider />``` component is not being rendered as a HTML, this is because only common HTML tags React components will be rendered into the browser DOM any other Component is just a wrapper with the logic needed to render the desired HTML.

### Our first component

Lets replace the boilerplate code inside our ```<App/>``` component with or own ```IndexPage``` component.

```src/components/IndexPage.jsx```

```javascript
import React, { Component } from 'react';

class IndexPage extends Component {
  render() {
    return (
      <div className='index-page'>
        This our brand new Page
      </div>
    );
  }
}

export default IndexPage;
```

Then just require our new component in ```App.js``` and replace all the content for our own.

```javascript
import React, { Component } from 'react';
import { Provider } from 'react-redux'
import store from './redux/store'
import IndexPage from './components/IndexPage'
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <IndexPage />
      </Provider>
    );
  }
}

export default App;
```

Now our app will render

```html
<div class="index-page">This our brand new Page</div>
```

Again neither ```<Provider />``` or ```<IndexPage />``` are being rendered as HTML tags but since ```IndexPage``` is rendering some common react components in its inside, they are being rendered to the browser DOM.

## 05 State and Properties

The react components can have two different types of data sources, the properties and the state. The properties are data sent to them from the outside and the state is data managed by itself, when you change one of them the component will detect the change and will re-render it self to reflect the changes.

### Props

Every component can access an instance attribute called props (```this.props```), this will contain all the props passed from the outside to the component.

Passing props to the component is as easy as setting attributes in a HTML tag.

```javascript
<Component prop1="value" magicProp={3} boolProp />
```

props can be strings ```"value"```, js expresions ```{3}``` or inplisit booleans ```boolProp```

Lets pass some props to our ```IndexPage```

```src/App.js```
```javascript
<IndexPage title="Rick and Morty characters"/>
```

now we can use the ```title``` prop in ```IndexPage```

```src/components/IndexPage.js```
```javascript
class IndexPage extends Component {
  render() {
    return (
      <div className='index-page'>
        {this.props.title}
      </div>
    );
  }
}
```

This will render as

```html
<div class="index-page">Rick and Morty characters</div>
```

### State

This one works a little different from the props attribute, ```this.state``` contains values you know will change and will be produced inside by our component.

To change our state we use the method ```setState()``` every time we need to trigger a new render so the final render reflects this state change.

let's decalre a new initial state in our ```IndexPage``` and render one of its members

```javascript
class IndexPage extends Component {
  state = { numberOfCharacters: 0 }

  render() {
    return (
      <div className='index-page'>
        {this.props.title}: {this.state.numberOfCharacters}
      </div>
    );
  }
}
```

this will render as
```html
<div class="index-page">Rick and Morty characters: 0</div>
```

now to trigger a state change lets add a ```<button>``` that will change ```numberOfCharacters``` every time we press it.

```javascript
class IndexPage extends Component {
  state = { numberOfCharacters: 0 }

  addCharacter = () => {
    const newValue = this.state.numberOfCharacters + 1

    this.setState({ numberOfCharacters: newValue })
  }

  render() {
    return (
      <div className='index-page'>
        {this.props.title}: {this.state.numberOfCharacters}
        <button onClick={this.addCharacter}>Add character</button>
      </div>
    );
  }
}
```

this will render as:
```html 
<div class="index-page">
  Rick and Morty characters: 0
  <button>Add character</button>
</div>
```

the button now has a function binded to the ```onClick``` event.

## 06 Interact with the Redux state

To start populating our ```Redux``` state we need to trigger and action and then use our action payload in the reducer to change the current state, once changed the ```<Provider>``` will, well, provide our app with the new state.

### Load characters action

Let's create our first action ```loadCharacters```

```src/redux/constants.js```
```javascript
export const LOAD_CHARACTERS = 'LOAD_CHARACTERS'
```

```src/redux/actions.js```
```javascript
import * as constants from './constants'

export function loadCharacters() {
  return {
    type: constants.LOAD_CHARACTERS,
    characters: [
      { id: 1, name: 'Rick' },
      { id: 2, name: 'Morty' },
      { id: 3, name: 'Llamas' }
    ]
  }
}
```

Here we create a new function (action) that returns an action descriptor of type ```LOAD_CHARACTERS``` and with a payload called characters which contains an array of characters.

When dispatching this action, the reducer will decide what to do with the action descriptor that the action provided.

```src/redux/reducer.js```
```javascript
import Immutable from 'immutable'
import * as constants from './constants'

export default function reducer(state = Immutable.Map(), action) {
  switch (action.type) {
    case constants.LOAD_CHARACTERS:
      return state.set('characters', action.characters)
    default:
      return state
  }
}

```

Here the reducer set the attribute ```characters``` as the array of characters from the action payload.

### Connect components with Redux

Now that our Redux state is populated ```{ characters: [] }```.

To give Redux capabilities to our components, such as dispatch an action or access the state, we need to ```connect``` our component.

```javascript
import React, { Component } from 'react'
import { connect } from 'react-redux'
import * as actions from '../redux/actions'

class IndexPage extends Component {
  state = { numberOfCharacters: 0 }

  componentWillMount() {
    this.props.loadCharacters()
  }

  render() {
    const characters = this.props.characters.map(character => {
      return <li key={character.id}>{character.name}</li>
    })

    return (
      <div className='index-page'>
        {this.props.title}: {this.props.characters.length}
        <ul>
          {characters}
        </ul>
      </div>
    );
  }
}

function mapStateToProps(state) {
  return {
    characters: state.get('characters') || []
  }
}

function mapDispatchToProps(dispatch) {
  return {
    loadCharacters: () => {
      dispatch(actions.loadCharacters())
    }
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(IndexPage)
```

## 07 Retrieving data from API

Now let's get some real characters form the public API https://rickandmortyapi.com to retrieve some data.

The endpoint is going to be https://rickandmortyapi.com/api/character?page=number_of_page, which is going to send data in the next format:

```json
{
  "info": {
    "count": Number,
    "pages": Number,
    "next": String,
    "prev": String
  },
  "results": [
    {
      "id": Number,
      "name": String,
      "status": String,
      "species": String,
      "type": String,
      "gender": String,
      "origin": {
        "name": String,
        "url": String
      },
      "location": {
        "name": String,
        "url": String
      },
      "image": String,
      "episode": [String],
      "url": String,
      "created": String
      }
  ]
}
```
### Async actions

Right now, our current action ```LOAD_CHARACTERS``` is a sync action, in other words the app will be blocked until the action finish to be dispatched.

We are using ```axios``` which is a promise based HTTP client for the browser and node.js :sunglasses:.

We have a problem...

```src/redux/actions.js```
```javascript
import * as constants from './constants'
import axios from 'axios'

export function loadCharacters(page = 1) {
  axios.get(`https://rickandmortyapi.com/api/character?page=${page}`)
  .then(function (response) {
    // By the time this happens the action already finished.
    console.log(response)
  })
  return {
    type: constants.LOAD_CHARACTERS,
    characters: [
      { id: 1, name: 'Rick' },
      { id: 2, name: 'Morty' },
      { id: 3, name: 'Llamas' }
    ]
  }
}
```

This won't work because the action is synchronous and by the time the asynchronous request finish, well nothing will be able to access the response. So another way is using await which will not work either because the action function is not declared as async.

The solution is to make the action an asynchronous action but not in the traditional way but in a redux-thunk way by returning a function instead of an object descriptor, this function will be called passing the ```dispatch``` redux function that we will use to dispatch a secondary action that will actually populate the state with the retrieved data.

```src/redux/actions.js```
```javascript
import * as constants from './constants'
import axios from 'axios'

export function loadCharacters(page = 1) {
  return dispatch => {
    axios.get(`https://rickandmortyapi.com/api/character?page=${page}`)
    .then(function (response) {
      const characters = response.data.results
      const pages = response.data.info.pages
      const count = response.data.info.count

      dispatch(populateCharacters(characters, count, pages))
    })
  }
}

function populateCharacters(characters, count, pages) {
  return {
    type: constants.POPULATE_CHARACTERS,
    characters,
    count,
    pages
  }
}
```

This way once we have the retrieved data we can populate the redux state.

Let's change our constant name

```src/redux/constants.js```
```javascript
export const POPULATE_CHARACTERS = 'POPULATE_CHARACTERS'
```

And our reducer to set the pages and total count

```src/redux/reducer.js```
```javascript
import * as constants from './constants'

export default function reducer(state = Immutable.Map(), action) {
  switch (action.type) {
    case constants.POPULATE_CHARACTERS:
      return state.set('characters', action.characters)
                  .set('count', action.count)
                  .set('pages', action.pages)
    default:
      return state
  }
}
```

Now or index page should be showing more that 3 characters

### Change page

Our API returns only 20 characters at a time, so we need to call our new action for the page we need, let's build some page buttons.

```src/components/IndexPage.js```
```javascript
import React, { Component } from 'react'
import { connect } from 'react-redux'
import * as actions from '../redux/actions'

class IndexPage extends Component {
  state = { page: 1, previousDisabled: true, nextDisabled: true }

  componentWillMount() {
    this.props.loadCharacters()
  }

  componentWillReceiveProps(nextProps) {
    const nextDisabled = nextProps.pages === this.state.page

    this.setState({ nextDisabled })
  }

  goToPreviousPage = () => {
    const page = this.state.page - 1

    this.goToPage(page)
  }

  goToNextPage = () => {
    const page = this.state.page + 1

    this.goToPage(page)
  }

  goToPage = (page) => {
    const previousDisabled = page === 1
    const nextDisabled = this.props.pages === page

    this.props.loadCharacters(page)
    this.setState({ page, previousDisabled, nextDisabled })
  }

  render() {
    const characters = this.props.characters.map(character => {
      return <li key={character.id}>{character.name}</li>
    })

    return (
      <div className='index-page'>
        {this.props.title}: {this.props.count}
        <ul>
          {characters}
        </ul>
        <div>
          <button
            disabled={this.state.previousDisabled}
            onClick={this.goToPreviousPage}
          >
            Previous
          </button>
          {this.state.page}
          <button
            disabled={this.state.nextDisabled}
            onClick={this.goToNextPage}
          >
            Next
          </button>
        </div>
      </div>
    );
  }
}

function mapStateToProps(state) {
  return {
    characters: state.get('characters') || [],
    count: state.get('count') || 0,
    pages: state.get('pages') || 0
  }
}

function mapDispatchToProps(dispatch) {
  return {
    loadCharacters: (page) => {
      dispatch(actions.loadCharacters(page))
    }
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(IndexPage)
```

Now we can list all characters navigating through the pages.

## 08 Styling

Now that we have actual characters data and we can retrieved as we wish, we can focus on building our interface with React (Finally), we are going to create a couple of components additionally to the ```IndexPage``` one and apply some css to our page so it can look pretty.

Let's begin...

Lets Start building the page header as component

```src/components/Header.js```
```javascript
import React, { Component } from 'react'

import logo from './assets/logo.png'
import './Header.css'

export default class Header extends Component {
  render() {
    return (
      <div className="main-header">
        <div className="api-reference">
          Using
          <a href="https://rickandmortyapi.com/" target="blank"> The Rick and Morty API </a>
          by
          <a href="https://axelfuhrmann.com/" target="blank"> Axel Fuhrmann.</a>
        </div>
        <img src={logo} alt="Rick and Morty" />
        <div className="header-bar">
          <h2>
            Character list
          </h2>
          <h4>
            Total: {this.props.count}
          </h4>
        </div>
      </div>
    )
  }
}
```

```src/components/Header.css```
```css
.main-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #e7e7e7;
}

.main-header > .api-reference {
  align-self: flex-end;
  padding: 5px 10px 0 0;
}

.api-reference > a {
  text-decoration: none;
  color: #00b0c8;
}

.main-header > img {
  max-height: 100px;
  margin-top: 5px;
  margin-bottom: 10px;
}

.main-header > .header-bar {
  width: 100%;
  text-align: center;
  background-color: #e2e2e2;
  color: #00b0c8;
}

.header-bar > h2,h4 {
  margin: 2px 0 2px 0;
  padding: 0;
}
```

we can now use it as

```javascript
<Header />
```

Now lets create a component for our character listing

```src/components/CharacterList.js```
```javascript
import React, { Component } from 'react'
import CharacterItem from './CharacterItem'
import './CharacterList.css'

export default class CharacterList extends Component {
  render() {
    const characters = this.props.characters.map(character => {
      return <CharacterItem
        key={character.id}
        character={character}
        onClick={this.props.onSelect}
        selected={character.id === this.props.selected}
      />
    })

    return (
      <div className="character-list">
        {characters}
      </div>
    )
  }
}
```

```src/components/CharacterList.css```
```css
.character-list {
  display: flex;
  flex-wrap: wrap;
  border: 1px solid #00b0c8;
  border-radius: 8px;
  overflow: hidden;
}
```

Which depends on a CharacterItem component

```src/components/CharacterItem.js```
```javascript
import React, { Component } from 'react'
import './CharacterItem.css'

export default class CharacterItem extends Component {
  onClick = () => {
    if(this.props.onClick) {
      this.props.onClick(this.props.character)
    }
  }

  render() {
    const character = this.props.character
    const className = this.props.selected ? 'character-item selected' : 'character-item'

    return (
      <div key={character.id} className={className} onClick={this.onClick} >
        <img src={character.image} alt={character.name} />
        <div className="character-description">
          <h4>{character.name}</h4>
          <p>{character.species}</p>
        </div>
      </div>
    )
  }
}
```

```src/components/CharacterItem.css```
```css
.character-list > .character-item {
  width: calc(50% - 10px);
  display: flex;
  background-color: #f3f3f3;
  padding: 5px;
  cursor: pointer;
}

.character-list > .character-item:hover, .character-list > .character-item.selected {
  background-color: #00b0c8;
}

.character-item > img {
  height: 50px;
  border-radius: 8px;
}

.character-item > .character-description {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 5px;
  color: #292929;
}
.character-item:hover > .character-description, .character-item.selected > .character-description {
  color: white;
}

.character-description > h4, p {
  margin: 2px;
  padding: 0;
}
```

we can now use it as

```javascript
<CharacterList />
```

Let's also create an independent component for the pagination

```src/components/IndexPage.js```
```javascript
import React, { Component } from 'react'
import { connect } from 'react-redux'
import * as actions from '../redux/actions'
import Header from './Header'
import CharacterList from './CharacterList'
import CharacterPreview from './CharacterPreview'
import Pager from './Pager'

import './IndexPage.css'

class IndexPage extends Component {
  state = { selected: null }

  componentWillMount() {
    this.props.loadCharacters()
  }

  goToPage = (page) => {
    this.props.loadCharacters(page)
  }

  onSelect = (character) => {
    this.setState({
      selected: character.id
    })
  }

  render() {
    const selectedCharacter = this.props.characters.find(character => {
      return character.id === this.state.selected
    })

    return (
      <div className="index-page">
        <Header count={this.props.count} />
        <Pager
          total={this.props.pages}
          onChange={this.goToPage}
        />
        <div className="index-characters">
          <CharacterList
            characters={this.props.characters}
            selected={this.state.selected}
            onSelect={this.onSelect}
          />
          <CharacterPreview character={selectedCharacter} />
        </div>
      </div>
    )
  }
}

function mapStateToProps(state) {
  return {
    characters: state.get('characters') || [],
    count: state.get('count') || 0,
    pages: state.get('pages') || 0
  }
}

function mapDispatchToProps(dispatch) {
  return {
    loadCharacters: (page) => {
      dispatch(actions.loadCharacters(page))
    }
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(IndexPage)
```

```src/components/Pager.css```
```css
.pager {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 10px;
}

.pager > button {
  padding: 5px;
  margin: 5px;
  background-color: #00b0c8;
  border: none;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  font-size: 15px;
}

.pager > button:hover {
  background-color: #0093a7;
}
```

Let's use this new components in our ```IndexPage```

```src/components/Pager.js```
```javascript
import React, { Component } from 'react'
import './Pager.css'

export default class Pager extends Component {
  state = {
    page: 1,
    previousDisabled: true,
    nextDisabled: true
  }

  componentWillReceiveProps(nextProps) {
    const nextDisabled = nextProps.total === this.state.page

    this.setState({ nextDisabled })
  }

  goToPreviousPage = () => {
    const page = this.state.page - 1

    this.goToPage(page)
  }

  goToNextPage = () => {
    const page = this.state.page + 1

    this.goToPage(page)
  }

  goToPage = (page) => {
    const previousDisabled = page === 1
    const nextDisabled = this.props.pages === page

    this.setState({ page, previousDisabled, nextDisabled })

    if(this.props.onChange) {
      this.props.onChange(page)
    }
  }

  render() {
    return (
      <div className="pager">
        <button
          disabled={this.state.previousDisabled}
          onClick={this.goToPreviousPage}
        >
          Previous
        </button>
        {this.state.page}/{this.props.total}
        <button
          disabled={this.state.nextDisabled}
          onClick={this.goToNextPage}
        >
          Next
        </button>
      </div>
    )
  }
}
```

```src/components/IndexPage.css```
```css
.index-characters {
  display: flex;
  padding: 10px;
}
```

That's it we have finished our example App.

## 09 Wrapping all up

So our app is finished, it make use of ```Redux``` to manage our app state and also act as a data repository, we used ```React``` to present that data to the user in an easy to use and visual way, we also learn how to retrieve information from a remote source and how to build components to encapsulate logic where it belongs.


## 10 Conclusion
We know now how to rapidly build a React App from scratch using the ```create-react-app``` library and we have a basic understanding how to use ```React``` and ```Redux```, that's good news, but we also need to learn a lot more about how to create quality code for our application, so from now on some concepts I recommend to research for are re React ```PropTypes```, the ```jest``` test library or any other one toy feel comfortable with and the ```enzyme``` library to test our components directly.
