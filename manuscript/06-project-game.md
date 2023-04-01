# Project: A Game

It's time for your first project chapter! Project chapters are where we build something a little more involved together so you have a chance both to put the skills you've been learning to use and also see an example of how professional programmers design and build projects.

## Building A Hangman Game

The project we're going to build in this chapter is a Hangman game.

I'm sure you're familiar with it already, but if not, Hangman is a game where you try to guess a word one letter at a time. With every wrong guess, more of the "hangman" is drawn. When you draw the entire hangman, you've got one more wrong guess before you lose the game.

Our word list is taken from the top 10,000 most-used words in the English language, according to MIT. We'll actually filter that list a bit to make sure we're only getting words with at least 5 characters to make it more challenging.

## Getting Started

To get started, clone the project repository from GitHub:

```sh
git clone https://github.com/jasonsbarr/hangman.git
```

You can see the finished project in the `finish` directory. We'll be working in the `start` directory so you can start fresh from the beginning.

## Project Design

First, let's quickly design the whole project.

Even for a simple game, there are a lot of things to think about. How will you keep track of the game state? How will you check to see if the game is over? What options should the players have while playing? What third-party libraries will you use so you don't have to invent everything from scratch?

Let's keep the main program simple. We'll prompt the user for input, and give them a few options. Options to play the game, get help, and quit the game sound good to me.

We'll run the program in a loop. If the player asks for help, we'll print directions on the screen and then prompt again. If the player asks to quit, we'll print a goodbye message and break out of the loop. That leaves playing the game as the obvious last option.

To make handling game play as simple as possible, we'll keep track of the game state and derive each step of the gameplay from the game state.

That way we can minimize the number of variables we have to keep track of and reduce bugs by always having a single source of truth for gameplay.

We'll design the gameplay in more detail below.

### Project Setup

We're going to install 2 dependencies for this project. First, we'll use the Simple IO package to handle a couple of tasks: getting user input and handling the file with the words in it.

Secondly, let's use the Figlet package so we can print out some text in a really neat ASCII art format to give the game some style.

Enter the `start` directory to get started.

Start setting up the project by running `npm init` and answering the prompts. It will default to naming the project after the `start` directory. If you're ok with just using all the defaults, just use `npm init -y` to skip the prompts.

Now install dependencies:

```sh
npm install figlet @jasonsbarr/simple-io
```

Finally, open `package.json` in your editor and add `"type": "module"` to your configuration. Your `package.json` file should look something like this:

```json
{
  "name": "hangman",
  "version": "1.0.0",
  "type": "module",
  "description": "CLI hangman game",
  "main": "hangman.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Jason Barr <jason@jasonsbarr.com>",
  "license": "MIT",
  "dependencies": {
    "@jasonsbarr/simple-io": "^0.1.0",
    "figlet": "^1.5.2"
  }
}
```

If it's not exactly the same, no worries. The only things you need to be the same are the `dependencies` object and `"type": "module"`. Use any license you like (ISC is the default, as you can see above I've used MIT). I've released the game code as open source under the MIT license, so you can use or modify it in any way you see fit.

## The Main Loop and Functions

Create a directory called `src` and open a new file in it named `start.js`. Import the `figlet` package as well as the `input` function:

```js
import figlet from "figlet";
import { input } from "@jasonsbarr/simple-io";
```

As you can see, `figlet` uses a default export whereas Simple IO uses named exports.

Now stub out an exported function `start`, like this:

{title: "`src/start.js` from line 3:"}
```js
/**
 * Starts the game and allows the player to choose their option
 */
export function start() {

}
```

### The Help Function

Now create a new file in the `src` directory named `help.js`. This is where the help message for the player will go. Here's the function to print the help text:

{title: "`src/help.js`"}
```js
/**
 * Prints game help and exits
 */
export function help() {
  console.log("*** HANGMAN HELP ***");
  console.log("Guess a word, one letter at a time.");
  console.log("With every letter you miss, you draw a part of your man.");
  console.log("After 7 misses...");
  console.log("YOU LOSE.");
}

```

### The Play Function

Now create a new file in `src` named `play.js` and stub out the `play` function as an export:

{title: "`src/play.js`"}
```js
/**
 * Play the game
 */
export function play() {

}
```

### Creating The Main Loop

Now that you have the helper functions stubbed out, you can create the main loop that controls whether the player enters the game, prints the help message, or quits. Open `src/start.js` and import the helper functions:

{title: "In `src/start.js` from line 3:"}
```js
import { help } from "./help.js";
import { play } from "./play.js";
```

Let's use Figlet to print a nice welcome message when the user fires up the game:

{title: "In `src/start.js` from line 9:"}
```js
export function start() {
    console.log(figlet.textSync("Welcome to Hangman!"));
}
```

Now we're going to initialize the `direction` variable to hold the direction the player inputs, and get their input in a loop until they decide to exit the game:

{title: "In `src/start.js` from line 9:"}
```js
export function start() {
  console.log(figlet.textSync("Welcome to Hangman!"));
  let direction = "";

  // Main loop
  while (direction !== "Q") {
    direction = input("P for play, H for help, or Q for quit: ");

    switch (direction.toUpperCase()) {
      case "P":
        play();
        break;
      case "H":
        help();
        break;
      case "Q":
        console.log(figlet.textSync("Goodbye!"));
        process.exit(0);
      default:
        console.log(`${direction} is not a valid instruction.`);
    }
  }
}
```

Note that if the user enters "Q" for "Quit" we print "Goodbye" and then it's followed by `process.exit(0);`. The `process.exit` function is how you let Node.js know to exit the program entirely. Passing it an argument of 0 communicates to your shell (terminal or command prompt) that the process exited successfully, without errors.

Now that you have the main loop in an exported function, in your project's `start` directory create a new file `hangman.js` and import the `start` function into it:

{title: "`hangman.js`"}
```js
import { start } from "./src/start.js";

start();
```

I've done it like this so you have an interface to interact with that calls the game program and runs it. If you open your terminal (or Command Prompt on Windows), navigate to the `start` directory, and enter `node hangman.js`, you'll see that the main loop is executing.

Of course, if you choose "P" right now it's not going to do anything yet. Let's fix that!

## Designing The Game Play

Let's take a moment now to think through the game play in a little more detail.

When a game is started, the game needs to take the following steps:

1. Open the `data/words.txt` file
2. Randomly pick a word from the file
3. Set up the game with the chosen word
4. Display a prompt to the user to make their first guess

Then, for each guess the player makes, the following needs to happen:

1. Check to see if the user entered a letter or "quit"
2. If the instruction is "quit," exit the game
3. If it's otherwise not a letter, re-prompt the user for a valid letter
4. If the user enters a letter, it needs to check the guess against the letters in the word
5. If the letter is in the word, update the prompt to show what letters have been correctly guessed
6. If the letter is not in the word, add 1 to the count of incorrect guesses and update the prompt accordingly
7. Check to see if either the user is out of guesses or if they've guessed the entire word correctly
8. If either of those is true, the game is over and it should display the correct endgame message
9. If neither of those is true, display the updated prompt so the user can make their next guess

It sounds like we need to keep track of the word being guessed against, the number of incorrect guesses, and the letters that have been guessed. We'll handle that with the game state object.

## The Game State

Open `src/play.js` and define a data type for the game state object using a `@typedef` docblock:

{title: "In `src/play.js`, before the `play` function:"}
```js
/**
 * @typedef GameState
 * @property {string} word
 * @property {number} guessesUsed
 * @property {string[]} lettersGuessed
 */
```

Let's go ahead and define an initial game state value. We'll reset the game state to this every time a new game begins:

```js
const initialGameState = {
  word: "",
  guessesUsed: 0,
  lettersGuessed: []
};
```

Finally, we're going to set the max number of guesses before the player loses to 7. It's good practice to avoid using "magic numbers" in our code. Magic numbers are constant number values that just come out of nowhere, like magic. You may know what the value means now, but there's no guarantee someone else looking at your code will have any idea. It's better to define such values as constants in your code so that way everywhere the value is used it will be obvious what the purpose of the value is.

Above the `@typedef` for `GameState`, add:

```js
const MAX_GUESSES = 7;
```

Using ALL_CAPS_WITH_UNDERSCORES is a convention for constant values.

Now add the following line at the beginning of the `play` function:

{title: "In `src/play.js`"}
```js
let gameState = initialGameState;
```

Now every time a new game starts, it will set the game state to the initial state we've defined. We've used `let` so we can rebind new values to the `gameState` variable. We won't mutate the game state directly; instead, we'll derive the next state from the current state and a user action, then rebind the variable to the new game state. This prevents us from having to deal with the pitfalls related to mutating reference type data.

## Getting The Word

The next thing that needs to happen in the game is picking a word.

All the words are in `data/words.txt`. To pick a word we need to open this file, break the text up into an array whose values are each individual word, and then use a random number to pick the index from the array for the word.

The Simple IO package has a function `readLines` that reads the lines of a file into an array. Since each word in `words.txt` is on its own line, this will do nicely.

At the top of `src/play.js`, import the `readLines` function. We'll go ahead and import `input` too, because we known we'll be using it later.

{title: "`src/play.js`, line 1:"}
```js
import { input, readLines } from "@jasonsbarr/simple-io";
```

We also need a random integer to get the word from a random index of the array returned by `readlines`. JavaScript has the `Math.random` method for random numbers, but it picks a number between 0 and 1. That's not what we need; we need a random integer.

We'll need a function to convert the random value generated by `Math.random` into an integer. Below the `play` function, enter this function to generate a random index:

```js
/**
 * Gets random integer from 0 to top
 * @param {number} top
 * @returns {number}
 */
function randomInt(top) {
  return Math.floor(Math.random() * top);
}
```

Note that we're passing in a value for the highest value we want the function to be able to return. The file `data/words.txt` has 10,000 words in it, but we're not going to use all of them. We're only going to use words with 5 or more characters in them. That means we need the highest possible value returned by this function to equal the last index of the array of words after we've filtered out all the words with fewer than 5 characters.

To get the array of words from `data/words.txt`, we need to tell the `readLines` function where to find the file. We'll need to import a couple of things from the Node.js base modules to do that. First, we'll import the `path` module. Then we'll import a helper function to convert a file URL to a path.

If we didn't do this, and just gave `readLines` the relative path to `data/words.txt` from inside the `src/play.js` file, it wouldn't work right. Node.js would try to use whatever directory you were running the program from as the base URL for the relative path, which would only work if you were running the program from the `src` directory. We need to make sure Node.js always uses the exact same information to set the path to retrieve the data. Since we're writing this program using ES2015 modules, we have access to the `import.meta.url` property which gives us the file URL of the current file. We'll give that to the `fileURLToPath` helper function and use the `path.join` method to ensure that, no matter what directory we run the program from, Node.js always looks in the same place for `data/words.txt`.

Import the `fileURLToPath` helper function and the `path` module at the top of `src/play.js`, above the import statement that's already there:

```js
import path from "node:path";
import { fileURLToPath } from "node:url";
```

You don't **have** to do your imports in this order, but I always import core modules first, then modules from NPM packages, then modules from the program I'm working on so I always have consistent structure for my imports.

Now let's create the `getWord` function below `randomInt`:

{title: "In `src/play.js` below the `randomInt` function:"}
```js
/**
 * Get random word of at least 5 characters from data/words.txt
 * @returns {string}
 */
function getWord() {
  const words = readLines(
    path.join(fileURLToPath(import.meta.url), "../../data/words.txt")
  ).filter(word => word.length >= 5);
  return words[randomInt(words.length - 1)];
}
```

We create the `words` array by first reading the text from `data/words.txt` using the `readLines` function. Use `import.meta.url` to get a consistent reference to the file we're working in, use the `fileURLToPath` function to convert that URL to a path, then use `path.join` to get an absolute path to the location of the `data/words.txt` file.

Next, we filter that array getting only the words that are at least 5 characters in length.

Finally, we return a random index from the `words` array, giving the `randomInt` function a top value of `words.length - 1`. Remember, we subtract 1 from the array length because array indexes start at 0.

### Derive The Game State

Now that we have the ability to randomly choose a word from the list of words, we can update our game state with the selected word.

Update the `play` function accordingly:

```js
export function play() {
  let gameState = initialGameState;
  gameState = { ...gameState, word: getWord() };
  console.log("Let's play!");
}
```

## Guessing A Letter

This is where the real action in the game happens. We're going to write the function for guessing a letter to update and return a new game state that tells the main play function everything it needs to know about how to proceed.

Since this function is so important, you might think it needs to be complicated. It's actually surprisingly simple!

The function takes the letter guessed and the current game state as inputs, and it returns a new derived game state.

First, we check to see if the letter has already been guessed, in which case we simply return the game state as is.

If the letter hasn't already been guessed, we need to check if the letter is in the word currently being guessed. If it's not, we need to add one to the number of wrong guesses used. Then, in either case, we add the letter to the array of letters that have been guessed.

{title: "In `src/play.js` below the `getWord` function:"}
```js
/**
 * Derives new GameState from guessed letter
 * @param {string} guess letter guessed
 * @param {GameState} gameState
 * @returns {GameState}
 */
function guessLetter(guess, gameState) {
  if (gameState.lettersGuessed.includes(guess)) {
    return gameState;
  }

  let newState = { ...gameState, lettersGuessed: [ ...gameState.lettersGuessed, guess ] };
  if (gameState.word.includes(guess)) {
    return newState;
  } else {
    newState.guessesUsed++;
    return newState;
  }
}
```

It's ok in this case to directly update the `guessesUsed` property on the new game state object, because we're not mutating the original state. Once you've copied the original state, any operations you need to perform in order to update the state should be fine.

## Checking To See if The Game Is Over

To check if the game is over, we need to check 2 things:

- If the number of guesses is equal to the max number of guesses
- If all the letters in the word have been guessed

This function will take the current game state as its parameter and return a boolean showing whether the game is over.

For the first check, all we need to do is check if the number of `guessesUsed` on the game state object is equal to `MAX_GUESSES`:

{title: "In `src/play.js` below the `guessLetter` function:"}
```js
/**
 * Checks to see if game is over
 * @param {GameState} gameState
 * @returns {boolean}
 */
function isGameOver(gameState) {
  if (gameState.guessesUsed >= MAX_GUESSES) {
    return true;
  }
}
```

The second check is a little more complicated. We need to see if every letter in the word has already been guessed.

The simplest way to do this is to get rid of any duplicate characters in the word and then see if there are as many correct guesses as there are individual letters in the word. We can use a Set to deduplicate characters in the word:

```js
function isGameOver(gameState) {
  // initial check
  let letters = [ ...new Set(gameState.word.split("")) ];
}
```

The `String.prototype.split` method splits the string into an array using whatever delimiter you give it. In this case it's an empty string, so the method breaks the string into an array of its individual (UTF-16) characters. Passing that array into the `Set` constructor gets rid of any duplicate characters, and then since Sets are iterable we simply spread the Set of characters into a new array.

Next, we'll loop over the letters that have already been guessed. For each one that's in the array of letters from the word, we increment a counter. If we get through the loop and the counter is equal to the length of the array of letters, we know all the letters in the word have been guessed. Note that this only works because in the `guessLetter` function we don't add duplicate guessed letters to the array of letters guessed.

{title: "Continuing in the `isGameOver` function in `src/play.js`"}
```js
function isGameOver(gameState) {
  // everything through setting the letters array, above
  let correctGuesses = 0;

  for (let guess of gameState.lettersGuessed) {
    if (letters.includes(guess)) {
      correctGuesses++;
    }
  }

  return correctGuesses === letters.length;
}
```

That gives us our complete `isGameOver` function:

{title: "In `src/play.js`"}
```js
/**
 * Checks to see if game is over
 * @param {GameState} gameState
 * @returns {boolean}
 */
function isGameOver(gameState) {
  if (gameState.guessesUsed >= MAX_GUESSES) {
    return true;
  }

  let letters = [ ...new Set(gameState.word.split("")) ];
  let correctGuesses = 0;

  for (let guess of gameState.lettersGuessed) {
    if (letters.includes(guess)) {
      correctGuesses++;
    }
  }

  return correctGuesses === letters.length;
}
```

## Display Functions

We have a few functions remaining to define, which are presentational in nature.

We need to show the current state of the hangman, which letters have been guessed and how many remain to be guessed, and if the game is over we need to show the endgame message based on whether the player has won or lost.

### Drawing The Hangman

We draw a different hangman based on how many incorrect guesses the player has used. Remember, with JavaScript multiline strings it indents exactly as the text is indented from the left margin starting on the 2nd line, so text after the 1st line needs to be flush to the left:

{title: "In `src/play.js` below the `isGameOver` function"}
```js
/**
 * Draws the hangman
 * @param {number} used number of guesses used
 * @returns {string}
 */
function drawMan(used) {
  return used >= 6 ? String.raw`       ____
      |    |
      |    O
      |   /|\
      |    |
      |  _/ \_
______|______` : used === 5 ? String.raw`       ____
      |    |
      |    O
      |   /|\
      |    |
      |  _/
______|______` : used === 4 ? String.raw`       ____
      |    |
      |    O
      |   /|\
      |    |
      |
______|______` : used === 3 ? String.raw`       ____
      |    |
      |    O
      |   /|
      |    |
      |
______|______` : used === 2 ? String.raw`       ____
      |    |
      |    O
      |    |
      |    |
      |
______|______` : used === 1 ? String.raw`       ____
      |    |
      |    O
      |
      |
      |
______|______` : String.raw`       ____
      |    |
      |
      |
      |
      |
______|______`;
}
```

The `String.raw` method ensures that exactly what's inside the backticks gets saved, so you don't have to mess around with escape sequences. We'll go into escape sequences in the next chapter. You'll also note that `String.raw` takes its argument without parentheses. For backtick strings, you can actually define functions that govern how the string is processed. This is called a *tagged template string*. Tagged templates are a very complex topic and beyond the scope of this book.

### Printing The Word as You Guess

Now we need a function to print the letters that have been guessed correctly and blanks for characters that have not yet been guessed. We'll use underscores for the letters that haven't been guessed yet, and put spaces between the characters so it's easy to tell at a glance what characters still need to be guessed.

This function prints based on the `gameState`, which it takes as a parameter.

{title: "In `src/play.js` below `drawMan`"}
```js
/**
 * Draw correctly guessed letters with lines for unguessed letters
 * @param {GameState} gameState
 */
function writeWordWithGuesses(gameState) {
  const wordLetters = gameState.word.split("");
  let guessed = "";

  for (let letter of wordLetters) {
    if (gameState.lettersGuessed.includes(letter)) {
      guessed += letter + " ";
    } else {
      guessed += "_ ";
    }
  }

  console.log(guessed);
}
```

### Printing The End Game Message

The last function we need is to print the endgame message based on whether the player has won or lost. If they've won, we'll reiterate what the word was so they can see it written normally, and also show how many guesses they had left. If they've lost, we'll show what the word was.

{title: "In `src/play.js` below `writeWordWithGuesses`"}
```js
/**
 * Display endgame message depending on whether the player won
 * @param {GameState} gameState ending game state
 */
function endGame(gameState) {
  if (gameState.guessesUsed < MAX_GUESSES) {
    console.log(
      `That's right, ${gameState.word}! You won with ${MAX_GUESSES - gameState.guessesUsed} guesses left!`
    );
  } else {
    console.log(`Sorry, you lost. The word was ${gameState.word}.`)
  }
}
```

## Putting It All Together

Now we can put the whole game together including the main game loop that controls the flow of the game from input to input. Go back to the top of the `src/play.js` file and edit the `play` function.

In the main game loop, we need to:

1. Draw the hangman in its current state
2. Write out the word with correctly guessed letters showing and blanks for letters not guessed yet
3. Take a letter or the word 'quit' as input
4. Process the guess if it's a letter, or break out of the loop if they want to quit

Here's what the finished `play` function looks like. Note that the empty `console.log` calls just print blank lines.

{title: "The updated `play` function in `src/play.js`"}
```js
/**
 * Play the game
 */
export function play() {
  let gameState = initialGameState;
  gameState = { ...gameState, word: getWord() };
  console.log("Let's play!");

  // main game loop
  while (true) {
    console.log(drawMan(gameState.guessesUsed));
    console.log();
    writeWordWithGuesses(gameState);
    console.log();
    let guess = input("Guess a letter, or enter 'quit' to quit: ");

    if (guess === "quit") {
      console.log("Goodbye!");
      break;
    } else if (!/^[a-zA-Z]$/.test(guess)) {
      console.log("Your guess must be a letter.");
      continue;
    }

    gameState = guessLetter(guess.toLowerCase(), gameState);

    if (isGameOver(gameState)) {
      endGame(gameState);
      break;
    }
  }
}
```

The unfamiliar syntax on line 38 is a *regular expression*, which we'll cover in detail in the next chapter.

Here's the complete `src/play.js` file:

{title: "`src/play.js`"}
```js
import path from "node:path";
import { fileURLToPath } from "node:url";
import { input, readLines } from "@jasonsbarr/simple-io";

const MAX_GUESSES = 7;

/**
 * @typedef GameState
 * @property {string} word
 * @property {number} guessesUsed
 * @property {string[]} lettersGuessed
 */
const initialGameState = {
  word: "",
  guessesUsed: 0,
  lettersGuessed: []
};

/**
 * Play the game
 */
export function play() {
  let gameState = initialGameState;
  gameState = { ...gameState, word: getWord() };
  console.log("Let's play!");

  // main game loop
  while (true) {
    console.log(drawMan(gameState.guessesUsed));
    console.log();
    writeWordWithGuesses(gameState);
    console.log();
    let guess = input("Guess a letter, or enter 'quit' to quit: ");

    if (guess === "quit") {
      console.log("Goodbye!");
      break;
    } else if (!/^[a-zA-Z]$/.test(guess)) {
      console.log("Your guess must be a letter.");
      continue;
    }

    gameState = guessLetter(guess.toLowerCase(), gameState);

    if (isGameOver(gameState)) {
      endGame(gameState);
      break;
    }
  }
}

/**
 * Gets random integer from 0 to top
 * @param {number} top
 * @returns {number}
 */
function randomInt(top) {
  return Math.floor(Math.random() * top);
}

/**
 * Get random word of at least 5 characters from data/words.txt
 * @returns {string}
 */
function getWord() {
  const words = readLines(
    path.join(fileURLToPath(import.meta.url), "../../data/words.txt")
  ).filter(word => word.length >= 5);
  return words[randomInt(words.length - 1)];
}

/**
 * Derives new GameState from guessed letter
 * @param {string} guess letter guessed
 * @param {GameState} gameState
 * @returns {GameState}
 */
function guessLetter(guess, gameState) {
  if (gameState.lettersGuessed.includes(guess)) {
    return gameState;
  }

  let newState = { ...gameState, lettersGuessed: [ ...gameState.lettersGuessed, guess ] };
  if (gameState.word.includes(guess)) {
    return newState;
  } else {
    newState.guessesUsed++;
    return newState;
  }
}

/**
 * Checks to see if game is over
 * @param {GameState} gameState
 * @returns {boolean}
 */
function isGameOver(gameState) {
  if (gameState.guessesUsed >= MAX_GUESSES) {
    return true;
  }

  let letters = [ ...new Set(gameState.word.split("")) ];
  let correctGuesses = 0;

  for (let guess of gameState.lettersGuessed) {
    if (letters.includes(guess)) {
      correctGuesses++;
    }
  }

  return correctGuesses === letters.length;
}

/**
 * Draws the hangman
 * @param {number} used number of guesses used
 * @returns {string}
 */
function drawMan(used) {
  return used >= 6 ? String.raw`       ____
      |    |
      |    O
      |   /|\
      |    |
      |  _/ \_
______|______` : used === 5 ? String.raw`       ____
      |    |
      |    O
      |   /|\
      |    |
      |  _/
______|______` : used === 4 ? String.raw`       ____
      |    |
      |    O
      |   /|\
      |    |
      |
______|______` : used === 3 ? String.raw`       ____
      |    |
      |    O
      |   /|
      |    |
      |
______|______` : used === 2 ? String.raw`       ____
      |    |
      |    O
      |    |
      |    |
      |
______|______` : used === 1 ? String.raw`       ____
      |    |
      |    O
      |
      |
      |
______|______` : String.raw`       ____
      |    |
      |
      |
      |
      |
______|______`;
}

/**
 * Draw correctly guessed letters with lines for unguessed letters
 * @param {GameState} gameState
 */
function writeWordWithGuesses(gameState) {
  const wordLetters = gameState.word.split("");
  let guessed = "";

  for (let letter of wordLetters) {
    if (gameState.lettersGuessed.includes(letter)) {
      guessed += letter + " ";
    } else {
      guessed += "_ ";
    }
  }

  console.log(guessed);
}

/**
 * Display endgame message depending on whether the player won
 * @param {GameState} gameState ending game state
 */
function endGame(gameState) {
  if (gameState.guessesUsed < MAX_GUESSES) {
    console.log(
      `That's right, ${gameState.word}! You won with ${MAX_GUESSES - gameState.guessesUsed} guesses left!`
    );
  } else {
    console.log(`Sorry, you lost. The word was ${gameState.word}.`)
  }
}
```

Now all you need to do is enter `node hangman.js` from the `start` directory in your terminal (or Command Prompt on Windows) and you can play your game!

## Recap

In this chapter we built your first real-world JavaScript project, a Hangman game! This is an important step, and I hope you're proud of yourself. I'm proud of you!

This is your first big project, but it won't be your last.

In the next chapter you'll learn how to work with strings like a pro, including that confusing-looking regular expression syntax you saw in this chapter.
