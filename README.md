[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# React Components Props

The basic unit you'll be working with in ReactJS is a component. Components are
pieces of our application that we can define once and reuse all over the place.

## Objectives

After this lesson, you will be able to:

- Describe props and why we need them.
- Create a component that renders props.
- Create nested components that pass props.

## Review

Let's start up our server, and review the code in `src/App.js` and
`src/Movie.js`. What have we done so far?

- Our top level component `App` uses the `Movie` component
- The `Movie` component renders movie data

> Why do our `App` and `Movie` components look different syntactically? Because
> there are two different ways to declare components in React. For components
> that have no methods attached to them, we can use arrow functions that return
> JSX. If we need more complex components, we need to use the `class` syntax.
> Both our components could actually be arrow functions right now, but we did
> our `Movie` component the long way to demonstrate that syntax.

## Component Data with Props

What are props? Props are simply arguments passed into a component, as though
they were arguments to a function. The component can then use this data to
render something or pass the data on to another component.

The React framework was built to handle data that changes over time. Props allow
data to flow downward into components from central source (generally, an API),
without needing any code inside the components that receive props to handle
changes in this data.

Components that just render data based on their props are known as "stateless"
or "functional" components. They're very easy to reason about, because they take
data and produce markup, without any side effects or internal state. In React,
we should aim to make as many of our components stateless as possible.

### Code-along: Display multiple movies
Our movie component isn't very dynamic. It can still only render a pre-determined movie. Let's change that. First, let's hard code an array of movie titles in App.js. Then, we'll iterate through our array, and render one Movie component for each title. To pass the title into the Movie component, we'll need to use "props".

First let's add an array of movies to our `App` component.
```js
const App = () => {

  const movies = [
    {
      id: 1,
      title: 'Dr. Strangelove'
    },
    {
      id: 2,
      title: 'Eraserhead'
    },
    {
      id: 3,
      title: 'Fantastic Mr. Fox'
    }
  ]

  return (
    <div>
      <h1>Welcome to React!</h1>
      <Movie />
    </div>
  )
}
```

Then we can iterate through that array and render the `Movie` component for each item in the movie array.

```js
  return (
    <div>
      <h1>Welcome to React!</h1>
      
      {movies.map(movie => <Movie />)}
      
    </div>
  )
}
```

Next we need to pass the movie title to our `Movie` component.  We also need to pass a unique `key` so that React can keep track of the components we created inside our loop.

```js
  return (
    <div>
      <h1>Welcome to React!</h1>
      
      {movies.map(movie => <Movie key={movie.id} title={movie.title}/>)}
      
    </div>
  )
}
```

Our `Movie` component is still only rendering the original movie.  We need to adjust the `Movie` component so that it uses the `props` we passed in.

```js
import React, {Component} from 'react';

class Movie extends Component {

  render () {

    return (
      <h1>{this.props.movie.title}</h1>
    )
  }
}

export default Movie
```

You should now see the title of each movie rendering on the page.

### Code-along: Passing multiple props to a component

Right now, `Movie` just expects one prop, and that's all we're giving it. Of
course, we often want components to display more complex information. To do so,
we can pass multiple props to our component! Let's expand on our movies array
to add directors and a cast list.

```js
const movies = [
  {
    id: 1,
    title: 'Dr. Strangelove',
    director: 'Stanley Kubrick',
    cast: [
      'Peter Sellers',
      'George C. Scott',
      'Slim Pickens'
    ]
  },
  {
    id: 2,
    title: 'Eraserhead',
    director: 'David Lynch',
    cast: [
      'Jack Nance',
      'Charlotte Stewart',
      'Jeanne Bates'
    ]
  },
  {
    id: 3,
    title: 'Fantastic Mr. Fox',
    director: 'Wes Anderson',
    cast: [
      'George Clooney',
      'Meryl Streep',
      'Bill Murray'
    ]
  }
]
```

Now that we have additional information, let's pass that to the component as
well. Remember that we'll also have to change the component definition so that
it is using the new props we passed to it. Also, we have to return a single
element from the render method, but you can always wrap multiple elements in a
`div` tag.

> Note: We typically want to pass our props individually, instead of as an
> object. This helps keep our components consistent and specific. It also
> prevents us from referring to our props in a manner like
> `this.props.movie.title` where `this` already refers to an instance of a
> `Movie` component.

Now we can use this new information as props, just like normal. We'll pass the
director and entire array of actors through to the movie component. First,
update your App component:

```jsx
const App = () => (
  <div>
    <h1>Welcome to React!</h1>
    {movies.map(movie => (
      <Movie key={movie.id} title={movie.title} cast={movie.cast} director={movie.director}/>
    ))}
  </div>
)
```

If you check your application afterwards, nothing has changed. Remember, a
component will just ignore any props it receives that it doesn't use. But, we
want to use the props! So, second, update your `Movie` class' `render` method
to return some more JSX.

```jsx
<div>
  <h1>{this.props.title}</h1>
  <p>Directed by {this.props.director}</p>
  <p>Starring: {this.props.cast}</p>
</div>
```

If you check the page now, you'll see React prints the entire array, as that's
what was passed in. It's a start, but we can improve upon this. Try to iterate
through that `cast` array and display each actor individually.

## Nested Components with Props

Since we're going to be rendering many actors and they will all share common
properties, it would be a great time to make another component!

### Using props in a functional component

An Actor component will be pretty simple, it will receive a couple of props to
render, but won't need any interactivity or logic. That means it's a great use
case for a functional component, with the arrow function syntax.

However, we haven't seen an example yet of passing props into a functional
component. To do it, we need to make sure we understand "destructuring", a
feature added to JS in ES6. Here's an example.

```js
const me = {
  name: 'Caleb',
  favoriteFood: 'Tacos'
}

// isolating object properties the old way
const name = me.name
const favoriteFood = me.favoriteFood

// isolating object properties with destructuring
const { name } = me
const { favoriteFood } = me
```

These two methods of pulling properties out of an object produce the
same result. The parameters to React functional components usually use
destructuring.

Instead of this:

```jsx
const Developer = (props) => (
  <p>My name is {props.name}</p>
)
```

We typically do this:

```jsx
const Developer = ({ name }) => (
  <p>My name is {name}</p>
)
```

Getting comfortable with this pattern will make it easier to read React
documentation and tutorials.

### Lab: Create an `Actor` component

Create an `Actor` component that will receive two props: `name`,
a string representing the actor's full name, and `role`, a string describing
the character that actor plays in the film. Your component should display this
information like this:

```text
Peter Sellers as President Merkin Muffley
George C. Scott as General Buck Turgidson
```

Here's some updated data, including roles for each actor.

```js
const movies = [
  {
    id: 1,
    title: 'Dr. Strangelove',
    director: 'Stanley Kubrick',
    cast: [
      {
        name: 'Peter Sellers',
        role: 'President Merkin Muffley'
      },
      {
        name: 'George C. Scott',
        role: 'General Buck Turgidson'
      },
      {
        name: 'Slim Pickens',
        role: 'Major T.J. "King" Kong'
      }
    ]
  },
  {
    id: 2,
    title: 'Eraserhead',
    director: 'David Lynch',
    cast: [
      {
        name: 'Jack Nance',
        role: 'Henry Spencer'
      },
      {
        name: 'Charlotte Stewart',
        role: 'Mary X'
      },
      {
        name: 'Jeanne Bates',
        role: 'Mrs. X'
      }
    ]
  },
  {
    id: 3,
    title: 'Fantastic Mr. Fox',
    director: 'Wes Anderson',
    cast: [
      {
        name: 'George Clooney',
        role: 'Mr. Fox'
      },
      {
        name: 'Meryl Streep',
        role: 'Mrs. Fox'
      },
      {
        name: 'Bill Murray',
        role: 'Badger'
      }
    ]
  }
]
```

Create an `Actor` component, and use it render each actor inside the
`Movie` component.

## Additional Resources

- [Intro to Components](https://generalassembly.wistia.com/medias/h64z7lp1ir)
- [Returning multiple elements from a single Component using React.Fragment](https://reactjs.org/docs/fragments.html)
- [React Components and Props](https://reactjs.org/docs/components-and-props.html)
- [Thinking in React - Steps 1 and 2](https://reactjs.org/docs/thinking-in-react.html)
- [Component Props | Codecademy](https://www.codecademy.com/courses/react-101/lessons/this-props)

## [License](LICENSE)

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
1. All software code is licensed under GNU GPLv3. For commercial use or
   alternative licensing, please contact legal@ga.co.
