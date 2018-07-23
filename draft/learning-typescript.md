# TypeScriptの書き方を光速で学ぶ


## 変数

```
let a: boolean = true;
let b: number = 1;
let c: string = 'foo';
```

## 配列

```
let a: string[] = ['foo', 'bar', 'baz'];
let b: Array<number> = [1, 2, 3];
```

## タプル

```
let a: [string, number] = ['foo', 1];
```

## ユニオン

```
let a: string | number;
a = 'foo';
a = 1;
```

## 列挙型

```
enum Seasons {
  SPRING,
  SUMMER,
  AUTUMN,
  WINTER
};

let season: Seasons = Seasons.SUMMER;

switch (season) {
  case Seasons.SPRING:
    console.log('春');
    break;
  case Seasons.SUMMER:
    console.log('夏');
    break;
  case Seasons.AUTUMN:
    console.log('秋');
    break;
  case Seasons.WINTER:
    console.log('冬');
    break;
  default:
}

// 夏
```

## 関数

```
function triangle(a: number, b: number): number {
  return a * b / 2;
}
```

### デフォルト引数

```
function func(a: string = 'foo'): string {
  return a;
}
```

### オプション引数

```
function func(a?: string): string {
  return 'foo';
}
```

### 可変長引数

```
function func(...a: string[]): number {
  return a.length;
}

func('foo', 'bar', 'baz'); // 3
```

## 型アサーション

```
function func(a: string): string {
  return a;
}

func(<any>1); // 1
func(2 as any); // 2
```

## クラス

```
class User {
  constructor(public name: string, public age: number) {
  }

  get userName(): string {
    return this.name;
  }
  set userAge(num: number) {
    this.age = num;
  }

  profile(): string {
    return this.name + ": " + this.age;
  }
}

let man = new User('Taro', 20);
man.userAge = 21;

console.log(man.profile()); // Taro: 21
```

### 継承

```
class Employee extends User {
	constructor(public position: string) {
		super('Jiro', 21)
	}

	profile(): string {
		return super.profile() + ', ' + this.position;
	}
}

var man = new Employee('Engineer');

console.log(man.profile()); // Jiro: 21, Engineer
```

### 抽象クラス

```
abstract class Person {
  abstract greeting(stg: string): string;
}

class User extends Person {
  constructor(public name: string) {
    super()
  }

  greeting(stg: string = 'Helo'): string {
    return stg + ' ' + this.name;
  }
}

let man = new User('Taro');
console.log(man.greeting()); // Hello Taro
```

### インターフェース

```
interface User {
  name: string;
  age: number;
  profile(): string;
}

class Employee implements User {
  constructor(public name: string, public age: number) {
  }

  profile(): string {
    return this.name + ": " + this.age;
  }
}
```


## オブジェクト型


###　プロパティシグネチャ

```
let obj: {
  name: string,
  age: number,
}
```

### メソッドシグネチャ

```
let obj: {
  func(a: string): string;
}
```

### コールシグネチャ

```
let obj: {
  (a: string): string;
}
```

### コンストラクタシグネチャ

```
let obj: {
  new (name: string): User;
}

Class User {
  constructor
}


















































































