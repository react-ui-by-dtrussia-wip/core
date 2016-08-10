# React UI

Standard model of UI development


## Why?

When building React apps a multitude of components is created. They end up scattered across the project, making it hard to control and use them. React UI tries to solve that problem by encapsulating all of the components into a single dependency that can be used across the app:

```javascript
import UI from 'src/components/ui';

class SomeContainer extends Component {
  render() {
    return (
      <div>
      	<UI.Label>Hello, GitHub!</UI.Label>
      	<UI.Button caption="OK" />
      </div>
    );
  }
}
```


## How?

```sh
npm install --save @react-ui/core
```

Unlike other UI related libraries, React UI doesn't include any built-in components. It rather suggests a pattern for managing components in an app. In fact, React UI exports only a single function that is used for preparing UI for your app:

```javascript
/* src/components/ui/index.js */

import ReactUI from '@react-ui/core';

import defaultStyles from 'src/styles/ui';
import basicComponents from 'src/components/ui/basic';

const UI = reactUI(defaultStyles)(
  basicComponents,
);

export default UI;
```

React UI pattern revolves around the following three aspects:

  * single point of initialization

  * styles are passed to components as a parameter, thus, easing UI theming

  * UI components can be grouped in layers, where a given layer has access to the components of parent layers. This exact property helps to achieve the proper UI composition.

Components that are passed to `ReactUI` must be wrapped in a function call that accepts `styles` passed during UI creation:

```javascript
/* src/components/ui/basic/button.jsx */

import cn from 'classnames';

export default (styles = {}) => {
  return class extends Component {
    render() {
      <div className={cn(styles.button, this.props.className)}>
        {this.props.children}
      </div>
    }
  }
}
```


## Examples

`UI` object can be destructured to avoid long names:
```javascript
import UI from 'src/components/ui';
const {
  Button,
  Grid: { Row, Col }
} = UI;

class AnotherContainer extends Component {
  render() {
    return (
      <Row>
        <Col>One</Col>
        <Col>Two</Col>
        <Col>
          <div>
            <Button>Press Me</Button>
          </div>
        </Col>
      </Row>
    );
  }
}
```

Components can be styled in place by passing `className` or `style` properties:
```javascript
import UI from 'src/components/ui';
const {
  Button,
  Grid: { Container, Row, Col },
} = UI;

class ButtonsBox extends Component {
  render() {
    return (
      <Container style={{ backgroundColor: `red` }}>
        <Row>
          <Col>
            <Button className="success">Done</Button>
            <Button className="fail">Cancel</Button>
          </Col>
        </Row>
      </Container>
    );
  }
}
```
