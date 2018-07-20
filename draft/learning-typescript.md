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
  constructor(private name: string, private age: number) {
  }

  get userName(): string {
    return this.name;
  }
  set userAge(num: number) {
    this.age = num;
  }

  say(): string {
    return 'Hello ' + this.name;
  }
}

let man = new User('Taro', 20);

console.log(man.say()); // Hello Taro
```




















