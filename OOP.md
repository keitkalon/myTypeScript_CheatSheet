```TypeScript
const badWords = [];

class Player {
  public readonly first: string;
  readonly last: string; // this is also public
  public isCaptain: boolean = false;
  static description: string = "Player"; // property of a class an not of an instance
  score = 0;
  power= 10;
  protected slogan: string = "BOOOYAH"
  private id: string =  "795875" // private
  constructor(first, last){
     this.first = first;
     this.last = last;
  }
  static random(): Player { // method of a class
     return new Player("Unknown","Anonymous");
  }
  shouted(): void {
     console.log(`${this.first.toUpperCase()} ${this.slogan.toUpperCase()}!!!`);
  }
  tired (): void {
     this.power -= 1;
  }
  get Id(): string{ // getter
     return this.#id;
  }
  private internalMethod(): string { // private method
     return "Secret";
  }
  get fullname() string{ // getter
     return this.first + " " + this.last;
  }
  set slogan(value: string): void { // setter
     if(!badWords.includes(value) ){
        #slogan = values;
     } else{
        throw new Error("No bad words")
     };
  }
}

class Captain extends Player { 
  constructor(first: string, last: string, public players: Players []) {
    super(first: string, last: string);
    this.#isCaptain = true;
    this.players = players;
  }
}

const player1 : Player = new Player("John","McUp")
const player2 : Player = new Player("Ian","McDown")
const player3 : Player = Player.random();
const player4 : Captain = new Captain("Andy","McCaptain", [player1, player2, player3])


player1.shouted();
player2.shouted();
console.log(player3)

```
private identifier `#id` JS & TS
private modifire `private id` TS
