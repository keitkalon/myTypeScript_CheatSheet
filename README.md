# myTypeScript_CheatSheet

## Installation
- install node js
- `npm -v` // check npm version
- `npm install tyepescript --save-dev` // only for one project
- `npm install -g typescript` // for global
- `tsc -v` // check typescript version

## Compile TypeScript
- `tsc file.ts` // create a .js file base on your .ts file

## Types
Variables:
```TypeScript
let myString: string = "Words";
let myNumber: number = 1;
let myBoolean: boolean = true;
let myAny: any = true; //better don`t use it
let undefinedString: string;
let names1: string[] = ["Mary", "John"];
let names2: Array<string> = ["Mary", "John"];
let board: string[][] = [["X","O","O"]["O","X","X"]["X","O","X"]];
let id: number | string = 21;
let id: number | string = "R21";
let names1: (string | number)[] = [21, "R21"];
```

Methods:
```TypeScript
function greet(person: string = "stranger", age: number, funny: boolean, timeofday: string): string{
  if(age < 30 && funny)
    retun `Hi there, ${person}`;
  else{
    return `Good ${timeofday}, ${person}`
  {
}

const sum = (x: number, y: number): number =>{
return x + y;
}

function randomStringOrNumber(num: number): number | string {
  if(Math.random() < 0,5){
    return num.toString();
  }
  return num;
}

function print(message: string): void {
  console.log(message)
}

function neverEnds(): never {  // for functions that end the program or run forever
  while(true){
    //do staff
  }
}
```

Objects:
```TypeScript
const coordinates: {latitude: number, longitude: number} = {latitude: 255, longitude: 459};

function printName(person: {first: string; last: strinh}): void  {
  console.log(`${person.first} ${person.last}`);
}
```

Objects Type Aliases:
```TypeScript
type Coordinates = {
  readonly latitude: number; // cannot be modified
  longitude: number;
  altitude?: number; // "?" not required 
}
const coordinates: Coordinates = {latitude: 255, longitude: 459};

type Person = {
  first: string;
  middle?: string; // "?" not required 
  last: string;
  readonly id?: string; "?" not required, and cannot be modified
}
function printName(person: Person): void  {
  console.log(`${person.first} ${person.last}`);
}
```

Objects Intersection Types:
```TypeScript
type Color = {
  color: string;
}
type Hat = {
  size: string;
}
type ColorfulHat = Hat & Color & { model: string};

const happyHat: ColorfulHat = {
  size = medium;
  color = yellow;
  model = bubbles;
}
```

Objects Union Types with type narrowing:
```TypeScript
function printAge(age: number | string) : void {
  console.log(`You are ${age} years old`);
}

printAge(25);
printAge("twenty five");

function printAge(age: number | string) : void {

  if(typeof age === "string"){  // type narrowing
    console.log(`You are ${age} years old`);
  } else {
    console.log(`You are ${age} years old`);
  }

}
```
