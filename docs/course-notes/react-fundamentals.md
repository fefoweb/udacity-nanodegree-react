---
title: Udacity React Fundamentals
description: Notes by James Priest
---
<!-- markdownlint-disable MD022 MD032 -->
<!-- # React Nanodegree -->
# React Fundamentals

[<-- back to React Nanodegree homepage](../index.html)

<!-- ### Links
#### Resources
- [Create Amazing Forms by Pete LePage](https://goo.gl/i0vY1M) - input types, datalist, labels, autocomplete attributes, autofill pitfalls, real-time validation

#### Samples
- [Slider sample](../exercises/wf4-9/index.html) - Uses both mouse and touch events -->

---

## 1. Why React
### 1.1 Introduction
[![rf1](../assets/images/rf1-small.jpg)](../assets/images/rf1.jpg)

Before we actually dive into the syntax of React, let's take a step back and talk about what makes React special.

- It's compositional model
- It's declarative nature
- The way data flows through a Component
- That React is really just JavaScript

### 1.2 What is Composition
#### Benefits of Composition
Because the concept of composition is such a large part of what makes React awesome and incredible to work with, let's dig into it a little bit. Remember that composition is just combining simple functions together to create complex functions. There are a couple of key ingredients here that we don't want to lose track of. These ingredients are:

- simple functions
- combined to create another function

Composition is built from simple functions. Let's look at an example:

```js
function getProfileLink (username) {
 return 'https://github.com/' + username
}
```

This function is ridiculously simple, isn't it? It's just one line! Similarly, the `getProfilePic` function is also just a single line:

```js
function getProfilePic (username) {
 return 'https://github.com/' + username + '.png?size=200'
}
```

These are definitely simple functions, so to compose them, we'd just *combine* them together inside another function:

```js
function getProfileData (username) {
 return {
   pic: getProfilePic(username),
   link: getProfileLink(username)
 }
}
```

Now we *could* have written `getProfileData` *without* composition by providing the data directly:

```js
function getProfileData (username) {
 return {
 pic: 'https://github.com/' + username + '.png?size=200',
 link: 'https://github.com/' + username
 }
}
```

There's nothing technically wrong with this at all; this is entirely accurate JavaScript code. But this *isn't* composition. There are also a couple of potential issues with this version which  isn't using composition. If the user's link to GitHub is needed somewhere else, then duplicate code would be needed. A good function should follow the "DOT" rule:

> *Do One Thing*

This function is doing a couple of different (however minor) things; it's creating two different URLs, storing them as properties on an object, and then returning that object. In the composed version, each function just does one thing:

- `getProfileLink` – just builds up a string of the user's GitHub profile link
- `getProfilePic` – just builds up a string the user's GitHub profile picture
- `getProfileData` – returns a new object

#### React & Composition
React makes use of the power of composition, heavily! React builds up pieces of a UI using **components**. Let's take a look at some pseudo code for an example. Here are three different components:

```html
<Page />
<Article />
<Sidebar />
```

Now let's take these *simple* components, combine them together, and create a more complex component (aka, composition in action!):

```html
<Page>
 <Article />
 <Sidebar />
</Page>
```

Now the Page component has the Article and Sidebar components inside. This is just like the earlier example where getProfileData had getProfileLink and getProfilePic inside it.

We'll dig into components soon, but just know that composition plays a huge part in building React components.

#### Composition Recap
Composition occurs when simple functions are combined together to create more complex functions. Think of each function as a single building block that does one thing (DOT). When you combine these simple functions together to form a more complex function, this is **composition**.

##### Further Research
- [Compose me That: Function Composition in JavaScript](https://www.linkedin.com/pulse/compose-me-function-composition-javascript-kevin-greene)
- [Functional JavaScript: Function Composition For Every Day Use](https://hackernoon.com/javascript-functional-composition-for-every-day-use-22421ef65a10)

### 1.3 Declarative Code
#### Imperative Code
A lot of JavaScript is **imperative code**. If you don't know what "imperative" means here, then you might be scratching your head a bit. According to the dictionary, "imperative" means:

> *expressing a command; commanding*

When JavaScript code is written *imperatively*, we tell JavaScript **how** we want something done. Think of it as if we're giving JavaScript commands on exactly what steps it should take. For example, I give you the humble `for` loop:

```js
const people = ['Amanda', 'Farrin', 'Geoff', 'Karen', 'Richard', 'Tyler']
const excitedPeople = []

for (let i = 0; i < people.length; i++) {
 excitedPeople[i] = people[i] + '!'
}
```

If you've worked with JavaScript any length of time, then this should be pretty straightforward. We're looping through each item in the `people` array, adding an exclamation mark to their name, and storing the new string in the `excitedPeople` array. Pretty simple, right?

This is imperative code, though. We're commanding JavaScript what to do at every single step. We have to give it commands to:

- set an initial value for the iterator - (`let i = 0`)
- tell the `for` loop when it needs to stop - (`i < people.length`)
- get the person at the current position and add an exclamation mark - (`people[i] + '!'`)
- store the data in the `i`th position in the other array - (`excitedPeople[i]`)
- increment the `i` variable by one - (`i++`)

Remember the example of keeping the air temperature at 71º? In my old car, I would turn the knob to get the cold air flowing. But if it got too cold, then I'd turn the knob up higher. Eventually, it would get too warm, and I'd have to turn the knob down a bit, again. I'd have to manage the temperature myself with every little change. Doesn't this sound like an imperative situation to you? I have to manually do multiple steps. It's not ideal, so let's improve things!

#### Declarative Code
In contrast to imperative code, we've got **declarative** code. With declarative code, we don't code up all of the steps to get us to the end result. Instead, we *declare* what we want done, and JavaScript will take care of doing it. We tell JavaScript **what** we want done. This explanation is a bit abstract, so let's look at an example. Let's take the imperative `for` loop code we were just looking at and refactor it to be more declarative.

With the imperative code we were performing all of the steps to get to the end result. What _is_ the end result that we actually want, though? Well, our starting point was just an array of names:

```js
const people = ['Amanda', 'Farrin', 'Geoff', 'Karen', 'Richard', 'Tyler']
```

The end goal that we want is an array of the same names but where each name ends with an exclamation mark:

```js
["Amanda!", "Farrin!", "Geoff!", "Karen!", "Richard!", "Tyler!"]
```

To get us from the starting point to the end, we'll just use JavaScript's .map() function to declare what we want done.

```js
const excitedPeople = people.map(name => name + '!')
```

That's it! Notice that with this code we haven't:

- created an iterator object
- told the code when it should stop running
- used the iterator to access a specific item in the `people` array
- stored each new string in the `excitedPeople` array

...all of those steps are taken care of by JavaScript's `.map()` Array method.

> .map() and .filter()
>
> A bit rusty on JavaScript's .map() and .filter() Array methods? Or perhaps they're brand new to you. In either case, we'll be diving into them in the React is "just JavaScript" section. Hold tight!

#### React is Declarative
We'll get to writing React code very soon, but let's take another glimpse at it to show how it's declarative.

```html
<button onClick={activateTeleporter}>Activate Teleporter</button>
```

It might seem odd, but this is valid React code and should be pretty easy to understand. Notice that there's just an `onClick` attribute on the button...we aren't using `.addEventListener()` to set up event handling with all of the steps involved to set it up. Instead, we're just declaring that we want the `activateTeleporter` function to run when the button is clicked.

#### Declarative Code Recap
*Imperative* code instructs JavaScript on **how** it should perform each step. With *declarative* code, we tell JavaScript **what** we want to be done, and let JavaScript take care of performing the steps.

React is declarative because we write the code that we want, and React is in charge of taking our declared code and performing all of the JavaScript/DOM steps to get us to our desired result.

##### 1.3 Further Research

- Tyler's [Imperative vs Declarative Programming](https://tylermcginnis.com/imperative-vs-declarative-programming/) blog post
- [Difference between declarative and imperative in React.js?](https://stackoverflow.com/questions/33655534/difference-between-declarative-and-imperative-in-react-js) from StackOverflow

### 1.4 Unidirectional Data Flow
Before React, one popular technique for managing state changes in an app over time, was to use data bindings,
so that when data changes in one place, those changes are automatically reflected in other places in the app.

Any part of the app that had that data could also change it. But, as the app grows this technique makes it difficult to determine how a change in one place automatically and implicitly affects the rest of the app.

React uses an explicit method for passing data between components that makes it a lot easier to track changes to the state, and how they affect other places of the app.

This is called unidirectional data flow because the data flows one way from parent elements down to children.

#### Data-Binding In Other Frameworks
Front-end frameworks like [Angular](https://angular.io/) and [Ember](https://emberjs.com/) make use of two-way data bindings. In two-way data binding, the data is kept in sync throughout the app no matter where it is updated. If a model changes the data, then the data updates in the view. Alternatively, if the user changes the data in the view, then the data is updated in the model. Two-way data binding sounds really powerful, but it can make the application harder to reason about and know where the data is actually being updated.

##### 1.4 Further Research

- [Angular's two-way data binding](https://angular.io/guide/template-syntax#two-way)
- [Ember's two-way data binding](https://guides.emberjs.com/v2.13.0/object-model/bindings/)

#### React's Data-flow
Data moves differently with React's unidirectional data flow. In React, the data flows from the parent component to a child component.

[![rf2](../assets/images/rf2-small.jpg)](../assets/images/rf2.jpg)<br>
*Data flows down from parent component to child component. Data updates are sent to the parent component where the parent performs the actual change.*

In the image above, we have two components:

- a parent component
- a child component

The data lives in the parent component and is passed down to the child component. Even though the data lives in the parent component, both the parent and the child components can use the data. However, if the data must be updated, then only the parent component should perform the update. If the child component needs to make a change to the data, then it would send the updated data to the parent component where the change will actually be made. Once the change *is* made in the parent component, the child component will be passed the data (that has just been updated!).

Now, this might seem like extra work, but having the data flow in one direction and having one place where the data is modified makes it much easier to understand how the application works.

#### 1.4 Quiz
##### Question 1 of 2
A `FlightPlanner` component stores the information for booking a flight. It also contains `DatePicker` and `DestinationPicker` as child components. Here's what the code might look like:

```html
<FlightPlanner>
 <DatePicker />
 <DestinationPicker />
</FlightPlanner>
```

If this were a React application, which component should be in charge of making updates to the data? Check all that apply.

- [x] `FlightPlanner`
- [ ] `DatePicker`
- [ ] `DestinationPicker`


Now let's say that the `FlightPlanner` component has two child components: `LocationPicker` and `DatePicker`. `LocationPicker` itself is a parent component that has two child components: `OriginPicker` and `DestinationPicker`.

##### Question 2 of 2
If the following sample code were a React application, which of the following components should be in charge of making updates to data? Check all that apply.

```html
<FlightPlanner>

 <LocationPicker>
  <OriginPicker />
  <DestinationPicker />
 </LocationPicker>

 <DatePicker />

</FlightPlanner>
```

- [x] `FlightPlanner` - is the parent component and stores all of the flight data, any changes to the data should be made by this component.
- [ ] `DatePicker` - receives the data from its parent
- [x] `LocationPicker` - is a parent component, it would make sense that it would handle all changes for its child components.
- [ ] `OriginPicker` - receives the data from its parent
- [ ] `DestinationPicker` - receives the data from its parent

#### Data Flow in React Recap
In React, data flows in only one direction, from parent to child. If data is shared between sibling child components, then the data should be stored in the parent component and passed to both of the child components that need it.

### 1.5 React is Just JavaScript
[![rf3](../assets/images/rf3-small.jpg)](../assets/images/rf3.jpg)

#### It's Just JavaScript
One of the great things about React is that a lot of what you'll be using is regular JavaScript. To make sure you're ready to move forward, please take a look at the following code:

```js
const shelf1 = [{name: 'name1', shelf: 'a'},{name: 'name2', shelf: 'a'}];
const shelf2 = [{name: 'name3', shelf: 'b'},{name: 'name4', shelf: 'b'}];
const allBooks = [...shelf1, ...shelf2];

const filter = books => shelf => books.filter(b => {
  return b.shelf === shelf;
});

const filterBy = filter(allBooks);
const booksOnShelf = filterBy('b');
```

If *any* of the code above looks confusing, or if you simply need a refresher on E6, please go through [our ES6 course](https://classroom.udacity.com/courses/ud356) before moving forward.

Here's a couple links for a quick refresher.

- [Udacity ES6 course - Syntax notes](https://james-priest.github.io/100-days-of-code-log-r2/ES6-Syntax.html) - destructuring, spread, & rest operators
- [Currying and ES6 Arrow Functions](http://codekirei.com/posts/currying-with-arrow-functions/) - with double arrow functions

Over the past couple of years, functional programming has had a large impact on the JavaScript ecosystem and community. Functional programming is an advanced topic in JavaScript and fills hundreds of books. It's too complex to delve into the benefits of functional programming (we've got to get to React content, right?!?). 

React builds on a lot of the techniques of functional programming...techniques that you'll learn as you go through this program. However, there are a couple of important JavaScript functions that are vital to functional programming that we should look at. These are the Array's `.map()` and `.filter()` methods.

#### Array's .map() Method
If you're not familiar with JavaScript's [Array .map() method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map), it gets called on an existing array and returns a new array based on what is returned from the function that's passed as an argument. Let's take a look:

```js
const names = ['Karen', 'Richard', 'Tyler'];

const nameLengths = names.map( name => name.length );
```

Let's go over what's happening here. The .map() method works on arrays, so we have to have an array to start with:

```js
const names = ['Karen', 'Richard', 'Tyler'];
```

We call `.map()` on the `names` array and pass it a function as an argument:

```js
names.map( name => name.length );
```

The arrow function that's passed to `.map()` gets called *for each item* in the `names` array! The arrow function receives the first name in the array, stores it in the `name` variable and returns its length. Then it does that again for the remaining two names.

`.map()` returns a new array with the values that are returned from the arrow function:

```js
const nameLengths = names.map( name => name.length );
```

So `nameLengths` will be a new array `[5, 7, 5]`. This is important to understand; **the .map() method returns a new array, it does not modify the original array.**

This was just a brief overview of how the `.map()` method works. For a deeper dive, check out [.map() on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

#### .map() Quiz
Use the provided music data array and the `.map()` method to create a new array that contains items in the format:

```html
<album-name> by <artist> sold <sales> copies
```

Store the new array in an `albumSalesStrings` array. So the first item in the `albumSalesStrings` array should be "25 by Adele sold 1731000 copies"

```js
/* Using .map()
 *
 * Using the musicData array and .map():
 *   - return a string for each item in the array in the following format
 *     <album-name> by <artist> sold <sales> copies
 *   - store the returned data in a new albumSalesStrings variable
 *
 * Note:
 *   - do not delete the musicData variable
 *   - do not alter any of the musicData content
 *   - do not format the sales number, leave it as a long string of digits
 */

const musicData = [
    { artist: 'Adele', name: '25', sales: 1731000 },
    { artist: 'Drake', name: 'Views', sales: 1608000 },
    { artist: 'Beyonce', name: 'Lemonade', sales: 1554000 },
    { artist: 'Chris Stapleton', name: 'Traveller', sales: 1085000 },
    { artist: 'Pentatonix', name: 'A Pentatonix Christmas', sales: 904000 },
    { artist: 'Original Broadway Cast Recording', 
      name: 'Hamilton: An American Musical', sales: 820000 },
    { artist: 'Twenty One Pilots', name: 'Blurryface', sales: 738000 },
    { artist: 'Prince', name: 'The Very Best of Prince', sales: 668000 },
    { artist: 'Rihanna', name: 'Anti', sales: 603000 },
    { artist: 'Justin Bieber', name: 'Purpose', sales: 554000 }
];

// SOLUTION
const albumSalesStrings = 
  musicData.map(obj => `${obj.name} by ${obj.artist} sold ${obj.sales} copies`);

console.log(albumSalesStrings);
```

#### Array's .filter() Method
JavaScript's [Array .filter() method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) is similar to the `.map()` method:

- it is called on an array
- it takes a function as an argument
- it returns a new array

The difference is that the function passed to `.filter()` is used as a test, and only items in the array that pass the test are included in the new array. Let's take a look at an example:

```js
const names = ['Karen', 'Richard', 'Tyler'];

const shortNames = names.filter( name => name.length < 6 );
```

Just as before, we have the starting array:

```js
const names = ['Karen', 'Richard', 'Tyler'];
```

We call `.filter()` on the `names` array and pass it a function as an argument:

```js
names.filter( name => name.length < 6 );
```

Again, just like with `.map()` the arrow function that's passed to `.filter()` gets called *for each item* in the `names` array. The first item (i.e. 'Karen') is stored in the `name` variable. Then the test is performed - this is what's doing the actual filtering. It checks the length of the name. If it's 6 or greater, then it's skipped (and not included in the new array!). But if the length of the name is less than 6, then `name.length < 6` returns true and the name _is_ included in the new array!

And lastly, just like with `.map()` the `.filter()` method returns a new array instead of modifying the original array:

```js
const shortNames = names.filter( name => name.length < 6 );
```

So `shortNames` will be the new array `['Karen', 'Tyler']`. Notice that it only has two names in it now, because 'Richard' is 7 characters and was filtered out.

This was just a brief overview of how the `.filter()` method works. For a deeper dive, check out [.filter() on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

#### .filter() Quiz
Use the provided music data array and the `.filter()` method to create a new array that only contains albums with names between 10 and 25 characters long. Store the new array in a variable called `results`.

```js
/* Using .filter()
 *
 * Using the musicData array and .filter():
 *   - return only album objects where the album's name is
 *     10 characters long, 25 characters long, or anywhere in between
 *   - store the returned data in a new `results` variable
 *
 * Note:
 *   - do not delete the musicData variable
 *   - do not alter any of the musicData content
 */

const musicData = [
    { artist: 'Adele', name: '25', sales: 1731000 },
    { artist: 'Drake', name: 'Views', sales: 1608000 },
    { artist: 'Beyonce', name: 'Lemonade', sales: 1554000 },
    { artist: 'Chris Stapleton', name: 'Traveller', sales: 1085000 },
    { artist: 'Pentatonix', name: 'A Pentatonix Christmas', sales: 904000 },
    { artist: 'Original Broadway Cast Recording', 
      name: 'Hamilton: An American Musical', sales: 820000 },
    { artist: 'Twenty One Pilots', name: 'Blurryface', sales: 738000 },
    { artist: 'Prince', name: 'The Very Best of Prince', sales: 668000 },
    { artist: 'Rihanna', name: 'Anti', sales: 603000 },
    { artist: 'Justin Bieber', name: 'Purpose', sales: 554000 }
];

// SOLUTION
const results =
  musicData.filter(album => album.name.length >= 10 && album.name.length <= 25);

console.log(results);
```

#### Combining .map() And .filter() Together
What makes `.map()` and `.filter()` so powerful is that they can be combined. Because both methods return arrays, we can chain the method calls together so that the returned data from one can be a new array for the next.

```js
const names = ['Karen', 'Richard', 'Tyler'];

const shortNamesLengths =
  names.filter( name => name.length < 6 ).map( name => name.length );
```

To break it down, the `names` array is filtered, which returns a new array, but then `.map()` is called on that new array, and returns a new array of its own! This new array that's returned from `.map()` is what's stored in shortNamesLengths.

#### .filter() First!
On a side note, you'll want to run things in this order - `.filter()` first and then `.map()`. Because `.map()` runs the function once for each item in the array, it will be faster if the array were already filtered.

#### .filter() and .map() Quiz
Using the same music data, use `.filter()` and `.map()` to filter and map over the list and store the result in a variable named `popular`. Use `.filter()` to filter the list down to just the albums that have sold over 1,000,000 copies. Then chain `.map()` onto the returned array to create a new array that contains items in the format:

```html
<artist> is a great performer
```

The first item in the `popular` array will be 'Adele is a great performer'.

```js
/* Combining .filter() and .map()
 *
 * Using the musicData array, .filter, and .map():
 *   - filter the musicData array down to just the albums that have 
 *     sold over 1,000,000 copies
 *   - on the array returned from .filter(), call .map()
 *   - use .map() to return a string for each item in the array in the
 *     following format: "<artist> is a great performer"
 *   - store the array returned form .map() in a new "popular" variable
 *
 * Note:
 *   - do not delete the musicData variable
 *   - do not alter any of the musicData content
 */

const musicData = [
    { artist: 'Adele', name: '25', sales: 1731000 },
    { artist: 'Drake', name: 'Views', sales: 1608000 },
    { artist: 'Beyonce', name: 'Lemonade', sales: 1554000 },
    { artist: 'Chris Stapleton', name: 'Traveller', sales: 1085000 },
    { artist: 'Pentatonix', name: 'A Pentatonix Christmas', sales: 904000 },
    { artist: 'Original Broadway Cast Recording', 
      name: 'Hamilton: An American Musical', sales: 820000 },
    { artist: 'Twenty One Pilots', name: 'Blurryface', sales: 738000 },
    { artist: 'Prince', name: 'The Very Best of Prince', sales: 668000 },
    { artist: 'Rihanna', name: 'Anti', sales: 603000 },
    { artist: 'Justin Bieber', name: 'Purpose', sales: 554000 }
];

const popular = musicData
  .filter(album => album.sales > 1000000)
  .map(album => `${album.artist} is a great performer`);

console.log(popular);

```

#### React is Just JavaScript Recap
React builds on what you already know - JavaScript! You don't have to learn a special template library or a new way of doing things.

Two of the main methods that you'll be using quite a lot are:

- `.map()`
- `.filter()`

It's important that you're comfortable using these methods, so take some time to practice using them. Why not look through some of your existing code and try converting your `for` loops to `.map()` calls or see if you can remove any `if` statements by using `.filter()`.

### 1.6 Recap
Let's recap on some of the things we covered in this lesson on why React is great:

- its compositional model
- its declarative nature
- the way data flows from parent to child
- and that React is really just JavaScript

#### Lesson Challenge

Read these 3 articles that cover some of the essentials of React: 

- [Virtual DOM](https://reactjs.org/docs/optimizing-performance.html#avoid-reconciliation)
- [The Diffing Algorithm](https://reactjs.org/docs/reconciliation.html#the-diffing-algorithm)
- [How Virtual-DOM and diffing works in React](https://medium.com/@gethylgeorge/how-virtual-dom-and-diffing-works-in-react-6fc805f9f84e)

Answer the following questions (in your own words) and share your answers with your Study Group.

1. What is the “Virtual DOM”?
2. Explain what makes React performant.
3. Explain the Diffing Algorithm to someone who does not have any programming experience.

## 2. Rendering UI w/ React
### 2.1 Rendering UI Intro
[![rf4](../assets/images/rf4-small.jpg)](../assets/images/rf4.jpg)

React uses JavaScript objects to create React elements. We'll use these React elements to describe what we want the page to look like, and React will be in charge of generating the DOM nodes to achieve the result.

Recall from the previous lesson the difference between imperative and declarative code. The React code that we write is declarative because we aren't telling React *what* to do; instead, we're writing React elements that describe what the page should look like, and React does all of the implementation work to get it done.

Enough theory, let's get to it and create some elements!

### 2.2 Elements and JSX
#### Watch First
We'll be looking at using React's `.createElement()` method in the next couple of videos. For starters, here is its signature:

```js
React.createElement( /* type */, /* props */, /* content */ );
```

We'll take a deep dive into what all that entails in just a bit! We'll start things out with a project that's already set up. For now, don't worry about creating a project or coding along. There will be plenty of hands-on work for you to do soon enough!

We'll start building our in-class project, Contacts App, in the next section. If you would like to code along for the next few videos, you can use this [React Sandbox](https://codesandbox.io/s/new).

> #### 💡 Trying Out React Code 💡
>React is an extension of JavaScript (i.e., a JavaScript library), but it isn't built into your browser. You wouldn't be able to test out React code samples in your browser console the way you would if you were learning JavaScript. In just a bit, we'll see how to install and use a React environment!

#### React.createElement

```js
import React from "react";
import ReactDOM from "react-dom";

const element = React.createElement("div",  null, "hello world");

ReactDOM.render(element, document.getElementById("root"));
```

[![rf5](../assets/images/rf5-small.jpg)](../assets/images/rf5.jpg)<br>
**Live Demo:** [React Element on CodeSandbox](https://codesandbox.io/s/427v5vvkmw)

#### ReactDOM
One thing to keep in mind is that we could be rendering out to different destinations.  For that reason, ReactDOM was split out of the React library. Some other destinations include:

- render on the server
- native devices
- VR environments

[![rf6](../assets/images/rf6-small.jpg)](../assets/images/rf6.jpg)

#### Rendering Elements onto the DOM
In the previous video, we used ReactDOM's `render()` method to render our element onto a particular area of a page. In particular, we rendered the element onto a DOM node called `root`. But where did this root come from?

Apps built with React typically have a single `root` DOM node. For example, an HTML file may contain a `<div>` with the following:

```html
<div id="root"></div>
```

By passing this DOM node into `getElementById()`, React will end up controlling the entirety of its contents. Another way to think about this is that this particular `<div>` will serve as a "hook" for our React app; this is the area where React will take over and render our UI!

#### Question 1 of 3
What will `myBio` hold when the following code is run?

```js
import React from 'react';

const myBio = React.createElement('div', null, 'Hi, I love porcupines.');
```

- [ ] a reference to a DOM node
- [ ] a DOM node itself
- [x] a JavaScript object
- [ ] a JavaScript class

#### DOM nodes - className

```js
import React from "react";
import ReactDOM from "react-dom";

const element = React.createElement(
  "div",
  {
    className: "welcome-message"
  },
  "hello world"
);

console.log(element);

ReactDOM.render(element, document.getElementById("root"));

```

[![rf7](../assets/images/rf7-small.jpg)](../assets/images/rf7.jpg)<br>
**Live Demo:** [React Element on CodeSandbox](https://codesandbox.io/s/427v5vvkmw)

When we're creating these React elements we must remember that we are describing DOM nodes not HTML elements. For that reason we must use things like 'className' rather than 'class' since 'class' is a reserved word.

> Virtual DOM  - objects that describe real DOM nodes
>
> When we call `React.createElement` we haven't actually created anything in the DOM yet. It's not until we cal `render()` that the browser actually creates a real DOM element.

#### Question 2 of 3
React allows a lot of HTML attributes to be passed along to the React element. Look through [all supported HTML attributes](https://reactjs.org/docs/dom-elements.html#all-supported-html-attributes) in the React docs and select which of the following attributes are allowed:

- [x] poster
- [x] id
- [x] marginWidth
- [ ] for - ('for' is a reserved word so instead we can use 'htmlFor')

I just used React's `.createElement()` method to construct a "React element". The `.createElement()` method has the following signature:

```js
React.createElement( /* type */, /* props */, /* content */ ); 
```

Let's break down what each item can be:

- `type` – either a string or a React Component
  
  This can be a string of any existing HTML element (e.g. `'p'`, `'span'`, or `'header'`) or you could pass a React *component* (we'll be creating components with JSX, in just a moment).
- `props` – either `null` or an object
  
  This is an object of HTML attributes and custom data about the element.
- `content` – `null`, a string, a React Element, or a React Component
  
  Anything that you pass here will be the content of the rendered element. This can include plain text, JavaScript code, other React elements, etc.

#### Nested Elements

```js
import React from "react";
import ReactDOM from "react-dom";

const element = React.createElement(
  "ol",
  null,
  React.createElement("li", null, "James"),
  React.createElement("li", null, "Mark"),
  React.createElement("li", null, "Steve")
);

console.log(element);

ReactDOM.render(element, document.getElementById("root"));

```

[![rf8](../assets/images/rf8-small.jpg)](../assets/images/rf8.jpg)<br>
**Live Demo:** [React Element on CodeSandbox](https://codesandbox.io/s/427v5vvkmw)

#### List Data
Now, what we currently have is fine but most of the time when you need a list, you'll probably have the items in an array somewhere.

Instead of writing out child elements one by one, React lets us provide an array of elements to use as children. This makes it easier to work with existing arrays of data.

So, let's say we have an array here of people that we want to dynamically generate these list items from this array. We could just map over the people array and for each person, we will generate a list item.

And instead of hard-coding the we will just use `person.name` to get the same result.

```js
import React from "react";
import ReactDOM from "react-dom";

const people = [
  { name: 'James' },
  { name: 'Mark' },
  { name: 'Steve' }
];

const element = React.createElement(
  "ol",
  null,
  people.map((person) => (
    React.createElement('li', null, person.name)
  ))
);

ReactDOM.render(element, document.getElementById("root"));
```

[![rf9](../assets/images/rf9-small.jpg)](../assets/images/rf9.jpg)<br>
**Live Demo:** [React Element on CodeSandbox](https://codesandbox.io/s/427v5vvkmw)

The thing I like about using JavaScript to generate these elements is that I didn't need any special syntax to map over the array. Instead, I just used array.map.

I didn't need a templating language to give me some 'repeat' or 'mapping' or 'each' syntax to loop over the array. I can use JavaScript which I already know.

Another thing that's interesting here is that this person object was already in scope. So, I didn't need a templating language to give me that concept of scope. I just use the person object in the JavaScript function scope. There's nothing new to learn here.

Now one thing to note, when you're using an array as children is that React is going to complain if you don't give it a key.

If we look at the console here in the browser, you'll see a warning. 

> Each child in an array or iterator should have a unique "key" prop. Check the top-level render call using ol.

[![rf10](../assets/images/rf10-small.jpg)](../assets/images/rf10.jpg)

What does that mean? Well, remember, when we added the class name to the div, the second argument which we assigned a `null` to, is for props to our component.

So, let's give this item a unique key prop. Something that is unique about each of these objects. In this case, the name would work because that's unique for each of the objects.

```js
import React from "react";
import ReactDOM from "react-dom";

const people = [{ name: "James" }, { name: "Mark" }, { name: "Steve" }];

const element = React.createElement(
  "ol",
  null,
  people.map(person =>
    React.createElement("li", { key: person.name }, person.name)
  )
);

ReactDOM.render(element, document.getElementById("root"));
```

[![rf11](../assets/images/rf11-small.jpg)](../assets/images/rf11.jpg)<br>
**Live Demo:** [React Element on CodeSandbox](https://codesandbox.io/s/427v5vvkmw)

So, you'll notice here that the warning goes away. Now, we're not going to go too deep  nto the key prop in this lesson. But know that if you are mapping over an array with React and you're creating elements for each item in that array, each element needs its own unique key prop.

#### .createElement() Returns only One Root Element
Recall that `React.createElement();` creates a single React element of a particular type. We'd normally pass in a tag such as a `<div>` or a `<span>` to represent that type, but the content argument can be *another* React element.

Consider the following example:

```js
const element = React.createElement('div', null,
 React.createElement('strong', null, 'Hello world!')
);
```

Here, "Hello world!" will be wrapped in a `<div>` when this React element renders as HTML. **While we can indeed nest React elements, remember the overall call just returns a single element.**

#### JSX
Now that we've learned how to create elements and how to nest them, it can get pretty tedious if we're just using these nested create element calls to create large portions of our app.

What we need is an HTML-like syntax that we can use in our JavaScript.

This is exactly what JSX does.

> JSX is a syntax extension to JavaScript, that lets us write JavaScript code that looks a little bit more like HTML, making it more concise and easier to follow. Let's check it out.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './styles.css';

const people = [{ name: 'James' }, { name: 'Mark' }, { name: 'Steve' }];

const element = (
  <ol>
    <li>{people[0].name}</li>
  </ol>
);

ReactDOM.render(element, document.getElementById('app'));
```

Whenever we want JSX to evaluate some JavaScript for us, we have to wrap that piece of JavaScript in curly braces. This could be any JavaScript expression you want including some math, a ternary, or any other valid JavaScript.

Let's create our list again. Everything between curly braces is JavaScript and everything between angle brackets is JSX. The code alternates between the two but is much more concise than the nested `createElement` calls.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './styles.css';

const people = [{ name: 'James' }, { name: 'Mark' }, { name: 'Steve' }];

const element = (
  <ol>
    {people.map(person => (
      <li key={person.name}>{person.name}</li>
    ))}
  </ol>
);

ReactDOM.render(element, document.getElementById('app'));
```

**Live Demo:** [React Simple JSX on CodeSandbox](https://codesandbox.io/s/xl6jw432no)

As we said earlier whenever we give React an array, we need to give a unique key prop to each one of the repeating elements, list item in this case.

You'll notice it looks like we're assigning values to HTML attributes. We do this by opening up a JavaScript expression and using `person.name` as the value of the key prop.

Another thing to note is that event though we're using JSX which is nice and concise, this code gets compiled down to real JavaScript using  `createElement` inside our 'bundle.js'.

#### Question 3 of 3
Consider the following example in JSX:

```jsx
const greeting = (
 <div className='greeting'>
 <h2>Hello world!</h2>
 </div>
);
```

If we want to output the same HTML, what goes into 1, 2, and 3 when calling `createElement()`?

```js
const greeting = React.createElement(
 __1__,
 { className: 'greeting' },
 React.createElement(
 __2__,
 {},
 __3__
 )
);
```

- [ ] 'h2', 'div', 'Hello world!'
- [ ] 'div', 'h2', 'Hello world!'
- [x] 'div', 'h2', 'Hello world!'
- [ ] 'Hello world', 'div', 'h2'

#### JSX returns *One* main element, too
When writing JSX, keep in mind that it must only return a single element. This element may have any number of descendants, but there must be **a single root element wrapping your overall JSX** (typically a `<div>` or a `<span>`). Check out the following example:

```jsx
const message = (
 <div>
   <h1>All About JSX:</h1>
   <ul>
     <li>JSX</li>
     <li>is</li>
     <li>awesome!</li>
   </ul>
 </div>
);
```

See how there's only one `<div>` element in the code above and that all other JSX is nested inside it? This is how you have to write it if you want multiple elements. To be completely clear, the following is incorrect and will cause an error:

```jsx
const message = (
 <h1>All About JSX:</h1>
 <ul>
   <li>JSX</li>
   <li>is</li>
   <li>awesome!</li>
 </ul>
);
```

In this example, we have two sibling elements that are both at the root level (i.e. `<h1>` and `<ul>`) . This won't work and will give the error:

Syntax error: Adjacent JSX elements must be wrapped in an enclosing tag

Since we know that JSX is really just a syntax extension for `.createElement()`, this makes sense; `.createElement()` takes in only one tag name (as a string) as its first argument.

#### Intro to Components
So far we've seen how `.createElement()` and JSX can help us produce some HTML. Typically, though, we'll use one of React's key features, Components, to construct our UI. Components refer to *reusable* pieces of code ultimately responsible for returning HTML to be rendered onto the page. More often than not, you'll see React components written with JSX.

Since React's main focus is to streamline building our app's UI, there is only one method that is absolutely required in any React component class: `render()`.

React provides a base component class that we can use to group many elements together and use them as if they were one element.

You could think about React components as the factories that we use to create React elements. So, by building custom components or classes, we can easily generate our own custom elements.

Let's go ahead and build our first component class!

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './styles.css';

class ContactList extends React.Component {
  render() {
    const people = [{ name: 'Greg' }, { name: 'Mark' }, { name: 'Steve' }];

    return (
      <ol>
        {people.map(person => (
          <li key={person.name}>{person.name}</li>
        ))}
      </ol>
    );
  }
}

ReactDOM.render(<ContactList />, document.getElementById('app'));
```

**Live Demo:** [First Component on CodeSandbox](https://codesandbox.io/s/m7xqr84m48)

> #### 💡 Declaring Components in React 💡
> In the previous video, we defined the ContactList component like so:
>
> ```js
> class ContactList extends React.Component {
>   // ...
> }
> ```
>
> In other words, we are defining a component that's really just a JavaScript class that inherits from React.Component.
> 
> In real-world use (and throughout this course), you may also see declarations like:
>
> ```js
> class ContactList extends Component {
>   // ...
> }
> ```
>
> Both ways are functionally the same, but be sure your module imports match accordingly! That is, if you choose to declare components like the example directly above, your import from React will now look like:
>
> ```js
> import React, { Component } from 'react';
> ```

#### Creating Elements Recap
In the end, remember that React is only concerned with the View layer of our app. This is what the user sees and interacts with. As such, we can use `.createElement()` to render HTML onto a document. More often than not, however, you'll use a syntax extension to describe what your UI should look like. This syntax extension is known as JSX, and just looks similar to plain HTML written right into a JavaScript file. The JSX gets transpiled to React's `.createElement()` method that outputs HTML to be rendered in the browser.

A great mindset to have when building React apps is to think in components. Components represent the modularity and reusability of React. You can think of your component classes as factories that produce instances of components. These component classes should follow the single responsibility principle and just "do one thing". If it manages too many different tasks, it may be a good idea to decompose your component into smaller subcomponents.

##### Further Research

- [Rendering Elements](https://reactjs.org/docs/rendering-elements.html) from the React docs

### 2.3 Create React App
Skip past these older instruction to see how [Create React App](https://github.com/facebook/create-react-app) is installed on newer versions of node.js.

> #### Old instructions (for reference)
> ##### 💡Before Installing create-react-app💡
> If you already have Node.js on your machine, it might be a good idea to upgrade or reinstall to make sure you have the latest version. Keep in mind that Node.js now comes with npm by default.
>
> ##### MacOS
> 1. Install Homebrew by running
> ```bash
> /usr/bin/ruby -e "$curl -fsSL
> https://raw.githubusercontent.com/Homebrew/install/master/install)"
> ```
>   in the terminal
> 2. Check that it was installed by running `brew --version`. You should see the version number that was installed.
> 3. Run `brew install node`.
> 4. Run `node --version`.
> 5. Check that npm was installed as well by running `npm --version`.
> 6. Run `brew install yarn --without-node`.
> 7. Run `npm --version`.
> 8. Run `yarn install && yarn --version`
>
> ##### Windows
> 1. Please download the [Node.js Installer](https://nodejs.org/en/download/), go through the installation process, and restart your computer once you're done.
> 2. Please follow the yarn [installation instructions](https://yarnpkg.com/lang/en/docs/install).
> 3. Run `yarn --version` to make sure yarn has been successfully installed.
>
> ##### Linux
> 1. Please follow [these instructions](https://www.ostechnix.com/install-node-js-linux) to install [Node.js](https://nodejs.org/en/download/).
> 2. Run `sudo apt-get install -y build-essential`.
> 3. Please follow the yarn [installation instructions](https://yarnpkg.com/lang/en/docs/install).
> 4. Run `yarn --version` to make sure yarn has been successfully installed.
>
> #### Install create-react-app globally
> Install Create React App (through the command-line with [npm](https://www.npmjs.com/get-npm)).
> ```bash
> npm install -g create-react-app
> ```
>
> If get permission errors, check out [this article on global package installs](https://docs.npmjs.com/getting-started/fixing-npm-permissions) in the npm documentation.
>
> Note that to find out where global packages are installed, you can run
> ```bash
> npm list -g --depth=0
> ```
> in your console (more information [here](https://stackoverflow.com/questions/5926672/where-does-npm-install-packages)).

#### What is Create React App
JSX is awesome, but it does need to be transpiled into regular JavaScript before reaching the browser. Typically this is done with two tools.

- [Babel](https://github.com/babel/babel) - a transpiler which converts JSX & ES6 to vanilla JavaScript
- [Webpack](https://github.com/webpack/webpack) - a build tool which bundles all our assets (JavaScript, CSS, images, etc.) for web projects

To streamline this initial configuration, Facebook published [Create React App](https://github.com/facebook/create-react-app) to manage the setup for us!

This tool is incredibly helpful to get started in building a React app, as it sets up everything we need with *zero configuration*!

The [Create React App - Quick Overview](https://github.com/facebook/create-react-app#quick-overview) shows how to use this tool to scaffold a React project without having to do a global install which was detailed in the old instructions.

#### Scaffolding Your React App
Let's do the following:

```bash
npx create-react-app contacts
cd contacts
npm start
```

Now, create-react-app installs `react`, `react-dom`, and the `react-scripts` package.

React-scripts encapsulates a lot of powerful libraries.

- It installs Babel so we can use the latest JavaScript syntax as well as JSX.
- It also installs Webpack, so we can generate the build
- It installs Webpack dev server, which gives us the auto-reload behavior we've seen up until this point.

As with all abstractions, you can peel back the layers on react-scripts one at a time, if you really want to see what's under the hood. But for now, `create-react-app` is a great way to get started quickly with the latest technologies without having to put in all the time needed to learn them before you get started with React.

##### The Yarn Package Manager
Both in the following video and in the output of create-react-app, we're told to use `yarn start` to start the development server.

[Yarn](https://yarnpkg.com/) is a package manager that's similar to NPM. Yarn was created from the ground up by Facebook to improve on some key aspects that were slow or lacking in NPM.

If you don't want to install Yarn, you don't have to! What's great about it is that almost every use of yarn can be swapped with npm and everything will work just fine! So if the command is `yarn start`, you can use `npm start` to run the same command.

#### create-react-app Recap
Facebook's `create-react-app` is a command-line tool that scaffolds a React application.

Using this, there is no need to install or configure module bundlers like Webpack, or transpilers like Babel. These come preconfigured (and hidden) with `create-react-app`, so you can jump right into building your app!

Check out these links for more info about create-react-app:

- [create-react-app](https://github.com/facebook/create-react-app) on GitHub
- [create-react-app Release Post](https://reactjs.org/blog/2016/07/22/create-apps-with-no-configuration.html) from the React blog
- [Updates to create-react-app](https://reactjs.org/blog/2017/05/18/whats-new-in-create-react-app.html) from the React blog

### 2.4 Composing with Components
Earlier, we said that components are the building blocks of React. But what is actually meant by that?

If you look at the API and documentation for React, they're relatively small. The vast majority of React's API is all about components. They are the main unit of encapsulation that React gives us.

Components are great, because they help us break down the UI into smaller pieces. These pieces have clear responsibilities and well-defined interfaces. This is valuable when building a large app, because it lets us work on tiny pieces of the app without inadvertently affecting the rest of it.

Another great thing about components, is that they encourage us to build applications using composition instead of inheritance.

So, let's talk a little bit about what it means to use composition to build user interfaces and how React let's us do that.

We open up the index.js and paste in the `<ContactList />` component. And instead of rendering everything inside of the App, I'm going to render it the ContactList.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

class ContactList extends React.Component {
  render() {
    const people = this.props.contacts;

    return (
      <ol>
        {people.map(person => (
          <li key={person.name}>{person.name}</li>
        ))}
      </ol>
    );
  }
}

function App() {
  return (
    <div className="App">
      <ContactList />
      <ContactList />
      <ContactList />
    </div>
  );
}

const rootElement = document.getElementById('root');
ReactDOM.render(<App />, rootElement);
```

**Live Demo:** [Simple Composition on CodeSandbox](https://codesandbox.io/s/p98n45z1wq)

We can already see how easy it is to create our own custom elements as we've talked about before, and compose them together. We can take the ContactList and put it right inside the application.

Encapsulating many elements inside of a component gives us a few advantages.

For one, it's really easy to reuse all of those elements. For example, if I wanted multiple copies of the ContactsLists, I could just copy and paste this line three times and get three identical copies of those elements.

Another nice property of these components is that they have a very clean interface so I can configure different components differently just by giving them different props.

Take for example, our ContactList. Let's say, in the first ContactList I want to show three names and in the second contact list, I want to show a completely different set of contacts.

So what I would actually like to do is to be able to configure these ContactLists independently of one another. We can do this with the use of  prop that we pass each ContactList component.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

class ContactList extends React.Component {
  render() {
    const people = this.props.contacts;

    return (
      <ol>
        {people.map(person => (
          <li key={person.name}>{person.name}</li>
        ))}
      </ol>
    );
  }
}

function App() {
  return (
    <div className="App">
      <ContactList
        contacts={[{ name: 'James' }, { name: 'Mark' }, { name: 'Steven' }]}
      />
      <ContactList
        contacts={[{ name: 'Evi' }, { name: 'Sarah' }, { name: 'Susan' }]}
      />
      <ContactList
        contacts={[{ name: 'Spot' }, { name: 'Rover' }, { name: 'Fido' }]}
      />
    </div>
  );
}

const rootElement = document.getElementById('root');
ReactDOM.render(<App />, rootElement);
```

**Live Demo:** [Simple Composition on CodeSandbox](https://codesandbox.io/s/p98n45z1wq)

You can see we were able to reuse the elements from ContactList but configure them completely separately. This makes it really easy to reuse  these components by just passing in little bits of configuration via the props.

These two principles,

- the ability to encapsulate a bunch of elements in a component
- the ability to easily reuse each component by being able to configure each one differently and independently via props

are two really important and fundamental keys to understanding the composition model of React.

#### Favor Composition Over Inheritance
You might have heard before that it’s better to “favor composition over inheritance”. This is a principle that I believe is difficult to learn today. Many of the most popular programming languages make extensive use of inheritance, and it has carried over into popular UI frameworks like the Android and iOS SDKs.

In contrast, React uses composition to build user interfaces. Yes, we extend React.Component, but we never extend it more than once. Instead of extending base components to add more UI or behavior, we compose elements in different ways using nesting and props. You ultimately want your UI components to be independent, focused, and reusable.

So if you’ve never understood what it means to “favor composition over inheritance” you’ll definitely learn using React!

### 2.5 Component Recap
The principles we've discussed in this lesson are absolutely fundamental to getting the most out of a React.

Just to recap...

1. We learned about how JSX just uses JavaScript to let us describe the UI by creating elements instead of writing these rigid string templates.
2. We also learned how to encapsulate groups of elements in React components, and how to build larger portions of the UI by composing those components together.
  > - Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
  >
  > - Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen.

3. We also installed Create React App, and used it to get a quick start on using the latest technologies commonly used to create a modern React application.

But React's code we use in encapsulation story gets really interesting in the next lesson where we talk about how each of these little components can hold, and manage it's own state.

## 3. State Management
### 3.1 Project Setup
[![rf12](../assets/images/rf12-small.jpg)](../assets/images/rf12.jpg)

Three new concepts that we'll be covering are:

- **Props** - Allows you to pass data into components
- **Funcitonal Components** - An alternative, and probably more intuitive approach to creating components.
- **Controlled Components** - Allows you to hook up the forms in your application to your component state

#### Contacts App
We will be building a Contacts app that shows a list of contacts. Each contact has an avatar, name, and twitter handle.

The app will also have a search bar that will allow you to filter the contacts or reset the the state to show all contacts.

It will also allow you to remove a contact and add a contact by entering a name, handle, and uploading an image.

[![rf13](../assets/images/rf13-small.jpg)](../assets/images/rf13.jpg)

What we should think about is how to build out this application. The way we build a large React application is by building out a bunch of small applications or components.

We need to consider how we'd structure this out if we were building this out of components.

#### Front End Project Files
Create React App will generate a number of default files and starter code that we need to get rid of. There will be two sets of changes you need to make, delete the starter content and add files that we're providing you.

We've done this step this for you. Here's the project repo

- [https://github.com/udacity/reactnd-contacts-app](https://github.com/udacity/reactnd-contacts-app)

Follow these instructions.

- Clone the repo with
  - `git clone https://github.com/udacity/reactnd-contacts-app.git`
- checkout the 'starter-files-added' remote branch with this command
  - `git checkout -b starter-files-added origin/starter-files-added`

#### Back End Server
The Contacts app project that we're building is a front-end project. However, we'll eventually be storing the contacts on a backend server. Since we're only really focusing on the front-end for this course, we've gone ahead and built this server for you so you can focus on just the React parts of this program.

The server is a simple Node/Express app. The repo for the project is at

- [https://github.com/udacity/reactnd-contacts-server2](https://github.com/udacity/reactnd-contacts-server2).

All you need to do is:

- clone the project with
  - `git clone https://github.com/udacity/reactnd-contacts-server2.git`
- install the project dependencies with
  - `npm install`
- start the server with
  - `node server.js`

Once you've started the server, you can forget about it. The Contacts project we're working on will interact with this server, but we won't ever modify any of the server code.

> #### 💡 Running Two Servers💡
> At this point, you should be running two different servers on your local machine:
>
> - Front-end development server: Accessible on **port 3000**
>   - (`npm start` or `yarn start`)
> - Back-end server: Accessible on **port 5001**
>   - (`node server.js`)
>
> Please be sure that both are running before moving on in this Lesson.

### 3.2 Pass Data with Props
Here we have a function whose whole prupose is to fetch a user. The problem is we need to tell the function whcih user to fetch. This is done by passing a parameter to our function.

[![rf14](../assets/images/rf14-small.jpg)](../assets/images/rf14.jpg)

The same thing holds true for React components. In the same way we pass data to a function, we can pass data to a component.

Here the whole purpose of this component is to display a user to the UI. In order to do this we add a custom attribute to our component and give it a value.

[![rf15](../assets/images/rf15-small.jpg)](../assets/images/rf15.jpg)

Now we can access that value from inside our component definition by using `this.props.username`.

In fact any attributes that are added to a component are accessible inside of that component.

[![rf16](../assets/images/rf16-small.jpg)](../assets/images/rf16.jpg)

Here we'll use this `contacts` array temporarily. Eventually, we'll be grabing this from our backend server.

```js
const contacts = [
 {
   "id": "karen",
   "name": "Karen Isgrigg",
   "handle": "karen_isgrigg",
   "avatarURL": "http://localhost:5001/karen.jpg"
 },
 {
   "id": "richard",
   "name": "Richard Kalehoff",
   "handle": "richardkalehoff",
   "avatarURL": "http://localhost:5001/richard.jpg"
 },
 {
   "id": "tyler",
   "name": "Tyler McGinnis",
   "handle": "tylermcginnis",
   "avatarURL": "http://localhost:5001/tyler.jpg"
 }
];
```

We want to create a new component that will take in this array as `props` and be responsible for looping over that array to show a contact record for each item in the array.

[![rf17](../assets/images/rf17-small.jpg)](../assets/images/rf17.jpg)

Whenever we want to build out a new component we create a new file for it. In this case we'll want to name the file "ListContacts.js" because that's what the component will be responsible for.

#### ListContacts - array as props
We start with our import statement. Then we create our class and extend Component. We'll usually want to immediately jump down to the bottom and do our export statement as well.

```jsx
// ListContacts.js
import React, { Component } from 'react';

class ListContacts extends Component {
  render() {
    return (
      <ol className="contact-list">

      </ol>
    )
  }
}

export default ListContacts;
```

We create the render method and inside our return statement and we start with an ordered list.

Next we need to figure out a way of passing the array from App to our child component.

We first start by importing our ListContacts component. We are then able to use it in our App's render method. We then create a contacts prop and pass in the contacts array.

```jsx
// App.js
import React, { Component } from 'react';
import ListContacts from './ListContacts';

const contacts = [
  {
    id: 'karen',
    name: 'Karen Isgrigg',
    handle: 'karen_isgrigg',
    avatarURL: 'http://localhost:5001/karen.jpg'
  }
  // additional elements...
];

class App extends Component {
  render() {
    return (
      <div>
        <ListContacts contacts={contacts} />
      </div>
    );
  }
}

export default App;
```

We can think of this as we would a function invocation. We can pass data to that function in the same way we're passing data to our component as a prop.

Lastly if we want to test we can console.log the following in ListContacts.js and view this in our Console.

```jsx
class ListContacts extends Component {
  render() {
    console.log('Props', this.props);
    return (
      <ol className="contact-list">

      </ol>
    );
  }
}
```

[![rf18](../assets/images/rf18-small.jpg)](../assets/images/rf18.jpg)

Argument is to function what props is to component.

#### Question 2 of 4
If there were a `<Clock />` component in an app you're building, how would you pass a `currentTime` prop into it?

- [ ] `<Clock {new Date().getTime()} />`
- [ ] `<Clock this.props={new Date().getTime()} />`
- [x] `<Clock currentTime={new Date().getTime()} />`
- [ ] `<Clock this.currentTime={new Date().getTime()} />`

#### ListContacts - map over array

Next we map over the contacts array and create a list item outputting the name for each. We create a key attribute and output the id.

```jsx
class ListContacts extends Component {
  render() {
    return (
      <ol className="contact-list">
        {this.props.contacts.map(contact => (
          <li key={contact.id}>
            {contact.name}
          </li>
        ))}
      </ol>
    );
  }
}
```

Notice that we use a parens instead of curly braces after the ES6 arrow function in order to get the implicit return.

The reason we need to add a key is because eventually one of those list items may change, and by having a unique key attribute on each list item, React is able to perfomantly know which list item has changed and can update just that item rather than having to recreate the entire list every time.

[![rf19](../assets/images/rf19-small.jpg)](../assets/images/rf19.jpg)

#### QUESTION 3 OF 4
Using the `<Clock />` component example:

```jsx
<Clock currentTime='3:41pm' />
```

How would you access the value 3:41pm from inside the component?

- [ ] Clock.currentTime
- [ ] currentTime
- [ ] this.currentTime
- [x] this.props.currentTime

#### ListContacts - display data & inline style
Now instead of just showing the names, we want to fill out the rest of the component.

We start by adding a className to the list item. We then create a div which will be used for the avatar. We'll want to add an inline style to this div so we can pass in the specific image url for this avatar that is contained in our contacts array.

The style attribute has two curly braces. The first tells React that we're using JavaScript and the second is the object that we're passing in with the inline style rules.

Then we create another div to contain paragraph elements for the name and handle.

```jsx
class ListContacts extends Component {
  render() {
    return (
      <ol className="contact-list">
        {this.props.contacts.map(contact => (
          <li key={contact.id} className="contact-list-item">
            <div
              className="contact-avatar"{% raw %}
              style={{
                backgroundImage: `url(${contact.avatarURL})`
              }}{% endraw %}
            />
            <div className="contact-details">
              <p>{contact.name}</p>
              <p>{contact.handle}</p>
            </div>
            <button className="contact-remove">Remove</button>
          </li>
        ))}
      </ol>
    );
  }
}
```

Lastly, we create a button for the Remove action which is not hooked up yet.

[![rf20](../assets/images/rf20-small.jpg)](../assets/images/rf20.jpg)<br>
**Live Demo:** [Contacts App on CodeSandbox](https://codesandbox.io/s/qk7olqz52j)

#### Question 4 of 4
How do you pass multiple props individually to a component?

- [x] `<Clock time={Date.now()} zone='MST' />`{% raw %}
- [ ] `<Clock props={{time: Date.now(), zone: 'MST'}} />`
- [ ] `<Clock [time=Date.now(), zone='MST'] />`
- [ ] `<Clock props={[Date.now(), 'MST']} />`{% endraw %}

#### Passing Data With Props Recap
A prop is any input that you pass to a React component. Just like an HTML attribute, a prop name and value are added to the Component.

```jsx
// passing a prop to a component
<LogoutButton text='Wanna log out?' />
```

In the code above, `text` is the prop and the string 'Wanna log out?' is the value.

All props are stored on the `this.props` object. So to access this `text` prop from inside the component, we'd use `this.props.text`:

```jsx
// access the prop inside the component
...
render() {
 return <div>{this.props.text}</div>
}
...
```

##### Further Research
- [Components and Props](https://reactjs.org/docs/components-and-props.html) from the React Docs

### 3.3 Exercise Prep
#### Workspaces
In this program, you'll be able to practice what you've learned right inside the classroom!

A Workspace is a development environment integrated into the Udacity Classroom. Your Workspace is backed by a Linux virtual machine (Ubuntu). You have access to a terminal, so you have complete control over installing packages and modifying content.

#### Exercises in Workspaces
Each Workspace contains an instructions.md file that contains the instructions for the exercise. Each Workspace also contain a Possible Solution folder located inside of the src folder. We recommend not looking inside the Possible Solution folder until you have finished the exercise on your own. That way, you can practice recalling and applying what you've learned, thereby solidifying your understanding of the material.

[![rf22](../assets/images/rf22-small.jpg)](../assets/images/rf22.jpg)

#### Preservation Information
The first time you open your Workspace, a new virtual machine is created just for you. Any files that you modify in /home/workspace or any new files you add in /home/workspace are automatically backed up and saved. The next time you come back to the Workspace, any previous changes will be preserved.

If you don't interact with the Workspace for 30 minutes, the Workspace will be suspended. The Workspace becomes idle by any of the following:

- not interacting with the browser tab of the Workspace
- closing the browser tab with the Workspace
- if your laptop goes to sleep
- etc

#### Restoring Your Workspace
If your Workspace has been suspended after a period of inactivity, just click the "Wake Up Workspace" button to restore it. Remember that none of your data is lost.

#### Project Development
Think of your Workspace as a normal computer:

- Open up the files you need to edit (saving is done automatically).
- Open terminal windows as necessary.
  - The terminal should start at /home/workspace, so make sure to cd to the correct directory as necessary.
- Start the project
  - start a terminal (no need to cd anywhere)
  - run `npm install`
  0- run `npm start`
- Open the src folder and start working on the exercise.
- To view your project, click the "Open Preview Tab" button located in the lower left of the screen.
  - Running `npm start` causes Create React App to display a URL of http://localhost:3000/. Because your Workspace is running in a virtual machine, typing http://localhost:3000/ into your browser will not access the local host of the VM, so make sure to use the "Open Preview Tab" button.

#### Committing to Github
We strongly recommend committing your files to Github whenever you're working on coding projects. Workspaces provide a convenient way to do that - just use the Workspace Terminal.

To commit files from your Workspace directly to Github:

1. Set up a new Github repository.
2. Use the Workspace terminal to commit files to Github as usually would. If you need a refresher, click [here](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/). Don't forget to add your `node_modules` folder to the `.gitignore` file.

#### Unable to Access Your Workspace?
If you are unable to access your Workspace in the Classroom it could be because you have "3rd Party Cookies" disabled in your browser. Workspaces need to set a "3rd party cookie" to enable access.

Check out this [Workspace troubleshooting FAQ](https://udacity.zendesk.com/hc/en-us/articles/115004653246) for information on how to enable 3rd party cookies for your browser.

### 3.4 Ex 1 - Passing Data
This exercise consisted of the following instructions.

> #### Instructions
> Use React and the `profiles`, `users`, and `movies` data in App.js to display a list of users alongside their favorite movies.
>
> ##### Example
>
> Jane Cruz's favorite movie is Planet Earth 1.

The data looks like this.

```js
// data.js
const profiles = [
  {
    id: 1,
    userID: '1',
    favoriteMovieID: '1',
  },
  // more records...
];

const users = {
  1: {
    id: 1,
    name: 'Jane Cruz',
    userName: 'coder',
  },
  // more records...
};

const movies = {
  1: {
    id: 1,
    name: 'Planet Earth 1',
  },
  // more records...
};

export {profiles, users, movies};
```

#### 3.4 Solution
The entry point is **index.jsx**. It imports our styles.css and App component and then renders that App component.

```jsx
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

import './styles.css';

const rootElement = document.getElementById('root');
ReactDOM.render(<App />, rootElement);
```

App will act as the parent component. It imports **data.js** and passes that data as `props` to the FavoriteMovies component.

```jsx
// App.js
import React, { Component } from 'react';
import './App.css';
import FavoriteMovies from './FavoriteMovies';
import { profiles, users, movies } from './data.js';

class App extends Component {
  render() {
    return (
      <div>
        <header className="App-header">
          <img src="logo.svg" className="App-logo" alt="logo" />
          <h1 className="App-title">ReactND - Coding Practice</h1>
        </header>
        <h2>Favorite Movies</h2>
        <FavoriteMovies profiles={profiles} users={users} movies={movies} />
      </div>
    );
  }
}

export default App;
```

The FavoriteMovies component maps over the props data and gets both user's name and the movie name from the lookup object stores.

```jsx
// FavoriteMovies.js
import React, { Component } from 'react';

class FavoriteMovies extends Component {
  render() {
    const { profiles, users, movies } = this.props;
    return (
      <ol>
        {profiles.map(profile => (
          <li key={profile.id}>
            {users[profile.userID].name}'s favorite movie is{' '}
            {movies[profile.favoriteMovieID].name}
          </li>
        ))}
      </ol>
    );
  }
}

export default FavoriteMovies;
```

Here's the final result.

[![rf21](../assets/images/rf21-small.jpg)](../assets/images/rf21.jpg)<br>
**Live Demo:** [Ex 1 - Passing Data on CodeSandbox](https://codesandbox.io/s/42xj4xq7l4)

### 3.5 Ex 2 - Passing Data
The instructions for this exercise are:

> #### Instructions
> Let's do something a little bit more complicated. Instead of displaying a
list of users and their movies, this time you need to display a list of movies.
>
> For each movie in the list, there are two options:
>
> 1. If the movie has been favorited, then display a list of all of the users who said that this movie was their favorite.
> 2. If the movie has *not* been favorited, display some text stating that no one favorited the movie.
>
> As you go about tackling this project, try to make the app *modular* by breaking it into resusable React components.
>
> ##### Example
>
> ```html
> <h2>Forrest Gump</h2>
> <p>Liked By:</p>
> <ul>
>   <li>Nicholas Lain</li>
> </ul>
>
> <h2>Get Out</h2>
> <p>Liked By:</p>
> <ul>
>   <li>John Doe</li>
>   <li>Autumn Green</li>
> </ul>
>
> <h2>Autumn Green</h2>
> <p>None of the current users liked this movie</p>
> ```

Same data as before.

```js
// data.js
const profiles = [
  {
    id: 1,
    userID: '1',
    favoriteMovieID: '1',
  },
  // more records...
];

const users = {
  1: {
    id: 1,
    name: 'Jane Cruz',
    userName: 'coder',
  },
  // more records...
};

const movies = {
  1: {
    id: 1,
    name: 'Planet Earth 1',
  },
  // more records...
};

export {profiles, users, movies};
```

#### 3.5 Solution
The data is the same as the previous exercise except this time it had some more complexity.

It required one component to loop through the movies (PopularMovies). Then it required a child component (UserList) to display the names of people that favorited that movie.

Within App I just passed the *profiles*, *users*, *movies* as `props` to my FavoriteMovies component.

```jsx
// Appp.js
import React, { Component } from 'react';
import './App.css';
import { profiles, users, movies } from './data.js';
import PopularMovies from './PopularMovies';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src="logo.svg" className="App-logo" alt="logo" />
          <h1 className="App-title">ReactND - Coding Practice</h1>
          <p>Exercise 2 - Passing Data</p>
        </header>
        <main className="App-main">
          <h2>How Popular is Your Favorite Movie?</h2>
          <PopularMovies profiles={profiles} users={users} movies={movies} />
        </main>
      </div>
    );
  }
}

export default App;
```

Next I destructured `props` and created *moviesArr* from the movies object.

```jsx
// PopularMovies.js
import React, { Component } from 'react';
import UserList from './UserList';

class PopularMovies extends Component {
  render() {
    const { profiles, users, movies } = this.props;
    const moviesArr = Object.values(movies);
    return (
      <div className="PopularMovies-container">
        {moviesArr.map(movie => (
          <div key={movie.id} className="PopularMovies-cell">
            <h3>{movie.name}</h3>
            <UserList movieID={movie.id} users={users} profiles={profiles} />
          </div>
        ))}
      </div>
    );
  }
}

export default PopularMovies;
```

*moviesArr* follows this format:

- `[{id: 1, name: 'movie 1'}, {id:2, name: 'movie 2'}, ...]`

Now I can map over the movies to output my movie name and pass the following to my UserList component.

- *movieID*
- *users*
- *profiles*

In UserList I filter the profiles array by returning only those users that have favorited the specific *movieID* we're on.

```jsx
// UserList.js
import React, { Component } from 'react';

class UserList extends Component {
  render() {
    const { movieID, profiles, users } = this.props;
    const filteredProfiles = profiles.filter(
      profile => Number(profile.favoriteMovieID) === movieID
    );

    // console.log(filteredProfiles);
    if (!filteredProfiles || filteredProfiles.length === 0) {
      return <p>None of the current users liked this movie</p>;
    }

    return (
      <div>
        <p>Liked by:</p>
        <ul>
          {filteredProfiles.map(profile => (
            <li key={profile.userID}>{users[profile.userID].name}</li>
          ))}
        </ul>
      </div>
    );
  }
}

export default UserList;
```

Next, I check if no *filteredProfiles* array items exist then I output a message and return.

Otherwise I map over *filteredProfiles* and resolve the *users.name* based on the *profile.userID*.

Lastly I added some styling so it lays out nicely.

[![rf23](../assets/images/rf23-small.jpg)](../assets/images/rf23.jpg)<br>
**Live Demo:** [Ex 2 - Passing Data on CodeSandbox](https://codesandbox.io/s/m3mny1540p)

Here's a diagram of how the various components lay out within the UI.

[![rf26](../assets/images/rf26-small.jpg)](../assets/images/rf26.jpg)<br>
**Live Demo:** [Ex 2 - Passing Data on CodeSandbox](https://codesandbox.io/s/m3mny1540p)

### 3.6 Functional Components
[![rf24](../assets/images/rf24-small.jpg)](../assets/images/rf24.jpg)

Up until this point we've used JavaScript's class syntax with a render() method to build out our components.

```jsx
// class component
class User extends React.Component {
  render() {
    return (
      <p>Username: {this.props.username}</p>
    )
  }
}
```

Eventually we'll be adding more methods to these classes but if all our component has is a render() method then we can use a regular old function to render out our component.

```jsx
// stateless functional component
function Username(props) {
  return (
    <p>Username: {props.username}</p>
  )
}
```

Notice, however, that we're no longer accessing the components props by using the 'this' keyword. Instead, our functional component is being passed its props as the first argument to the function.

#### Question 1 of 2
When is it appropriate to use a Stateless Functional Component? Check all that apply.

- [ ] When the component needs to initialize some data
- [x] When all the component needs is to just render something
- [ ] When the component doesn't have any props passed in
- [ ] When the component does not use JSX

#### Question 2 of 2
If the <IngredientList /> Component in the following code is a Stateless Functional Component, how would you access the items prop inside the Component?

```html
<IngredientList items={ingredient.items} />
```

- props.items

We do away with the 'this' keyword.

#### Refactor ListContacts
Next we are going to refactor our ListContacts component following the rules above.

Here's the component defined as a class.

```jsx
// ListContact.js
import React, { Component } from 'react';

class ListContacts extends Component {
  render() {
    return (
      <ol className="contact-list">
        {this.props.contacts.map(contact => (
          <li key={contact.id} className="contact-list-item">
            <div
              className="contact-avatar"{% raw %}
              style={{
                backgroundImage: `url(${contact.avatarURL})`
              }}{% endraw %}
            />
            <div className="contact-details">
              <p>{contact.name}</p>
              <p>{contact.handle}</p>
            </div>
            <button className="contact-remove">Remove</button>
          </li>
        ))}
      </ol>
    );
  }
}

export default ListContacts;
```

After making our changes we have a slightly simpler component in the form of a stateless functional component.

```jsx
// ListContact.js
import React from 'react';

function ListContacts(props) {
  return (
    <ol className="contact-list">
      {props.contacts.map(contact => (
        <li key={contact.id} className="contact-list-item">
          <div
            className="contact-avatar"{% raw %}
            style={{
              backgroundImage: `url(${contact.avatarURL})`
            }}{% endraw %}
          />
          <div className="contact-details">
            <p>{contact.name}</p>
            <p>{contact.handle}</p>
          </div>
          <button className="contact-remove">Remove</button>
        </li>
      ))}
    </ol>
  );
}

export default ListContacts;
```

This required us to drop the 'this' keyword and simply return the UI code from the function that takes props as it's first argument.

#### Stateless Functional Components Recap
If your component does not keep track of internal state (i.e., all it really has is just a `render()` method), you can declare the component as a Stateless Functional Component.

Remember that at the end of the day, React components are really just JavaScript functions that return HTML for rendering. As such, the following two examples of a simple Email component are equivalent:

```jsx
// class component
class Email extends React.Component {
 render() {
   return (
     <div>
       {this.props.text}
     </div>
   );
 }
}
```

```jsx
// stateless functional component
const Email = (props) => (
 <div>
   {props.text}
 </div>
);
```

In the latter example (written as an ES6 function with an implicit return), rather than accessing `props` from `this.props`, we can pass in props directly as an argument to the function itself. In turn, this regular JavaScript function can serve as the Email component's `render()` method.

##### Further Research
- [Functional Components vs. Stateless Functional Components vs. Stateless Components](https://tylermcginnis.com/functional-components-vs-stateless-functional-components-vs-stateless-components/) from Tyler

### 3.7 Ex - Fn Components
The exercise instructions are.

> #### Instructions
> Modify this app to use Stateless Functional Components. Remember that for
performance reasons, if a component doesn't need to hold state, we'd want to
make it a Stateless Functional Component.
>
> This exercise will help you practice passing data into Stateless Functional Components.

#### App.js

```jsx
// before
class App extends Component {
  render() {
    return (
      <div>
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">ReactND - Coding Practice</h1>
        </header>
        <h1>How Popular is Your Favorite Movie?</h1>
        <MovieCardsList profiles={profiles} movies={movies} users={users} />
      </div>
    );
  }
}
```

```jsx
// after
function App (props) {
  return (
    <div>
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <h1 className="App-title">ReactND - Coding Practice</h1>
      </header>
      <h1>How Popular is Your Favorite Movie?</h1>
      <MovieCardsList profiles={profiles} movies={movies} users={users} />
    </div>
  )
}
```

#### MovieCardsList

```jsx
//before
class MovieCardsList1 extends Component {
  render() {
    const { profiles, users, movies } = this.props;
    const usersByMovie = {};

    profiles.forEach(profile => {
      const movieID = profile.favoriteMovieID;

      if (usersByMovie[movieID]) {
        usersByMovie[movieID].push(profile.userID);
      } else {
        usersByMovie[movieID] = [profile.userID];
      }
    });

    const movieCards = Object.keys(movies).map(id => (
      <MovieCard
        key={id}
        users={users}
        usersWhoLikedMovie={usersByMovie[id]}
        movieInfo={movies[id]}
      />
    ));

    return <ul>{movieCards}</ul>;
  }
}
```

```jsx
// after
function MovieCardsList(props) {
  const { profiles, users, movies } = this.props;
  const usersByMovie = {}; // create empty object

  profiles.forEach(profile => {
    const movieID = profile.favoriteMovieID; // get movieID as key

    if (usersByMovie[movieID]) { // loop thru ea. profile item
      usersByMovie[movieID].push(profile.userID); // push user onto array
    } else { // else if movie key does not exit
      usersByMovie[movieID] = [profile.userID]; // assign user array to key
    }
  });

  const movieCards = Object.keys(movies).map(id => (
    <MovieCard
      key={id}
      users={users}
      usersWhoLikedMovie={usersByMovie[id]}
      movieInfo={movies[id]}
    />
  ));

  return <ul>{movieCards}</ul>;
}
```

#### MovieCard

```jsx
// before
class MovieCard extends Component {
  render() {
    const { users, usersWhoLikedMovie, movieInfo } = this.props;

    return (
      <li key={movieInfo.id}>
        <h2>{movieInfo.name}</h2>
        <h3>Liked By:</h3>

        {!usersWhoLikedMovie || usersWhoLikedMovie.length === 0 ? (
          <p>None of the current users liked this movie.</p>
        ) : (
          <ul>
            {usersWhoLikedMovie &&
              usersWhoLikedMovie.map(userId => {
                return (
                  <li key={userId}>
                    <p>{users[userId].name}</p>
                  </li>
                );
              })}
          </ul>
        )}
      </li>
    );
  }
}
```

```jsx
// after
function MovieCard(props) {
  const { users, usersWhoLikedMovie, movieInfo } = props;

  return (
    <li key={movieInfo.id}>
      <h2>{movieInfo.name}</h2>
      <h3>Liked By:</h3>

      {!usersWhoLikedMovie || usersWhoLikedMovie.length === 0 ? (
        <p>None of the current users liked this movie.</p>
      ) : (
        <ul>
          {usersWhoLikedMovie &&
            usersWhoLikedMovie.map(userId => {
              return (
                <li key={userId}>
                  <p>{users[userId].name}</p>
                </li>
              );
            })}
        </ul>
      )}
    </li>
  );
}
```

Here's a diagram showing the layout of the various components within the UI.

[![rf27](../assets/images/rf27-small.jpg)](../assets/images/rf27.jpg)<br>
**Live Demo:** [Ex - Functional Component on CodeSandbox](https://codesandbox.io/s/p3roynm48q)

### 3.8 Add Component State
Earlier in this Lesson, we learned that `props` refer to attributes from parent components. In the end, props represent "read-only" data that are *immutable*.

A component's state, on the other hand, represents mutable data that ultimately affects what is rendered on the page. State is managed internally by the component itself and is meant to change over time, commonly due to user input (e.g., clicking on a button on the page).

In this section, we'll see how we can encapsulate the complexity of state management to individual components.

#### State
At this point, you've learned about

1. Creating components
2. Composing components together
3. Passing data to components

However, we still haven't talked about what may be the best part of React, state management.

Because of React's component model, we're able to encapsulate the complexity of state management to individual components. This allows us to build a large application by building out a bunch of smaller applications, which are really just components.

To add state to our components, all we need to do is add a state property to our class whose value is an object. This object represents the state of our component.

Each key in the object, represents a distinct piece of state for this component.

[![rf25](../assets/images/rf25-small.jpg)](../assets/images/rf25.jpg)

Now, just as we did with props, we can access the username property on our state by doing `this.state.username`.

What I really love about React is how it allows my brain to separate two important, yet complex concepts.

1. How the component looks
2. The current state of my application

Because of the separation, the UI, or how the application looks, is simply a function of the application state.

With React your two concerns are:

- Which state is in my application
- How does my UI change based off of that state

> #### 💡 Class Fields 💡
> In the code above, we put the state object directly inside the class...not in a constructor() method!
>
> ```jsx
> class User extends React.Component {
>   state = {
>     username: 'Tyler'
>   }
> }
> ```
>
> ...rather than:
>
> ```jsx
> class User extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {
>       username: 'Tyler'
>     };
>   }
> }
> ```
>
> This is slightly different from Facebook's [Setting the Initial State docs](https://reactjs.org/docs/react-without-es6.html#setting-the-initial-state).
>
> Having state outside the `constructor()` means it is a [class field](https://github.com/tc39/proposal-class-fields), which is a proposal for a new change to the language. It currently isn't supported by JavaScript, but thanks to Babel's fantastic powers of transpiling, we can use it!
>
> The benefit of declaring state using the class field syntax is that we have less code to type if we don't need to create the `constructor()` method.

#### Add State to Contacts App
Right now our contacts array is living independently, outside our App component.

```jsx
// App.js
import React, { Component } from 'react';
import ListContacts from './ListContacts';

const contacts = [
  {
    id: 'karen',
    name: 'Karen Isgrigg',
    handle: 'karen_isgrigg',
    avatarURL: 'http://localhost:5001/karen.jpg'
  },
  {
    id: 'richard',
    name: 'Richard Kalehoff',
    handle: 'richardkalehoff',
    avatarURL: 'http://localhost:5001/richard.jpg'
  },
  {
    id: 'tyler',
    name: 'Tyler McGinnis',
    handle: 'tylermcginnis',
    avatarURL: 'http://localhost:5001/tyler.jpg'
  }
];

class App extends Component {
  render() {
    return (
      <div>
        <ListContacts contacts={contacts} />
      </div>
    );
  }
}

export default App;
```

Having this data live in the scope of the App component isn't necessarily bad but the problem is that it's outside of React's visibility.

React will have no knowledge of any changes made to this data such as when we add, edit, or delete records.  

One of React's strengths is state management and by having this data outside of our component we are not utilizing that strength. For that reason we need to move our contacts array into the local state of our App component.

That way when we start modifying our contacts array, React will see those changes and update the UI based on those changes.

What we'll do is add a `state` property to our local component. This takes in a state object and represents the state for the entire App component.

We'll create a `contacts` property on that state object and then copy the value of the contacts array to that property.

```jsx
// App.js
class App extends Component {
  state = {
    contacts: [
      {
        id: 'karen',
        name: 'Karen Isgrigg',
        handle: 'karen_isgrigg',
        avatarURL: 'http://localhost:5001/karen.jpg'
      },
      {
        id: 'richard',
        name: 'Richard Kalehoff',
        handle: 'richardkalehoff',
        avatarURL: 'http://localhost:5001/richard.jpg'
      },
      {
        id: 'tyler',
        name: 'Tyler McGinnis',
        handle: 'tylermcginnis',
        avatarURL: 'http://localhost:5001/tyler.jpg'
      }
    ]
  };
  render() {
    return (
      <div>
        <ListContacts contacts={this.state.contacts} />
      </div>
    );
  }
}
```

Now in order to get access to the local component state we need to access it with `this.state.contacts`.

So, the UI looks the same but what we've done is moved our contacts array from being in the scope of the module to actually being a property on the local component state.

[![rf28](../assets/images/rf28-small.jpg)](../assets/images/rf28.jpg)<br>
**Live Demo:** [Contacts App on CodeSandbox](https://codesandbox.io/s/kjpv2kv2o)

> #### ️⚠️ Props in Initial State ⚠️
> When defining a component's initial state, avoid initializing that state with `props`. This is an error-prone anti-pattern, since state will only be initialized with props when the component is first created.
>
> ```jsx
> this.state = {
>   user: props.user
> }
> ```
>
> In the above example, if props are ever updated, the current state will not change unless the component is "refreshed." Using props to produce a component's initial state also leads to duplication of data, deviating from a dependable "source of truth."

#### Quiz Question
What is true about state in React? Please select all that apply:

- [x] A component's state can be defined at initialization.
- [x] State that is needed by multiple components needs to be lifted up to the closest common ancestor.
- [ ] State should be used when you want to store information that will never change.
- [x] A component can alter it's own internal state.

#### State Recap
By having a component manage its own state, any time there are changes made to that state, React will know and *automatically* make the necessary updates to the page.

This is one of the key benefits of using React to build UI components: when it comes to re-rendering the page, we just have to think about updating state.

- We don't have to keep track of exactly which parts of the page change each time there are updates.
- We don't need to decide how we will efficiently re-render the page.

React compares the previous output and new output, determines what has changed, and makes these decisions for us. This process of determining what has changed in the previous and new outputs is called **Reconciliation**.

#### Further Research
- [Identify Where Your State Should Live](https://reactjs.org/docs/thinking-in-react.html#step-4-identify-where-your-state-should-live)

### 3.9 Updating State
Now that we have state inside of our application, the next step is to figure out how to update it. Your natural intuition might be to update the state directly.


```jsx
// ⚠️ This is wrong ⚠️
this.state.username = 'James'
```

Unfortunately, that's not going to work. The reason is because by mutating the state directly, React will have no idea that the state of your component actually changed.

#### setState()
To solve this problem, React gives you a helper method called `setState()`. There are two ways to use setState.

- setState function - used when new state is based on previous state
- setState object - used when new state doesn't depend on previous state

The first way is by passing setState a function.

```jsx
// setState passing a function
this.setState(prevState => ({ // <- implicit return of an object
  count: prevState.count + 1
}))
```

The function will be passed the previous state as its first argument.
The object returned from this function will be merged with the current state to form the new state of the component.

The second pattern is to pass an object. This object will be merged with the current state to form the new state of the component.

```jsx
// setState passing an object
this.setState({
  username: 'James'
})
```

Typically, you want to use the functional setState when the new state of your component depends on the previous state.

[![rf29](../assets/images/rf29-small.jpg)](../assets/images/rf29.jpg)

For everything else, you can use the object setState. However, I always prefer to use the functional setState.

Regardless of how you use setState, the end result will always be the same: Whenever you invoke setState, React by default is going to re-render your entire application and update the UI.

This is why we say that in React, your UI is just a function of your state. Once your state changes, your UI will automatically update accordingly.

#### Hooking the delete button up to Contact App
Now that our state is living inside of our component, the next step is to hook up the delete buttons so that the specific contact is deleted whenever the button is clicked.

The reason we're able to do this is because our state is living inside of our App component.

#### removeContact() method
We're going to add a method to our App component which is responsible for taking in a specific contact and then deleting that contact from our contacts array.

We do this by creating a `removeContact` method that takes in a `contact` object and calls `setState()` passing in the `currentState`.

We specify the property we're updating (contacts) and set it to the new value. We use filter to remove the contact we want to delete.

```jsx
// using an ES6 arrow function to define the method means we don't
// have to bind 'this' to it in the constructor!
  removeContact = contact => {
    this.setState(currentState => ({
      contacts: currentState.contacts.filter(c => {
        return c.id !== contact.id;
      })
    }));
  };
```

Then to allow our ListContacts component to use this method we must pass it as props so we can hook it up to the delete button. Just as we can pass data as props, we can also pass functions as props as well.

**Once again, the reason the `removeContact()` method is living inside our App component is because that is where our data lives.**

Here is the completed App component.

```jsx
// App.js
class App extends Component {
  state = {
    contacts: [
      {
        id: 'karen',
        name: 'Karen Isgrigg',
        handle: 'karen_isgrigg',
        avatarURL: 'http://localhost:5001/karen.jpg'
      },
      // more records..
     ]
  };
  removeContact = contact => {
    this.setState(currentState => ({
      contacts: currentState.contacts.filter(c => {
        return c.id !== contact.id;
      })
    }));
  };
  render() {
    return (
      <div>
        <ListContacts
          contacts={this.state.contacts}
          onDeleteContact={this.removeContact}
        />
      </div>
    );
  }
}
```

#### onClick method
Next we need hook up our `onDeleteContact` method to the delete button's `onClick` method inside our ListContacts component.

```jsx
// Method 1
<button
  className="contact-remove"
  onClick={() => props.onDeleteContact(contact)}
>
  Remove
</button>
```

One thing to note is that since we need to pass in `contact` as an argument we need to wrap the `onDeleteContact` function call in an arrow function.

One other way of doing this would be to do the following.

```jsx
// Method 2
<button
  className="contact-remove"
  onClick={props.onDeleteContact.bind(null, contact)}
>
  Remove
</button>
```

> #### ⚠️ Avoid arrow functions & .bind() in render() ⚠️
> We generally want to avoid declaring arrow functions or binding in render() for optimal performance.
>
> You can set up [this ESLint rule (jsx-no-bind)](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md) to help alert you to this issue.
>
> See Cory House articles on Medium:
>
> - [React Pattern: Extract Child Components to Avoid Binding](https://medium.freecodecamp.org/react-pattern-extract-child-components-to-avoid-binding-e3ad8310725e)
> - [Why Arrow Functions and bind in React’s Render are Problematic](https://medium.freecodecamp.org/why-arrow-functions-and-bind-in-reacts-render-are-problematic-f1c08b060e36)

This is the updated app with delete contact feature implemented.

[![rf30](../assets/images/rf30-small.jpg)](../assets/images/rf30.jpg)<br>
**Live Demo:** [Contacts App on CodeSandbox](https://codesandbox.io/s/kjpv2kv2o)

#### How State is Set
Earlier in this lesson, we saw how we can define a component's state at the time of initialization. Since state reflects *mutable* information that ultimately affects rendered output, a component may also update its state throughout its lifecycle using `this.setState()`. As we've learned, when local state changes, React will trigger a re-render of the component.

There are two ways to use `setState()`. The first is to merge state updates. Consider a snippet of the following component:

```jsx
class Email extends React.Component {
 state = {
   subject: '',
   message: ''
 }
 // ...
});
```

Though the initial state of this component contains two properties (`subject` and `message`), they can be updated independently. For example:

```jsx
this.setState({
 subject: 'Hello! This is a new subject'
})
```

This way, we can leave `this.state.message` as-is, but replace `this.state.subject` with a new value.

The second way we can use `setState()` is by passing in a function rather than an object. For example:

```jsx
this.setState(prevState => ({
 count: prevState.count + 1
}))
```

Here, the function passed in takes a single `prevState` argument. When a component's new state depends on the previous state (i.e., we are incrementing `count` in the previous state by 1), we want to use the functional `setState()`.

#### Quiz Question
What is true about setting state in our components? Please check all that apply:

- [x] Whenever `setState()` is called, the component also calls `render()` with the new state
- [x] State updates can be merged by passing in an object to `setState()`
- [ ] Updating state directly is ideal when you want to re-render a component (i.e. preferring 'this.state.message = 'hi there';` rather than `this.setState({ message: 'hi there' });`)
- [x] State updates can be asynchronous (i.e. `setState()` can accept a function with the previous state as its first argument)
- [ ] `setState()` should be called within the component's `render()` method

> In the end, your UI is just a function of your state. Being able to leverage React's automatic re-renders when resetting state can give your app's users a truly dynamic experience.

#### setState() Recap
While a component can set its state when it initializes, we expect that state to change over time, usually due to user input. The component is able to change its own internal state using `this.setState()`.

Each time state is changed, React knows and will call `render()` to re-render the component. This allows for fast, efficient updates to your app's UI.

Further Research
[Using State Correctly](https://reactjs.org/docs/state-and-lifecycle.html) from the React Docs

### 3.10 Ex - Managing State
This exercise started with a static page and a set of instructions.

[![rf31](../assets/images/rf31-small.jpg)](../assets/images/rf31.jpg)


> #### Instructions
> Create a game that shows an equation of the form
>
> - **X + Y + Z = P**
>
> Here, X, Y, and Z should be random numbers, and P should be the proposed answer. The user should be able to answer whether it is true that the sum of X, Y, and Z equals the proposed answer P.
>
> The user gets a point for each question the user answers correctly. The score is displayed in this format
>
> - **[*# of correct answers*] / [*# of questions answered*]**
>
> Every time the user answers a question, a new question that uses randomly generated numbers is displayed.
>
> Remember that a Component's constructor is the first thing that runs when the object is created. The render method gets called automatically every time the state changes inside of the component and anytime the value of the component's props changes.
>
> This exercise will help you practice what you've learned in the course so far, including the trickiest part of React - managing state.

The starter app consisted of the following:

```jsx
// App.js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

const value1 = Math.floor(Math.random() * 100);
const value2 = Math.floor(Math.random() * 100);
const value3 = Math.floor(Math.random() * 100);
const proposedAnswer = Math.floor(Math.random() * 3) + value1 + value2 + value3;
const numQuestions = 0;
const numCorrect = 0;

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">ReactND - Coding Practice</h1>
        </header>
        <div className="game">
          <h2>Mental Math</h2>
          <div className="equation">
            <p className="text">
              {`${value1} + ${value2} + ${value3} = ${proposedAnswer}`}
            </p>
          </div>
          <button>True</button>
          <button>False</button>
          <p className="text">
            Your Score: {numCorrect}/{numQuestions}
          </p>
        </div>
      </div>
    );
  }
}

export default App;
```

#### 3.10 Solution
I the divided the game into three separate components.

- App.js
- Game.js
- Score.js

The `numQuestions` and `numCorrect` values are tracked in App's state.

I defined state as a Class Field in order to reduce the amount of code I would have to type if I included it in the constructor.

I've included what the constructor would look like for illustrative purposes.

I've also shown the handlers defined as a Class Method using an arrow function which eliminates us having to bind the handler to 'this' in the constructor.

```jsx
// App.js
import React, { Component } from 'react';
import './App.css';
import Game from './Game.js';
import Score from './Score.js';

class App extends Component {
  /*constructor(props) {
    super(props);
    this.state = {
      numQuestions: 0,
      numCorrect: 0
    }
    this.handleButtonClick = this.handleButtonClick.bind(this);
  }*/
  // state as a Class field means we can avoid defining it in the constructor
  state = {
    numQuestions: 0,
    numCorrect: 0
  };
  /*handleButtonClick(e) {
    // this form means we need to bind 'this' in constructor
    console.log('e.target', e.target.innerText);
    this.setState(currState => ({
      numQuestions: currState.numQuestions + 1
    }))
  }*/
  handleButtonClick = isCorrect => {
    // this form doesn't require us to bind 'this' in constructor
    this.setState(currState => ({
      numQuestions: currState.numQuestions + 1,
      numCorrect: isCorrect ? currState.numCorrect + 1 : currState.numCorrect
    }));
  };
  // this form allows us to avoid using arrow functions & .bind()
  //   in render() when creating lists with .map() (see below...)
  // It is unnecessary here since we are not mapping over multiple <Game />s
  renderGame = game => <Game onButtonClick={this.handleButtonClick} />;

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src="logo.svg" className="App-logo" alt="logo" />
          <h1 className="App-title">ReactND - Coding Practice</h1>
          <p>Exercise - Managing State</p>
        </header>
        <main className="App-main">
          {/*
            // These cause unnecessary re-render of Game components
            <Game onButtonClick={(e) => this.handleButtonClick(e)} />
            <Game onButtonClick={this.handleButtonClick.bind(this)} />
            */}
          <Game onButtonClick={this.handleButtonClick} />
          {this.renderGame()}
          <Score
            numQuestions={this.state.numQuestions}
            numCorrect={this.state.numCorrect}
          />
        </main>
      </div>
    );
  }
}

export default App;
```

There are two articles that show how to avoid unnecessary renders when setting up handler functions.

> #### ⚠️ Avoid arrow functions & .bind() in render() ⚠️
> We generally want to avoid declaring arrow functions or binding in render() for optimal performance.
>
> You can set up [this ESLint rule (jsx-no-bind)](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md) to help alert you to this issue.
>
> See Cory House articles on Medium:
>
> - [React Pattern: Extract Child Components to Avoid Binding](https://medium.freecodecamp.org/react-pattern-extract-child-components-to-avoid-binding-e3ad8310725e)
> - [Why Arrow Functions and bind in React’s Render are Problematic](https://medium.freecodecamp.org/why-arrow-functions-and-bind-in-reacts-render-are-problematic-f1c08b060e36)

Here's the Game component.

```jsx
// Game.js
import React from 'react';

class Game extends React.PureComponent {
  constructor(props) {
    super(props);
    const gameNums = this.generateNums();
    this.state = {
      value1: gameNums[0],
      value2: gameNums[1],
      value3: gameNums[2],
      proposedAnswer: gameNums[3]
    };
  }

  generateNums = () => {
    const value1 = Math.floor(Math.random() * 100);
    const value2 = Math.floor(Math.random() * 100);
    const value3 = Math.floor(Math.random() * 100);
    const proposedAnswer =
      Math.floor(Math.random() * 3) + value1 + value2 + value3;
    return [value1, value2, value3, proposedAnswer];
  };

  updateState = () => {
    const gameNums = this.generateNums();
    this.setState(prevState => ({
      value1: gameNums[0],
      value2: gameNums[1],
      value3: gameNums[2],
      proposedAnswer: gameNums[3]
    }));
  };

  checkAnswer = answer => {
    const isEqual =
      this.state.value1 + this.state.value2 + this.state.value3 ===
      this.state.proposedAnswer;
    return answer === isEqual;
  };

  onButtonClick = e => {
    // console.log('e.target', e.target.innerText);
    this.updateState();
    const answer = Boolean(e.target.innerText === 'False' ? false : true);
    const isCorrect = this.checkAnswer(answer);
    // console.log('isCorrect', isCorrect);
    this.props.onButtonClick(isCorrect);
  };

  render() {
    // const { onButtonClick } = this.props;
    console.log(`Game component rendered`);
    console.log(
      `${this.state.value1 + this.state.value2 + this.state.value3} === ${
        this.state.proposedAnswer
      }`
    );
    return (
      <div className="game">
        <h2>Mental Math</h2>
        <div className="equation">
          <p className="text">{`${this.state.value1} + ${this.state.value2} + ${
            this.state.value3
          } = ${this.state.proposedAnswer}`}</p>
        </div>
        <button onClick={e => this.onButtonClick(e)}>True</button>
        <button onClick={this.onButtonClick}>False</button>
      </div>
    );
  }
}

export default Game;
```

Here's the Score component.

```jsx
// Score.js
import React from 'react';

const Score = props => {
  const { numCorrect, numQuestions } = props;
  return (
    <p className="text">
      Your Score: {numCorrect}/{numQuestions}
    </p>
  );
};

export default Score;
```

Here's a screenshot of the working application.

[![rf32](../assets/images/rf32-small.jpg)](../assets/images/rf32.jpg)<br>
**Live Demo:** [Mental Math App on CodeSandbox](https://codesandbox.io/s/vvx41ykywy)

### 3.11 PropTypes
#### Type checking a Component's Props with PropTypes
As we implement additional features into our app, we may soon find ourselves debugging our components more frequently. For example, what if the props that we pass to our components end up being an unintended data type (e.g. an object instead of an array)?

PropTypes is a package that lets us define the data type we want to see right from the get-go and warn us during development if the prop that's passed to the component doesn't match what is expected.

To use PropTypes in our app, we need to install [prop-types](https://facebook.github.io/react/docs/typechecking-with-proptypes.html):

```bash
npm install --save prop-types
```

Alternatively, if you have been using [yarn](https://www.npmjs.com/package/yarn) to manage packages, feel free to use it as well to install:

```bash
yarn add prop-types
```

Let's jump right in and see how it's used!

#### Add PropTypes to Contacts App
We first start by adding the import to the top of the file we're using it in.

```jsx
import PropTypes from 'prop-types';
```

What it allows us to do is add a property to our ListTypes component that will define the props that this component should receive.

```jsx
// ListContacts.js
ListContacts.propTypes = {
  contacts: PropTypes.array.isRequired,
  onDeleteContact: PropTypes.func.isRequired
}
```

App.js is where we include our ListContacts component. It specifies both contacts and onDeleteContact props.

```jsx
// App.js
  render() {
    return (
      <div>
        <ListContacts
          contacts={this.state.contacts}
          // onDeleteContact={this.removeContact}
        />
      </div>
    );
  }
}
```

If we comment out the second prop we will receive an error message in console telling us exactly what the problem is.

[![rf33](../assets/images/rf33-small.jpg)](../assets/images/rf33.jpg)

It will also notify us if we pass in the wrong datatype.

The best thing about PropTypes is related to 3rd party components.

When we use a component that wasn't created by us we can look at the PropTypes to instantly know what datatype needs to be passed in as props.

#### PropTypes Question
Consider this component:

```jsx
import PropTypes from 'prop-types';

class Email extends React.Component {
  render() {
    return (
      <h3>Message: {this.props.text}</h3>
    );
  }
}

Email.propTypes = {
  text: // ???
};
```

We want to validate that a text prop is indeed being passed in, and that its data type is a string. What should the value of the above object's text key be?

```jsx
Email.propTypes = {
  text: PropTypes.string.isRequired
};
```

#### PropTypes Recap
All in all, PropTypes is a great way to validate intended data types in our React app. Type checking our data with PropTypes helps us identify these bugs during development to ensure a smooth experience for our app's users.

#### Further Research

- [prop-types](https://www.npmjs.com/package/prop-types) library from npm
- [Typechecking With Proptypes](https://facebook.github.io/react/docs/typechecking-with-proptypes.html) from the React Docs

### 3.12 Controlled Components
Typically when you're using forms in a web app, the form state lives inside of the DOM. But as we've talked about, the whole point of React is to more effectively manage state inside of your application.

So, how do we handle forms in React? We can solve this problem with what React calls **controlled components**.

Controlled components are components which render a form, but the "Source of Truth" for the form state lives inside of the component state rather than inside of the DOM.

The reason they're called controlled components, is because React is controlling the state of the form. Here, we have a component which is rendering a form with a single input element.

The first thing to notice is that we've added a value attribute to our input of `this.state.email`.

[![rf34](../assets/images/rf34-small.jpg)](../assets/images/rf34.jpg)

What this means is that the text in the input field is going to be whatever the email property of our component state is. Therefore, the only way to update the state in the input field, is to update the email property of the component state.

[![rf35](../assets/images/rf35-small.jpg)](../assets/images/rf35.jpg)

As you can tell, this is a true controlled component because React is in control of the email property of our state. If we want the input field to change, we can create a `handleChange` method that uses `setState` to update the email address.

[![rf36](../assets/images/rf36-small.jpg)](../assets/images/rf36.jpg)

Whenever the input field is changed, we can call this method by passing it to the `onchange` attribute.

Although controlled components require a little bit more typing, they do have some benefits.

- First, they support instant input validation.
- Second, they allow you to conditionally disable or enable form buttons.
- Third, they enforce input formats.

Now, notice that all of these benefits have to do with updating the UI based on some user input. This is the heart of not only controlled components,
but also React in general.

If the state of our application changes, then our UI updates based on that new state.

#### React Developer Tools
While building React apps, it may be tricky at times to see exactly is going on in your components. After all, with so many props being passed and accessed, numerous nested components, and all the JSX yet to be rendered as HTML, it can be tough to put things into perspective!

**React Developer Tools** allows you to inspect your component hierarchy along with their respective props and states. Once you install the [Chrome extension](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en-US), open the Chrome console and check out the **React** tab. For a detailed overview, feel free to check out the [official documentation](https://github.com/facebook/react-devtools).

Let's see it in action below!

#### Contacts App - Search form as a controlled component
What we're going to do is build out the "search contact" section of our Contacts App. This will allow us to filter our list, which introduces the topic of forms in React.

[![rf38](../assets/images/rf38-small.jpg)](../assets/images/rf38.jpg)

Now we could bypass React and manage our forms directly through the DOM but that doesn't make a whole lot of sense.

React is really good at managing state. So, many times, it makes sense to also put your form state inside of your component state.

What we're going to do is bind our input field to whatever the value of a certain property on our state is. That's going to allow us to update the UI, based on this form data.

So, if I type a "t", you'll notice that we are filtering our list, but we're also showing the search results section as well.

[![rf37](../assets/images/rf37-small.jpg)](../assets/images/rf37.jpg)

A good rule of thumb is, if you want your form data to update the UI in any other way besides just the input field itself, then make your form a controlled component where React is controlling the state of the input field.

If you're not worried about having your UI update based off the form data, then you can go ahead and stick the form somewhere in the DOM and grab it when you need it.

#### Contacts App - Refactoring ListContacts
We're going to be adding state to our ListContacts component because we're going to bind our new search input field to our component state.

This means we refactor from using a stateless functional component to a class component.

In doing this we can move our PropTypes definition into our class by creating a 'static propTypes' property. We'll also need to add 'this' keyword to all instances of 'props'.

```jsx
// ListContacts.js
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class ListContacts extends Component {
  static propTypes = {
    contacts: PropTypes.arrayOf(
      PropTypes.shape({
        id: PropTypes.string.isRequired,
        name: PropTypes.string.isRequired,
        handle: PropTypes.string.isRequired,
        avatarURL: PropTypes.string.isRequired
      })
    ),
    onDeleteContact: PropTypes.func.isRequired
  };
  render() {
    return (
      <ol className="contact-list">
        {this.props.contacts.map(contact => (
          <li key={contact.id} className="contact-list-item">
            <div
              className="contact-avatar"{% raw %}
              style={{
                backgroundImage: `url(${contact.avatarURL})`
              }}{% endraw %}
            />
            <div className="contact-details">
              <p>{contact.name}</p>
              <p>{contact.handle}</p>
            </div>
            <button
              className="contact-remove"
              onClick={() => this.props.onDeleteContact(contact)}
            >
              Remove
            </button>
          </li>
        ))}
      </ol>
    );
  }
}

export default ListContacts;
```

Now our app is back to normal. Next, here's what we are going to do.

- Create a 'state' class field
- Add a 'query' property to our component's state object & set it equal to an empty string
- Wrap our ordered list with a div and set the className to "list-contacts"
- Inside that div we'll create another div and add an input field with the following attributes
  - className of 'search-contacts'
  - a type of 'text'
  - placeholder will be "Search Contacts"
  - value={this.state.query} - this sets the value of the control to our state
  - onChange={this.updateQuery} - this allows us to update state on every keystroke
- Add an 'updateQuery' method which will set state based on the input field value

Whenever the input field changes, we want to update the query property on our state. Doing this updates the value inside of the input field.

Lastly, we add a call to 'JSON.stringify' passing it the state in order to see what our state is.

```jsx
// ListContacts.js
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class ListContacts extends Component {
  static propTypes = {
    contacts: PropTypes.arrayOf(
      PropTypes.shape({
        id: PropTypes.string.isRequired,
        name: PropTypes.string.isRequired,
        handle: PropTypes.string.isRequired,
        avatarURL: PropTypes.string.isRequired
      })
    ),
    onDeleteContact: PropTypes.func.isRequired
  };
  state = {
    query: ''
  };
  updateQuery = e => {
    const query = e.target.value;
    this.setState(() => ({
      query: query.trim()
    }));
  };
  render() {
    return (
      <div className="list-contacts">
        {JSON.stringify(this.state)}
        <div className="list-contacts-top">
          <input
            className="search-contacts"
            type="text"
            placeholder="Search Contacts"
            value={this.state.query}
            onChange={this.updateQuery}
          />
        </div>
        <ol className="contact-list">
          {this.props.contacts.map(contact => (
            <li key={contact.id} className="contact-list-item">
              <div
                className="contact-avatar"{% raw %}
                style={{
                  backgroundImage: `url(${contact.avatarURL})`
                }}{% endraw %}
              />
              <div className="contact-details">
                <p>{contact.name}</p>
                <p>{contact.handle}</p>
              </div>
              <button
                className="contact-remove"
                onClick={() => this.props.onDeleteContact(contact)}
              >
                Remove
              </button>
            </li>
          ))}
        </ol>
      </div>
    );
  }
}

export default ListContacts;
```

Here's what the final outcome looks like.

[![rf39](../assets/images/rf39-small.jpg)](../assets/images/rf39.jpg)<br>
**Live Demo:** [Contacts App on CodeSandbox](https://codesandbox.io/s/vvx41ykywy)

Note that the `value` attribute is set on the `<input>` element. Since the displayed value will always be the value in the component's state, we can treat state as the "single source of truth" for the form's state.

To recap how user input affects the ListContacts component's own state:

1. The user enters text into the input field.
2. The `onChange` event listener invokes the `updateQuery()` function.
3. `updateQuery()` then calls `setState()`, merging in the new state to update the component's internal state.
4. Because its state has changed, the ListContacts component re-renders.

Let's see how we can leverage this updated state to filter our contacts. To help us with our filtering we'll need the following packages:

- [escape-string-regexp](https://www.npmjs.com/package/escape-string-regexp)
- [sort-by](https://www.npmjs.com/package/sort-by)

```bash
npm install --save escape-string-regexp sort-by
```

#### 3.12 Question 1 of 3
What is a Controlled Component?

- [ ] A component which controls the state of its children
- [x] A component which renders a form, but the source of truth  for that form state lives inside of the component state rather than inside the DOM
- [ ] A component which controls the UI for its children components
- [ ] A component which renders a form, but the source of truth for that form state lives inside of DOM rather than inside the component

#### Filter and Sort Contacts
Next we want to filter our list based on whatever we type in the input field.

We start by destructuring both state and props.

```jsx
  render() {
    const { query } = this.state;
    const { contacts, onDeleteContact } = this.props;
```

We then go through and update each reference to remove 'this.state' and 'this.props'

Next we want to filter our contacts based on whatever is in the input field.

We convert to lowercase and assign the result to a new variable call showingContacts. We then use this new variable in our .map() method.

```jsx
    const showingContacts = contacts.filter(contact =>
      contact.name.toLowerCase().includes(query.toLowerCase())
    );
```

Here is the entire code.

```jsx
// ListContacts.js
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class ListContacts extends Component {
  static propTypes = {
    contacts: PropTypes.arrayOf(
      PropTypes.shape({
        id: PropTypes.string.isRequired,
        name: PropTypes.string.isRequired,
        handle: PropTypes.string.isRequired,
        avatarURL: PropTypes.string.isRequired
      })
    ),
    onDeleteContact: PropTypes.func.isRequired
  };
  state = {
    query: ''
  };
  updateQuery = e => {
    const query = e.target.value;
    this.setState(() => ({
      query: query.trim()
    }));
  };
  render() {
    const { query } = this.state;
    const { contacts, onDeleteContact } = this.props;

    const showingContacts = contacts.filter(contact =>
      contact.name.toLowerCase().includes(query.toLowerCase())
    );

    return (
      <div className="list-contacts">
        <div className="list-contacts-top">
          <input
            className="search-contacts"
            type="text"
            placeholder="Search Contacts"
            value={query}
            onChange={this.updateQuery}
          />
        </div>
        <ol className="contact-list">
          {showingContacts.map(contact => (
            <li key={contact.id} className="contact-list-item">
              <div
                className="contact-avatar"{% raw %}
                style={{
                  backgroundImage: `url(${contact.avatarURL})`
                }}{% endraw %}
              />
              <div className="contact-details">
                <p>{contact.name}</p>
                <p>{contact.handle}</p>
              </div>
              <button
                className="contact-remove"
                onClick={() => onDeleteContact(contact)}
              >
                Remove
              </button>
            </li>
          ))}
        </ol>
      </div>
    );
  }
}

export default ListContacts;
```


So again, because we want to update the UI, we stick the input field on
the component state. Whenever the component state changes that causes a re-render which then filters our contacts based on whatever the query is.

[![rf40](../assets/images/rf40-small.jpg)](../assets/images/rf40.jpg)<br>
**Live Demo:** [Contacts App on CodeSandbox](https://codesandbox.io/s/vvx41ykywy)

#### 3.12 Question 2 of 3
Which benefit applies to Controlled Components that doesn't apply to "uncontrolled" components?

- [ ] Controlled Components are more the "React way" of doing things
- [ ] Controlled Components are less typing
- [ ] Controlled Components are more performant
- [x] Controlled Components allow you to update your UI based on teh form itself

#### Showing The Displayed Contacts Count
We're almost done working with the controlled component! Our last step is to make our app display the count of how many contacts are being displayed out of the overall total.

We add the following after our search input.

```jsx
        {showingContacts.length !== contacts.length && (
          <div className="showing-contacts">
            <span>
              Now showing {showingContacts.length} of {contacts.length}
            </span>
            <button onClick={this.clearQuery}>Show all</button>
          </div>
        )}
```

We also add the clearQuery method above render.

```jsx
  clearQuery = () => {
    this.setState(() => ({
      query: ''
    }));
  };
```

Here's the completed code.

```jsx
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class ListContacts extends Component {
  static propTypes = {
    contacts: PropTypes.arrayOf(
      PropTypes.shape({
        id: PropTypes.string.isRequired,
        name: PropTypes.string.isRequired,
        handle: PropTypes.string.isRequired,
        avatarURL: PropTypes.string.isRequired
      })
    ),
    onDeleteContact: PropTypes.func.isRequired
  };
  state = {
    query: ''
  };
  updateQuery = e => {
    const query = e.target.value;
    this.setState(() => ({
      query: query.trim()
    }));
  };
  clearQuery = () => {
    this.setState(() => ({
      query: ''
    }));
  };
  render() {
    const { query } = this.state;
    const { contacts, onDeleteContact } = this.props;

    const showingContacts = contacts.filter(contact =>
      contact.name.toLowerCase().includes(query.toLowerCase())
    );

    return (
      <div className="list-contacts">
        <div className="list-contacts-top">
          <input
            className="search-contacts"
            type="text"
            placeholder="Search Contacts"
            value={query}
            onChange={this.updateQuery}
          />
        </div>
        {showingContacts.length !== contacts.length && (
          <div className="showing-contacts">
            <span>
              Now showing {showingContacts.length} of {contacts.length}
            </span>
            <button onClick={this.clearQuery}>Show all</button>
          </div>
        )}
        <ol className="contact-list">
          {showingContacts.map(contact => (
            <li key={contact.id} className="contact-list-item">
              <div
                className="contact-avatar"{% raw %}
                style={{
                  backgroundImage: `url(${contact.avatarURL})`
                }}{% endraw %}
              />
              <div className="contact-details">
                <p>{contact.name}</p>
                <p>{contact.handle}</p>
              </div>
              <button
                className="contact-remove"
                onClick={() => onDeleteContact(contact)}
              >
                Remove
              </button>
            </li>
          ))}
        </ol>
      </div>
    );
  }
}

export default ListContacts;
```

[![rf41](../assets/images/rf41-small.jpg)](../assets/images/rf41.jpg)<br>
**Live Demo:** [Contacts App on CodeSandbox](https://codesandbox.io/s/vvx41ykywy)

Here's the [documentation on Controlled Components](https://reactjs.org/docs/forms.html#controlled-components) which shows additional examples.

#### 3.12 Question 3 of 3
Which of the following is true about Controlled Components? Please check all that apply:

- [x] Each update to the state has an associated handler function
- [x] Form elements receive their current value via an attribute
- [x] Form input values are generally stored in the component's state
- [ ] `<textarea>` and `<select>` cannot be controlled elements
- [x] Event handlers for a controlled element update the component's state

#### Controlled Components Recap
Controlled components refer to components that render a form, but the "source of truth" for that form state lives inside of the component state rather than inside of the DOM. The benefits of Controlled Components are:

- instant input validation
- conditionally disable/enable buttons
- enforce input formats

In our ListContacts component, not only does the component render a form, but it also controls what happens in that form based on user input.

In this case, event handlers update the component's state with the user's search query. And as we've learned: any changes to React state will cause a re-render on the page, effectively displaying our live search results.