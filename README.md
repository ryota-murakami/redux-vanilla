<h1 align="center">Redux Vanilla</h1>

<p align="center">
  <img src="https://github.com/ryota-murakami/redux-vanilla/blob/master/logo.png" alt="Redux Vanilla Logo" width="200">
</p>

<p align="center">
<a href="https://badge.fury.io/js/redux-vanilla"><img src="https://badge.fury.io/js/redux-vanilla.svg" alt="npm version" height="18"></a>
<a href="https://circleci.com/gh/ryota-murakami/redux-vanilla" rel="nofollow"><img src="https://camo.githubusercontent.com/959670902fe42ffc7a987e5988407d2db8e58ec3/68747470733a2f2f636972636c6563692e636f6d2f67682f72796f74612d6d7572616b616d692f72656475782d76616e696c6c612e7376673f7374796c653d737667" alt="CircleCI" data-canonical-src="https://circleci.com/gh/ryota-murakami/redux-vanilla.svg?style=svg" style="max-width:100%;"></a>
<a href="https://github.com/facebook/jest"><img src="https://camo.githubusercontent.com/665df681e8f100f94bde7716561b73d1552888ca/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f7465737465645f776974682d6a6573742d3939343234662e737667" alt="tested with jest" data-canonical-src="https://img.shields.io/badge/tested_with-jest-99424f.svg" style="max-width:100%;"></a>
<a href="https://github.com/prettier/prettier"><img src="https://camo.githubusercontent.com/c83b8df34339bd302b7fd3fbb631f99ba25f87f8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f636f64655f7374796c652d70726574746965722d6666363962342e737667" alt="code style: prettier" data-canonical-src="https://img.shields.io/badge/code_style-prettier-ff69b4.svg" style="max-width:100%;"></a>
<a href="https://codecov.io/gh/ryota-murakami/redux-vanilla" rel="nofollow"><img src="https://camo.githubusercontent.com/bb5967dbca9dfe8c5148a08dadff95dd3697d5a1/68747470733a2f2f636f6465636f762e696f2f67682f72796f74612d6d7572616b616d692f72656475782d76616e696c6c612f6272616e63682f6d61737465722f67726170682f62616467652e737667" alt="codecov" data-canonical-src="https://codecov.io/gh/ryota-murakami/redux-vanilla/branch/master/graph/badge.svg" style="max-width:100%;"></a>
 <a href="/ryota-murakami/redux-vanilla/blob/master/README.md#contributors"><img src="https://camo.githubusercontent.com/5accfd193468f1ae11f72c104809e8e3125646e2/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f616c6c5f636f6e7472696275746f72732d312d6f72616e67652e7376673f7374796c653d666c61742d737175617265" alt="All Contributors" data-canonical-src="https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square" style="max-width:100%;"></a>
</p>

> :ice_cream:Zero Abstraction React Redux binding library

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [⚠️  Disclaimer](#--disclaimer)
- [Why](#why)
- [Motivation](#motivation)
- [Install](#install)
- [Usage](#usage)
- [API](#api)
  - [`<Provider store={Store} />`](#provider-storestore-)
  - [`connect(component: ComponentType<any>)`](#connectcomponent-componenttypeany)
- [Inspiration](#inspiration)
- [Contributors](#contributors)
- [LICENSE](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## ⚠️  Disclaimer
Don't use in production, this is a experimental package.

## Why
- for my experiment of development npm
- I wanted to know how to work [react-redux](https://github.com/reduxjs/react-redux) through reinventing the wheel
- I wanted to get raw Redux store in React Component

## Motivation

> if you use [react-redux](https://github.com/reactjs/react-redux) already, you can do almost exacty the same with: `connect((store) => store)(Component);` 
> thanks [reactiflux](https://www.reactiflux.com/) community!

`Redux Vanilla` provide primitive Redux store object and state/dispatch short hand as a props.  
you can feel free access raw store as well as [counter-vanilla](https://github.com/reactjs/redux/blob/master/examples/counter-vanilla/index.html) in redux official repository.

```js
class App extends Component {
  render() {
    const { store, state, dispatch } = this.props
    return (
      <div>

        <p>{store.getState().count}</p>
        <button onClick={() => store.dispatch({ type: 'INCLEMENT' })} />

        <p>{state.count}</p>
        <button onClick={() => dispatch({ type: 'INCLEMENT' })} />
        
      </div>
    )
  }
}

export defaunt connect(App)
```

this means you can get a **raw Redux from props.**:tada:

## Install
- reuqirement: `react` `redux`
```
yarn add react redux redux-vanilla
```

## Usage

index.js
```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { createStore } from 'redux'
import { Provider } from 'redux-vanilla'

const initialState = {
  upVote: 0,
  downVote: 0
}

const reducer = (state, action) => {
  switch (action.type) {
    case 'UP_VOTE':
      return { ...state, upVote: state.upVote + 1 }
    case 'DOWN_VOTE':
      return { ...state, downVote: state.downVote + 1 }
    default:
      return state
  }
}

const store = createStore(
  reducer,
  initialState
)

// 1. SetUp Provider with Redux store
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

App/index.js
```js
import React, { Component } from 'react'
import { connect } from 'redux-vanilla'
import { Container } from './layout'
import { Header, Footer, Row, Button } from './component'

class App extends Component {
  render() {
    // 3. receive raw Redux store & state/dispatch shorthand. enjoy!
    const { store, state, dispatch } = this.props

    return (
      <Container>
        <Header />
        <Row>
          <Text red>{state.upVote}</Text>
          <Text green>{store.getState().downVote}</Text>
        </Row>
        <Row>
          <Button onClick={() => dispatch({ type: 'UP_VOTE' })}>
            + UpVote
          </Button>
          <Button onClick={() => store.dispatch({ type: 'DOWN_VOTE' })}>
            + DownVote
          </Button>
        </Row>
        <Footer />
      </Container>
    )
  }
}

// 2. connect to React Component
export default connect(App)
```

the repository contain [example](https://github.com/ryota-murakami/redux-vanilla/tree/master/example) and published [Live Demo](https://redux-vanilla.netlify.com/):computer:  
you could see [redux-devtools chrome extention](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=ja) on the page.

## API

### `<Provider store={Store} />`

use as Parent Component against connect Component.
`store` only accept redux `createStore()` return Object.  
also can't work without store props.

```js
const store = createStore(reducer)

ReactDOM.render(
  <Provider store={store}>
      <App />
  </Provider>,
  document.getElementById('root')
)
```

### `connect(component: ComponentType<any>)`

connected Component will recive following three props.

- `store`: Redux store object created by `createStore()`.
- `state`: `store.getState()` short hand, it doesn't any customize by `Redux Vanilla`
- `dispatch`: `store.dispatch` short hand, it doesn't any customize by `Redux Vanilla`

```js
class App extends Component {
  render() {
    const { store, state, dispatch } = this.props
    return (
      <div>
        // raw
        <p>{store.getState().count}</p>
        <button onClick={() => store.dispatch({ type: 'INCLEMENT' })} />
        // short hand
        <p>{state.count}</p>
        <button onClick={() => dispatch({ type: 'INCLEMENT' })} />
      </div>
    )
  }
}

export defaunt connect(App)
```

## Inspiration
- [react-redux](https://github.com/reactjs/react-redux)
- [counter-vanilla](https://github.com/reactjs/redux/blob/master/examples/counter-vanilla/index.html)

## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
| [<img src="https://avatars1.githubusercontent.com/u/5501268?s=400&u=7bf6b1580b95930980af2588ef0057f3e9ec1ff8&v=4" width="100px;"/><br /><sub><b>ryota-murakami</b></sub>](http://ryota-murakami.github.io/)<br />[💻](https://github.com/ryota-murakami/redux-vanilla/ryota-murakami/redux-vanilla/commits?author=ryota-murakami "Code") [📖](https://github.com/ryota-murakami/redux-vanilla/ryota-murakami/redux-vanilla/commits?author=ryota-murakami "Documentation") [⚠️](https://github.com/ryota-murakami/redux-vanilla/ryota-murakami/redux-vanilla/commits?author=ryota-murakami "Tests") |
| :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind welcome!

## LICENSE
MIT
