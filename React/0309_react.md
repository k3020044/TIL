# 3/9 타입스크립트

---

### 자바스크립트와의 차이점

- 자바스크립트

```jsx
function sum(a, b) {
  return a + b;
}
//정적 타입을 지원하지 않으므로 어떤 타입의 반환값을 리턴해야 하는지 명확하지 않음
```

- 타입스크립트

```jsx
function sum(a:number, b:number) {
  return a + b;
}
//정적 타입을 지원, 컴파일 단계에서 오류를 포착할 수 있는 장점이 있음
```

### 기본 타입

```jsx
// string
let car:string = 'bmw'

// number
let age:number = 30

// boolean
let isAdult:boolean = true

// array
let a:number[] = [1, 2, 3]
let a:Array<number> = [1, 2, 3]

let week:string[] = ['mon', 'tue', 'wed']
let week:Array<string> = ['mon', 'tue', 'wed']

// tuple
let b:[string, number] // 튜플 첫번째 원소는 문자, 두번째 원소는 숫자라는 뜻

// void : 반환값이 없을 때
function sayHello():void{
    console.log('hello')
}

// never : 항상 에러를 반환하거나 영원히 끝나지 않을 때
function showError():never{
    throw new Error()
}

function infLoop():never{
    while (true) {
    }
}

// enum : 비슷한 값들끼리 묶어줄 때
enum Os {
    Window = 3,
    Ios = 10,
    Android
}

let myOs:Os
myOs = Os.Window

// null, undefined
let a:null = null
let b:undefined = undefined
```

### 인터페이스(interface)

```jsx
// object 객체 만들기
type Score = 'A' | 'B' | 'C' | 'F';

interface User {
    name : string;
    age : number;
    gender? : string; // ?는 optional한 속성을 나타낼 때
    readonly birthYear : number // readonly는 변경 불가능한 읽기 전용 속성 나타날 때
    [grade:number] : Score, // 숫자 변수 : 미리 지정한 타입 형식으로 입력 받을 때

let user : User = {
    name : 'kim',
    age : 30,
    birthYear : 2000,
    1 : 'A',
    2 : 'B'
}

user.age = 10; // 변수값 변경, 추가 가능
user.gender = 'male'

// 함수 만들기
// number
interface Add {
    (num1:number, num2:number): number;
}

const add : Add = function(x, y){
    return x + y;
}

add(10, 20);

// boolean
interface IsAdult {
    (age:number):boolean;
}

const a:IsAdult = (age) => {
    return age > 19;
}

a(33) // true

// class
interface Car {
    color: string;
    wheels: number;
    start(): void;
}

class Bmw implements Car {
    color;
    wheels = 4;
    constructor(c:string){
        this.color = c;
    }
    start(){
        console.log('go');
    }
}

const b = new Bmw('green');
console.log(b) // Bmw: { 'wheels':4, 'color': 'green'}

interface Benz extends Car { // 확장해서 사용도 가능함
    door: number;
    stop(): void;
}

const benz : Benz = {
    door : 5,
    stop(){
        console.log('stop');
    },
    color : 'black',
    wheels : 3,
    start(){
        console.log('go');
    }
}

interface Car {
    color: string;
    wheels: number;
    start(): void;
}

interface Toy {
    name: string;
}

interface ToyCar extends Car, Toy { // 여러개도 extends 가능
    price : number;
}
```

### 함수

```jsx
//
function hello(name?: string) {
    return `hello, ${name || "world"}` // 입력값 있으면 반환하고, 없으면 world 반환

const result = hello();

//
function hello(age?: number, name: string): string {
    if (age !== undefined) {
        return `hello, ${name}. you are ${age}`;
    } else {
        return `hello, ${name}`;
    }
}

console.log(hello("kim"))

//
function add(...nums: number[]) {
    return nums.reduce((result, num) => result + num, 0);
}

add(1, 2, 3) // 6

//
interface User {
    name: string;
}

const Sam: User = {name:'Sam'}

function showName(this:User, age:number, gender: 'm'|'f'){
    console.log(this.name, age, gender)
}

const a = showName.bind(Sam);
a(30, 'm')

// 매개변수의 타입에 따라서 다르게 동작하는 함수
interface User {
    name: string;
    age: number;
}

function join(name: string, age: string): string;
function join(name: string, age: number): User;
function join(name: string, age: number | string): User | string {
    if (typeof age === "number") {
        return {
            name,
            age,
        };
    } else {
        return "나이는 숫자로 입력해주세요";
    }
}

const sam: User = join("Sam", 30);
const jane: string = join("Jane", "30")
```

### 리터럴(literal), 유니온/교차 타입

```jsx
// literal

type Job = "police" | "developer" | "teacher";

interface User {
    name : string;
    job : Job;
}

const user: User = {
    name : "Bob",
    job: "developer"
}

// union type(or)

interface Car {
    name: "car";
    color: string;
    start(): void;
}

interface Mobile {
    name: "mobile";
    color: string;
    call(): void;
}

function getGift(gift: Car | Mobile) {
    console.log(gift.color);
    if(gift.name === "car"){
    gift.start();
    } else {
        gift.call();
        }
    }

// intersection type(and)

interface Car {
    name: string;
    start(): void;
}

interface Toy {
    name: string;
    color: string;
    price: number;
}

const toyCar: Toy & Car = {
    name: "타요",
    start() {},
    color: "blue",
    price: 1000
};
```

### 클래스

```jsx
public - 자식 클래스, 클래스 인스턴스 모두 접근 가능
protected - 자식 클래스에서 접근 가능
private - 해당 클래스 내부에서만 접근 가능

class Car {
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log("start");
    }
}

const bmw = new Car("red");
```

### 제네릭(generics)

```jsx
// 선언할 때는 type을 정하지 않고, 입력받을 때 정하는 방법

function getSize<T>(arr: T[]): number {
    return arr.length;
}

const arr1 = [1, 2, 3];
getSize<number>(arr1);

const arr2 = ["a", "b", "c"];
getSize<string>(arr2);

//
interface Mobile<T> {
    name: string;
    price: number;
    option: T;
}

const m1: Mobile<object> = {
    name: "iphone",
    price: 1000,
    option: {
        color: "red",
        coupon: false
    },
};

const m2: Mobile<string> = {
    name: "galaxy",
    price: 900,
    option: "good"
};
```

### 유틸리티 타입

```jsx
// keyof

interface User {
    id: number;
    name: string;
    age: number;
    gender: "m" | "f";
}

type UserKey = keyof User; // 'id' | 'name' | 'age' | 'gender'  
const uk:UserKey = "age";

// Partial : 모든 속성을 optional하게

interface User {
    id: number;
    name: string;
    age: number;
    gender: "m" | "f";
}

let admin: Partial<User> = {
    id: 1,
    name: "Bob",
};

// Required : 모든 속성을 필수로

interface User {
    id: number;
    name: string;
    age?: number;
}

let admin: Required<User> = {
    id: 1,
    name: "Bob",
    age: 30,
};

// Readonly : 모든 속성을 변경 불가하도록

interface User {
    id: number;
    name: string;
    age?: number;
}

let admin: Readonly<User> = {
    id: 1,
    name: "Bob",
};

// Record<Key,Type>
// 첫번째
const score: Record<"1" | "2" | "3" | "4", "A" | "B" | "C" | "D"> = {
    1: "A",
    2: "C",
    3: "B",
    4: "D",
};

// 두번째
type Grade = "1" | "2" | "3" | "4";
type Score = "A" | "B" | "C" | "D";

const score: Record<Grade, Score> = {
    1: "A",
    2: "C",
    3: "B",
    4: "D",
};

// 
interface User {
    id: number;
    name: string;
    age: number;
}

function isValid(user: User) {
    const result: Record<keyof User, boolean> = {
        id: user.id > 0,
        name: user.name !== "",
        age: user.age >0,
    };
    return result;
}

// Pick<Type, Key> : 특정 속성만 선택

interface User {
    id: number;
    name: string;
    age: number;
}

const admin: Pick<User, "id" | "name"> = {
    id: 1,
    name: "Sam",
};

// Omit<Type, Key> : 특정 속성만 생략

interface User {
    id: number;
    name: string;
    age: number;
}

const admin: Pick<User, "age"> = {
    id: 1,
    name: "Sam",
};

// Exclude<T1,T2> : T1에서 T2 속성을 삭제

type T1 = string | number;
type T2 = Exclude<T1, number>; // string만 남게됨

// NonNullable<Type> : null, undefined 삭제

type T1 = string | null | undefined | void;
type T2 = NonNullable<T1>; // string, void만 남음
```