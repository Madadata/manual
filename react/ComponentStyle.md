# Stateless Functional Component

* [when to use?](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)
  * no lifecycle method
  * no state to keep, only render data from props.

* code snippet

```
// Madadata.jsx
import React, { PropTypes } from 'react';

// you can do this with function declaration. (function Madadata = ())
const Madadata = props => <div>{props.name}</div>;

Madadata.propTypes = {
  name: PropTypes.string.isRequired,
};

// don't use context if you don't know what it is.
// Madadata.contextTypes = {}

export default Madadata;
```

* signature

```
(props, context) => JSXElement;
```

* Notes
  * capitalize the **first character** of your component.
  * optional params of this function can be **props**, **context**.
  * specify the propTypes of your component for props and context **explicitly**.
  * export your component as **default**.
  * name the file as **ComponentName.jsx**.

* Styles
  * prefer PascalCase for component name
  * prefer arrow function
  * If JSX spans for several lines
    ```
    const Madadata = props => (
      <div>
        {props.name}
      </div>
    );
    ```
  * prefer collapsing props in propTypes and contextTypes and prefer isRequired to enforce checking prop existence.
    ```
    Madadata.propTypes = {
      name: PropTypes.string.isRequired,
    };

    Madadata.contextTypes = {
      store: PropTypes.object.isRequired,
    }
    ```

# PureComponent

* when to use?
  * has lifecycle method
  * has state to keep.
  * UI is determined only by state, props.
    * UI does not depend on global variable.
    * UI does not use any utility function that has variables from outside.
    * UI does not have children that are not PureComponent.

* code snippet

```
// PureMadadata.jsx
import React, { PureComponent, PropTypes } from 'react';

class PureMadadata extends PureComponent {

  constructor(props, context) {
    super(props, context);
    this.state = { value: 'madadata' };
    this.handleSomethingClick = this.handleSomethingClick.bind(this);
  }

  componentWillMount() {
    // * call this.setState here to update state, the first render will pick up state changes.
    // * will only be invoked once.
  }

  componentDidMount() {
    // * do setTimeout, setInterval, addEventListeners
    // * do ajax calls (dispatch actions that will be handled by redux-saga)
    // * access any refs to your children
  }

  componentWillReceiveProps(nextProps) {
    // * componentWillReceiveProps enables you to decide what to do with next props before the component is rendered.
    // * this.props -> old props
    // * nextProps -> new props
    // * will not be called for initial render
    // * call this.setState here to update state, the next render will pick up state changes.
  }

  shouldComponentUpdate(nextProps, nextState) {
    // * it will always return true by default. This method enables you to check to see whether you want react to re-render this component with new props and state. It's a costly operation and also a performance optimization point.
    // * PureComponent will help you **shallow** compare the this.props and nextProps, this.state and nextState. If you need to do compare deeply, explicitly do this in the shouldComponentUpdate method of a Component instead of PureComponent.
  }

  componentWillUpdate(nextProps, nextState) {
    // * componentWillUpdate enables you to perform preparation before and update occurs.
    // * invoked immediately before rendering.
    // * you might do:
    //   1. set a variable for particular use
    //   2. animate your DOM elements
    //   3. dispatch action to do something to your store
    //   ...
  }

  componentDidUpdate(prevProps, prevState) {
    // * componentDidUpdate gives you a chance to operate on the DOM.
    // * it will not be called in the initial render.
  }

  componentWillUnmount() {
    // * perform necessary cleanup, clearTimer, clearEventListener ....
  }

  handleSomethingClick() {

  }

  render() {

  }

}

PureMadadata.propTypes = {};
PureMadadata.contextTypes = {};

export default PureMadadata;

```

* Grammers
  * if you don't do anything other than invoke the constructor of super class

    ```
    constructor() {
      super();
    }
    ```
    you should omit the constructor.
  * if you want to use props or both props and context in your constructor, you need to pass them to the constructor of its super class.

    ```
    constructor(props) {
      super(props);
      // this.props....
    }

    constructor(props, context) {
      super(props, context);
      // this.props, this.context...
    }
    ```
  * bind event-handler in constructor.

* styles
  * constructor: follow this order: super -> set initial state -> bind event-handler
    ```
    constructor() {
      super();
      this.state = {};
      this.handleSomethingClick = this.handleSomethingClick.bind(this);
    }
    ```
  * event-handler: if the handler is used by DOMElement named **button**, the corresponding event is **Click**, you should write it as

    ```
    constructor() {
      ...
      this.handleButtonClick = this.handleButtonClick.bind(this);
    }

    handleButtonClick() {

    }

    render() {
      return <div className={styles.button} onClick={this.handleButtonClick}></div>
    }
    ```

    that is to say, the event-handler should be named **handleElementEvent**
  * order of Component methods should be as follows:
    [constructor] -> [lifecycle methods] -> [self-defined methods] -> render
  * if render method is to long, and it's a combination of several parts. Extract each part as a single method. For instance:

    ```
    class Madadata extends PureComponent {
      render() {
        return (
          <div className={styles.container}>
            <div className={styles.header}>
            </div>
            <div className={styles.body}>
            </div>
            <div className={styles.footer}>
            </div>
          </div>
        );
      }
    }
    ```

    you'd better extract header, body and footer out like:

    ```
    class Madadata extends PureComponent {

      getHeaderJSX() { // or you can name it **renderHeader**
        return <div className={styles.header}></div>;
      }

      getBodyJSX() { // or you can name it **renderBody**
        return <div className={styles.body}></div>;
      }

      getFooterJSX() { // or you can name it **renderFooter**
        return <div className={styles.footer}></div>;
      }

      render() {
        const headerJSX = this.getHeaderJSX();
        const bodyJSX = this.getBodyJSX();
        const footerJSX = this.getFooterJSX();
        return (
          <div className={styles.container}>
            {headerJSX}
            {bodyJSX}
            {footerJSX}
          </div>
        );
      }
    }
    ```

    and these extracted methods should be right above render method.

# Component
* when to use?
  * has lifecycle method
  * has state to keep.
  * UI is not pure component.

* [others] same with PureComponent.

# Redux

* when to use?

* styles
  * prefer arrow functions for mapStateToProps and mapDispatchToProps:

  ```
  const mapStateToProps = state => ({
    prop1: state.**.prop1,
    prop2: state.**.prop2,
  });

  const mapDispatchToProps = ({
    actionName: actionCreator1,
    actionName: actionCreator2,
  });

  connect(mapStateToProps, mapDispatchToProps)(Component);
  ```

  * prefer verb for action and actionCreator

  ```
  // action.js
  export LOAD_METADATA = 'appName/moduleName/LOAD_METADATA';
  export function loadMetadata() {}
  ```

  * prefer **payload** for prop of an action besides **type**.
  ```
  export function loadMetadata(something) {
    return {
      type: LOAD_METADATA,
      payload: something,
    };
  }
  ```

* proposes
  * prefer using ES6 Symbol for action type.

  ```
  export LOAD_METADATA = Symbol();
  ```

  so that we don't have to type long words and disable eslint-max-len. Any there will be no name clashes.
