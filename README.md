![image](https://github.com/ienouali/React-TDD-first-step/assets/54776723/e28fbace-2d5d-4151-beed-089dccc1a137)
# React TDD: First Step

## Overview
TDD stands for Test-Driven Development. It is a software development approach where tests are written before the actual code implementation. In the context of React, TDD involves writing tests for React components and their functionality before writing the code to implement those components.
In this comprehensive article, we will embark on a journey of building a React app by employing the approach of test-driven development (TDD). Moreover, we will leverage the powerful testing frameworks Jest and Enzyme to ensure the utmost precision and reliability in our development process.
Overview of Our Test-driven React App
we will construct a fundamental counter application comprised of various UI components. Each component will possess its own dedicated set of tests located in a corresponding test file. To begin, we will define epics and user stories that align with our app requirements

## USER STORIES :
As a user, I need to increment the counter.
As a user, I need to decrement the counter.
As a user, I need to reset the counter.

## Setup
Initially, our approach involves utilizing Create React App to establish a React project in the following manner
```bash
$ npx create-react-app react-counter 
$ cd react-counter
$ npm start
```

Next, we will proceed to integrate Jest, Enzyme, and additional dependencies into our project in the following manner:
``` bash
$ npm i -D enzyme 
$ npm i -D react-test-renderer enzyme-adapter-react-16
```

Additionally, we will include a setupTests.js file within the src directory: As Create React App executes the setupTests.js file before each test, it ensures the proper execution and configuration of Enzyme.
```javascript
import { configure } from ‘enzyme’;
import Adapter from ‘enzyme-adapter-react-16’;
 
configure({ adapter: new Adapter() });
```

## Shallow Render Test
As you may be aware, the typical TDD process follows these steps:
1- Introduce a test.
2- Execute all tests, expecting the tests to fail.
3- Write code to fulfill the test requirements.
4- Run all tests to validate the changes.
5- Refactor the code if necessary.
6- Repeat the cycle.
Therefore, our next step involves initiating the initial test for a shallow render and subsequently implementing the code to fulfill this test. We will create a new spec file called App.spec.js within the src/components/App directory, adhering to the following structure:

```javascript
import React from ‘react’;
import { shallow } from ‘enzyme’;
import App from ‘./App’;


describe(‘App’, () => {
  it(‘should render a <div />’, () => {
     const container = shallow(<App />); 
     expect(container.find(‘div’).length).toEqual(1);
  });
 });
```

Then, run this cmd to run all tests:
```bash
$ npm run test
```
Expecting to see the test fails.

## App Component
Next, we will create the App component to fulfill the test. navigate to the App.jsx file located in the src/components/App directory and incorporate the following code

```javascript
import React from ‘react’;

const App = () => <div className=”container” />;

export default App;
```

Next, run the test again.
npm run test
The first test should now pass successfully.
Next, we have to update the index.js file to import the App component as follows:

```javascript
import React from "react"
import ReactDOM from "react-dom"
import App from "./components/App/App"
import * as serviceWorker from "./serviceWorker"

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
)

serviceWorker.unregister()
```
## Including Counter Component
the Counter component will be included in the App component, requiring us to modify the App.spec.js file to verify its presence. Additionally, we will declare the container variable outside the initial test case to ensure the shallow render test is performed before each test case.

```javascript
import React from "react"
import { shallow } from "enzyme"
import App from "./App"
import Counter from "../Counter/Counter"

describe("App", () => {
  let container

  beforeEach(() => (container = shallow(<App />)))

  it("should render a <div />", () => {
    expect(container.find("div").length).toEqual(1)
  })

  it("should render the Counter Component", () => {
    expect(container.containsMatchingElement(<Counter />)).toEqual(true)
  })
})
```

At this point, executing npm test will result in a test failure due to the absence of the Counter component.

## Counter Shallow Rendering Test

Next, we will proceed with the creation of a new directory called "Counter" within the "src/components" directory. Within this directory, we will create a file named "Counter.spec.js".
inside Counter.spec.js file:

```javascript
import React from "react"
import { shallow } from "enzyme"
import Counter from "./Counter"

describe("Counter", () => {
  let container

  beforeEach(() => (container = shallow(<Counter />)))

  it("should render a <div />", () => {
    expect(container.find("div").length).toBeGreaterThanOrEqual(1)
  })
})
```

## Counter Component
Afterward, we will create a new file called "Counter.jsx" and define the identical variables and methods based on the user stories.

```javascript
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    console.log('increment counter.');
  };

  const decrement = () => {
    console.log('decrement counter.');
  };

	const reset = () => {
	    console.log('reset counter to 0.');
	  };

  return <div className="counter-container" />;
};

export default Counter;
```

The current implementation in the Counter.spec.js file should successfully pass the test and render a <div />, but it should not render the Counter Component because it has not been added to the app component yet.
To incorporate the Counter component into the App.jsx file, we will make the following additions:

```javascript
import React from 'react';
import './App.css';
import Counter from '../Counter/Counter';

const App = () => (
  <div className="app-container">
    <Counter />
  </div>
);

export default App;
```
Now, run the test again

```bash
$ npm run test
```
And all the tests should be passed.
Write the CounterButton Shallow Rendering Test
In order to create the CounterButton Component, we require three buttons: Increment, Decrement, and Reset. Consequently, the Counter.spec.js file needs to be modified to verify the presence of the CounterButton component within the Counter component.

### Counter.spec.js
```javascript
it("should render instances of the CounterButton component", () => {
    expect(container.find("CounterButton").length).toEqual(3)
  })
```

Now, let’s add the CounterButton.spec.js file in a new directory called CounterButton under the src/components directory, and let’s add the test to the file like this:
```javascript
import React from "react"
import { shallow } from "enzyme"
import CounterButton from "./CounterButton"

describe("CounterButton", () => {
  let container

  beforeEach(() => {
    container = shallow(
      <CounterButton
        buttonAction={jest.fn()}
        buttonValue={""}
      />
    )
  })

  it("should render a <div />", () => {
    expect(container.find("div").length).toBeGreaterThanOrEqual(1)
  })
})
```

Now, if you run the test, you will see the test fails.
Let’s create the CounterButton*.jsx* file

```javascript
import React from 'react';
import PropTypes from 'prop-types';

const CounterButton= ({ buttonHandler, buttonValue }) => (
 <div className="button-container">
    <p className="button-value">{buttonValue}</p>
  </div>
);

CounterButton.propTypes = {
  buttonHandler: PropTypes.func.isRequired,
  buttonValue: PropTypes.string.isRequired,
};

export default CounterButton;
```

When executing npm test at this point, it will result in failure as we have not included the CounterButton components within the Counter component yet. To address this, we should import the CounterButton component and include three instances of it within the Counter.jsx file.
```javascript
const Counter = () => {

  const [count, setCount] = useState(0);

  const increment = () => {
    console.log('increment counter.');
  };

  const decrement = () => {
    console.log('decrement counter.');
  };

const reset = () => {
    console.log('reset counter to 0.');
  };

  return <div className="counter-container">
							<div className="count-value"></div>
			        <div className="counter-button-container">
			          <CounterButton buttonHandler={increment} buttonValue={'Increment'} />
			          <CounterButton buttonHandler={decrement} buttonValue={'Decrement'} />
			          <CounterButton buttonHandler={reset} buttonValue={'Reset'} />
			        </div>
					</div>
};
```

## Refactoring the Counter
We're planning to refactor the counter as we aim to incorporate methods such as increment, decrement, and reset. First, let's update the Counter.spec.js file.
```javascript
import { mount } from 'enzyme';
import Counter from './Counter';

describe('mounted Counter', () => {
  let wrapper;

  beforeEach(() => {
    wrapper = mount(<Counter />);
  });

  it('invokes increment when the increment button is clicked', () => {
    const spy = jest.spyOn(Counter.prototype, 'increment');
    expect(spy).toHaveBeenCalledTimes(0);
    wrapper.find('.increment').first().simulate('click');
    expect(spy).toHaveBeenCalledTimes(1);
  });

  it('invokes decrement when the decrement button is clicked', () => {
    const spy = jest.spyOn(Counter.prototype, 'decrement');
    expect(spy).toHaveBeenCalledTimes(0);
    wrapper.find('.decrement').first().simulate('click');
    expect(spy).toHaveBeenCalledTimes(1);
  });

  it('invokes reset when the reset button is clicked', () => {
    const spy = jest.spyOn(Counter.prototype, 'reset');
    expect(spy).toHaveBeenCalledTimes(0);
    wrapper.find('.reset').first().simulate('click');
    expect(spy).toHaveBeenCalledTimes(1);
  });
});
```

If you run the test, you will see the added tests fail since we haven’t updated the CounterButton component yet. Let’s update the CounterButton component to add the click event:
```javascript
const CounterButton= ({ buttonHandler, buttonValue }) => (
 <div className="button-container" onClick={() => buttonHandler()}>
    <p className="button-value">{buttonValue}</p>
  </div>
);
```

Now, the tests should pass successfully, Next, we are going to add more tests to check the state when each function is invoked in the mounted Counter test case:

```javascript
it('should render initial count value of 0', () => {
    expect(wrapper.find('.count-value').text()).toBe('0');
  });

  it('should increment count when the increment button is clicked', () => {
    wrapper.find('CounterButton').at(0).prop('buttonHandler')();
    wrapper.update();
    expect(wrapper.find('.count-value').text()).toBe('1');
  });

  it('should decrement count when the decrement button is clicked', () => {
    wrapper.find('CounterButton').at(1).prop('buttonHandler')();
		wrapper.update();
    expect(wrapper.find('.count-value').text()).toBe('-1');
  });

  it('should reset count to 0 when the reset button is clicked', () => {
    wrapper.find('CounterButton').at(2).prop('buttonHandler')();
		wrapper.update();
    expect(wrapper.find('.count-value').text()).toBe('0');
  });
```

If you run the tests, you will see them fail since we haven’t implemented each method yet. So let’s implement each function to pass the tests:
```javascript
const increment = () => {
	  setCount(prevCount => prevCount + 1);
  };

  const decrement = () => {
      setCount(prevCount => prevCount - 1);
  };

const reset = () => {
      setCount(0);
  };
```

And finally, you will see the tests pass if you run them.

## Wrapping Up
Therefore, we have successfully built a basic React application using Test-Driven Development (TDD).


#
##### Imad Eddine NOUALI
###### Frontend Engineer | React Specialist.
