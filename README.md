# myTypeScript_CheatSheet

## Installation

* Install Node.js
* `npm -v` — check npm version
* `npm i -D typescript` — install TypeScript for one project
* `npm i -g typescript` — global install (optional)
* `tsc -v` — check TypeScript version

## Configuration

* In your project folder, run `tsc --init` to create `tsconfig.json`.
* **Top level (outside `compilerOptions`)**:

  * `"include": ["src/ts/**/*"]` — which files to compile
  * `"exclude": ["dist", "build"]` — optional; TS auto‑ignores `node_modules`
  * *(Use `files` only if you need an explicit allow‑list.)*
* **Inside `compilerOptions`** (common setup):

  ```json
  {
    "compilerOptions": {
      "target": "ES2020",
      "module": "ESNext",
      "lib": ["ES2020", "DOM"],
      "strict": true,
      "outDir": "./dist",
      "rootDir": "./src",
      "noEmitOnError": true,
      "esModuleInterop": true,
      "skipLibCheck": true,
      "forceConsistentCasingInFileNames": true
      // "allowJs": false,
      // "checkJs": false
    },
    "include": ["src/**/*"]
  }
  ```

## Compile TypeScript

* `tsc file.ts` — compile a single file
* `tsc -w` — watch mode for the project
* `tsc` — compile the whole project (uses `tsconfig.json`)
* ⚠️ `-outFile` is only for non‑module or AMD/System modules. For bundling CommonJS/ESM, use esbuild/rollup/webpack/tsup.

## Types

```ts
let myString: string = "Words";
let myNumber: number = 1;
let myBoolean: boolean = true;
let myAny: any = true; // try to avoid `any`
let undefinedString: string | undefined; // assign later or allow undefined
let names1: string[] = ["Mary", "John"];
let names2: Array<string> = ["Mary", "John"];
let board: string[][] = [["X", "O", "O"], ["O", "X", "X"], ["X", "O", "X"]];
let id: number | string = 21;
id = "R21";
let arrayUnion: (string | number)[] = [21, "R21"];
let myTuple: [string, number] = ["R", 21]; // fixed length & positions
```

## Methods

```ts
function greet(person: string, age: number, funny: boolean, timeOfDay: string = "day"): string {
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

function neverEnds(): never {
  while (true) {
    // do stuff forever
  }
}
```

## Objects

```ts
const coordinates: { latitude: number; longitude: number } = { latitude: 255, longitude: 459 };

function printName(person: { first: string; last: string }): void {
  console.log(`${person.first} ${person.last}`);
}
```

## Object Type Aliases

```ts
type Coordinates = {
  readonly latitude: number; // cannot be reassigned
  longitude: number;
  altitude?: number; // optional
};

const point: Coordinates = { latitude: 255, longitude: 459 };

type Person = {
  first: string;
  middle?: string; // optional
  last: string;
  readonly id?: string; // optional + readonly
};

function printPersonName(person: Person): void {
  console.log(`${person.first} ${person.last}`);
}
```

## Object Intersection Types

```ts
type Color = { color: string };
type Hat = { size: string };
type ColorfulHat = Hat & Color & { model: string };

const happyHat: ColorfulHat = {
  size: "medium",
  color: "yellow",
  model: "bubbles",
};
```

## Union Types with Narrowing

```ts
function printAge(age: number | string): void {
  if (typeof age === "string") {
    console.log(`You are ${age} years old`);
  } else {
    console.log(`You are ${age} years old`);
  }
}

printAge(25);
printAge("twenty five");
```

## Literal Types

```ts
let zero: 0 = 0;
let choice: "yes" | "no" | "maybe";
let dayOfWeek: "Monday" | "Tuesday" | "Wednesday" | "Thursday" | "Friday" | "Saturday" | "Sunday";
```

## Enums

```ts
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

const enum StartFromTwoChoice {
  no = 2,
  yes, // 3
  maybe, // 4
}
```

## Tuples

```ts
let myTuple2: [string, number] = ["R", 21];
let color: [number, number, number] = [155, 255, 27];

type HTTPResponse = [number, string];
const response: HTTPResponse[] = [[200, "OK"], [404, "Not Found"]];
```

## Array Union

```ts
let myArray: (number | boolean)[] = [true, 15, 25, false];
```

## Interfaces

```ts
interface PersonI {
  first: string;
  middle?: string;
  last: string;
  born: number;
  readonly id?: string | number;
  sayHi(name: string): string;
}

const thomas: PersonI = {
  first: "Thomas",
  last: "Hardy",
  born: 1977,
  id: "A1234",
  sayHi: (name: string = "mate") => {
    return `Hi ${name}`;
  },
};
```

## Interface merging (declaration merging)

```ts
interface Dog {
  breed: string;
  bark(): string;
}

interface Dog {
  name: string;
}

const ax: Dog = {
  breed: "Dalmatian",
  name: "Ax",
  bark: () => "woof",
};
```

## Interface extends (including multiple)

```ts
interface Named { name: string }

interface DogBase {
  breed: string;
  bark(): string;
}

interface ServiceDog extends DogBase, Named {
  job: "drug sniffer" | "bomb sniffer" | "guide dog";
}

const jack: ServiceDog = {
  breed: "German Shepherd",
  name: "Jack",
  job: "drug sniffer",
  bark: () => "woof",
};
```

## Generics

```ts
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

### Generics in TSX/JSX files

```ts
// note the trailing comma after T to avoid JSX parsing
const getRandomElementJsx = <T,>(list: T[]): T => {
  const randIndex = Math.floor(Math.random() * list.length);
  return list[randIndex];
};
getRandomElementJsx<string>(["Hi", "Hello"]);
```

## Generic merge

```ts
function merge<T, U>(object1: T, object2: U): T & U {
  return {
    ...object1,
    ...object2,
  } as T & U;
}
const carOwner = merge({ name: "John" }, { cars: ["Volvo", "Subaru"] });

function mergeConstrained<T extends object, U extends object>(o1: T, o2: U): T & U {
  return { ...o1, ...o2 } as T & U;
}
```

## Generic Extends

```ts
interface Lengthy { length: number }

function doubleLength<T extends Lengthy>(thing: T): number {
  return thing.length * 2;
}
```

## Generic Default

```ts
function emptyArray<T = number>(): T[] {
  return [] as T[];
}
```

## Typeof Guards

```ts
function triple(value: number | string): number | string {
  if (typeof value === "string") {
    return value.repeat(3);
  }
  return value * 3;
}
```

## Truthiness Guards

```ts
const printLetters = (word: string | null) => {
  if (!word) {
    console.log("No word has been provided");
    return;
  }
  // use word as string
};

const printLetters2 = (word?: string) => {
  if (word) {
    // Do string stuff
  }
};
```

## `in` Property Guard

```ts
interface Movie {
  title: string;
  duration: number;
}
interface TvShow {
  title: string;
  episodes: number;
  episodeDuration: number;
}

function getDuration(film: Movie | TvShow): number {
  if ("episodeDuration" in film) {
    return film.episodes * film.episodeDuration;
  }
  return film.duration;
}
```

## `as` (type assertion) Guard (prefer property checks when possible)

```ts
interface Cat { name: string; meow(): string }
interface Dog2 { name: string; bark(): string }

function makeNoise(pet: Dog2 | Cat): string {
  if ("meow" in pet) {
    return pet.meow();
  }
  return pet.bark();
}
```

## `instanceof` Guard

```ts
function printFullDate(date: string | Date): void {
  if (date instanceof Date) {
    console.log(date.toUTCString());
  } else {
    console.log(date);
  }
}
```

## Predicate Guard (`is`)

```ts
interface Cat2 {
  name: string;
  numLives: number;
}
interface Dog3 {
  name: string;
  breed: string;
}

function isCat(pet: Cat2 | Dog3): pet is Cat2 {
  return (pet as Cat2).numLives !== undefined;
}

function makeNoise2(pet: Dog3 | Cat2): string {
  if (isCat(pet)) {
    return "Meow";
  }
  return "Woof";
}
```

## Discriminated Unions

```ts
interface Rooster {
  type: "Rooster";
  category: "Bird";
  name: string;
  age: number;
  weight: number;
}
interface Cow {
  type: "Cow";
  category: "Mammal";
  name: string;
  breed: string;
  weight: number;
}
interface Pig {
  type: "Pig";
  category: "Mammal";
  name: string;
  breed: string;
  weight: number;
}

type FarmAnimal = Pig | Rooster | Cow;

function printIfFarmAnimalHasFeathers(animal: FarmAnimal): void {
  switch (animal.category) {
    case "Bird":
      console.log(`${animal.type} has feathers`);
      break;
    case "Mammal":
      console.log(`${animal.type} does not have feathers`);
      break;
  }
}
```

---

### Bonus tips

* Prefer `unknown` over `any` when you must accept arbitrary values—then **narrow**.
* Enable `strict` and keep it on. Add targeted `// @ts-expect-error` only when you truly mean it.
* For Node ESM projects, consider `"module": "NodeNext"` and matching `"type": "module"` in `package.json`.
