# TypeScript Classes Cheat Sheet

## Basic Class

```ts
class Person {
  name: string;
  age: number;
  id: number;

  constructor(_name: string, _age: number, _id: number) {
    this.name = _name;
    this.age = _age;
    this.id = _id;
  }
}

const John: Person = new Person("John", 23, 1);
const Maria: Person = new Person("Maria", 33, 2);
const Tony: Person = new Person("Tony", 45, 3);

const employees: Person[] = [John, Maria, Tony];

const employeeAges: number[] = employees.map((p) => p.age);
const employeeNames: string[] = employees.map((p) => p.name);

// Create a dictionary keyed by id, with whole Person objects
const employeeDict: Record<number, Person> = employees.reduce(
  (acc, curr) => ({ ...acc, [curr.id]: curr }),
  {} as Record<number, Person>
);

// Create a dictionary keyed by id, with name and age only
const employeeDict2: Record<number, { name: string; age: number }> = employees.reduce(
  (acc, curr) => ({ ...acc, [curr.id]: { name: curr.name, age: curr.age } }),
  {} as Record<number, { name: string; age: number }>
);

console.log(employeeDict2);
```

---

## Interfaces & Implements

```ts
interface Colorful {
  color: string;
}

class Jacket implements Colorful {
  constructor(public brand: string, public color: string) {}
}

const jacket = new Jacket("Tommy Hilfiger", "blue");
```

---

## Abstract Classes

```ts
abstract class Employee {
  constructor(public salary: number, public id: string) {}
  abstract getPay(): number;
}
```

---

## Inheritance and Modifiers

```ts
class Player extends Employee {
  public readonly first: string;
  public readonly last: string;
  public isCaptain: boolean = false;

  static description: string = "Player"; // static property

  score = 0;
  power = 10;

  private slogan: string = "BOOOYAH";

  constructor(first: string, last: string, salary: number, id: string) {
    super(salary, id);
    this.first = first;
    this.last = last;
  }

  // static method
  static random(): Player {
    return new Player("Unknown", "Anonymous", 2500, "#3");
  }

  // instance methods
  shouted(): void {
    console.log(`${this.first.toUpperCase()} ${this.slogan.toUpperCase()}!!!`);
  }

  tired(): void {
    this.power -= 1;
  }

  // getter
  get Id(): string {
    return this.id;
  }

  // private method
  private internalMethod(): string {
    return "Secret";
  }

  // getter
  get fullname(): string {
    return this.first + " " + this.last;
  }

  // setter
  set setSlogan(value: string) {
    const badWords: string[] = [];
    if (!badWords.includes(value)) {
      this.slogan = value;
    } else {
      throw new Error("No bad words");
    }
  }

  // abstract method implementation
  getPay(): number {
    return this.salary;
  }
}

class Captain extends Player {
  constructor(
    first: string,
    last: string,
    public players: Player[],
    salary: number,
    id: string
  ) {
    super(first, last, salary, id);
    this.isCaptain = true;
    this.players = players;
  }
}
```

---

## Usage

```ts
const player1: Player = new Player("John", "McUp", 1500, "#1");
const player2: Player = new Player("Ian", "McDown", 2000, "#2");
const player3: Player = Player.random();
const player4: Captain = new Captain("Andy", "McCaptain", [player1, player2, player3], 4000, "#4");

player1.shouted();
player2.shouted();
console.log(player4);
```

---

## Access Modifiers Summary

* `public` → accessible everywhere (default)
* `private` → accessible only inside the class
* `protected` → accessible in the class and subclasses
* `readonly` → cannot be reassigned after initialization

---

## Private Fields vs Private Modifiers

* **Private field (`#id`)** → JavaScript native, truly private at runtime (works in JS & TS).
* **Private modifier (`private id`)** → TypeScript compile-time only; not enforced at runtime.

```ts
class Example {
  #secret: string; // JS private field
  private id: string; // TS private modifier

  constructor(secret: string, id: string) {
    this.#secret = secret;
    this.id = id;
  }
}
```
