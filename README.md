# myTypeScript_CheatSheet

## Installation
- Install Node.js
- `npm -v` // check npm version
- `npm install typescript --save-dev` // only for one project
- `npm install -g typescript` // for global installation
- `tsc -v` // check TypeScript version

## Configuration
- Navigate to your project directory
- `tsc --init` // create tsconfig.json file
- Open your tsconfig file:
  - Outside of `compilerOptions`:
    - "files": [ "index.ts", "file.ts", "utilities.ts" ] // All files to include in compilation; others will be ignored
    - "include": [ "src/ts" ] // Compile only files from the `src/ts` directory. Delete the `files` option; patterns like "*.use.ts" can be used
    - "exclude": [ "src/ts/dontTouch.ts", "src/ts/ignore.ts" ] // Files to exclude from compilation; patterns like "dontUse/**/*", "**/*.test.ts" can also be used. Node modules should be excluded unless specified
  - Inside `compilerOptions`:
    - "outDir": "./src/js/compiled" // Directory for compiled .js files; default is alongside .ts files
    - "target": "es5" // JavaScript version for compiled files; can be `es6`, `es2015`, or `es2016` ~ `es2022`, `esnext`
    - "strict": true // Enables all strict options; options can be configured individually
    - "lib": [ "es6", "dom" ] // Specify library files to include
    - "module": "commonjs" // Specify the module system
    - "allowJs": true // Allow JavaScript files in the project
    - "checkJs": true // Type-check JavaScript files
    - "noEmitOnError": true // Prevent emitting files on compilation errors

## Compile TypeScript
- `tsc file.ts` // Create a .js file based on your .ts file
- `tsc -outFile common.js f1.ts f2.ts f3.ts` // Create a single .js file from multiple .ts files
- `tsc -w file.ts` // Watch mode: compiles automatically on changes
- `tsc` // Compile all files in the project

## Types
Variables:
```typescript
let myString: string = "Words";
let myNumber: number = 1;
let myBoolean: boolean = true;
let myAny: any = true; // Better to avoid using `any`
let undefinedString: string;
let names1: string[] = ["Mary", "John"];
let names2: Array<string> = ["Mary", "John"];
let board: string[][] = [["X", "O", "O"], ["O", "X", "X"], ["X", "O", "X"]];
let id: number | string = 21;
id = "R21";
let arrayUnion: (string | number)[] = [21, "R21"];
let myTuple: [string, number] = ["R", 21]; // Fixed length and type per index
```

## Methods
```typescript
function greet(person: string = "stranger", age: number, funny: boolean, timeOfDay: string): string {
  if (age < 30 && funny) {
    return `Hi there, ${person}`;
  } else {
    return `Good ${timeOfDay}, ${person}`;
  }
}

const sum = (x: number, y: number): number => {
  return x + y;
};

function randomStringOrNumber(num: number): number | string {
  if (Math.random() < 0.5) {
    return num.toString();
  }
  return num;
}

function print(message: string): void {
  console.log(message);
}

function neverEnds(): never { // For functions that end the program or run forever
  while (true) {
    // Do stuff
  }
}
```

## Objects
```typescript
const coordinates: { latitude: number, longitude: number } = { latitude: 255, longitude: 459 };

function printName(person: { first: string; last: string }): void {
  console.log(`${person.first} ${person.last}`);
}
```

## Object Type Aliases
```typescript
type Coordinates = {
  readonly latitude: number; // Cannot be modified
  longitude: number;
  altitude?: number; // Optional property
};

const coordinates: Coordinates = { latitude: 255, longitude: 459 };

type Person = {
  first: string;
  middle?: string; // "?" not required 
  last: string;
  readonly id?: string; "?" not required, and cannot be modified
}

function printName(person: { first: string; last: string }): void {
  console.log(`${person.first} ${person.last}`);
}
```

## Object Intersection Types
```typescript
type Color = {
  color: string;
};
type Hat = {
  size: string;
};
type ColorfulHat = Hat & Color & { model: string };

const happyHat: ColorfulHat = {
  size: "medium",
  color: "yellow",
  model: "bubbles",
};
```

## Objects Union Types with Type Narrowing
```typescript
function printAge(age: number | string): void {
  if (typeof age === "string") { // Type narrowing
    console.log(`You are ${age} years old`);
  } else {
    console.log(`You are ${age} years old`);
  }
}

printAge(25);
printAge("twenty five");
```

## Objects Literal Types
```typescript
let zero: 0 = 0;
```

Objects Literal Union Types:
```typescript
let choice: "yes" | "no" | "maybe";
let dayOfWeek: "Monday" | "Tuesday" | "Wednesday" | "Thursday" | "Friday" | "Saturday" | "Sunday";
```

## Enums
```typescript
enum Choice {
  No, // 0
  Yes, // 1
  Maybe, // 2
}

enum StringChoice {
  No = "No",
  Yes = "Yes",
  Maybe = "Maybe",
}

const enum HeterogenousChoice {
  No = 0,
  Yes = 1,
  Maybe = "Maybe",
}

const enum StartFromTwoChoice {
  no = 2, //2
  yes, //3
  maybe, //4
}
```

## Tuples
```typescript
let myTuple: [string, number] = ["R", 21];
let color: [number, number, number] = [155, 255, 27];

type HTTPResponse = [number, string];
const reponse: HTTPResponse[]: [[200, "OK"],[404, "Not found"]];
```
## Array Union
```TypeScript
let myArray: (number | boolean) = [true, 15, 25, false];
```

## Interfaces
```typescript
interface Person {
  first: string;
  middle?: string; // Optional property
  last: string;
  born: number;
  readonly id?: string | number; // Optional and immutable
  sayHi(name: string): string; // Method definition
}

const thomas: Person = {
  first: "Thomas",
  last: "Hardy",
  born: 1977,
  id: "A1234",
  sayHi: (name: string = "mate") => {
    return `Hi ${name}`;
  },
};
```

## Interface are partial
```TypeScript
interface Dog {
  breed: string;
  bark(): string;
};

interface Dog {
  name: string;
};

const ax: Dog = {
    breed: Dalmatian,
    name: "Ax",
    bark() => "woof";
  }
} 
```
## Interface extends multiple interfaces
```TypeScript
interface Dog {
  breed: string;
  bark(): string;
};
interface Dog {
  name: string;
};

interface ServiceDog exteds Dog {
  job: "drug sniffer" | "bomb sniffer" | "guide dog";
}
const jack: ServiceDog = {
    breed: "German Sheppard",
    name: "Jack",
    job: "drug sniffer",
    bark() => "woof",
  }
}
```

## Generics
```typescript
function identity<T>(item: T): T {
  return item;
}
identity<number>(7);
identity<string>("hello");

function getRandomElement<T>(list: T[]): T {
  const randIndex = Math.floor(Math.random() * list.length);
  return list[randIndex];
}
```

```TypeScript
function getRandomElement<T>(list: T[]):T{
  const randIndex = Math.floor(Math.ranom() * list.lenght)
  retun list[randIndex];
}
getRandomElement<string>(["Hi", "Hello"])
```

## Generic in TypeScript for jsx
//`<T,>`
```TypeScript
const getRandomElement = <T,>(list: T[]):T => {
  const randIndex = Math.floor(Math.ranom() * list.lenght)
  retun list[randIndex];
}
getRandomElement<string>(["Hi", "Hello"])
```

## Generic for merged Objects
//`<T,U,V>`
```TypeScript
function merge<T,U>(object1: T, object2: U): T&U{
  retun {
      ...object1,
      ...object2,
  };
}
const  carOwner = merge({name: "John"}, {cars: ["Volvo", "Subaru"]});
```

## Generic for merged Objects with extends
//`<T extends object,U extends object,V extends object>`
```TypeScript
function merge<T extends object,U extends object>(object1: T, object2: U): T&U{
  retun {
      ...object1,
      ...object2,
  };
}
const  carOwner = merge({name: "John"}, {cars: ["Volvo", "Subaru"]});
```

## Generic Extends
```TypeScript
interface Lengthy {
 length: number;
}
function doubleleLength<T extends Lengthyt>(thing: T): number{
  retun thing.length *2
}
```

## Generic Default
// T = DefaultValue
```TypeScript
function emptyArray<T = number>(thing: T): number{
  retun [];
}
```

## Generic in a class
```TypeScript
interface Song {
  title : string;
  artist: string;
}
interface Video {
  title: string;
  creator: string;
  resolution: string;
}
class Playlist <T> {
  public queue: T[] = []
  add(el: T){
    this.queue.push(el)
  }
}
const myMusicPlaylist = new Playlist<Song>();
const myVideoPlaylist = new Playlist<Video>();
```

## Typeof Guards
//if (typeof  === "") narrowing down the type; usualy with optional type
```TypeScript
function triple(value: number| string): number | string{
   if(typeof value === "string"){
        //do methods of that class type
        return value.repeat(3);
   }
    return value * 3; 
}
```

## Truthiness Guard
//eliminate falsely value
```TypeScript
const printLetters = (word: string : null)=> {
    if(!word){
        console.log("No word has been provided");
    }
}
const printLetters = (word?: string)=> {
    if(word){
        // Do string stuff
    }
}
```

## Property Guard by in
//narrowing by include property
```TypeScript
interface Movie {
    title: string,
    duration: number
}
interface TvShow {
    title: string,
    episode: number
    episodeDuration: number
}

function getDuration(film: Movie | TvShow): number{
  if("episodeDuration" in media){
      // Do TvShow methods
      return film.episode * film.episodeDuration; 
  }
    return film.duration;
}
```
## Property Guard by as
```TypeScript
function makeNoise(pet: Dog | Cat): bool{
    return (pet as Cat).meow !== undefined
}
```

# Instanceof Guard
//narrowing by include property
```TypeScript
function printFullDate(date: string | Date): void{
    if(date insatnceof Date){
        // Do Date methods
        console.log(date.toUTCString()); 
    }else{
        console.log(date);
    }
}
```

# Guard by Predicate with `is` and `as` 
//returning `pet is Cat`
```TypeScript
interface Cat {
  name: string;
  numLives: number;
}
interface Dog {
  name: string;
  bareed: string;
}

function isCat(pet: Cat | Dog): pet is Cat {  // Guard by Predicate with `is` and `as` 
    return (pet as Cat).numlives !== undefined
}

function makeNoise(pet: Dog | Cat): string{
    if(isCat(pet)){
      return "Meow";
    }
    return "Woof";
}
```

# Discriminated unions 
// adding a property that diferenciate like category or type
```TypeScript
interface Rooster {
  name: string;
  age: number;
  weight: number;
  type: "Rooster";
  category:"Birds"
}
interface Cow {
  name: string;
  bareed: string;
  weight: string;
  type: "Cow";
  category:"Mammals"
}

interface Pig {
  name: string;
  bareed: string;
  weight: string;
  type: "Pig";
  category:"Mammals"
}

type FarmAnimal = Pig | Rooster | Cow;

function printIfFarmAnimalHasFeathers(animal: FarmAnimal){
    switch(animal.category){
      case("Bird"):
        Console.log(`${animal.type} has feathers`)
      case("Mammals")
        Console.log(`${animal.type} does not has feathers`)
    }
}
