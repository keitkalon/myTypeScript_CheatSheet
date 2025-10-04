# myTypeScript_CheatSheet DOM

## Setup (DOM + TypeScript)

* Install TypeScript if you haven’t: `npm i -D typescript`
* `tsc --init` — create `tsconfig.json`
* `mkdir src dist` — create source & build folders
* `touch src/index.ts` — create entry TypeScript file
* `tsc -v` — verify TypeScript version

## Configure TypeScript for the DOM

Open `tsconfig.json` and set (key points only):

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["DOM", "ES2021", "WebWorker"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "noEmitOnError": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"]
}
```

**Notes**

* Use camelCase: **`outDir`** (not `outdir`), and it’s a **string**, not an array.
* If you omit `lib`, TypeScript picks sensible defaults; adding `"DOM"` ensures browser DOM typings.
* `include` should match your actual layout; here we compile everything under `src/`.

## Configure a simple dev server (optional)

In the **project root** (not inside `src/`):

```bash
npm init -y
npm i -D lite-server
```

Add scripts to **root** `package.json`:

```json
{
  "scripts": {
    "start": "lite-server",
    "build": "tsc -w"
  }
}
```

Create an `index.html` in the **project root** (alongside `package.json`):

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>TS DOM</title>
</head>
<body>
  <button id="btn">Click me</button>
  <ul id="todolist"></ul>
  <form id="todoform">
    <input id="todoinput" type="text" placeholder="New todo" />
    <button type="submit">Add</button>
  </form>
  <script type="module" src="./dist/index.js"></script>
</body>
</html>
```

Run in two terminals:

* `npm run build` — TypeScript compiler in watch mode
* `npm start` — serve and auto‑reload

## Compile TypeScript

* In your TS terminal: `tsc -w` — watch & compile, report errors on change

---

# DOM + TypeScript Patterns

## Optional Chaining `?.` (safe access when maybe null)

```ts
const btn = document.getElementById("btn");
btn?.addEventListener("click", () => {
  alert("clicked");
});
```

## Non‑Null Assertion `!` (you guarantee it’s not null)

```ts
const btn2 = document.getElementById("btn")!; // you are sure it exists
btn2.addEventListener("click", () => {
  alert("clicked");
});
```

## Type Assertions for DOM elements

```ts
const btnEl = document.getElementById("btn") as HTMLButtonElement;
const myPicture = document.querySelector(".profile-image") as HTMLImageElement; // class selector
const input = document.getElementById("todoinput") as HTMLInputElement;

input.value = "enter your value";

let secret: any = "Hi there"; // `any` is lowercase
const numChars = (secret as string).length;
```

---

## Small To‑Do App (TypeScript + DOM)

```ts
interface Todo {
  text: string;
  completed: boolean;
}

const btn = document.getElementById("btn") as HTMLButtonElement | null;
const input = document.getElementById("todoinput") as HTMLInputElement;
const form = document.querySelector<HTMLFormElement>("#todoform")!;
const list = document.querySelector<HTMLUListElement>("#todolist")!;

const STORAGE_KEY = "todos";

const todos: Todo[] = readTodos();
todos.forEach(createTodo);

function readTodos(): Todo[] {
  const todosJSON = localStorage.getItem(STORAGE_KEY);
  if (todosJSON === null) return [];
  try {
    return JSON.parse(todosJSON) as Todo[];
  } catch {
    return [];
  }
}

function saveTodos() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
}

form.addEventListener("submit", submitHandler);

function submitHandler(e: SubmitEvent) {
  e.preventDefault();
  const text = input.value.trim();
  if (!text) return;
  const newTodo: Todo = { text, completed: false };
  createTodo(newTodo);
  todos.push(newTodo);
  saveTodos();
  input.value = "";
}

function createTodo(todo: Todo) {
  const newLI = document.createElement("li");
  const checkbox = document.createElement("input");
  checkbox.type = "checkbox";
  checkbox.checked = todo.completed;

  checkbox.addEventListener("change", () => {
    todo.completed = checkbox.checked;
    saveTodos();
  });

  newLI.append(todo.text);
  newLI.append(" ");
  newLI.append(checkbox);
  list.append(newLI);
}
```

---

## Generics with `querySelector`

```ts
// <input id="username" type="text" placeholder="enter your username" />
const inputEl = document.querySelector<HTMLInputElement>("#username")!;
const btn3 = document.querySelector<HTMLButtonElement>("#btn")!;
console.dir(inputEl);
inputEl.value;
```
