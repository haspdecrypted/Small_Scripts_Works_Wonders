To take input till 20 characters and store in heap memory in ReactJS, you can follow these steps:

1. Create a state variable in your React component to hold the input value:

```
const [inputValue, setInputValue] = useState('');
```

2. Render an input element in your component and attach an onChange event handler to update the state variable with the input value:

```
<input type="text" value={inputValue} onChange={(event) => setInputValue(event.target.value)} />
```

3. Validate the input value to make sure it has no more than 20 characters:

```
const isValidInput = inputValue.length <= 20;
```

4. To store the input value in heap memory, you can create a new string object using the `new` keyword and pass in the input value as a parameter:

```
const inputString = new String(inputValue);
```

5. Finally, you can use the `isValidInput` variable to conditionally render an error message if the input is invalid:

```
{!isValidInput && <div>Please enter a value with no more than 20 characters.</div>}
```

Note that storing strings as objects using the `new` keyword is not recommended in JavaScript, as it can lead to memory leaks. It is generally better to store strings as primitive values. However, if you have specific requirements for storing strings in heap memory, you can use the `new String()` syntax.

<h4> Example on above logic </h4>
Sure, here's an example implementation of taking input till 20 characters, storing it in heap memory and validating it in a React component:

```
import React, { useState } from 'react';

function InputComponent() {
  const [inputValue, setInputValue] = useState('');

  const isValidInput = inputValue.length <= 20;
  const inputString = new String(inputValue);

  const handleChange = (event) => {
    setInputValue(event.target.value);
  };

  return (
    <div>
      <input type="text" value={inputValue} onChange={handleChange} />
      {!isValidInput && <div>Please enter a value with no more than 20 characters.</div>}
      <p>Input value: {inputString}</p>
    </div>
  );
}

export default InputComponent;
```

In this example, we create a state variable `inputValue` using the `useState` hook to hold the input value. We also create a `isValidInput` variable to validate the input value and a `inputString` variable to store the input value as a string object.

In the `handleChange` function, we update the state variable `inputValue` with the new input value.

In the return statement, we render an input element with the `value` attribute set to `inputValue` and the `onChange` event handler set to `handleChange`. We also conditionally render an error message if the input is invalid using the `isValidInput` variable. Finally, we display the input value as a string object using the `inputString` variable.

Note that in this example, we are using the `new String()` syntax to store the input value as a string object. However, as I mentioned before, this is not recommended in JavaScript unless you have specific requirements for storing strings in heap memory. In most cases, it is better to store strings as primitive values.
