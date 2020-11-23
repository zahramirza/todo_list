# Todo List

In this task, we will create a very basic ToDo list application.

The application should support adding/deleting todo items (just one line of text per item), and it should present the todo list in a clean and well-formatted layout.

You can work on the assignment in groups, however, each participant should submit individually by forking the original repository, integrating the solution into a local clone of the fork, and pushing the code back to his or her Github profile.

**Everybody should send me their Github username after finishing the assignment!**

## Instructions

### 0. Update your git installation

Github has recently stopped using the name `master` for the default branch for reasons of language inclusivity; instead, they are using the name `main` now. We will follow this convention and update our local git installation.

Open a Terminal/cmd window and enter this command:

```
git config --global init.defaultBranch main
```


### 1. Create a fork of this repository

In class on we have seen how to create a *fork* on Github (you can also follow these [instructions](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/fork-a-repo)).

After successfully forking this repository (https://github.com/martingasser/todo_list), you should see an exact copy of it in your Github profile.

### 2. Clone the fork on your local computer

On your computer, open a Terminal/cmd-window and navigate to the directory that contains all your programming assignments.


> It is advisable to place all code in a designated location.
> I am using a directory called `devel` which sits in my home directory to keep everything related to software development (but you are free to choose any name - just make sure to remember it).
> After starting the Terminal/cmd-window, you already are in your home directory.
> On Windows, this is `C:\Users\[Username]\`, on macOS it's `/Users/[Username]`, and on Linux, it is `/home/[Username]`. If you are doing this for the first time, use the command `mkdir devel` to create the development directory. Then use `cd devel` to change to this directory. When you start the Terminal window next time, you will only need the `cd` command. I will simply use the term *command line* to refer to the Terminal/Cmd window from now on.

Enter the command

```git clone https://github.com/[yourusername]/todo_list``` 

on the command line to clone your fork of the main repository to your computer.

### 3. Create a Vue project inside the cloned directory

After cloning the repository from Github, use the command `cd todo_list` to change to the new directory. Now we have to create a Vue project *inside* this directory.

Use this command to accomplish this goal:

```
vue create --preset martingasser/psdaai .
```

For convenience, use the preset I created for you. If you are asked 

```
? Generate project in current directory? (Y/n)
```

answer with `Y` (just type the key and press enter).

Now you should have a working Vue setup in this directory.

### 4. Test the setup

As you are already in the project directory, you can use the command

```
npm run serve
```

to test whether creating the project worked. You should be able to open the URL `http://localhost:8080/` in a browser, and you should see a welcome page.

Go back to the command line and press `Ctrl-C` to quit the web server.


### 5. Prepare the source code

On the command line, enter the command `code .` to open an editor. Now you can see the code for your project.

In the left sidebar of the editor, you can see the directory structure. Open the `src` directory by clicking on it. Then click on the file `App.vue`.

As we will implement a new application, you can remove almost all existing code from this file. 

You can copy and paste the following code to the file:

```html
<template>
</template>

<script>
export default {
  name: 'App',
  components: {
  }
}
</script>

<style>
</style>
```

Go back to the sidebar. Open the directory `src/components`. As we don't need this anymore, you can also delete the file `HelloWorld.vue`.

### 6. Write the code

In the first step, we will work on the `<template>...</template>` section. We will add a wrapper `div` and a `form` for the input text field.

```html
<template>
  <div class="wrapper">
    <div id="newtodo">
      <input type="text" name="todo-text"
        id="todo-text" placeholder="New todo">
      <button>Add</button>
    </div>
  </div>
</template>
```

We add the CSS class wrapper to the outermost `div`, and we will use this class to apply some styling later on. Inside the wrapper, there is another `div` with id `newtodo`. This `div` contains the actual text input element and a button.

Now remember what we did for the calculator example - we defined data on the application object in the `script` section that reflected the state of the user interface elements in the `template` section. Let's do that for this example as well:

```html
<script>
export default {
  name: 'App',
  components: {
  },
  data () {
      return {
          newTodoText: "",
          todos: []
      }
  }
}
</script>
```

Here, we define a `data()` function that returns a data object. The data object contains

1. The text that is entered into the input field and should be used for a new todo item
2. A list of todo objects (a list in JavaScript is defined simply with `[]`)

Let's continue thinking about functionality - we need functions for

1. Adding
2. Removing

todo items. So let's define them in the `methods` section:

```html
<script>
export default {
  name: 'App',
  components: {
  },
  data () {
      return {
          newTodoText: "",
          todos: []
      }
  },
  methods: {
      addTodo () {
      },
      removeTodo (item) {
      }
  }
}
</script>
```

What should the functions do? `addTodo` should append a new todo item to the list `todos`, `removeTodo` should remove a given item from the list `todos`. Read the comments in the code carefully!

```html
<script>
export default {
    name: 'App',
    components: {
    },
    data () {
        return {
            newTodoText: "",
            todos: []
        }
    },
    methods: {
        addTodo () {
            // check if this.newTodoText is set (prevent adding of empty items)
            if (this.newTodoText) {
                // this.todos.push() adds a new entry to the back of a list
                // a todo item consists of the text and an id, and we will simply use the current time for the id
                this.todos.push({
                    text: this.newTodoText,
                    id: Date.now()
                })
                // after addind the todo item, clear the text
                this.newTodoText = ''
            }
        },
        removeTodo (item) {
            // the function gets a todo item as an argument
            // this.todos.filter() filters out all occurences of this item from the list this.todos
            // we simply re-assign the filtered list of todos to list.todos
            this.todos = this.todos.filter(_item => _item !== item)
        }
    }
}
</script>
```

And that's basically all the JavaScript code we need for the application, so let's go back to the `template` section.

We have to add a `v-model` attribute to the input element, in order to have the `newTodoText` data element reflect the state of the input element (adding the `.trim` suffix to the `v-model` attribute makes it remove and whitespace automatically). We also add a `v-on:click` event listener to the button and have it call the `addTodo()` function when it is clicked.

```html
<template>
  <div class="wrapper">
    <div id="newtodo">
      <input type="text" name="todo-text" v-model.trim="newTodoText"
        id="todo-text" placeholder="New todo">
      <button v-on:click="addTodo()">Add</button>
    </div>
  </div>
</template>
```

The only thing that's missing now is the rendering of the todo list. So let's add a HTML unordered list (`<ul>...</ul>`):


```html
<template>
  <div class="wrapper">
    <div id="newtodo">
      <input type="text" name="todo-text" v-model.trim="newTodoText"
        id="todo-text" placeholder="New todo">
      <button v-on:click="addTodo()">Add</button>
    </div>
    <ul class="todo">
      <li v-for="todo in todos" :key="todo.id">
        <span>{{ todo.text }}</span>
      </li>
    </ul>
  </div>
</template>
```

Look at the `v-for` attribute in the `<li>` (list item) HTML element. What this does is: It creates a `<li>....</li>` block for each todo in the list of todos. Inside each `<li>` block, we simply output the text of the todo, making it appear in the resulting HTML page.

We also add a `todo` CSS class to the `<ul>` element, to make styling easier.

Now we can display a list of todos. However, we also need one delete button per todo:

```html
<template>
  <div class="wrapper">
    <div id="newtodo">
      <input type="text" name="todo-text" v-model.trim="newTodoText"
        id="todo-text" placeholder="New todo">
      <button v-on:click="addTodo()">Add</button>
    </div>
    <ul class="todo">
      <li v-for="todo in todos" :key="todo.id">
        <span>{{ todo.text }}</span>
        <button v-on:click="removeTodo(todo)">Remove</button>
      </li>
    </ul>
  </div>
</template>
```

As you can see in the code, the `v-on:click` event handler in the `button` element is used to call the function `removeTodo()`, and the current todo in the list is passed to the function as well.

We will add one more thing: If the list of todos is empty, it should display the text "Add a new todo in the input above".

```html
<template>
  <div class="wrapper">
    <div id="newtodo">
      <input type="text" name="todo-text" v-model.trim="newTodoText"
        id="todo-text" placeholder="New todo">
      <button v-on:click="addTodo()">Add</button>
    </div>
    <ul class="todo" v-if="todos.length">
      <li v-for="todo in todos" :key="todo.id">
        <span>{{ todo.text }}</span>
        <button v-on:click="removeTodo(todo)">Remove</button>
      </li>
    </ul>
    <p class="none" v-else>Add a new todo in the input above</p>
  </div>
</template>
```

Now the application is functionally complete. What's missing is the styling. I will not focus on CSS this time, so simply paste this snippet as the `<style>...</style>` section.


```html
<style>
*, *::before, *::after {
  box-sizing: border-box;
}

html, body {
  font: 16px/1.2 BlinkMacSystemFont, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  padding: 10px;
}

.wrapper {
  width: 75%;
  margin: 0 auto;
}

#newtodo {
  width: 100%;
}

input {
  width: 90%;
}

button {
  border: 1px solid green;
  background: green;
  color: white;
  font-size: 0.8rem;
  padding: 2px 4px;
  cursor: pointer;
  width: 10%;
}


form #todo-text {
  width: 100%;
  padding: 10px;
  border: 1px solid #777;
}

ul, li {
  margin: 0;
  padding: 0;
}

p.none {
  color: #888;
  font-size: small;
}

.todo li {
  display: flex;
  margin: 5px 0;
}

.todo li span {
  flex: 1;
}


.todo li button {
  border: 1px solid red;
  background: red;
  color: white;
  font-size: 0.8rem;
  padding: 2px 4px;
  cursor: pointer;
}
</style>
```

Now, your application should look like this:

![Todo list screenshot](todo.png)

Clicking `Add` should add a todo item to the list, clicking `Remove` should remove the corresponding todo item.

### 7. Commit code and push to Github

Now, return to the commandline. Entering

```
git status
```

should report this:

```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.browserslistrc
	.gitignore
	babel.config.js
	package-lock.json
	package.json
	public/
	src/

no changes added to commit (use "git add" and/or "git commit -a")
```

It's ok to simply add all untracked files (files that are not in the repository) to the git repo. Do that by entering

```
git add .
```

Doing a

```
git status
```

again will show this:

```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   .browserslistrc
	new file:   .gitignore
	modified:   README.md
	new file:   babel.config.js
	new file:   package-lock.json
	new file:   package.json
	new file:   public/favicon.ico
	new file:   public/index.html
	new file:   src/App.vue
	new file:   src/assets/logo.png
	new file:   src/main.js
```

This means that your files are in the *staging area* and ready to be committed to the repository.

Then,

```
git commit -m "Working implementation of Todo list application"
```

will result in

```
[main 6841ffb] Working implementation of Todo list application
 11 files changed, 11475 insertions(+), 1 deletion(-)
 create mode 100644 .browserslistrc
 create mode 100644 .gitignore
 create mode 100644 babel.config.js
 create mode 100644 package-lock.json
 create mode 100644 package.json
 create mode 100644 public/favicon.ico
 create mode 100644 public/index.html
 create mode 100644 src/App.vue
 create mode 100644 src/assets/logo.png
 create mode 100644 src/main.js
```

This means that all your files have been committed to your local repository and are ready to be pushed back to the Github remote repository.

```
git push
```

will do that.

Congratulations, you're done!
