<p align="center">
  <img src="http://res.cloudinary.com/ptavarez/image/upload/c_scale,w_336/v1522767779/react-logo.png" alt="React"/>
</p>

# Dro's Introduction To React

React is a JavaScript library created by Facebook for easily creating user interfaces. Facebook's main reason for creating React was to solve the problem of building large applications where data changes over time.

This repo will cover how to include the React library in your code and how to create a few simple components that reads data entered in by the user.

## Prerequisites

-   Familiarity with the DOM
-   Experience with HTML
-   Experience with javascript

## Objectives

By the end of this, developers should be able to:
-   Include the React library in their code
-   Explain what the Virtual DOM is and why it's important
-   Explain what JSX is and why it's important
-   Explain what components are
-   Build a counter with React

## Preparation

1.  Fork and clone this repository.
1.  Create a new branch, `<your-name>IsAwesome`, for your work.
1.  Checkout to the `<your-name>IsAwesome` branch.

## Installing React

React only needs 3 scripts to be included for our web page. The first is the actual React library, the second is for React's virtual DOM, and the third is the Babel transpiler which will convert the JSX code into JavaScript so the browser can understand our code.

By including the Babel transpiler script and placing our code within the text/babel script tags, our JSX code will automatically be converted in the browser when we open the file.

Open up the index.html file in your favorite text-editor to see an example of the scripts needed.

*(Psss... The example shown is a great way to try React, but it's not suitable for production. It slowly compiles JSX with Babel in the browser and uses a large development build of React.)*

### What's JSX?

Within code that uses React, you may see tags that look similar to HTML tags, but they are used directly within JavaScript code. **JSX** is a JavaScript syntax extension that makes it a bit easier to create HTML tags and stacked components in large UI's.

### Virtual DOM

You should already be familiar with the DOM. The **Virtual DOM** gives us a javascript
representation of the actual DOM. When changes are made to the view we want to
show, we update the Virtual DOM first, which then checks the difference between what
was changed to what is currently rendered, and changes ONLY the pieces that need
to be changed rather than re-rendering the entire page. You can think about it
like a staging area for changes that will eventually be implemented.

### Components

The basic unit we'll be working with in React is a **component**. Components are pieces of an application that we can define once and reuse all over the place. They're a way to modularize or compartmentalize features of our applications.

With components, there is more integration and less separation of HTML, CSS, and JavaScript.
- Instead of creating a few large files, you would organize your web app into small, reusable components that encompass their own content, presentation, and behavior.

## Let's Code: React Counter

Now that we got a little introduction to React, let's get our hands dirty by creating a counter! You will be introduced to several React properties such as state and render, and how to insert React components into the DOM.

If you haven't already, open up your index.html file in a text editor. Starter code is provided to get the ball rolling :grin:

### What are we doing?

We'll create a button that will update a variable and the DOM to display a new number. With vanilla JavaScript (no libraries or frameworks) we'll have to create some sort of function that listens for changes to a variable and then updates the DOM with the new variables. With React, modifying data and updating the DOM becomes very easy, as we'll see it requires no extra work.

### Add HTML elements via render function

Lets add a **render** funtion within the Counter component. This will allow us to render HTML elements on our page. We want two buttons with some words to describe what's going on.

Add the following code:
```javascript
render: function() {
  return (
    <div>
      <p> Number of clicks: </p>
      <p> The threshold is set to: </p>
      <button> Click me! </button>
      <button> Reset </button>
    </div>
  )
}
```

In your terminal, enter the following command:
```sh
open index.html
```
Look at that! We rendered buttons within a div element without adding a single line of code to it. If your browser didn't open, find the **index.html** file, right click, and hit `Open With` to then select your favorite browser.

### Update and reset the counter

Lets add two functions, one to update the counter and another to reset it.

Above the **render** function, add the following code:
```javascript
updateCounter: function() { },

resetCounter: function() { },
```

We now need to update our buttons to listen for a click by a user to invoke our new functions.

Modify the two buttons within the **render** function like so:
```html
<div>
  <p> Number of clicks: </p>
  <p> The threshold is set to: </p>
  <button onClick={this.updateCounter}> Click me! </button>
  <button onClick={this.resetCounter}> Reset </button>
</div>
```

Our **Counter** component should now look like this:

```javascript
let Counter = React.createClass({

  updateCounter: function() { },

  resetCounter: function() { },

  render: function() {
    return (
      <div>
        <p> Number of clicks: </p>
        <p> The threshold is set to: </p>
        <button onClick={this.updateCounter}> Click me! </button>
        <button onClick={this.resetCounter}> Reset </button>
      </div>
    )
  }

})
```

Checkout out your page on your browser and click them buttons. Ok... Nothing's happening. Well that's because we haven't added any code to our **updateCounter** or **resetCounter** functions yet! We'll add some code to those functions later.

(*Psss... What happens if you throw in some console.logs in these functions* :eyes:)

## React State

One of the great things about React is that we don't have to worry about updating our UI when data changes. We can simply change the data, and React's virtual DOM will figure out what to change and how to do it efficiently. All we have to do is modify the component's state.

Lets add a method called **getInitialState**, which React calls when the component is first created to setup some variables.

Add the following code above of the **updateCounter** and **resetCounter** functions:
```javascript
getInitialState: function() {
  return {
    count: 0,
    threshold: this.props.threshold
  }
},
```
Now we need to update our **render** function. The states being changed are within the p elements, so lets add the appropriate code.

The p elements should look like this:
```javascript
<p> Number of clicks: {this.state.count} </p>
<p> The threshold is set to: {this.state.threshold}</p>
```

One more thing... We need to update the **ReactDom.render** component, which is located in the bottom of the script tag.

Lets make a quick update:
```javascript
ReactDOM.render(
  <Counter threshold={5} />,
  document.getElementById('main')
)
```

Open the index.html file again... See that? We got some numbers!

### Updating the state of our component

We got some buttons showing, but they ain't really doing much huh? Lets fix that. Our goal is to update the state of our component by clicking two different buttons. When we click on the `click me` button, the count should increase by one. When we click on the `reset` button, the count and treshold should reset to it's initial state.

We update the state of our component using the **setState** function. To update our counter, we want to not only add +1 to the counter, but also alert the user when the treshold is met. We also want to increase the treshold by 5 after the user is alerted.

Lets add some code to our updateCounter function:
```javascript
updateCounter: function() {
  this.setState({ count: this.state.count + 1 })
  if (this.state.count === this.state.threshold) {
    alert('You passed the threshold!')
    this.setState({ threshold: this.state.threshold + 5 })
  }
},
```

If you click the `click me` button, your counter state should be updating! What if you want to reset without refreshing the browser? We could do that.

Let's add the following code to our resetCounter function:
```javascript
resetCounter: function() {
  this.setState(this.getInitialState())
},
```

By now, you should be able to click on a button that adds +1 to a counter, and then reset that counter by pressing an alternative button.

If that's not working, make sure your code looks as follows:
```javascript
let Counter = React.createClass({

  getInitialState: function() {
    return {
      count: 0,
      threshold: this.props.threshold
    }
  },

  updateCounter: function() {
    this.setState({ count: this.state.count + 1 })
    if (this.state.count === this.state.threshold) {
      alert('You passed the threshold!')
      this.setState({ threshold: this.state.threshold + 5 })
    }
  },

  resetCounter: function() {
    this.setState(this.getInitialState())
  },

  render: function() {
    return (
      <div>
        <p> Number of clicks: {this.state.count} </p>
        <p> The threshold is set to: {this.state.threshold}</p>
        <button onClick={this.updateCounter}> Click me! </button>
        <button onClick={this.resetCounter}> Reset </button>
      </div>
    )
  }

})

ReactDOM.render(
  <Counter threshold={5} />,
  document.getElementById('main')
)
```

## Conclusion

Congratulations... You just created a web page with Reactjs! The beauty of the above component is that all we have to manage is what the data is initially, and how it's updated. The DOM is updated automatically for us once the data, or state, changes.

### Further Learning

<<<<<<< HEAD
If you found this interesting and want to dive in a little deeper, go to Reactjs' website and follow their intro to react [tutorial](https://reactjs.org/tutorial/tutorial.html). The tutorial teaches you how to build a React app using `create-react-app`. Create React App is a tool built by developers at Facebook to help you build React applications. It saves you from time-consuming setup and configuration. You simply run one command and create react app sets up the tools you need to start your React project!
=======
If you found this interesting and want to dive in a little deeper, go to Reactjs' website and follow their intro to react [tutorial](https://reactjs.org/tutorial/tutorial.html). The tutorial teaches you how to build a React app using *Create React App*. *Create React App* is a tool built by developers at Facebook to help you build React applications. It saves you from time-consuming setup and configuration. You simply run one command and create react app sets up the tools you need to start your React project!
>>>>>>> master

### References

This tutorial was inspired by the following sites:
- [General Assembly's React Repo](https://git.generalassemb.ly/ga-wdi-boston/react)
- [Coderbyte's Introduction to React](https://coderbyte.com/tutorial/introduction-to-react)
