# Radial React Project Style Guide

## Contents

* [Javascript](#javascript)
* [Common packages](#common-packages)
* [Directories and files](#directories-and-files)
* [Imports](#imports)
* [JSX](#jsx)
* [Whitespace](#whitespace)
* [Methods](#methods)
* [Common Patterns](#common-patterns)

## Javascript

* For general Javascript syntax see [this styleguide](https://github.com/airbnb/javascript) except for the following cases:

* Always use parenthesis around single parameter arrow functions.
  ```js
  // good
  cars.filter((car) => car.color == 'red');

  // bad
  cars.filter(car => car.color == 'red');
  ```

* Object whitespace
  ```js
  // good
  const car = {color: 'red', model: 'Saturn'};

  // bad
  const car = { color: 'red', model: 'Saturn' };
  // bad
  const car = {color:'red',model:'Saturn'};
  ```


## Common packages

## Directories and files

### Directory structure

### Filenames

* Use camelCase that matches the default export (this makes it easier to edit in a text editor)

## Imports

* Import modules in order of more general to more specific:
  - Utility libraries (e.g. lodash)
  - Frameworks (e.g. Redux)
  - Utility functions/constants
  - Actions
  - Components

* When there are more than 5 import statements, add blank lines between groups.

  ```js
  import { connect } from 'react-redux';
  import { Link } from 'react-router-dom';
  import { Card, Button } from 'semantic-ui-react';
  import { fetchUsers } from 'actions/users';
  import searchBy from 'utils/search';

  import Search from 'components/helpers/Search';
  import Page from 'components/layouts/Page';
  ```

## JSX

JSX leverages syntactic similarity with HTML so similar conventions should be observed.

* rendering
  ```js
  // good
  render() {
    return <Button text="Save" onChange={this.save}/>;
  }
  // good
  render() {
    return (
      <div className="action-bar">
        <Button text="Save" onChange={this.save}/>
        <Button text="Cancel" onChange={this.goBack}/>
      </div>
    );
  }

  // bad
  render() {
    return <div className="action-bar">
      <Button text="Save" onChange={this.save}/>
      <Button text="Cancel" onChange={this.goBack}/>
    </div>;
  }
  ```

* use double quotes for attributes
  ```js
  // good
  <Button text="Save" onChange={this.save}/>

  // bad
  <Button text='Save' onChange={this.save}/>
  ```

* Do not use braces around double quotes
  ```js
  // good
  <Button text="Save" onChange={this.save}/>

  // bad
  <Button text={"Save"} onChange={this.save}/>
  ```

* Do not use spaces around =
  ```js
  // good
  <Button text="Save" onChange={this.save}/>

  // bad
  <Button text = "Save" onChange = {this.save}/>
  ```

* Inline short components
  ```js
  // good
  <Button text="Save" onChange={this.save}/>

  // bad
  <Button
    text="Save"
    onChange={this.save}/>
  ```

* Multiline each prop on it's own line
  ```js
  // good
  <Input
    type="email"
    label="Email"
    placeholder="name@example.com"
    onChange={this.setValue('email')}
    onClick={this.selectText}/>

  // bad
  <Input
    type="email" label="Email" placeholder="name@example.com"
    onChange={this.setValue('email')} onClick={this.selectText}/>
  ```

* Keys should come first and may be on the first line when multilined.
  ```js
  // ok
  <ul>
    {cars.map((car) => (
      <Car key={car.id}
        car={car}
        detail="list"
        showPrice={true}/>
    ))}
  </ul>

  // bad
  <ul>
    {cars.map((car) => (
      <Car
        car={car}
        detail="list"
        key={car.id}
        showPrice={true}/>
    ))}
  </ul>
  ```

* Multiline contents if there are nested elements.
  ```js
  // good
  <p>Hi {name}, welcome to the site!</p>
  // good
  <p>
    <i className="icon-checkmark"/>
    <strong>Hi {name}</strong>, welcome to the site!
  </p>

  // bad
  <p><i className="icon-checkmark"/><strong>Hi {name}</strong>, welcome to the site!</p>
  ```

* Extract components, don't have multiple rendering functions. This will make the class shorter and more readable and make it easier to extract and reuse common components.
  ```js
  // good
  const ErrorMessage = (message) => {
    if (!message) return null;
    return <div className="error"><i className="icon-alert"/>{message}</div>;
  }

  class MyComponent extends React.Component {
    render() {
      const {cars, error} = this.props;
      return (
        <div>
          <ErrorMessage message={error}/>
          {cars.map((car) => (
            <Car key={car.id} car={car}/>
          ))}
        </div>
      );
    }
  }

  // bad
  class MyComponent extends React.Component {
    renderErrorMessage() {
      if (!this.props.error) return null;
      return <div className="error"><i className="icon-alert"/>{this.props.error}</div>;
    }

    showCars() {
      return this.props.cars.map((car) => (
        <Car key={car.id} car={car}/>
      ));
    }

    render() {
      return (
        <div>
          {this.renderErrorMessage()}
          {this.showCars()}
        </div>
      );
    }
  }
  ```

* No space before `/>`
  ```js
  // good
  <Button text="Save" onChange={this.save}/>

  // bad
  <Button text="Save" onChange={this.save} />
  ```

* Closing `>` on its own line
  ```js
  // good
  <Modal
    title="Confirm Changes"
    open={open}
    onClose={this.closeModal}
  >
    <p>This action cannot be undone</p>
    <Button text="Save" onChange={this.save}/>
  </Modal>

  // bad
  <Modal
    title="Confirm Changes"
    open={open}
    onClose={this.closeModal}>
    <p>This action cannot be undone</p>
    <Button text="Save" onChange={this.save}/>
  </Modal>
  ```
* Self closing
  ```js
  // good
  <Input
    type="email"
    label="Email"
    placeholder="name@example.com"
    onChange={this.setValue('email')}
    onClick={this.selectText}/>

  // bad
  <Input
    type="email"
    label="Email"
    placeholder="name@example.com"
    onChange={this.setValue('email')}
    onClick={this.selectText}
  />
  ```

* Conditionally displayed elements should use the `&&` pattern. If the condition is not trivial it should be extracted.
  ```js
  <div>
    {error && <div className="error">{error}</div>}
  </div>
  ```

* Inline Map functions should use implicit return and parenthesis instead of explicit return and braces where possible.
  ```js
  // good
  <ul>
    {cars.map((car) => (
      <li>{car.color}</li>
    ))}
  </ul>

  // bad
  <ul>
    {cars.map((car) => {
      return <li>{car.color}</li>;
    })}
  </ul>
  ```

## Whitespace

* Object whitespace
  ```js
  // good
  const car = {color: 'red', model: 'Saturn'};

  // bad
  const car = { color: 'red', model: 'Saturn' };
  // bad
  const car = {color:'red',model:'Saturn'};
  ```

* Import whitespace
  ```js
  // good
  import { Card, Button } from 'semantic-ui-react';

  // bad
  import {Card, Button} from 'semantic-ui-react';
  ```

* Destructuring
  ```js
  // good
  selectItem = ({target:{value:selectedItem=0}}) => this.setState({selectedItem})

  // bad
  selectItem = ({target: {value: selectedItem = 0}}) => this.setState({selectedItem})
  // bad
  selectItem = (e) => this.setState({selectedItem: e.target.value || 0})
  ```

* Collapse closing symbols
  ```js
  // good
  cars.filter((car) => {
    return car.color == 'red';
  });

  // less good
  cars.filter(
    (car) => {
      return car.color == 'red';
    }
  );
  ```

## Methods

* React component methods should be in [lifecycle order](https://reactjs.org/docs/react-component.html#the-component-lifecycle) with the exception of `render()` which should always be the last method.

* Unless a constuctor is necessary, define initial state in the class scope
  ```js
  // good
  class MyComponent extends React.Component {
    state = {
      active: true
    }
    ///...
  }

  // less good
  class MyComponent extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        active: true
      };
    }
    ///...
  }
  ```
* If a method does not call another class method (such as `setState()`) extract it outside the class.
  ```js
  // good
  const connect = () => {
    API.connect();
  }

  class MyComponent extends React.Component {
    render() {
      return <button onClick={connect}>Connect</button>;
    }
  }

  // bad
  class MyComponent extends React.Component {
    connect = () => {
      API.connect();
    }

    render() {
      return <button onClick={this.connect}>Connect</button>;
    }
  }
  ```


## Common Patterns

* Destructure const at top of render instead of `this.props...` and `this.state...` everywhere.
  ```js
  // good
  class MyComponent extends React.Component {
    render() {
      const {label, action} = this.props;
      return <button onClick={action}>{label}</button>;
    }
  }

  // bad
  class MyComponent extends React.Component {
    render() {
      return <button onClick={this.props.action}>{this.props.label}</button>;
    }
  }
  ```

* Prefer functional components. Destructure props.
  ```js
  export default function StatusMessage({message, level='warning'}) {
    if (!message) return null;
    return <div className={`status ${level}`}>{message}</div>;
  }
  ```
