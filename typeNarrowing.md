# TypeScript Type Guards Cheat Sheet

## `typeof` Guard

Use when narrowing primitives.

```ts
function triple(value: number | string): number | string {
  if (typeof value === "string") {
    // string methods available here
    return value.repeat(3);
  }
  return value * 3;
}
```

## Truthiness Guard

Eliminate **falsy** values (`""`, `0`, `null`, `undefined`, `NaN`, `false`).

```ts
const printLetters = (word: string | null) => {
  if (!word) {
    console.log("No word has been provided");
    return;
  }
  // `word` is string here
};

const printLetters2 = (word?: string) => {
  if (word) {
    // `word` is string here
  }
};
```

## Property Guard with `in`

Narrow using the presence of a property.

```ts
interface Movie {
  title: string;
  duration: number;
}
interface TvShow {
  title: string;
  episode: number; // episode count
  episodeDuration: number;
}

function getDuration(film: Movie | TvShow): number {
  if ("episodeDuration" in film) {
    // TvShow branch
    return film.episode * film.episodeDuration;
  }
  // Movie branch
  return film.duration;
}
```

## Narrowing with a Type Assertion (`as`)

Prefer property checks, but if you must assert:

```ts
interface Cat { meow(): string }
interface Dog { bark(): string }

function isCatLoose(pet: Dog | Cat): boolean {
  // Not as safe as an `in` check, but demonstrates `as`
  return (pet as Cat).meow !== undefined;
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

## Guard by Predicate (`is`)

Custom type predicate returns `pet is Cat`.

```ts
interface Cat {
  name: string;
  numLives: number;
}
interface Dog {
  name: string;
  breed: string;
}

function isCat(pet: Cat | Dog): pet is Cat {
  // Either property check or assertion-based check
  return (pet as Cat).numLives !== undefined;
  // return "numLives" in pet; // (safer alt.)
}

function makeNoise(pet: Dog | Cat): string {
  if (isCat(pet)) {
    return "Meow";
  }
  return "Woof";
}
```

## Discriminated Unions

Add a common literal property (e.g., `type` or `category`) to discriminate.

```ts
interface Rooster {
  name: string;
  age: number;
  weight: number;
  type: "Rooster";
  category: "Bird";
}
interface Cow {
  name: string;
  breed: string;
  weight: number;
  type: "Cow";
  category: "Mammal";
}
interface Pig {
  name: string;
  breed: string;
  weight: number;
  type: "Pig";
  category: "Mammal";
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
    default:
      // Exhaustiveness check (uncomment to make compiler error on new variants):
      // const _exhaustive: never = animal;
      break;
  }
}
```
