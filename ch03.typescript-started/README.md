# Chap 03. TypeScript 入門

TypeScript 本質上是向 JavaScript 語言添加了可選的靜態類型和基於類的物件導向編程，同時也支持諸如接口、命名空間、裝飾器等特性，它相當於 JavaScript 的超集。

TypeScript 使得代碼編寫更規範化，也更有利於項目的維護。

## 基本類型

- boolean
- number
- string
- array
- tuple
- enum
- any
- null 和 undefined
- void 類型
- never 類型

其中，array、enum、any、void 和 never 是 TypeScript 特有類型

- 靜態類型聲明變數來約束，在編譯時執行類型檢查，可避免一些類型混用的低級錯誤
- 在 TypeScript，數字都是浮點型。同時支持二進制、八進制、十進制和十六進制字面量
- tuple 用來表示已知元素數量和類型的陣列，各元素的類型不必相同

  ```typescript
  let x: [string, number];
  x = ['Angular', 25];
  ```

- enum 是一個可被命名的整型常數的集合，它為集合成員賦予有意義的名稱，增強可讀性

  ```typescript
  enum Color {Red, Green, Blue};
  let c: Color = Color.Blue;
  console.log(c); // 輸出: 2
  ```

  可手動修改默認的下標值。

  ```typescript
  enum Color {Red = 2, Blue, Green = 6};
  let c: Color = Color.Blue;
  console.log(c); // 輸出: 3
  ```

- 任意值是針對編程時類型不明確的變量所使用的一種數據類型，常用於以下三種情況：
  - 當變量的值會動態變化
  - 改寫現有代碼時
  - 定義存儲各種類型資料的陣列時
- null 和 undefined 是其他類型的子類型，可賦值給其他類型。在 TypeScript 中啟用嚴格的空校驗 (--strictNullChecks) 特性，就可使得 null 和 undefined 只能被賦值給 void 或本身對應的類型。
  - TypeScript 官方建議編碼啟用 --strictNullChecks 特性
- void 表示沒有任何類型。例如函數沒有返回值：

  ```typescript
  function hello():void {
    alert('Hello Angular');
  }
  ```

  對於可忽略返回值的 callback function，使用 void 類型會比任意值類型更安全一些。

- never 是其他類型 (包括 null 和 undefined) 的子類型，代表從不會出現的值。這意味 never 類型變數只能被 never 類型所賦值，在函數中它通常表現為拋出異常或無法執行到終止點 (例如無限循環)

  ```typescript
  let x: never;

  x = (() => { throw new Error('exception occur') })();
  
  function error(message: string): never {
    throw new Error(message);
  }
  
  function loop(): never {
    while(true) {
        
    }
  }
  ```

## 聲明和解構

- var
- let
- const

### 陣列解構

```typescript
let input = [1, 2];
let [first, second] = input;

[first, second] = [second, first]; // 變數交換

function f([first, second] = [number, number]) {
  console.log(first + second);
}
f([1, 2]);

let [first, ...rest] = [1, 2, 3, 4];
console.log(first);
console.log(rest);
```

### 物件解構

```typescript
let test = {x: 0, y: 10, width: 15, height: 20};
let {x, y, width, height} = test;
```

## 函數

### 函數定義

```typescript
function maxA(x: number, y: number): number {
  return x > y ? x : y;
}

let maxB = function (x: number, y: number): number {return x > y ? x : y;}
```

### 可選參數

在 TypeScript 中，被調函數的每個參數都是必傳的

```typescript
function max(x: number, y: number): number {
  return x > y ? x : y;
}

let result1 = max(2); // 報錯
let result2 = max(2, 4, 7); // 報錯
let result3 = max(2, 4);
```

TypeScript 提供了可選參數語法：

```typescript
function max(x: number, y?:number): number {
  if (y) {
    return x > y ? x : y;
  } else {
    return x;
  }
}
let result1 = max(2);
let result2 = max(2, 4, 7); // 報錯
let result3 = max(2, 4); 
```

### 默認參數

```typescript
function max(x: number, y = 4): number {
  return x > y ? x : y;
}
let result1 = max(2);
let result1 = max(2, undefined); // 如果沒有給 y 或傳的值是 undefined，這個參數的值就是設置的默認值
let result2 = max(2, 4, 7); // 報錯
let result3 = max(2, 4); 
```

### 剩餘參數

```typescript
function sum(x:number, ...restOfNumber: number[]): number {
  let result = x;
  for (let i = 0; i < restOfNumber.length; i++) {
    result += restOfNumber[i];
  }
  return result;
}
let result = sum(1, 2, 3, 4, 5); // result: 15
```

剩餘參數可以理解為個數不限的可選參數。

### 函數重載

TypeScript 支持函數重載：

```typescript
function css(config: {}) {
  
}
function css(config: string, value: string) {
  
}
function css(config: any, value?: any) {
  if (typeof config === 'string') {
    // ...
  } else if (typeof config === 'object') {
    // ...
  }
}
```

TypeScript 的函數重載透過查找重載列表來實現匹配，根據定義的優先順序來依次匹配。所以在實現重載函數，建議把最精確的定義放在最前面。

### 箭頭函數

JavaScript 的 this 是一個重要的概念，學習 TypeScript 有必要弄清楚 this 的工作機制，這能幫助我們避免一些隱蔽的 bug。

例如下面這段問題代碼：

```typescript
let gift = {
  gifts: ['teddy bear', 'spiderman', 'dinosaur', 'Thomas loco', 'toy bricks', 'Transformers'],
  giftPicker: function() {
    return function() {
      let pickedNumber = Math.floor(Math.random() * 6);
      return this.gifts[pickedNumber];
    }
  }
}

gift.giftPicker()(); // Uncaught TypeError: Cannot read property '0' of undefined
```

因為 giftPicker() 函數裡的 this 被設置成了 window 而不是 gift 對象。因為這裡沒有對 this 進行動態綁定，因此 this 就指向了 window 對象。

TypeScript 的箭頭函數很好地解決了這個問題，它在函數**創建時**就綁定了 this，而不是在函數**調用時**。

```typescript
let gift = {
  gifts: ['teddy bear', 'spiderman', 'dinosaur', 'Thomas loco', 'toy bricks', 'Transformers'],
  giftPicker: function() {
    return () => {
      let pickedNumber = Math.floor(Math.random() * 6);
      return this.gifts[pickedNumber];
    }
  }
}

gift.giftPicker()();
```

## 類

傳統 JavaScript 採用函數和基於原型 (Prototype) 的繼承來創建可重用的 “類”，這對於習慣 OO 的開發者來說不是很友好。好在 TypeScript 支持使用基於類的 OOP。

```typescript
class Car {
  engine: string;
  constructor(engine: string) {
    this.engine = engine;
  }
  drive(distanceInMeters: number = 0) {
    console.log(`A car runs ${distanceInMeters}m powered by ` + this.engine);
  }
}

let car = new Car('petrol');

car.drive(100);
```

### 繼承與多型

- 封裝
- 繼承
- 多型

是 OO 的三大特性。上面汽車的行為寫到一個類，即所謂的封裝。在 TypeScript 中，使用 extends 可方便實現繼承：

```typescript
class MotoCar extends Car {
  constructor(engine: string) {
    super(engine);
  }
}

class Jeep extends Car {
  constructor(engine: string) {
    super(engine);
  }
  drive(distanceInMeters: number = 10) {
    console.log('Jeep...');
    return super.drive(distanceInMeters);
  }
}
```

### 修飾符

- public
- private
- protected

#### public

在 TypeScript 裏，每個成員默認為 public。

```typescript
class Car {
  public engine: string;
  // ...
}
new Car().engine;
```

#### private

當類成員被標記成 private 時，就不能在類的外部訪問它了：

```typescript
class Car {
  private engine: string;
  // ...
}

new Car().engine; // 報錯
```

#### protected

protected 在派生類中仍然可以訪問，而不能外部訪問。

### 參數屬性