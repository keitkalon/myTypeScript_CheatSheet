```TypeScript
class Person {    
    name:string;
    age: number;
    id: number;
    constructor(_name:string, _number:number, _id : number){
        this.name = _name;
        this.age = _number;
        this.id = _id
    }
}

const John: Person = new Person('John', 23, 1);
const Maria: Person = new Person('Maria',33, 2);
const Tony: Person = new Person('Tony', 45, 3);

const Employee : Person[] = [John, Maria, Tony];

const EmployeeAge : number[] = Employee.map(p => p.age);

const EmployeeNames = Employee.map(p => p.name);

const EmployeeDict : {} = Employee.reduce((acc, curr) => ({...acc, [curr.id] : curr }) ,{});
const EmployeeDict2 : {} = Employee.reduce((acc, curr) => ({...acc, [curr.id] : {name : curr.name, age: curr.age} }) ,{});



console.log(EmployeeDict2 )

interface Colorful {
 color: string;
}

class Jacket implements Colorful{
 constructor(public brand: string, public color: string){}
}

const jacket = new Jacket("Tommy Hilfiger", "blue"); 
const badWords: string[] = [];

abstract class Employee {  
  constructor(public salary: number, public id: string){}
  abstract getPay(): number;
}

class Player extends Employee {
  public readonly first: string;
  readonly last: string; // this is also public
  public isCaptain: boolean = false;
  static description: string = "Player"; // property of a class an not of an instance
  score = 0;
  power= 10;
  private slogan: string = "BOOOYAH"
  public id: string =  "795875" // private
  constructor(first:string, last:string, salary : number, id: string){
  super(salary, id)
     this.first = first;
     this.last = last;
  }
  static random(): Player { // method of a class
     return new Player("Unknown","Anonymous", 2500, '#3');
  }
  shouted(): void {
     console.log(`${this.first.toUpperCase()} ${this.slogan.toUpperCase()}!!!`);
  }
  tired (): void {
     this.power -= 1;
  }
  get Id(): string{ // getter
     return this.id;
  }
  private internalMethod(): string { // private method
     return "Secret";
  }
  get fullname(): string{ // getter
     return this.first + " " + this.last;
  }
  set setSlogan(value: string){ // setter
     if(!badWords.includes(value)){
        this.slogan = value;
     } else{
        throw new Error("No bad words")
     };
  }
  getPay(): number{
     return this.salary;
  }
}

class Captain extends Player { 
  constructor(first: string, last: string, public players: Player [],salary: number, id: string ) {
    super( first, last,salary, id);
    this.isCaptain = true;
    this.players = players;
  }
}

const player1 : Player = new Player("John","McUp", 1500, "#1")
const player2 : Player = new Player("Ian","McDown", 2000,"#2" )
const player3 : Player = Player.random();
const player4 : Captain = new Captain("Andy","McCaptain", [player1, player2, player3], 4000, "#4")


player1.shouted();
player2.shouted();
console.log(player4)

```
private identifier `#id` JS & TS
private modifire `private id` TS
