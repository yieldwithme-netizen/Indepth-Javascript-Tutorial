# Do-While Loop

## Definition

The `do-while` loop is a variant of the while loop that executes the code block at least once before checking the condition. It continues to execute as long as the condition remains true. The condition is evaluated after each iteration, making it ideal when you need to guarantee at least one execution.

## Syntax

```javascript
do {
  // code block to be executed
} while (condition);
```

## How do-while Works

```javascript
let count = 1;

do {
  console.log(`Count is: ${count}`);
  count++;
} while (count <= 5);

// Output:
// Count is: 1
// Count is: 2
// Count is: 3
// Count is: 4
// Count is: 5
```

## do-while vs while Loop

```javascript
// while loop - may not execute at all
let i = 10;
while (i < 5) {
  console.log(i); // Never executes
}

// do-while loop - executes at least once
let j = 10;
do {
  console.log(j); // Executes once: "10"
  j++;
} while (j < 5);
```

## Examples

### 1. Basic Counter

```javascript
let number = 1;

do {
  console.log(number);
  number++;
} while (number <= 10);
```

### 2. User Input Validation

```javascript
let userInput;
let valid = false;

do {
  userInput = prompt('Enter a number between 1 and 100:');
  const num = parseInt(userInput);

  if (!isNaN(num) && num >= 1 && num <= 100) {
    valid = true;
    console.log(`Valid input: ${num}`);
  } else {
    console.log('Invalid input. Try again.');
  }
} while (!valid);
```

### 3. Menu System

```javascript
let choice;

do {
  console.log('\n=== Menu ===');
  console.log('1. Option A');
  console.log('2. Option B');
  console.log('3. Exit');

  choice = prompt('Enter your choice (1-3):');

  switch (choice) {
    case '1':
      console.log('You selected Option A');
      break;
    case '2':
      console.log('You selected Option B');
      break;
    case '3':
      console.log('Goodbye!');
      break;
    default:
      console.log('Invalid choice');
  }
} while (choice !== '3');
```

### 4. Generating Random Numbers

```javascript
let randomNum;
let attempts = 0;

do {
  randomNum = Math.floor(Math.random() * 10) + 1;
  attempts++;
  console.log(`Attempt ${attempts}: ${randomNum}`);
} while (randomNum !== 7);

console.log(`Found 7 after ${attempts} attempts!`);
```

### 5. Processing Array Elements

```javascript
const items = [10, 20, 30, 40, 50];
let index = 0;
let sum = 0;

do {
  sum += items[index];
  console.log(`Added ${items[index]}, sum = ${sum}`);
  index++;
} while (index < items.length);

console.log(`Total sum: ${sum}`);
```

### 6. Retry Logic

```javascript
async function fetchDataWithRetry(url, maxRetries = 3) {
  let attempt = 0;
  let data;

  do {
    attempt++;
    try {
      console.log(`Attempt ${attempt}...`);
      const response = await fetch(url);
      data = await response.json();
      console.log('Success!');
    } catch (error) {
      console.log(`Failed: ${error.message}`);
      if (attempt < maxRetries) {
        console.log('Retrying...');
      }
    }
  } while (!data && attempt < maxRetries);

  return data;
}
```

### 7. Number Guessing Game

```javascript
function guessNumber() {
  const secretNumber = Math.floor(Math.random() * 100) + 1;
  let guess;
  let attempts = 0;

  do {
    guess = parseInt(prompt('Guess a number (1-100):'));
    attempts++;

    if (guess < secretNumber) {
      console.log('Too low!');
    } else if (guess > secretNumber) {
      console.log('Too high!');
    }
  } while (guess !== secretNumber);

  console.log(`Correct! You got it in ${attempts} attempts.`);
}
```

### 8. Date Loop

```javascript
const startDate = new Date('2024-01-01');
const endDate = new Date('2024-01-10');
const currentDate = new Date(startDate);

do {
  console.log(currentDate.toISOString().split('T')[0]);
  currentDate.setDate(currentDate.getDate() + 1);
} while (currentDate <= endDate);
```

## Common Use Cases

### Form Validation

```javascript
function validateForm(formData) {
  let isValid;
  const errors = [];

  do {
    errors.length = 0; // Clear previous errors

    if (!formData.username || formData.username.length < 3) {
      errors.push('Username must be at least 3 characters');
    }

    if (!formData.email || !formData.email.includes('@')) {
      errors.push('Valid email is required');
    }

    if (!formData.password || formData.password.length < 6) {
      errors.push('Password must be at least 6 characters');
    }

    isValid = errors.length === 0;

    if (!isValid) {
      console.log('Validation errors:', errors);
      // Show errors to user
    }
  } while (!isValid);

  return true;
}
```

### Polling

```javascript
async function pollForUpdates() {
  let lastUpdate = null;

  do {
    const currentUpdate = await checkForUpdates();

    if (currentUpdate !== lastUpdate) {
      console.log('New update available!');
      lastUpdate = currentUpdate;
    }

    await sleep(5000); // Wait 5 seconds
  } while (isPollingEnabled());
}
```

### Database Operations

```javascript
function processDatabaseRecords(records) {
  let index = 0;
  let processed = 0;

  do {
    const record = records[index];

    if (record.isValid) {
      processRecord(record);
      processed++;
    }

    index++;
  } while (index < records.length);

  console.log(`Processed ${processed} of ${records.length} records`);
}
```

## Common Mistakes

### 1. Forgetting the Semicolon

```javascript
// WRONG: Missing semicolon causes syntax error
do {
  console.log('loop');
} while (condition)

// RIGHT: Always add semicolon
do {
  console.log('loop');
} while (condition);
```

### 2. Infinite Loop

```javascript
// WRONG: Condition never becomes false
let x = 0;
do {
  console.log(x);
  // Forgot to increment x
} while (x < 5);

// RIGHT: Ensure condition eventually changes
let x = 0;
do {
  console.log(x);
  x++; // Don't forget this!
} while (x < 5);
```

### 3. Using do-while When While is Better

```javascript
// UNNECESSARY: do-when when while would work
let arr = [];
do {
  // Process empty array - executes once for no reason
} while (arr.length > 0);

// BETTER: Use while
while (arr.length > 0) {
  // Only executes if there are items
}
```

## Quick Revision Summary

- Executes at least once before checking condition
- Syntax: `do { } while (condition);`
- Condition is checked at the end of each iteration
- Best for input validation and retry logic
- Always include semicolon after while condition
- Ensure condition eventually becomes false

## Related Topics

- [[While-Loop]] - While loop for pre-check iterations
- [[For-Loop]] - For loop variations
- [[Break-Continue]] - Loop control statements
- [[Loop-Techniques]] - Advanced loop patterns
- [[Recursion]] - Recursive alternatives to loops
- [[Iteration-Protocols]] - Iterators and generators
