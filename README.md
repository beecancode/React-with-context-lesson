# React with Context
​
![gif, context](https://media.giphy.com/media/5SBPr8uRojC3D2pW96/giphy.gif)

## What is Context?

>**Context** provides a way to **pass data** through the component tree **without having to pass props down manually at every level**.
>
>In a typical React application, data is passed top-down (parent to child) via props, but such usage can be cumbersome for certain types of props that are required by many components within an application. Context provides a way to share values like these between components without having to explicitly pass a prop through every level of the tree.

## When Might Context Be Useful?

>**Context** is designed to **share data** that can be considered **“global”** for a **tree of React components**, such as the current authenticated user, theme, or preferred language.



## Basic Context Walkthrough - Rendering Data Using Hooks & Context

### Setup

1. cd into the correct directory in our repo and `npx create-react-app context`
1. cd into the newly created context directory
1. open everything in your text editor
1. run `npm start` in your terminal to get the app running
1. Delete the starter code in App.js and replace it with:
​
```js
import { useState } from 'react'
​
const App = () => {
  return (
    <>
      <h1>Books</h1>
    </>
  )
}
​
export default App
```
​
### Create a basic books app
​
1. In a new terminal window `mkdir src/components`
1. Then `touch src/components/Book.js`
1. In Book.js:
​
```js
const Book = () => {
  return (
    <div>
      <h1>Book Component</h1>
    </div>
  )
}
​
export default Book
```
​
1. In App.js lets add a state with a few books and import our book component and have it display on the page:
​
```js
import { useState } from 'react'
import Book from './components/Book'
const App = () => {
  const [books, setBooks] = useState([
    {
      title: 'JSON Derulo, an Autobiography',
      price: 25,
      id: 74125,
    },
    {
      title: 'Bootstrap Essentials by Sometimes',
      price: 50,
      id: 78533,
    },
    {
      title: 'Tomato Wine Pairings',
      price: 40,
      id: 52369,
    },
  ])
  return (
    <>
      <h1>Books</h1>
      <Book />
    </>
  )
}
​
export default App
```
​
1. Lets map over our books and have our page display the title of the books, in App.js:
​
```js
{
  books.map((book) => {
    return <Book key={book.id} book={book} />
  })
}
```
​
1. In Book.js:
​
```js
const Book = (props) => {
  return (
    <div>
      <h1>{props.book.title}</h1>
    </div>
  )
}
​
export default Book
```
​
### Adding Context to our app
​
1. `mkdir src/contexts`
1. `touch src/contexts/BookContext.js`
1. In our new BookContext.js lets do a bit of setup before we get into the meat of context:


```js
import React, { useState, createContext } from 'react'
​
export const BookProvider = () => {}
```
​
1. Lets also move our state from App.js into here:
​
```js
import React, { useState, createContext } from 'react'
​
export const BookProvider = () => {
  const [books, setBooks] = useState([
    {
      title: 'JSON Derulo, an Autobiography',
      price: 25,
      id: 74125,
    },
    {
      title: 'Bootstrap Essentials by Sometimes',
      price: 50,
      id: 78533,
    },
    {
      title: 'Tomato Wine Pairings',
      price: 40,
      id: 52369,
    },
  ])
}
```
​
1. Next lets add our createContext(a function that we will be able to pull into other components to use the provider) and our provider(this will quite literally provide information to our other components):

> **createContext** creates a Context object. When React renders a component that subscribes to this Context object, it will read the current context value from the closest matching Provider above it in the tree.

> Every Context object comes with a **Provider** React component that allows consuming components to subscribe to context changes. The **Provider** component accepts a value prop to be passed to consuming components that are *descendants* of this Provider. One Provider can be connected to many consumers. Providers can be nested to override values deeper within the tree.

```js
export const BookContext = createContext()
```
​
AND
​
```js
return <BookContext.Provider></BookContext.Provider>
```
​
1. To be able to add our provider anywhere we may need it we need to provide some information to the Provider, we will add in props.children so that anything we wrap in the prodiver can get the information from the provider:
​
```js
return <BookContext.Provider>{props.children}</BookContext.Provider>
```
​
1. Also pass props into our funcitonal component for our provider.
​
1. Next lets do some refactoring, first lets delete the map in App.js and move it into Book.js with some changes:
​
```js
return (
  <div>
    {books.map((book) => {
      return (
        <div>
          <h4>Title: {book.title}</h4>
          <p>Price: ${book.price}</p>
        </div>
      )
    })}
  </div>
)
```
​
1. Render Book.js in App.js again (it will still be broken we will fix this soon)
​
1. Back in our BookContext.js lets pass in our books and function to the provider (value here is a keyword that the provider is looking for to be able to pass information to components):
​
```js
<BookContext.Provider value={[books, setBooks]}>
  {props.children}
</BookContext.Provider>
```
​
1. Lets wrap our Book component in App.js in our provider (first we need to import it):
​
```js
import Book from './components/Book'
import { BookProvider } from './contexts/BookContext'
const App = () => {
  return (
    <BookProvider>
      <>
        <h1>Books</h1>
        <Book />
      </>
    </BookProvider>
  )
}
​
export default App
```
​
1. The last thing we need to get this to work is to add our useContext to our Book.js:
​
```js
import { BookContext } from '../contexts/BookContext'
```
​
AND
​
```js
const [books, setBooks] = useContext(BookContext)
```
​
1. Thats it for being able to display data using react with hooks and context!

---



![please sir, can I have some more](https://media.giphy.com/media/D3OdaKTGlpTBC/giphy.gif)
# Dive in Deeper
[React Docs for Context](https://reactjs.org/docs/context.html#updating-context-from-a-nested-component)

[React API docs useContext](https://beta.reactjs.org/apis/react/useContext)

[W3 Schools useContext](https://www.w3schools.com/react/react_usecontext.asp)

[freecodecamp React Context for Beginners](https://www.freecodecamp.org/news/react-context-for-beginners/)

[How to use React Context Effectively](https://kentcdodds.com/blog/how-to-use-react-context-effectively)

[Awesome React Context - A Curated List of Resources Related to the React Context API](https://github.com/diegohaz/awesome-react-context)
