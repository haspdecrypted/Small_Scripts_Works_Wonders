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
