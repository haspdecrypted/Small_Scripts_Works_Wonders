You can add a submit button element that handles submit on click and auto-submit if the data entered is valid by following these steps:

- Create a state variable to hold the value of the input field.

```
const [inputValue, setInputValue] = useState('');
```

- Add an onChange event listener to the input field that updates the state variable with the value of the input field.

```
<input
  type="text"
  value={inputValue}
  onChange={(event) => setInputValue(event.target.value)}
/>
```

- Create a function to handle the form submission when the input value is valid.

```
const handleSubmit = () => {
  // perform form submission logic
};
```

- Create a function to check if the input value is valid and call the handleSubmit function if it is.

```
const handleInputValidation = () => {
  if (inputValue) {
    handleSubmit();
  }
};
```

- Add a button element with an onClick event listener that calls the handleInputValidation function.

```
<button type="button" onClick={handleInputValidation}>Submit</button>

```

- Add a useEffect hook that calls the handleSubmit function whenever the inputValue state variable changes and is valid.

```
useEffect(() => {
  if (inputValue) {
    handleSubmit();
  }
}, [inputValue]);

```

<p> Now, when the user types something in the input field and clicks the submit button or the input value is valid (i.e., not an empty string), the form will be submitted automatically. 
If the input value is not valid, nothing will happen when the submit button is clicked. </p>
