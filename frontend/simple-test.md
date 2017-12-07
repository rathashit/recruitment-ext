### React Component Code Review

- What does the following component do?
- Are there any problems with it? If so, what are they?
- Are there any improvements/changes you would make?

```jsx
import React from 'react'

class SomeListComponent extends React.Component {
  constructor (props) {
    this.state = { items: props.items }
  }

  shouldComponentUpdate (nextProps) {
    return nextProps.items !== this.props.items
  }

  handleClick (index) {
    this.props.onClick(index)
  }

  renderElement (item, index) {
    return <li onClick={() => this.handleClick(index)}>{item.text}</li>
  }

  render () {
    return (
      <ul style={{ backgroundColor: 'red', height: 100 }}>
        {this.state.items.map((item, i) => this.renderElement(item, i))}
      </ul>
    )
  }
}

export default SomeListComponent
```

### Answers
What does the following component do?
- This component prints a list of items that are passed from the parent component. On click of any of the item, the index of the item is passed through the onClick method that has been passed to this component by its parent component.

Are there any problems with it? If so, what are they?
- The state has the list of elements that is to be displayed in this component but when new set of elements are received by this component, the state is not updated so new list won't be rendered.
- Inside constructor there is no super call, so the props cannot be used in the constructor.
- The li elements don't have key so react won't be able to identify them uniquely.
- In my opinion shouldComponentUpdate is unnecessary as the component won't update unless the props or state changes.

Are there any improvements/changes you would make?
- My version

```jsx
import React from 'react'

class SomeListComponent extends React.Component {
  constructor (props) {
    super(props)

    this.state = { items: props.items }
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.items !== this.props.items)
      this.setState({ items: nextProps.items })
  }

  handleClick => index { this.props.onClick(index) }

  renderElement = (item, index) => {
    return <li key={index} onClick={() => this.handleClick(index)}>{item.text}</li>
  }

  render () {
    return (
      <ul style={{ backgroundColor: 'red', height: 100 }}>
        {this.state.items.map((item, i) => this.renderElement(item, i))}
      </ul>
    )
  }
}

export default SomeListComponent
```
