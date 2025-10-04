# TypeScript Generics Cheat Sheet

## Generics Basics

```ts
function identity<T>(item: T): T {
  return item;
}
identity<number>(7);
identity<string>("hello");
```

## Generic Functions

```ts
function getRandomElement<T>(list: T[]): T {
  const randIndex = Math.floor(Math.random() * list.length);
  return list[randIndex];
}

getRandomElement<string>(["Hi", "Hello"]);
```

## Generics in TSX/JSX

Use a trailing comma `<T,>` to avoid confusion with JSX:

```ts
const getRandomElement = <T,>(list: T[]): T => {
  const randIndex = Math.floor(Math.random() * list.length);
  return list[randIndex];
};

getRandomElement<string>(["Hi", "Hello"]);
```

## Merging Objects

```ts
function merge<T, U>(object1: T, object2: U): T & U {
  return {
    ...object1,
    ...object2,
  };
}

const carOwner = merge({ name: "John" }, { cars: ["Volvo", "Subaru"] });
```

## Merging Objects with Constraints

```ts
function mergeObjects<T extends object, U extends object>(object1: T, object2: U): T & U {
  return {
    ...object1,
    ...object2,
  };
}

const carOwner2 = mergeObjects({ name: "John" }, { cars: ["Volvo", "Subaru"] });
```

## Generic Constraints (Extends)

```ts
interface Lengthy {
  length: number;
}

function doubleLength<T extends Lengthy>(thing: T): number {
  return thing.length * 2;
}
```

## Generic Default Types

```ts
function emptyArray<T = number>(): T[] {
  return [];
}

const numbers = emptyArray(); // inferred as number[]
const strings = emptyArray<string>(); // explicit string[]
```

## Generics in Classes

```ts
interface Song {
  title: string;
  artist: string;
}

interface Video {
  title: string;
  creator: string;
  resolution: string;
}

class Playlist<T> {
  public queue: T[] = [];
  add(el: T) {
    this.queue.push(el);
  }
}

const myMusicPlaylist = new Playlist<Song>();
const myVideoPlaylist = new Playlist<Video>();
```
