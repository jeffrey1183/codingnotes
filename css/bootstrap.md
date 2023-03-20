# Bootstrap

## 快速體驗 Bootstrap 的魅力

開啟你的程式碼編輯器，像我是用 Virtual Studio Code，新增一個 index.html 並將下面程式碼貼到 index.html 檔案內，下面程式碼包含 CSS 和 JavaScript 的 CDN link：

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT" crossorigin="anonymous">
  </head>
  <body>
    <h1>Hello, world!</h1>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.bundle.min.js" integrity="sha384-u1OknCvxWvY5kfmNBILK2hRnQC3Pr17a+RTT6rIHI7NnikvbZlHgTPOOmMi466C8" crossorigin="anonymous"></script>
  </body>
</html>
```

然後在 body 的區塊，新增一個按鈕並點二下從資料夾開啟 index.html 檔案，確認按鈕有出現，就套入 Bootstrap 的格式。

```html
<button type="button" class="btn btn-primary">Primary</button>
```



### Breakpoint

手機上滿版，平板以上是 4 欄

md 是 medium 中型尺寸，這是 Bootstrap 裡的尺寸代號，作為尺寸的識別。在平板的直向，代號是 md，平板的橫向因為比較寬，就會用 lg，桌機叫 xl。



問題：平板直向 2 欄，平板橫向 4 欄



col-12 col-md-3

| Breakpoint        | Class infix | Dimensions |
| ----------------- | ----------- | ---------- |
| Extra small       | _None_      | <576px     |
| Small             | `sm`        | ≥576px     |
| Medium            | `md`        | ≥768px     |
| Large             | `lg`        | ≥992px     |
| Extra large       | `xl`        | ≥1200px    |
| Extra extra large | `xxl`       | ≥1400px    |



### Column

我們常用到的欄數是 2\~6 欄，2、3、4、6 最小公倍數是 12，格線系統把空間分割成 12 等分，如果我要把畫面對半切，那左側就是 6/12，右側也是 6/12。寫法上我們可以用 container，container 是定寬容器。定寬容器會有固定寬度，且在不同裝置下會有不同寬度。

如果不想固定寬度可以用 container-fluid。裡面的 col-6 是我們的格線，用來分割畫面，要放在row 裡面，在 row 裡面放我們的資料容器像 text。

```html
<div class="container-fuid">
      <div class="row">
        <div class="col-6">
          <div class="text">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Quasi, magnam! Maiores expedita dolorum fugit iure reiciendis. Quidem neque praesentium qui iure laborum porro, deleniti a fuga vitae est doloribus dolore?
          </div>
        </div>
        <div class="col-6">
          <div class="text">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Placeat distinctio, laboriosam, perspiciatis, error architecto quod ut eum quam id quasi dolorem nemo voluptate. A recusandae ad, sint voluptatem neque repudiandae.</div>
        </div>
      </div>
    </div>
```

### 格線的寫法

不管有幾欄，幫我自動分欄，像我下面寫 3 個就是 3 欄，4 個就是 4 欄。

```html
    <div class="container">
      <div class="row">
        <div class="col"></div>
        <div class="col"></div>
        <div class="col"></div>
      </div>
    </div>
```



格線系統快速的用法是用 class 名稱 col 並且放在 row 裡面。格線系統可以分成 3 個部分來看

1. 外部容器 container (定寬容器)或是 container-fluid(滿版)
2. Bootstrap 的格線系統是用 CSS 的 flex 原理去排列出來

row 主要有兩件事情

1. 把內部的容器分割，可以在 row 裡面

### Container

格線只是分割用途，格線總和是 12，如果總和小於 12 右邊就會多出一點點空間，如果大於 12 就會讓多出去的地方往下一行長，變成折行。



### Forms

* 在 [Bootstrap 有許多案例](https://getbootstrap.com/docs/4.3/components/forms/)可以練習，form-group class 是建構表單最簡單的方式，把 label、control、輔助的文字和表單驗證的訊息都包一起，form control 則是像  \<input> 和 \<textarea>  。
* \<label> 的 for 屬性跟 \<input> 的 id 屬性綁在一起，如果不想把 \<label> 和 \<input> 分開寫，也可以把 \<input> 塞進 \<label> 裡，可參考 [MDN 的文件](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/label)。
* [input type](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/button) 有各種類型，可以參考 MDN 的範例。
* 為了讓網頁無障礙，一定要知道 aria 相關的 attribute，在 form 的範例裡就會看到。ARIA 是 Accessible Rich Internet Applications 的縮寫，一些常用的 aria attribute 可以看[這篇文章](https://noiseyou99.medium.com/%E4%BD%BF%E7%94%A8aria-%E6%A8%99%E7%B1%A4%E5%81%9Aaccessibility-1e306099ddc4)簡單了解。



