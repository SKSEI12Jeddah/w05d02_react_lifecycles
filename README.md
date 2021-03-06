
## ![](https://s3.amazonaws.com/python-ga/images/GA_Cog_Medium_White_RGB.png)
<h1>The React Component Life Cycle</h1>

---

## The React Component Life Cycle

![](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/01/1_8WLidxhDJZR68Jy0giN5DQ.png?w=1600&ssl=1)
The life cycle comprises three main stages:

1. When the component is being created (known as **mounting**).

2. When the component is being **updated**.

3. When the component is being removed from the DOM (known as **unmounting**).

<aside class="notes">

You can think about this like: 

- Mounting – Birth of your component
- Update – Growth of your component
- Unmount – Death of your component

</aside>

---

## The React Component Life Cycle

These methods are called at specific points in the rendering process. You can use them to perform actions based on what's happening on the DOM.

* `componentDidMount()`, for example, is called immediately *after* a component is rendered to the DOM.
* `componentWillUnmount()` is called immediately *before* a component is removed from the DOM.



---

## Mounting, Updating, and Unmounting

* **Initializing/mounting**: For example, what happens when the component is created and inserted into the DOM? Was an initial state set? Methods are:
  - `constructor()`
  - `static getDerivedStateFromProps()`
  - `render()`
  - `componentDidMount()`


* **Updating**: For example, did an event happen that changed the state? What happens when a component is being re-rendered? Methods are:
  - `static getDerivedStateFromProps()`
  - `shouldComponentUpdate()`
  - `render()`
  - `getSnapshotBeforeUpdate()`
  - `componentDidUpdate()`


* **Destruction/unmounting**: For example, what needs to happen when we're done with the component? Method is:
  - `componentWillUnmount()`



---

## Let's Examine Each Category

![Lifecycles](https://i2.wp.com/programmingwithmosh.com/wp-content/uploads/2018/10/Screen-Shot-2018-10-31-at-1.44.28-PM.png?ssl=1)


<aside class="notes">

**Talking Points**:

- Here they are in diagram form.

- Again, you don't need to write these methods — they happen automatically, just like constructors did before we explicitly wrote one. You only have to worry about these if you want to change something in them. But, if you do, it's important to understand them!
- Note that `getDerivedStateFromProps()`, `shouldComponentUpdate()`, and `getSnapshotBeforeUpdate()` are not included in this diagram because they are less commonly leveraged.

</aside>


---

## The `constructor()` Method

```javascript
constructor(props) {
  super(props);
}
```

<aside class="notes">

- This is in the first part of the component life cycle: **initializing/mounting**. Like any JavaScript class, the `constructor()` method is called when a component is instantiated (i.e., when it's first rendered). We've already used these, but let's take a look in more detail.

- In a class constructor, you must call `super()` before you do anything else. So, a React component `constructor()` in its most basic form looks as is shown here.

- Even though you may see the constructor syntax used in some online resources, the best practices for how to write the initial state for components have changed in recent years. Instead, we can create a component and directly define the starting state without ever having to use a constructor as of React 16. The following will be a detailed guide of the older syntax as it will be likely that legacy React application might still use this syntax. Being familiar with both syntaxes is important.

</aside>

---

## The `constructor()` Method

```javascript
constructor(props) {
  super(props)
  this.state = {
    fruits: props.fruits
  }
}
```

<aside class="notes">



- You don't need to define a constructor if that's all it does, however. This happens automatically when your component is invoked. A common use of the constructor is to initialize state using the props passed to the component, as we've been doing.

- This constructor sets the initial `fruits` state of the component to the `fruits` prop passed to the component.


</aside>

## React v16

```javascript
state = {
    fruits: this.props.fruits
  }
```

<aside class="notes">

**Talking Points**:

- Calling state this way is best practice for new React project. React will automatically set the state and handle the constructor behind the scene.

- Note that constructor is no longer used. To get access to props passed down from the parent, you can access it through `this.props`.  Another thing React does behind the scene to make cleaner code!

</aside>

---

## The `constructor()` Method

Another common use of the `constructor()` method is to bind class methods.

For example, we could set:
```js
this.myFunctionName = this.myFunctionName.bind(this)
```

In JavaScript classes, methods aren't bound by default. So, if you pass a component's method to a child component _without_ binding it, its context will be bound to the child and not the parent, causing it to behave in a way other than how you intended.

Here's an example:

```javascript
class FruitTable extends Component {
  constructor(props) {
    super(props)
    this.state = {
      fruits: props.fruits
    };
    this.addFruit = this.addFruit.bind(this);
  }

  addFruit(fruit) {
    this.setState(prevState => ({
      fruits: prevState.fruits.concat(fruit)
    }))
  }
}
```

<aside class="notes">

**Talking Points**:

- Notice that, in the constructor, `this.addFruit` is bound to the class:
`this.addFruit = this.addFruit.bind(this);`

- Now, if we pass `this.addFruit` to a child component as an `onChange` callback, it will be bound to `FruitTable` and will update its state when it's invoked.

- You should not call `setState()` in the constructor if you need local state. Instead, assign the initial state to `this.state` directly in the constructor.

- You only need a constructor for two purposes: 1. Initializing local state by assigning an object to `this.state`. 2. Binding event handler methods to an instance.

</aside>

## React v16

```javascript
class FruitTable extends Component {
  state = {
      fruits: props.fruits
    }
  }

  addFruit = (fruit) => {
    this.setState(prevState => ({
      fruits: prevState.fruits.concat(fruit)
    }))
  }
}
```

<aside class="notes">

**Talking Points**:

- Using the new arrow function, we can bind the `this` context during the method definition. It will behave the same way as described above with the `constructor()` and `bind()`.

</aside>

---

## The `static getDerivedStateFromProps()` and `getSnapshotBeforeUpdate()` Methods


`getDerivedStateFromProps()` is invoked right before calling the `render()` method. It should return an object to update the state or null to update nothing.

`getSnapshotBeforeUpdate()` is invoked right before the most recently rendered output is committed (e.g., to the DOM).

_Note:_: These methods exists for *rare* use cases in which the state depends on changes in props over time (`getDerivedStateFromProps()`) or if you need to handle something such as scroll position before an update (`getSnapshotBeforeUpdate()`). It's a best practice to explore other lifecycle methods before using these.


<aside class="notes">

**Talking Points**:

- These methods are not as commonly used and can make your code overly verbose and your components overly complicated.
- Alternatives: If you want to perform a supplementary effect in response to props, use `componenentDidUpdate()`, which we will discuss later.
- Alternatives: Discuss memoization helpers for re-computing data when a prop changes.
- Alternatives: To reset its state on prop change, make the component fully controlled or uncontrolled with a key instead of using this method.

</aside>

---
![](https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/41e1b92f-a828-421d-abea-273706d40b1e/d5b0d65-a7cb14b9-7651-4197-b261-973055d091ac.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzQxZTFiOTJmLWE4MjgtNDIxZC1hYmVhLTI3MzcwNmQ0MGIxZVwvZDViMGQ2NS1hN2NiMTRiOS03NjUxLTQxOTctYjI2MS05NzMwNTVkMDkxYWMuanBnIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.L12wXwqf2Xcf82XJTpbxanoAXr1r_UyDhUAZQ54RM0k)

## The `componentDidMount()` and `componentWillUnmount()` Methods

The `componentDidMount` method is called once, immediately after your component is rendered to the DOM. If you want to make an AJAX request when your component first renders, do so here (_not_ in the constructor or in `componentWillMount()`).

In the following example, we fetch data from the server, then set the state of the component using the response:

```javascript
componentDidMount() {
  fetch(`http://api.com/${this.props.id}`)
    .then(response => response.json())
    .then(data => this.setState(prevState => ({ data })));
}
```

---

## The `componentDidMount()` and `componentWillUnmount()` Methods


```javascript
class FruitTable extends Component {
  componentDidMount() {
    document.addEventListener('dragover', this.handleDragStart)
  }

  componentWillUnmount() {
    document.removeEventListener('dragover', this.handleDragStart)
  }

  handleDragStart = (e) => {
    e.preventDefault()
    this.setState({ dragging: true});
  }
}
```

<aside class="notes">

**Talking Points**:

- Another common use for `componentDidMount()` is to bind event listeners to your component. You can then remove the event listeners in `componentWillUnmount()`, which is the only method in the last part of the component life cycle (when the component is being removed from the DOM). In the example seen on this slide, we bind and unbind an event listener for a drag-drop component. sNote that memory issues will arise when using event listeners without unbinding them after use!

- You may call `setState()` immediately in `componentDidMount()`. It will trigger an extra rendering but will happen before the browser updates the screen.

</aside>

---

## The `render()` Method

This is the one method that every React class component **must** have. It is called in two parts of the component life cycle: At the beginning, when the component is being initiated/mounted, and when the component is being updated.

- In `render()`, you return JSX using `this.props` and `this.state`.

The following component renders a single `prop` and a single `state` key: A car model and a speed. Once this component is mounted, its `speed` state will increase by `1` every second. You can try it out yourself in [this CodePen](https://codepen.io/SuperTernary/pen/zzMPGp).

```javascript
class Car extends Component {

  constructor(props) {
    super(props)

    // Sets initial state to either the speed prop, or 0 if the speed prop
    // is not passed to the component.
    this.state = {
      speed: this.props.speed || 0
    }

    // Binds this.incrementSpeed to class.
    // This way, when it's used as a callback for setTimeout,
    // `this` refers to the Car class.
    this.incrementSpeed = this.incrementSpeed.bind(this);
  }

  componentDidMount() {
    // Calls this.incrementSpeed after one second.
    window.setTimeout(this.incrementSpeed, 1000);
  }

  incrementSpeed() {
    // Sets the speed state to one unit higher than it was previously.
    this.setState(prevState => ({ speed: prevState.speed + 1 }));

    // Recursive method.
    // Increases speed by 1 again after one second.
    window.setTimeout(this.incrementSpeed, 1000);
  }

  render() {
    return (
      <div>
        This {this.props.model} is going {this.state.speed} miles per hour!
      </div>
    )
  }

}
```
---


## Let's break that down:

- `constructor()`

The initial state is set to either the `speed` prop, or `0` if the `speed` prop is not passed to the component.

We bind `this.incrementSpeed` to the class, so that when it's used as a callback for `setTimeout`, `this` refers to the `Car` class.

- `componentDidMount()`

Use [`window.setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) to call `this.incrementSpeed` after one second (1,000 ms).

- `incrementSpeed()`

`this.setState(prevState => ({ speed: prevState.speed + 1 }))`: The `speed` state is set to one higher than it was previously — we add `1`.

`window.setTimeout(this.incrementSpeed, 1000)`: The `incrementSpeed` method is [recursive](https://en.wikipedia.org/wiki/Recursion_computer_science) — it invokes itself as the timeout callback. After one second, `window.setTimeout` will call `this.incrementSpeed` again. The `speed` will increase by `1`, and a new timer will be set to do it again.


<aside class="notes">

## React v16

```javascript
class Car extends Component {
  // Sets initial state to either the speed prop, or 0 if the speed prop
  // is not passed to the component.
  state = {
    speed: props.speed || 0
  }

  componentDidMount() {
    // Calls this.incrementSpeed after one second.
    window.setTimeout(this.incrementSpeed, 1000);
  }

  // Binds this.incrementSpeed to class using the arrow syntax.
  // This way, when it's used as a callback for setTimeout,
  // `this` refers to the Car class.
  incrementSpeed = () => {
    // Sets the speed state to one unit higher than it was previously.
    this.setState(prevState => ({ speed: prevState.speed + 1 }));

    // Recursive method.
    // Increases speed by 1 again after one second.
    window.setTimeout(this.incrementSpeed, 1000);
  }

  render() {
    return (
      <div>
        This {this.props.model} is going {this.state.speed} miles per hour!
      </div>
    )
  }
}
```

<aside class="notes">
**Talking Points**:

- You should never set `state` in `render()` — `render()` should only react to changes in state or props, not create those changes.
- `render()` should be a "pure" function. It does not modify component state, it returns the same result every time it’s invoked, and it does not directly interact with the browser.

</aside>

---

## The `shouldComponentUpdate()` Method

`shouldComponentUpdate()` lets React know if a component’s output is *not* affected by the current change in state or props. The default returns `true` and re-renders on every state change.

If `shouldComponentUpdate()` returns `false`, then `render()` and `componentDidUpdate()` will not be invoked.

```javascript
shouldComponentUpdate(nextProps, nextState)
```


<aside class="notes">

**Talking Points**:

- In the vast majority of cases, you should rely on the default behavior to re-render on every state change and will not need to invoke `shouldComponentUpdate()`.
- `shouldComponentUpdate()` only exists as a performance optimization, and you shouldn't rely on it to "prevent" a re-rendering.


</aside>

---

## The `componentDidUpdate()` Method

```javascript
componentDidUpdate(prevProps, prevState, snapshot)

// Example expanding our previous render() example:

componentDidUpdate(prevProps) {
  // Don't forget to compare props!
  if (this.props.model !== prevProps.model) {
    this.fetchData(this.props.model);
  }
}

```



<aside class="notes">

**Talking Points**:

- `componentDidUpdate()` is invoked immediately after updating occurs and is a good place to operate on the DOM when the component has been updated.
- This is also a good place to do network requests as long as you compare the current props to previous props.
- This method is not called for the initial render.
- You may call `setState()` immediately in `componentDidUpdate()`, but it must be wrapped in a condition, or you’ll cause an infinite loop and a re-rendering.
- The re-rendering may not be visible to the user, but it can kill your performance!
- As mentioned before, `componentDidUpdate()` will not be invoked if `shouldComponentUpdate()` returns `false`.

</aside>

---


## Summary

React class components have life-cycle methods that are invoked at certain stages of their "lives" on the DOM. Some of the life-cycle methods you'll use frequently include:

- `constructor()`: Initializes state, binds methods.
- `componentDidMount()`: Makes AJAX requests, gets DOM refs, binds event listeners, sets `state` if necessary.
- `componentWillUnmount()`: Unbinds event listeners, performs other clean-up.
- `componentDidUpdate()`: Updates `state` based on changes in components.
- `render()`: Returns markup/UI.
