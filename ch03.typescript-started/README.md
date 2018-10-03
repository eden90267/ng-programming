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
