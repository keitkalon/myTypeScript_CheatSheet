# myTypeScript_CheatSheet DOM

## Setup DOM for typescript
- install tsc
- `tsc --init` // create tsconfig.json
- `mkdir src dist ` // create src & dist folder
- `touch src/index.ts` // create index.ts 
- `tsc -v` // check typescript version

## Configure Typescript for DOM
- open your tsconfig file
- inside of compilerOptions
    - `"outdir": ["./dist"]`// where we going to put the .js compiled files default is just beside .ts files
    - `"lib": []` // check this line is commented, to  get all DOM libraries, or you can add  `"lib": ["dom", es2021, "ScriptHost", "WebWorker"]` check what those mean, these files have `*.d.ts` like `lib.dom.d.ts` you can find the also on github.com/microsoft/TypeScript/blob/lib
 - outside of compilerOptions // where top level options are; those you have to write them yourself 
    - `"include": ["src/ts"]` // only compile the files from src directory delete "file" option; you can use patterns like ["*use.tsc"]

## Configure DOM for Typescript
- in src:
- `touch src/index.html` // create index html and add <script src="dist/index.js"/>
- `npm init -y` // create package.json in src
- lowercase "name" value
- `npm install lite-server` // add server to do live actualization on browser
- add in package.json at the key "scripts": { "start": "lite-server"} // when you write npm start will run lite-server
- open another terminal an run `npm start`  

## Compile TypeScript
- `in your typescript terminal
- `tsc -w` //  compile in watch mode, checks for errors and changes and display them in terminal

# DOM TYPESCRIPT

## Maby not Null Assertion Operator `?` 
// use everywhere where the variable is use
// JS&TS when you don`t know if will be null or not
```TypeScript
const btn = document.getElemntById("btn");
btn?.addEventListener("click", function () {
  alert("clicked")
})
```

## Non-Null Assertion Operator `!`
// use only once at assertion
// when you guarantee not to be null, it exist for sure
```TypeScript
const btn = document.getElemntById("btn")!;
btn.addEventListener("click", function () {
  alert("clicked")
})
```

## Type Assertion Operator
// if you get by using an elemet type you dont need to use TypeAssert Operator `querySeletor("ul")` or `getElementByTagName`
//  `as` or less use  `(<HtmlInputElement>input).value`  this dosent work on tsx
// when you guarantee what the type it will be, you will se all the classes in `lib.dom.d.ts` or in console when you select an element in the bottom at prototype of element
```TypeScript
const btn = document.getElemntById("btn")! as HTMLButtonElement;
const myPicture = document.querySelector("profile-image")! as HTMLImageElement;
const input = document.getElementById("todoinput")! as HTMInputElement;

input.value = "enter your value";
 
let secret: Any = "Hi there";
const numChar = (secret as string).length;
```

## Other Typescript Dom
// `e: SubmitedEvent`
```TypeScript
  const form = donument.querySelector("form")!;
  form.addEventListener("click", submitHandler);
  function submitHandler(e: SubmitEvent){
    e.preventDefault();
    console.log("Submited!");
  }
```
