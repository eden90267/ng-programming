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

參數屬性是透過給 constructor 參數添加一個訪問限定符 (public、protected 及 private) 來聲明的。參數屬性可以方便讓我們在一個地方定義並初始化類成員。

```typescript
class Car {
  constructor(protected engine: string) {}
  drive(distanceInMeters: number = 0) {
    console.log(`A car runs ${distanceInMeters}m powered by ` + this.engine);
  }
}
```

在 constructor 透過 protected engine: string 來創建和初始化 engine 成員屬性，把聲明和賦值合併到一處。

### 靜態屬性

類的靜態成員存在於類本身而不是類的實例上，類似於在實例屬性上使用 “this.” 來訪問屬性，我們使用 “類名.” 來訪問靜態屬性。可使用 static 關鍵字來定義類的靜態屬性：

```typescript
class Grid {
  static origin = {x: 0, y: 0};
  constructor(public scale: number) {}
  calculateDistanceFromOrigin(point: {x: number, y: number}) {
    let xDist = (point.x - Grid.origin.x);
    let yDist = (point.y - Grid.origin.y);
    return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
  }
}
```

### 抽象類

TypeScript 有抽象類的概念，它是供其他類繼承的基類，不能直接被實例化。不同於接口，抽象類必須包含一些抽象方法，同時也可以包含非抽象的成員。抽象方法不包含具體實現，並且必須在派生類中實現。

```typescript
abstract class Person {
  abstract speak(): void;
  walking(): void {
    console.log('Walking on the road');
  }
}

class Male extends Person {
  speak(): void {
    console.log('How are you?');
  }
}
```

- 接口更注重功能的設計
- 抽象類更注重結構內容的體現

## 模組

ES6 引入模組的概念，在 TypeScript 也支持模組的使用 (與 ES6 一樣)。

- import
- export

模組內部變數、函數和類是不可見的，除非明確使用 export 導出它們。類似的，如果想使用其他模組導出的變數、函數、類和接口，則必須先透過 import 導入它們。

模組使用模組加載器來導入它的依賴，模組加載器在代碼運行會查找並加載模組間的所有依賴。Angular 中，常用模組加載器有 SystemJS 和 Webpack。

### export

導出有分以下三種：

- 導出聲明

  ```typescript
  export const COMPANY = 'GF';
  
  export interface IdentityValidate {
    
  }
  
  export class ErpIdentityValidate implements IdentityValidate {
    
  }
  ```

- 導出語句
  - 需要對導出的模組進行重命名

  ```typescript
  export class ErpIdentityValidate implements IdentityValidate {
  }
  
  export {ErpIdentityValidate};
  export {ErpIdentityValidate as GFIdentityValidate};
  ```

- 模組包裝
  - 修改和擴展已有模組，並導出供其他模組調用

  ```typescript
  export {ErpIdentityValidate as RegExpBasedZipCodeValidator} from './ErpIdentityValidate';
  ```

  - 一個模組可以包裏多個模組，並把新的內容以一個新的模組導出

  ```typescript
  export * from './IdentityValidate';
  export * from './ErpIdentityValidate';
  ```

### import

導入如下兩種方式：

- 導入一個模組

  ```typescript
  import {ErpIdentityValidate} from './ErpIdentityValidate';
  ```

- 別名導入

  ```typescript
  import {ErpIdentityValidate as ERP} from './ErpIdentityValidate';
  ```

  我們也可對整個模組進行別名導入，將整個模組導入到一個變數，並透過它來訪問模組的導出部分：

  ```typescript
  import * as validator from './ErpIdentityValidate';
  ```

### default export

每個模組都可以有一個默認導出。使用 default 關鍵字。類和函數聲明可直接省略導出名來實現默認導出。默認導出有利於減少調用方調用模組的層數，省去一些冗余的模組前綴書寫：

```typescript
export default class ErpIdentityValidate implements IdentityValidate {
  
}
import validator from './ErpIdentityValidate';

export default function(s: string) {
  
}
import validate from './nameServiceValidate';

export default 'Angular'
import name from './constantService';
```

### 模組設計原則

模組設計中，共同遵循一些原則有利於更好編寫和維護專案代碼。以下列出幾點模組設計原則：

#### 盡可能在頂層導出

- 過多的 “.” 操作使得開發者要記住過多細節，所以盡量使用默認導出或在頂層導出

  ```typescript
  export default class ClassTest {}
  
  import ClassTest from './ClassTest';
  ```

- 若要返回多個對象，則採用頂層導出的方式

  ```typescript
  export class ClassTest {}
  export function FuncTest() {
  
  }
  
  import {ClassTest, FuncTest} from './ClassTest';
  ```

#### 明確列出導入對象的名稱

這樣只要接口不變，調用方式就可以不變。

```typescript
import {ClassTest, FuncTest} from './ClassTest';
```

#### 使用命名空間模式導出

```typescript
export class Dog {}
export class Cat {}

import * as myLargeModule from './MyLargeModule';
let x = new myLargeModule.Dog();
```

#### 使用模組包裝進行擴展

我們可能經常需要擴展一個模組的功能，推薦方案是不要去改變原來物件，而是導出一個新的物件來提供新的功能：

```typescript
export class ModuleA {
  
}

import {ModuleA} from './ModuleA';
class ModuleB extends ModuleA {
  
}
export {ModuleA, ModuleB}
```

## 接口

接口在 OOP 中具有極其重要的作用，在 GoF 的 23 種設計模式中，基本上都可見到接口的身影。長期以來，接口模式一直是 JavaScript 這類弱型別語言的軟肋，雖然有類似於 “鴨式辨型” 等的各種偽實現，但使用起來還是略為繁瑣。TypeScript 接口使用方式類似於 Java，同時還增加了更靈活的接口類型，包括屬性、函數、可索引 (Indexable Type) 和類等類型。

### 屬性類型接口

```typescript
interface FullName {
  firstName: string;
  secondName: string;
}

function printLabel(name: FullName) {
  console.log(name.firstName + ' ' + name.secondName);
}

let myObj = {age: 10, firstName: 'Jim', secondName: 'Raynor'};
printLabel(myObj);
```

這裡有兩點要注意：

- 傳給 printLabel() 方法的對象只要 “形式上” 滿足接口要求即可
- 接口類型檢查器不會去檢查屬性的順序，但要確保相應屬性存在且類型匹配

TypeScript 還提供可選屬性，對可能存在的屬性進行預定義，兼容不傳值的情況。

```typescript
interface FullName {
  firstName: string;
  secondName?: string;
}

function printLabel(name: FullName) {
  console.log(name.firstName + ' ' + name.secondName);
}

let myObj = {firstName: 'Jim'};
printLabel(myObj);
```

### 函數類型接口

須明確指定函數參數列表和返回值類型

```typescript
interface encrypt {
  (val: string, salt: string): string
}

let md5: encrypt;
md5 = function(val: string, salt: string) {
  let encryptValue = doMd5(val, salt);
  return encryptValue;
}
let pwd = md5('password', 'Angular');
```

- 函數參數個數需與接口定義的相同，位置與資料類型也須保持一致，參數名稱可以不一樣
- 函數返回值類型與接口定義的返回值類型要一致

### 可索引類型接口

可索引類型接口用來描述那些可以透過索引得到的類型，比如 `userArray[1]`、`userObject['name']` 等。它包含一個索引簽名，表示用來索引的類型與返回值類型，即透過特定索引來得到指定類型的返回值。

```typescript
interface UserArray {
  [index: number]: string;
}
interface UserObject {
  [index: string]: string;
}

let userArray: UserArray;
let userObject: UserObject;

userArray = ['張三', '李四'];
userObject = {'name': '張三'};

console.log(userArray[0]);
console.log(userObject['name']);
```

索引簽名支持字符串和數字兩種資料類型。即當使用數字類型來索引時，JavaScript 最終也會將它轉換成字符串類型後再去索引對象。

```typescript
console.log(userArray[0]);
console.log(userArray['0']);
```

### 類類型接口

類類型接口用來規範一個類的內容

```typescript
interface Animal {
  name: string;
  setName(n: string): void;
}
class Dog implements Animal {
  name: string;
  setName(n: string) {
    this.name = n;
  }
  constructor(n: string) {}
}
```

### 接口擴展

```typescript
interface Animal {
  
}
interface Person extends Animal {
  
}

class Programmer {
  
}

class ITGirl extends Programmer implements Person {
  
}
```

## 裝飾器

