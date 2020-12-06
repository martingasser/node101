# node.js first steps

In class we have already made the first steps in NodeJS. We have seen how node can be used on the commandline as a REPL (Read Evaluation Print Loop).

```
> node
Welcome to Node.js v12.15.0.
Type ".help" for more information.
> 
```

Now you can write JavaScript line by line, and it is interpreted immediately.

The REPL can be exited by pressing Ctrl-D.

However, we will use node mainly to run JavaScript files.

# Task 1

Create a new repository in Github (if you need help, you can find detailed instructions [here](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/create-a-repo))

- Repository name: node101
- Public
- Add a README

After creating the repository on Github, [clone](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/cloning-a-repository) it to your machine.

# Task 2

Change the working directory in your commandline window to the local repository.

```
cd node101
```

Now, use the command

```
npm -y init
```

to initialise a new node project. 

As we will use the moment.js library, we have to install it:

```
npm install moment
```

Now open a code editor in the current directory.

```
code .
```

# Task 3

We will create a file that saves a list of Todo items to a JSON file. The todo items will have the data fields we already know from our web application (`text`, `date`, `id`, `done`).

A typical todo object would be created like so:

```javascript
const todo = {
  text: 'Go shopping',
  date: moment('19.12.2020, 21:10', 'DD.MM.YYYY, hh:mm'),
  id: 1,
  done: false
}
```

The expression `moment('19.12.2020, 21:10', 'DD.MM.YYYY, hh:mm')` uses the moment library to convert the date string to a date object. Remember that the second argument to this function is the date format (which is `DD.MM.YYYY, hh:mm` in this example).

In the editor, create a file `createTodoList.js`.

Start with this skeleton:

```javascript
const fs = require('fs')
const moment = require('moment')

const todoList = [

]
```

Now, add three todo items to the todoList. You can choose text, date, and id freely, use the todo format above.

Hint: You can simply add objects to a list like this:

```javascript
const list = [
  {
    field: 'value1'
  },
  {
      field: 'value2'
  },
  // .....
]
```

Now, save the `todoList` to a JSON file.

The data is converted to JSON with

```javascript
const json = JSON.stringify(todoList)
```

and the file is written with

```javascript
fs.writeFileSync('todo.json', json)
```


# Task 4

Now read the JSON file back into a JavaScript program.

Create a file named `readTodoList.js` and import the necessary libraries:

```javascript
const fs = require('fs')
```

To read the data from the JSON file, use

```javascript
const data = fs.readFileSync('todo.json', 'utf8')
```

Now, we have the data in text format. However, we need the actual list of todo objects.

Use the function `JSON.parse()` to convert the JSON string back to JavaScript.

```javascript
const todoList = JSON.parse(data)
```

Now, print out the second todo item in the list and check with the file todo.json if it is correct.

```javascript
console.log(todoList[1])
```

# Task 5

Now, let's create a simple web server that serves the entire todo list in JSON format.

First, we have to install the express library in the commandline window:

```
npm install express
```

Now let's go back to the editor. Create a file called `index.js` and paste the code below into the file.

```javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Web server listening at http://localhost:${port}`)
})
```

Save the file and go back to the commandline window. You can test the web server by running the script `index.js` in node.

```
node index.js
```

Opening `http://localhost:3000/` in a browser should display the message 'Hello World!' (exit the server with Ctrl-C).

Now try to think about what's necessary to return the todo list instead of the string 'Hello World!'.

You can use the code from `readTodoList.js` to read the file:

```javascript
const fs = require('fs')

const data = fs.readFileSync('todo.json', 'utf8')
const todoList = JSON.parse(data)
```

The code `res.send('Hello World!')` is used to send a string (in this case 'Hello World!'). 
If we want to send a list, and object, or a list of objects, we have to use the function `res.json(todoList)` instead.

# Submit

Commit your code to your local repository

```
git add package.json
git add createTodoList.js
git add readTodoList.js
git add index.js
git commit -m 'node101'
```

and push it to your Github repo:

```
git push
```
