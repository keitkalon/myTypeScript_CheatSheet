# myTypeScript_CheatSheet

## Installation
- install node js
- `npm -v` // check npm version
- `npm install tyepescript --save-dev` // only for one project
- `npm install -g typescript` // for global
- `tsc -v` // check typescript version

## Configuration
- go into your project
- `tsc --init` // create tsconfig.json file
- open your tsconfig file
  - outside of compilerOptions // where top level options are; those you have to write them yourself 
    - `"file":  [ "index.ts", "file.ts", "utilities.ts"] ` // all the file that you want typescript to be included in compilation anything else will be ignored
    - `"include": ["src/ts"]` // only compile the files from src directory delete "file" option; you can use patterns like ["*use.tsc"]
    - `"exclude": ["src/ts/dontTuch.ts", src/ts/"ignore.ts"]` // don`t compile this files, you can use patterns like ["doteUse/**.*", **.test.ts]; the libraries ts files like node_modules should only be excluded if ASK QUESTIONS
  - inside of compilerOptions
    - `"outdir": ["./src/js/compiled"]`// where we going to put the .js compiled files default is just beside .ts files
    - `"target": "es5"` // javaScript version that yoor compiled files will be can be es6/es2015 or "es2016" ~ "es2022" or "esnext"
    - `"strict": true` // underneath this option you can find detailed strict options// strict: true activate all of them
    - `"lib"`
    - `"modules"`
    - `"allowJs": true` // add js files to your ts
    - `"checkJs": true` // checks also .js file for errors
    - `"noEmitOnError": true`// if there are error in ts don`t compile files

## Compile TypeScript
- `tsc file.ts` // create a .js file base on your .ts file
- `tsc -outFile common.js f1.ts f2.ts f3.ts` create a .js file base o multiple .ts files
- `tsc -outFile common.ts f2.ts f3.ts` create a .js file base o multiple .ts files
- `tsc -w file.ts` //  compile in watch mode, checks for errors and changes and display them in terminal
- `tsc -watch file.ts` //  the same as -w
- `tsc` compile all files in folder

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
let arrayUnion: (string | number)[] = [21, "R21"];
let myTuple: [string, number] = ["R", 21]; // fix length and fix type per tuple index
```

## Methods:
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

## Objects:
```TypeScript
const coordinates: {latitude: number, longitude: number} = {latitude: 255, longitude: 459};

function printName(person: {first: string; last: strinh}): void  {
  console.log(`${person.first} ${person.last}`);
}
```

## Objects Type Aliases:
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

## Objects Intersection Types:
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

## Objects Union Types with type narrowing:
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
## Objects Literal Types:
```TypeScript
let zero: 0 = 0;
```
Objects Literal Union Types:
```TypeScript
ler choice: yes | no | maybe;
let DaysOfWeek: "Monday" | "Tuesday" | "Wednesday" | "Thursday" | "Friday" | "Saturday" | "Sunday";
```
## Enums they are very verbose in JS
```TypeScript
enum Choice {
  no, //1
  yes, //2
  maybe, //3
}
enum Choice { // String Enums
  no = "No", 
  yes = "Yes" ,
  maybe = "Maybe", 
}
enum Choice { // Heterogenous Enums
  no = 0, 
  yes = 1 ,
  maybe = "Maybe", 
}
```
Const Enums not so verbose in JS
```TypeScript
const enum Choice {
  no = 2, //2
  yes, //3
  maybe, //4
}
const enum Choice { // String Enums
  no = "No", 
  yes = "Yes" ,
  maybe = "Maybe", 
}
const enum Choice { // Heterogenous Enums
  no = 0, 
  yes = 1 ,
  maybe = "Maybe", 
}
```
## Tuples:
```TypeScript
let myTuple: [string, number] = ["R", 21]; // fix length and fix type per tuple index
let color : [number, number, number] = [155, 255, 27];
type HTTPResponse = [number, string];
const reponse: HTTPResponse[]: [[200, "OK"],[404, "Not found"]];
// careful you can push or pop extra elements
```
## Array Union
```TypeScript
let myArray: (number | boolean) = [true, 15, 25, false];
```
## Interface 
```TypeScript
interface Person { // an Interface can be only an Object and can have methods, maine differance between Interface and Types
  first: string;
  middle?: string; // "?" not required
  last: string;
  born: number;
  readonly id?: number | string;
  sayHi: (name: string) => string; // this is a method that return string
  sayHi2(name: string)?: string; // this also works
  age(todayyear: number):number; 
};

const thomas: Person = {
  first: "Thomas",
  last: "Hardy",
  born: 1977,
  id: "A1234",
  sayHi: (name = "mate") => {
    return `Hi ${name}`;
  };
  age(year: number = this.born){
    return new Date().getFullYear() - year;    
  }
} 
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
```TypeScript
function identity<T>(item: T): T {
 return item;
}
identity<number>(7);
identity<string>("hello");
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
```
