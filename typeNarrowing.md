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
        Console.log(`${animal.type} has feathers`);
      case("Mammals")
        Console.log(`${animal.type} does not has feathers`);
      default:
        //we should never get here
        //Console.log(`${animal.type} does not have the category set or is unknown `);
        const shouldNeverGetHere: never = animal; // throwing an error if the type is not in swithch
    }
}
```
