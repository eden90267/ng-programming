# Angular 介紹

## 核心概念

Angular 有七大核心概念：

- Dependency Injection
- Templates
- Components
- Modules
- Directives
- Data Binding
- Services

Angular 現在更像是一個平台，而不是簡單的類庫或單一的框架。Angular 框架核心包含以下內容：

- 依賴注入
- 裝飾器支持
- Zones
- 編譯服務
- 變化監測
- 渲染引擎

其中 Zones 可以獨立於 Angular 使用在其他地方，並已經提交給 TC39、TC39 也考慮將其納入 ECMA 標準中。而渲染引擎也是平台獨立的，可方便地實施在桌面軟件和原生的移動客戶端中。

- 升級
  - ngUpgrade
- 工具
  - CLI Command
  - 語言服務
  - Augury
  - Material
  - Mobile
  - Universal
- 核心
  - Compile 編譯
  - 變化監測
  - Render
  - 依賴注入
  - 裝飾器
  - Zones
- 類庫
  - Router 路由
  - Animation 動畫
  - i18n 國際化

在此之上，還有不少其他的外部工具庫：

- Angular Material，Google 官方的 Material 設計風格的 UI 組件庫
- Angular Mobile Toolkit，它提供工具和代碼技巧來協助開發高性能的移動應用
- Angular Universal，用它實現後端渲染，從而加速首屏渲染和實現搜索引擎優化等

完善的工具體系：

- Angular CLI，為開發者提供了工作流自動化解決方案。其功能涵蓋了創建項目、生成組件、配置路由、代碼格式化、啟動開發伺服器、構建測試、進行測試、預處理 CSS 樣式和部署錢的構建，等等
- 語言服務採用 TypeScript 構建，支持 IDE 的代碼補全、語法檢查報錯、定義跳轉和方法提示等功能，顯著提升開發效率和編譯運行前的錯誤發現
- Augury 用於調試、分析性能和可視化查看應用組件樹，幫助開發者快速定位問題和調優的工具利器
- i18n 模組，用於語言國際化、符號時間等本地化
- 路由模組，用於構建多頁面的單頁應用
- 動畫模組，提供了基於聲明式的書寫體驗和完善 Hook 節點的功能
- Upgrade 模組，Angular 和 AngularJS 1.x 不是孤立的，透過 Upgrade 模組 (原 ngUpgrade) 能夠方便地將使用 1.x 開發的應用升級到 2.0 以上，面向未來編碼

## 平台亮點

- 超快性能
  - 優化渲染速度，更快檢驗變化，內部擁有性能基準的測試框架
  - 對視圖進行緩存，從而實現列表流暢滾動和頁面切換如絲般順滑
  - 首屏加載更快，使用 server-side render 和小型啟動庫使網絡加載更快
  - 移動端響應大幅度提升，原生支持各種手勢、觸摸

除 CLI Command 工具、Augury 審查工具和 TypeScript 語言服務外，也包括：

- 官方支持的代碼風格指南和檢查 (Lint/Style 工具)
- 快速搭建項目的 Yeoman Generator、Webpack Starter 等腳手架
- 提供其他語言版本的選擇 (Dart、ES5 等)

Angular CLI 工程化流程如下：
