# Sass

## 語法

Sass 語法有分兩種 分別為： `SCSS` 與 `Sass`

較為大宗的是 `SCSS` 語法 其寫法與原本的 css 較為相似

## 編譯

在網頁瀏覽器中 都只認識 css 檔案

所以寫好的 SCSS 都需要通過編譯器把它編譯成 css 檔後才能在網頁上使用

編譯的方式有三種：

1. 打包工具(如 webpack 或 gulp)

2. 編輯器內建套件(如 VS Code 的 Live Sass Compiler)

3. prepros 軟體

### `VS Code` 的 `Live Sass Compiler`

首先開啟 `VS Code` 編輯器

然後點擊左側選單列最下面的`方塊 icon` (這是用來搜尋並安裝套件的)

點進去後在搜索框輸入 `Live Sass Compiler` 安裝它

接下來我們將項目資料夾拖曳到 `VS Code` 編輯器中

然後我們在項目資料夾中新增一個 `SCSS` 資料夾

在 `SCSS` 資料夾中新增一個 `all.scss` 檔案

開啟該 `all.scss` 檔 會發現下方狀態列出現了一個新的按鈕 `Watch Sass` 點下去即可實時編譯

(如果看不到下方狀態列 可在最上面的選單中點擊`檢視`選擇`外觀`裡面的`顯示/隱藏狀態列`)

### prepros 軟體

首先到 [Prepros 官網](https://prepros.io/) 去安裝該軟體

安裝完成後打開該軟體 將項目資料夾拖曳至軟體中

然後我們在項目資料夾中新增一個 `SCSS` 資料夾

在 `SCSS` 資料夾中新增一個 `all.scss` 檔案 即可

接下來當你的 `all.scss` 編寫後 按下儲存

`prepros` 就會幫你做編譯 並在項目資料夾中生成 `css` 資料夾

裡面的 `all.css` 就是最後我們在網頁中要引入的樣式檔案了

## SCSS 語法

### & 連結符

在完全重複的名稱中可以通過連結符來減少撰寫的代碼

舉例如下：

```scss
a {
  color: red;
  &:hover {
    color: pink;
  }
}

// 編譯結果如下

a {
  color: red;
}

a:hover {
  color: pink;
}
```

### $ 變數

我們可以在檔案的最上方設定變數 並在下方代碼中使用

舉例如下：

```scss
$primary: #ff0000;
$lineH: 40px;

a {
  color: $primary;
  line-height: $lineH;
}

// 編譯結果如下

a {
  color: #ff0000;
  line-height: 40px;
}
```

### 顏色

`darken(color,%)` 是顏色加深

`lighten(color,%)` 是顏色加亮

括號中的第一個參數是顏色 可以傳入變數也可以直接傳入顏色

括號中的第二個參數的 % 則是要加深或加亮的百分比

舉例如下：

```scss
$primary: #ff0000;

.bg-dark {
  background-color: darken($primary, 20%);
}

.bg-light {
  background-color: lighten($primary, 20%);
}

.bg-base {
  background-color: $primary;
}

// 編譯結果如下

.bg-dark {
  background-color: #990000;
}

.bg-light {
  background-color: #ff6666;
}

.bg-base {
  background-color: #ff0000;
}
```

### 匯入

Sass 可以將多個小檔案匯出為一個 css 檔

這也讓我們能更好的管理代碼 將代碼分門別類

比如我們可以把變數單獨做成一支叫 `_var.scss` 的小檔案

(在匯入時 通常變數都放在最上面)

然後通過 `@import` 來將所有小檔案匯入到 `all.scss` 中

- 小檔案的名稱最前面需加上下底線以做區分 不然編譯器會自動將該檔案編譯成一支 css 檔

- 加上下底線的目的是讓編譯器知道這是要匯入用的 不須被編譯

- 要注意的是 在匯入到 `all.scss` 時 是不需要加上下底線的

舉例如下：

```scss
// 在 all.scss 匯入 _variable.scss
@import "./variable";

a {
  color: $red;
}
```

### css reset

[Reset](https://meyerweb.com/eric/tools/css/reset/) 是清空所有樣式

[Normailze](https://necolas.github.io/normalize.css/) 則是保有一些預設樣式(如 li 的 list-style 等)

通常匯入時放在變數與自己撰寫的樣式中間 不要放在自己撰寫的樣式下面

### @mixin

當撰寫的樣式中有很多重複的內容時 可以使用 `@mixin`

首先我們創建一支 `_mixin.scss`

然後在裡面撰寫重複的內容( `@mixin 自定義名稱 { 樣式代碼 }` )

比如今天有很多地方使用到了圖片取代文字

當我們建立好圖片取代文字的 `mixin` 後 在要使用的地方加上 `@include 自定義名稱;` 這行 即可

舉例如下：

```scss
@mixin hideText {
  display: block;
  text-indent: 110%;
  overflow: hidden;
  white-space: nowrap;
}

.header {
  width: 100vw;
  height: 50px;
  background-color: #fff;
  .logo {
    a {
      height: 50px;
      width: 140px;
      background: url("./img/logo.png") no-repeat;
      background-size: cover;
      @include hideText;
    }
  }
}

// 編譯結果為

.header {
  width: 100vw;
  height: 50px;
  background-color: #fff;
}

.header .logo a {
  height: 50px;
  width: 140px;
  background: url("./img/logo.png") no-repeat;
  background-size: cover;
  display: block;
  text-indent: 110%;
  overflow: hidden;
  white-space: nowrap;
}
```

### @mixin + 變數

在設置 @mixin 時也可以往裡面添加變數

比如某個樣式中同個顏色或尺寸重複使用多次 我們可以寫成這樣

```scss
@mixin box($color, $size) {
  width: $size;
  height: $size;
  margin: $size/10;
  background-color: $color;
  border: 2px solid $color;
}

.box {
  @include box(gold, 100px);
}

.box2 {
  @include box(red, 200px);
}

// 編譯結果
.box1 {
  width: 100px;
  height: 100px;
  margin: 10px;
  background-color: gold;
  border: 2px solid gold;
}

.box2 {
  width: 200px;
  height: 200px;
  margin: 20px;
  background-color: red;
  border: 2px solid red;
}
```

### @mixin + @content 撰寫 RWD

以下使用範例說明：

```scss
@mixin pad {
  @media (max-width: 768px) {
    @content;
  }
}

@mixin mobile {
  @media (max-width: 568px) {
    @content;
  }
}

.header {
  width: 960px;
  height: 60px;
  @include pad {
    width: 100%;
    height: 50px;
  }
  @include mobile {
    width: 100%;
    height: 40px;
  }
}

//編譯結果

.header {
  width: 960px;
  height: 60px;
}

@media (max-width: 768px) {
  .header {
    width: 100%;
    height: 50px;
  }
}

@media (max-width: 568px) {
  .header {
    width: 100%;
    height: 40px;
  }
}
```

### @for

類似 js 中的 for 迴圈 適用於數字的 常見於隔線系統中

使用方式有兩種:

```scss
// 第一種 from...to... 只輸出 1~3
@for $i from 1 to 4 {
  .box {
    background-color: $i;
  }
}

// 輸出後如下
.box {
  background-color: 1;
  background-color: 2;
  background-color: 3;
}

// 第二種 from...through... 會輸出 1~5
@for $i from 1 through 5 {
  .box {
    background-color: $i;
  }
}

// 輸出後如下
.box {
  background-color: 1;
  background-color: 2;
  background-color: 3;
}

// scss 無法直接將變數套到樣式名稱上
// 但可以通過 #{變數} 實現
@for $i from 1 through 5 {
  .box-#{$i} {
    opacity: $i * 20%;
  }
}

// 輸出後如下
.box-1 {
  opacity: 20%;
}

.box-2 {
  opacity: 40%;
}

.box-3 {
  opacity: 60%;
}

.box-4 {
  opacity: 80%;
}

.box-5 {
  opacity: 100%;
}
```

### @each

類似 @for 但用於字串 使用時須先定義一個集合變數

- 該集合變數又稱 `Sass map` 專門用來搭配 @each 的

- 若要抓取集合中的其中一個 需通過 `map-get(集合變數名, 'key值')`

如下：

```scss
// 定義的集合變數
$themes: (
  // key: value
  "primary": blue,
  "danger": red,
  "dark": #000
);

// 抓取 danger
.bg-danger {
  background-color: map-get($themes, "danger");
}
```

@each 使用方式如下：

```scss
$themes: (
  // key: value
  "primary": blue,
  "danger": red,
  "dark": #000
);

@each $key, $val in $themes {
  .box-#{$key} {
    background-color: $val;
  }
}

// 輸出結果如下
.box-primary {
  background-color: blue;
}

.box-danger {
  background-color: red;
}

.box-dark {
  background-color: #000;
}
```

### @extend

這個技巧適用於多個類名中存在相同樣式時

比如格線系統中的 `.col-1 ~ .col-12`

在撰寫 @for 時 就可以用到 如下：

```scss
%col {
  max-width: 100%;
  padding: 0 ($gutter-w / 2);
  flex-basis: 100%;
}

@for $i from 1 to 13 {
  .col-#{$i} {
    @extend %col;
  }
}

// 輸出後
.col-12,
.col-11,
.col-10,
.col-9,
.col-8,
.col-7,
.col-6,
.col-5,
.col-4,
.col-3,
.col-2,
.col-1 {
  max-width: 100%;
  padding: 0 ($gutter-w / 2);
  flex-basis: 100%;
}
```

## CSS/Sass 設計模式

### SMACSS

base 通常用來存放基本設定(全站設定) 比如一些值些針對標籤的設定

- `base` 內容可能就是 `body` `html` `a` 等標籤的樣式設定

`layout` 通常用來存放共用設定(網站設定) 比如頭部底部等在各個頁面都會出現的樣式

- `layout` 內容可能就是 `header` `footer` 等樣式

- `layout` 也可以處理一些共用的東西 在樣式名加上前綴詞用以辨認 `.l-xxx` or `.layout-xxx`

`module` 模組化的意思 比如把按鈕或表單等 做成一個模組化的樣式

- 先設置他的基本樣式 再以基本樣式為基底修改顏色主題、大小等

- 以 `bootstrap` 來說 `btn` 是按鈕基本樣式 `btn-primary` 是修改按鈕顏色樣式 `btn-lg` 是修改按鈕大小樣式

- module 會建議不要直接在樣式中指定標籤名稱(EX: `.header a` 會建議改為 `.header-link`)

### OOCSS

OOCSS 的原則是 盡量避免使用繼承選擇符(如 `.list > a` 這種) 它追求的是可以重複使用的樣式 在任何元素上都能套用 不拘泥於要套用在指定容器下的某個元素標籤

1. 結構與樣式分離

   結構像是`元素的大小、定位`等

   樣式則是`顏色、背景或邊框`等

   以 BS4 來說 `.btn` 就是`結構` `.btn-primary` 則是`樣式`

2. 容器與內容分離:

   容器型元件：如 `.grid` `.card` `form` 等

   內容型元件：如 `button` `input` `progress-bar` 等

## CSS 命名規範

### 大小駝峰

大駝峰指的是所有英文字的字首都大寫 EX: LastName

小駝峰則是指第一個英文字字首小寫其餘字首大寫 EX: lastName

- 當在設計 class 名稱時 建議將一個名稱使用大駝峰或小駝峰的方式撰寫 而不要用下底線或減號把字拆開 原因是會讓人誤以為它有兩層 舉例來說 `.bookList` 如果寫成 `.book-list` 會讓人以為在 `.book` 裡面有一個列表 `.book-list`

### BEM

BEM 指的是 block, element, modifire (即區塊, 元素, 修飾符)

區塊與元素之間用兩個下底線分隔(xxx\_\_xx) 區塊與修飾符 or 元素與修飾符之間則用兩個中線分隔(xxx--xx)

假設 我們有一個 `header` 它身上有陰影 它裡面有 LOGO

那 `.header` 就是 `header` 的區塊

`.header--shadow` 就是 `header` 的修飾符

`.header__logo` 就是 `header` 的元素

再舉例來說 我們有一個 `menu` 它裡面有三個按鈕 選中的時候有不同的樣式

那 `.menu` 就是 `menu` 的區塊

`.menu__item` 就是 `menu` 的元素

`.menu__item--active` 就是 `menu` 的元素(即 `.menu__item`)的修飾符

## 自制 grid 格線系統

首先要定義:

- 每個 `col` 之間間距為多少 ex: `30px`

- 整個 `container` 最大寬度為多少 ex: `1200px`, `960px`

- 要將 `col` 劃分為多少格 ex: `12`, `16`

將間距設為變數 `$gutter`, 格線系統共多少格設為變數 `$gridNum`

設計步驟如下:

```scss
$gutter: 30px;
$gridNum: 12;
$maxW: 1200px;
// 先將所有元素設置 box-sizing 為 border-box
* {
  box-sizing: border-box; // 設定後元素的內距和邊框就不會增加元素本身的寬度
}

// 接下來設置一個最外層 container 裡面放置 row
.container {
  max-width: $maxW;
  margin: 0 auto;
  padding: 0 ($gutter-w / 2); // 補上 row 的 margin
}

// 然後設置 row 裡面放置 col
.row {
  display: flex;
  margin: 0 (-($gutter-w / 2)); // 補上 col 的 padding
  flex-wrap: wrap; // 內容超過 100% 後會換行
}

// 再設置每個 col 的樣式
.col {
  max-width: 100%;
  padding: 0 ($gutter-w / 2); // col 之間的距離
  flex-basis: 100%; // col 佔據的寬度
}

// 最後設置當寬度大於中斷點 767px 時 各自的 col 寬度多少
@media (min-width: 767px) {
  @for $i from 1 to $gridNum + 1 {
    .col-#{$i} {
      max-width: 100% * ($i / $gridNum);
      flex-basis: 100% * ($i / $gridNum);
    }
  }
}
```
