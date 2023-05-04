You can add a submit button element that handles submit on click and auto-submit if the data entered is valid by following these steps:

1. Create a state variable to hold the value of the input field.

```jsx
const [inputValue, setInputValue] = useState('');
```

2. Add an onChange event listener to the input field that updates the state variable with the value of the input field.

```jsx
<input
  type="text"
  value={inputValue}
  onChange={(event) => setInputValue(event.target.value)}
/>
```

3. Create a function to handle the form submission when the input value is valid.

```jsx
const handleSubmit = () => {
  // perform form submission logic
};
```

4. Create a function to check if the input value is valid and call the handleSubmit function if it is.

```jsx
const handleInputValidation = () => {
  if (inputValue) {
    handleSubmit();
  }
};
```

5. Add a button element with an onClick event listener that calls the handleInputValidation function.

```jsx
<button type="button" onClick={handleInputValidation}>Submit</button>
```

6. Add a useEffect hook that calls the handleSubmit function whenever the inputValue state variable changes and is valid.

```jsx
useEffect(() => {
  if (inputValue) {
    handleSubmit();
  }
}, [inputValue]);
```

Now, when the user types something in the input field and clicks the submit button or the input value is valid (i.e., not an empty string), the form will be submitted automatically. If the input value is not valid, nothing will happen when the submit button is clicked.

<h4>Example for above logic</h4>

Sure, here's an example of how to implement auto-submit functionality in React JS with a submit button that handles submit on click and auto-submit if the data entered is valid:

```jsx
import { useState, useEffect } from 'react';

function AutoSubmit() {
  const [inputValue, setInputValue] = useState('');

  const handleSubmit = () => {
    console.log('Form submitted!');
    // perform form submission logic
  };

  const handleInputValidation = () => {
    if (inputValue) {
      handleSubmit();
    }
  };

  useEffect(() => {
    if (inputValue) {
      handleSubmit();
    }
  }, [inputValue]);

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={(event) => setInputValue(event.target.value)}
      />
      <button type="button" onClick={handleInputValidation}>
        Submit
      </button>
    </div>
  );
}

export default AutoSubmit;
```

In this example, the input field and the submit button are rendered inside a div element. The input value is stored in the inputValue state variable, and the handleSubmit function is called when the form is submitted. The handleInputValidation function checks if the input value is valid and calls the handleSubmit function if it is.

The useEffect hook is used to call the handleSubmit function whenever the inputValue state variable changes and is valid.

When the user types something in the input field and clicks the submit button or the input value is valid (i.e., not an empty string), the form will be submitted automatically. If the input value is not valid, nothing will happen when the submit button is clicked.
