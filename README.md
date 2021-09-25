# Full programming language

Full is a concept programming language aimed at what is not uniquely unthought of but uniquely unperfected - that which allows a single codebase to define a distributed software system composed of the following parts:

- Databases
- "Backend" servers
- Web clients
- Mobile (iOS/Android) clients
- Desktop (Win/Mac/Linux) clients
- etc...

Before we get bogged down by minutia, let's explore by example:

```lisp
(log "Hello world!")
```

```
$ full .
Hello world!
```

Basic printing should look recognizable. We'll use LISP syntax for its simple and broad form.

```lisp
(map
  (range 5)
  (fun x (log (* x "* ")))
)
```

```
$ full .
*
* *
* * *
* * * *
* * * * *
```

As above, this should be fairly recognizable. Now let's take a look at what **Full** is really meant for:

```lisp
(let db (database {
  todos: (set [] {bool done, str text})
}))
```

This expression: (1) defines a new database `db` in our code, which will automatically be deployed to an auto-scaled distributed database cloud, (2) bootstraps that database with a table called `todos`, which is created with an empty set (no data) and typed for objects with the following properties:

- `id`: a UUID
- `done`: a boolean (`true` or `false`)
- `text`: a string

Now we can create a multi-platform GUI app within the same file, adding below the above:

```lisp
(let gui (app {
  /: <page>
    <h1>Todos</h1>
    <input placeholder="Add a task..." on:enter={
      (fun val (+ db.todos {done: false, text: val}))
    }/>
    <ul>
      {(map db.todos (fun todo <li>
        <checkbox bind:selected={todo.done}/>
        {todo.text}
      </li>))}
    </ul>
  </page>
}))
```

This defines a generic GUI app, which can generate an app for any platform. The "backend" and "frontend" code is merged into one wholistic context - with powerful concise tools, like `<checkbox bind:selected={todo.done}/>` which creates a live-updated, two-way bind between the `done` property on each todo item in the database and the `selected` property of the checkbox UI element.

We can complete this project by deploying all of our infrastructure:

```lisp
(export gui [web ios android])
```

```
$ full .
Preparing application `gui` for export...
✔ Deploying new database `db`
✔ Compiling `gui`
✔ Deploying API functions
✔ Deploying web application
✔ Building iOS application
✔ Building Android application
Deployed 1 database, 2 functions, 1 web app.
Packaged 2 mobile applications for publishing.
Run details: https://ACME.io/runs/a80tRMj2mfSdl52
(web): https://web-as92DFkw4.ACME.io
```
