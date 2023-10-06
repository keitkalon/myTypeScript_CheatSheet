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
