# 《前端进阶 JavaScript 标准库》 

前端进阶必看的JavaScript 标准库

https://webvueblog.github.io/JavaScript-standard-library/

<img width="300px" src="https://user-images.githubusercontent.com/59645426/187019807-7785b3c9-9931-4bca-a28f-5e508a4c9839.jpg"/>

## 画图

要在网页中创建 2D 或者 3D 场景，必须在 HTML 文件中插入一个 <canvas> 元素，以界定网页中的绘图区域。这很简单，如下所示：

```
<canvas width="320" height="240"></canvas>
```

不明确指定宽高时，画布默认尺寸为 300 × 150 像素。

画布模板设置还有最后一步。我们需要获得一个对绘画区域的特殊的引用（称为“上下文”）用来在画布上绘图。可通过 HTMLCanvasElement.getContext() 方法获得基础的绘画功能，需要提供一个字符串参数来表示所需上下文的类型。

这里我们需要一个 2d 画布，在 `<script>` 元素内添加以下 JS 代码即可：

```
var ctx = canvas.getContext('2d');
```

好啦，现已万事具备！ctx 变量包含一个 CanvasRenderingContext2D 对象，画布上所有绘画操作都会涉及到这个对象。

开始前我们先初尝一下 canvas API。在 JS 代码中添加以下两行，将画布背景涂成黑色：

```
ctx.fillStyle = 'rgb(0, 0, 0)';
ctx.fillRect(0, 0, width, height);
```

这里我们使用画布的 fillStyle 属性（和 CSS 属性 色值 一致）设置填充色，然后使用 fillRect 方法绘制一个覆盖整个区域的矩形（前两个参数是矩形左上顶点的坐标，后两个参数是矩形的长宽，现在你知道 width 和 height 的作用了吧）。

### 2D 画布基础

如上文所讲，所有绘画操作都离不开 CanvasRenderingContext2D 对象（这里叫做 ctx）。许多操作都需要提供坐标来指示绘图的确切位置。画布左上角的坐标是 (0, 0)，横坐标（x）轴向右延伸，纵坐标（y）轴向下延伸。

![image](https://user-images.githubusercontent.com/59645426/216849521-056e4e2a-b4ac-4d32-9f99-c1d4c01e170c.png)

绘图操作可基于原始矩形模型实现，也可通过追踪一个特定路径后填充颜色实现。下面分别讲解。

### 描边（stroke）和线条宽度

目前我们绘制的矩形都是填充颜色的，我们也可以绘制仅包含外部框线（图形设计中称为描边）的矩形。你可以使用 strokeStyle 属性来设置描边颜色，使用 strokeRect 来绘制一个矩形的轮廓。

在上文的 JS 代码的末尾添加以下代码：

```
ctx.strokeStyle = 'rgb(255, 255, 255)';
ctx.strokeRect(25, 25, 175, 200);
```

默认的描边宽度是 1 像素，可以通过调整 lineWidth 属性（接受一个表示描边宽度像素值的数字）的值来修改。在上文两行后添加以下代码（读者注：此代码要放到两行代码中间才有效）：

```
ctx.lineWidth = 5;
```

### 绘制路径

可以通过绘制路径来绘制比矩形更复杂的图形。路径中至少要包含钢笔运行精确路径的代码以确定图形的形状。画布提供了许多函数用来绘制直线、圆、贝塞尔曲线，等等。

重新复制一份（1_canvas_template.html），然后在其中绘制新的示例。

一些通用的方法和属性将贯穿以下全部内容：

      beginPath()：在钢笔当前所在位置开始绘制一条路径。在新的画布中，钢笔起始位置为 (0, 0)。
      moveTo()：将钢笔移动至另一个坐标点，不记录、不留痕迹，只将钢笔“跳”至新位置。
      fill()：通过为当前所绘制路径的区域填充颜色来绘制一个新的填充形状。
      stroke()：通过为当前绘制路径的区域描边，来绘制一个只有边框的形状。
      路径也可和矩形一样使用 lineWidth 和 fillStyle / strokeStyle 等功能。

以下是一个典型的简单路径绘制选项：

```
ctx.fillStyle = 'rgb(255, 0, 0)';
ctx.beginPath();
ctx.moveTo(50, 50);
// 绘制路径
ctx.fill();
```

### 画圆

下面来看可在画布中绘制圆的方法—— arc() ，通过连续的点来绘制整个圆或者弧（arc，即局部的圆）。

在代码中添加以下几行，以向画布中添加一条弧。

```
ctx.fillStyle = 'rgb(0, 0, 255)';
ctx.beginPath();
ctx.arc(150, 106, 50, degToRad(0), degToRad(360), false);
ctx.fill();
```

arc() 函数有六个参数。前两个指定圆心的位置坐标，第三个是圆的半径，第四、五个是绘制弧的起、止角度（给定 0° 和 360° 便能绘制一个完整的圆），第六个是绘制方向（false 是顺时针，true 是逆时针）。

我们再来画一条弧：

```
ctx.fillStyle = 'yellow';
ctx.beginPath();
ctx.arc(200, 106, 50, degToRad(-45), degToRad(45), true);
ctx.lineTo(200, 106);
ctx.fill();
```

### 文本

画布可用于绘制文本。

以下两个函数用于绘制文本：

- fillText() ：绘制有填充色的文本。
- strokeText()：绘制文本外边框（描边）。

在 JS 代码底部添加以下内容：

```
ctx.strokeStyle = 'white';
ctx.lineWidth = 1;
ctx.font = '36px arial';
ctx.strokeText('Canvas text', 50, 50);

ctx.fillStyle = 'red';
ctx.font = '48px georgia';
ctx.fillText('Canvas text', 50, 150);
```

### 在画布上绘制图片

可在画布上渲染外部图片，简单图片文件、视频帧、其他画布内容都可以。这里我们只考虑简单图片文件的情况：

将图片源嵌入画布中，代码如下：

```
var image = new Image();
image.src = 'firefox.png';
```

这次我们尝试用 drawImage() 函数来嵌入图片，应确保图片先载入完毕，否则运行会出错。可以通过 onload 事件处理器来达成，该函数只在图片调用完毕后才会调用。在上文代码末尾添加以下内容：

```
image.onload = function() {
  ctx.drawImage(image, 50, 50);
}
```

保存刷新，可以看到图片成功嵌入画布中。

还有更多方式。如果仅需要显示图片的某一部分，或者需要改变尺寸，该怎么做呢？复杂版本的 drawImage() 可解决这两个问题。请更新 ctx.drawImage() 一行代码为：

```
ctx.drawImage(image, 20, 20, 185, 175, 50, 50, 185, 175);
```

      第一个参数不变，为图片引用。
      参数 2、3 表示裁切部分左上顶点的坐标，参考原点为原图片本身左上角的坐标。原图片在该坐标左、上的部分均不会绘制出来。
      参数 4、5 表示裁切部分的长、宽。
      参数 6、7 表示裁切部分左上顶点在画布中的位置坐标，参考原点为画布左上顶点。
      参数 8、9 表示裁切部分在画布中绘制的长、宽。本例中绘制时与裁切时面积相同，你也可以定制绘制的尺寸。

### 动画

一些 JavaScript 函数可以让函数在一秒内重复运行多次，这里最适合的就是 window.requestAnimationFrame()。它只取一个参数，即每帧要运行的函数名。下一次浏览器准备好更新屏幕时，将会调用你的函数。如果你的函数向动画中绘制了更新内容，则在函数结束前再次调用 requestAnimationFrame()，动画循环得以保留。只有在停止调用 requestAnimationFrame() 时，或 requestAnimationFrame() 调用后、帧调用前调用了 window.cancelAnimationFrame() 时，循环才会停止。

浏览器自行处理诸如“使动画匀速运行”、“避免在不可见的内容浪费资源”等复杂细节问题。

### 从服务器获取数据

```
const request = new XMLHttpRequest();

try {
  request.open('GET', 'products.json');

  request.responseType = 'json';

  request.addEventListener('load', () => initialize(request.response));
  request.addEventListener('error', () => console.error('XHR error'));

  request.send();

} catch (error) {
  console.error(`XHR error ${request.status}`);
}
```

### 操作文档

window 是载入浏览器的标签，在 JavaScript 中用Window对象来表示，使用这个对象的可用方法，你可以返回窗口的大小（参见Window.innerWidth和Window.innerHeight），操作载入窗口的文档，存储客户端上文档的特殊数据（例如使用本地数据库或其他存储设备），为当前窗口绑定event handler，等等。

navigator 表示浏览器存在于 web 上的状态和标识（即用户代理）。在 JavaScript 中，用Navigator来表示。你可以用这个对象获取一些信息，比如来自用户摄像头的地理信息、用户偏爱的语言、多媒体流等等。

document（在浏览器中用 DOM 表示）是载入窗口的实际页面，在 JavaScript 中用Document 对象表示，你可以用这个对象来返回和操作文档中 HTML 和 CSS 上的信息。例如获取 DOM 中一个元素的引用，修改其文本内容，并应用新的样式，创建新的元素并添加为当前元素的子元素，甚至把他们一起删除。

### 文档对象模型

      元素节点: 一个元素，存在于 DOM 中。
      根节点: 树中顶层节点，在 HTML 的情况下，总是一个HTML节点（其他标记词汇，如 SVG 和定制 XML 将有不同的根元素）。
      子节点: 直接位于另一个节点内的节点。例如上面例子中，IMG是SECTION的子节点。
      后代节点: 位于另一个节点内任意位置的节点。例如 上面例子中，IMG是SECTION的子节点，也是一个后代节点。IMG不是BODY的子节点，因为它在树中低了BODY两级，但它是BODY的后代之一。
      父节点: 里面有另一个节点的节点。例如上面的例子中BODY是SECTION的父节点。
      兄弟节点: DOM 树中位于同一等级的节点。例如上面例子中，IMG和P是兄弟。
      文本节点: 包含文字串的节点

### 移动和删除元素

也许有时候你想移动或从 DOM 中删除节点，这是完全可能的。

如果你想把具有内部链接的段落移到 sectioin 的底部，简单的做法是：

```
sect.appendChild(linkPara);
```

这样可以把段落下移到 section 的底部。你可能想过要做第二个副本，但是情况并非如此 — linkPara是指向该段落唯一副本的引用。如果你想做一个副本并也把它添加进去，只能用Node.cloneNode() 方法来替代。

删除节点也非常的简单，至少，你拥有要删除的节点和其父节点的引用。在当前情况下，我们只要使用Node.removeChild()即可，如下：

```
sect.removeChild(linkPara);
```

要删除一个仅基于自身引用的节点可能稍微有点复杂，这也是很常见的。没有方法会告诉节点删除自己，所以你必须像下面这样操作。

```
linkPara.parentNode.removeChild(linkPara);
```

### 操作样式

通过 JavaScript 以不同的方式来操作 CSS 样式是可能的。

首先，使用 Document.stylesheets返回CSSStyleSheet数组，获取绑定到文档的所有样式表的序列。然后添加/删除想要的样式。然而，我们并不想扩展这些特性，因此它们在操作样式方面有点陈旧和困难，而现在有了更容易的方法。

第一种方法是直接在想要动态设置样式的元素内部添加内联样式。这是用HTMLElement.style (en-US)属性来实现。这个属性包含了文档中每个元素的内联样式信息。你可以设置这个对象的属性直接修改元素样式。

要做个例子，把下面的代码行加到我们的例子中：

```
para.style.color = 'white';
para.style.backgroundColor = 'black';
para.style.padding = '10px';
para.style.width = '250px';
para.style.textAlign = 'center';
```

重新载入页面，你将看到样式已经应用到段落中。如果在浏览器的Page Inspector/DOM inspector中查看段落，你会看到这些代码的确为文档添加了内联样式：

```
<p style="color: white; background-color: black; padding: 10px; width: 250px; text-align: center;">We hope you enjoyed the ride.</p>
```

<hr>

## 视频和音频 API

HTML5 提供了用于在文档中嵌入富媒体的元素 — `<video>和<audio>` — 这些元素通过自带的 API 来控制视频或音频的播放，定位进度等。本文将向你展示如何执行一些常见的任务，如创建自定义播放控件。

### HTML5 视频和音频

`<video>和<audio>`元素允许我们把视频和音频嵌入到网页当中。就像我们在音频和视频内容文中展示的一样，一个典型的实现如下所示：

```
<video controls>
  <source src="rabbit320.mp4" type="video/mp4">
  <source src="rabbit320.webm" type="video/webm">
  <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.mp4">link to the video</a> instead.</p>
</video>
```

### HTMLMediaElement API

作为 HTML5 规范的一部分，HTMLMediaElement API 提供允许你以编程方式来控制视频和音频播放的功能—例如 HTMLMediaElement.play(), HTMLMediaElement.pause(),等。该接口对`<audio>和<video>`两个元素都是可用的，因为在这两个元素中要实现的功能几乎是相同的。让我们通过一个例子来一步步演示一些功能。

### 探索 HTML

打开 HTML index 文件。你将看到一些功能；HTML 由视频播放器和它的控件所控制：

```
<div class="player">
  <video controls>
    <source src="video/sintel-short.mp4" type="video/mp4">
    <source src="video/sintel-short.mp4" type="video/webm">
    <!-- fallback content here -->
  </video>
  <div class="controls">
    <button class="play" data-icon="P" aria-label="play pause toggle"></button>
    <button class="stop" data-icon="S" aria-label="stop"></button>
    <div class="timer">
      <div></div>
      <span aria-label="timer">00:00</span>
    </div>
    <button class="rwd" data-icon="B" aria-label="rewind"></button>
    <button class="fwd" data-icon="F" aria-label="fast forward"></button>
  </div>
</div>
```

我们从设置为hidden的自定义控件visibility开始。稍后在我们的 JavaScript 中，我们将控件设置为 visible, 并且从<video> 元素中移除controls 属性。这是因为，如果 JavaScript 由于某种原因没有加载，用户依然可以使用原生的控件播放视频。

默认情况下，我们将控件的opacity设置为 0.5 opacity，这样当您尝试观看视频时，它们就不会分散注意力。只有当您将鼠标悬停/聚焦在播放器上时，控件才会完全不透明。

我们使用 Flexbox(display: flex) 布置控制栏内的按钮，以简化操作。

按钮图标：

```
@font-face {
   font-family: 'HeydingsControlsRegular';
   src: url('fonts/heydings_controls-webfont.eot');
   src: url('fonts/heydings_controls-webfont.eot?#iefix') format('embedded-opentype'),
        url('fonts/heydings_controls-webfont.woff') format('woff'),
        url('fonts/heydings_controls-webfont.ttf') format('truetype');
   font-weight: normal;
   font-style: normal;
}

button:before {
  font-family: HeydingsControlsRegular;
  font-size: 20px;
  position: relative;
  content: attr(data-icon);
  color: #aaa;
  text-shadow: 1px 1px 0px black;
}
```

首先在 CSS 的最上方我们使用 @font-face 块来导入自定义 Web 字体。这是一种图标字体 —— 字母表中的所有字符都是各种常用图标，你可以尝试在程序中调用。

我们使用 ::before 选择器在每个 <button> 元素之前显示内容。

我们使用 content 属性将各情况下要显示的内容设置为 data-icon 属性的内容。例如在播放按钮的情况下，data-icon 包含大写的“P”。

我们使用 font-family 将自定义 Web 字体应用于我们的按钮上。在该字体中“P”对应的是“播放”图标，因此播放按钮上显示“播放”图标。

图标字体非常酷有很多原因 —— 减少 HTTP 请求，因为您不需要将这些图标作为图像文件下载。同时具有出色的可扩展性，以及您可以使用文本属性来设置它们的样式 —— 例如 color 和 text-shadow。

### 播放和暂停视频

让我们实现或许是最重要的控制——播放/暂停按钮。

首先，将以下内容添加到您代码的底部，以便于在单击播放按钮时调用 playPauseMedia()函数：

```
play.addEventListener('click', playPauseMedia);
```

现在定义 playPauseMedia() 函数——再次添加以下内容到您代码底部：

```
function playPauseMedia() {
  if(media.paused) {
    play.setAttribute('data-icon','u');
    media.play();
  } else {
    play.setAttribute('data-icon','P');
    media.pause();
  }
}
```

我们使用 if 语句来检查视频是否暂停。如果视频已暂停，HTMLMediaElement.paused 属性将返回 true，任何视频没有播放的时间，包括第一次加载时处于 0 的时间段都是视频暂停状态。如果已暂停，我们把 play 按钮的 data-icon 属性值设置成"u", 用以表示 "暂停" 按钮图标，并且调用HTMLMediaElement.play() 函数播放视频。 点击第二次，按钮将会切换回去——"播放"按钮图标将会再次显示，并且视频将会被HTMLMediaElement.paused() 函数暂停。

### 暂停视频

接下来，让我们添加处理视频停止的方法。添加以下的 addEventListener() 行在你之前添加的内容的下面：

```
stop.addEventListener('click', stopMedia);
media.addEventListener('ended', stopMedia);
```

click 事件很明显——我们想要在点击停止按钮的时候停止视频通过运行我们的 stopMedia() 函数。然而我们也希望停止视频当视频播放完成时——由ended 事件标记，所以我们也会设置一个监听器在此事件触发时运行函数。

接下来，让我们定义 stopMedia()—— 在 playPauseMedia() 后面添加以下函数：

```
function stopMedia() {
  media.pause();
  media.currentTime = 0;
  play.setAttribute('data-icon','P');
}
```

### 探索快进和快退

首先，在前面的代码之下添加以下两个addEventListener()：

```
rwd.addEventListener('click', mediaBackward);
fwd.addEventListener('click', mediaForward);
```

```
var intervalFwd;
var intervalRwd;

function mediaBackward() {
  clearInterval(intervalFwd);
  fwd.classList.remove('active');

  if(rwd.classList.contains('active')) {
    rwd.classList.remove('active');
    clearInterval(intervalRwd);
    media.play();
  } else {
    rwd.classList.add('active');
    media.pause();
    intervalRwd = setInterval(windBackward, 200);
  }
}

function mediaForward() {
  clearInterval(intervalRwd);
  rwd.classList.remove('active');

  if(fwd.classList.contains('active')) {
    fwd.classList.remove('active');
    clearInterval(intervalFwd);
    media.play();
  } else {
    fwd.classList.add('active');
    media.pause();
    intervalFwd = setInterval(windForward, 200);
  }
}
```

<hr>

## 客户端存储

现代 web 浏览器提供了很多在用户电脑 web 客户端存放数据的方法 — 只要用户的允许 — 可以在它需要的时候被重新获得。这样能让你存留的数据长时间保存，保存站点和文档在离线情况下使用，保留你对其站点的个性化配置等等。本篇文章只解释它们工作的一些很基础的部分。

### 客户端存储？

在其他的 MDN 学习中我们已经讨论过 静态网站（static sites）和动态网站（ dynamic sites）的区别。大多数现代的 web 站点是动态的— 它们在服务端使用各种类型的数据库来存储数据 (服务端存储), 之后通过运行服务端（ server-side）代码来重新获取需要的数据，把其数据插入到静态页面的模板中，并且生成出 HTML 渲染到用户浏览上。

客户端存储以相同的原理工作，但是在使用上有一些不同。它是由 JavaScript APIs 组成的因此允许你在客户端存储数据 (比如在用户的机器上)，而且可以在需要的时候重新取得需要的数据。这有很多明显的用处，比如：

- 个性化网站偏好（比如显示一个用户选择的窗口小部件，颜色主题，或者字体）。
- 保存之前的站点行为 (比如从先前的 session 中获取购物车中的内容，记住用户是否之前已经登陆过)。
- 本地化保存数据和静态资源可以使一个站点更快（至少让资源变少）的下载，甚至可以在网络失去链接的时候变得暂时可用。
- 保存 web 已经生产的文档可以在离线状态下访问。

通常客户端和服务端存储是结合在一起使用的。例如，你可以从数据库中下载一个由网络游戏或音乐播放器应用程序使用的音乐文件，将它们存储在客户端数据库中，并按需要播放它们。用户只需下载音乐文件一次——在随后的访问中，它们将从数据库中检索。

### 传统方法：cookies

客户端存储的概念已经存在很长一段时间了。从早期的网络时代开始，网站就使用 cookies 来存储信息，以在网站上提供个性化的用户体验。它们是网络上最早最常用的客户端存储形式。 因为在那个年代，有许多问题——无论是从技术上的还是用户体验的角度——都是困扰着 cookies 的问题。这些问题非常重要，以至于当第一次访问一个网站时，欧洲居民会收到消息，告诉他们是否会使用 cookies 来存储关于他们的数据，而这是由一项被称为欧盟 Cookie 条例的欧盟法律导致的。

由于这些原因，我们不会在本文中教你如何使用 cookie。毕竟它过时、存在各种安全问题，而且无法存储复杂数据，而且有更好的、更现代的方法可以在用户的计算机上存储种类更广泛的数据。 cookie 的唯一优势是它们得到了非常旧的浏览器的支持，所以如果您的项目需要支持已经过时的浏览器（比如 Internet Explorer 8 或更早的浏览器），cookie 可能仍然有用，但是对于大多数项目（很明显不包括本站）来说，您不需要再使用它们了。其实 cookie 也没什么好说的，document.cookie一把梭就完事了。

### 新流派：Web Storage 和 IndexedDB

现代浏览器有比使用 cookies 更简单、更有效的存储客户端数据的 API。

Web Storage API 提供了一种非常简单的语法，用于存储和检索较小的、由名称和相应值组成的数据项。当您只需要存储一些简单的数据时，比如用户的名字，用户是否登录，屏幕背景使用了什么颜色等等，这是非常有用的。

IndexedDB API 为浏览器提供了一个完整的数据库系统来存储复杂的数据。这可以用于存储从完整的用户记录到甚至是复杂的数据类型，如音频或视频文件。

您将在下面了解更多关于这些 API 的信息。

### 未来：Cache API

一些现代浏览器支持新的 Cache API。这个 API 是为存储特定 HTTP 请求的响应文件而设计的，它对于像存储离线网站文件这样的事情非常有用，这样网站就可以在没有网络连接的情况下使用。缓存通常与 Service Worker API 组合使用，尽管不一定非要这么做。 Cache 和 Service Workers 的使用是一个高级主题，我们不会在本文中详细讨论它，尽管我们将在下面的 离线文件存储 一节中展示一个简单的例子。

### 存储简单数据 — web storage

Web Storage API 非常容易使用 — 你只需存储简单的 键名/键值 对数据 (限制为字符串、数字等类型) 并在需要的时候检索其值。

### 为每个域名分离储存

每个域都有一个单独的数据存储区 (每个单独的网址都在浏览器中加载). 你 会看到，如果你加载两个网站（例如 google.com 和 amazon.com）并尝试将某个项目存储在一个网站上，该数据项将无法从另一个网站获取。

### 存储复杂数据 — IndexedDB

IndexedDB API（有时简称 IDB）是可以在浏览器中访问的一个完整的数据库系统，在这里，你可以存储复杂的关系数据。其种类不限于像字符串和数字这样的简单值。你可以在一个 IndexedDB 中存储视频，图像和许多其他的内容。

但是，这确实是有代价的：使用 IndexedDB 要比 Web Storage API 复杂得多。在本节中，我们仅仅只能浅尝辄止地一提它的能力，不过我们会给你足够基础知识以帮助你开始。

### 离线文件存储

上面的示例已经说明了如何创建一个将大型资产存储在 IndexedDB 数据库中的应用程序，从而无需多次下载它们。这已经是对用户体验的一个很大的改进，但仍然有一件事 - 每次访问网站时仍然需要下载主要的 HTML，CSS 和 JavaScript 文件，这意味着当没有网络连接时，它将无法工作。

这就是服务工作者和密切相关的Cache API 的用武之地。

服务工作者是一个 JavaScript 文件，简单地说，它是在浏览器访问时针对特定来源（网站或某个域的网站的一部分）进行注册的。注册后，它可以控制该来源的可用页面。它通过坐在加载的页面和网络之间以及拦截针对该来源的网络请求来实现这一点。

当它拦截一个请求时，它可以做任何你想做的事情（参见用例思路），但经典的例子是离线保存网络响应，然后提供响应请求而不是来自网络的响应。实际上，它允许您使网站完全脱机工作。

Cache API 是另一种客户端存储机制，略有不同 - 它旨在保存 HTTP 响应，因此与服务工作者一起工作得非常好。

### 一个 service worker 例子‘

让我们看一个例子，让你对这可能是什么样子有所了解。我们已经创建了另一个版本的视频存储示例，我们在上一节中看到了 - 这个功能完全相同，只是它还通过服务工作者将 Cache，CSS 和 JavaScript 保存在 Cache API 中，允许示例脱机运行！

### 注册服务工作者

首先要注意的是，在主 JavaScript 文件中放置了一些额外的代码（请参阅index.js）。首先，我们进行特征检测测试，以查看serviceWorker该Navigator对象中是否有该成员。如果返回 true，那么我们知道至少支持服务工作者的基础知识。在这里，我们使用该ServiceWorkerContainer.register()方法将sw.js文件中包含的服务工作者注册到它所驻留的源，因此它可以控制与它或子目录相同的目录中的页面。当其承诺履行时，服务人员被视为已注册。

### 安装 service worker

下次访问服务工作者控制下的任何页面时（例如，重新加载示例时），将针对该页面安装服务工作者，这意味着它将开始控制它。发生这种情况时，install会向服务工作人员发起一个事件; 您可以在服务工作者本身内编写代码来响应安装。

让我们看一下sw.js文件（服务工作者）中的一个例子。您将看到安装侦听器已注册self。此self关键字是一种从服务工作文件内部引用服务工作者的全局范围的方法。

在install 处理程序内部，我们使用ExtendableEvent.waitUntil()事件对象上可用的方法来表示浏览器不应该完成服务工作者的安装，直到其中的 promise 成功完成。

这是我们在运行中看到 Cache API 的地方。我们使用该CacheStorage.open()方法打开一个可以存储响应的新缓存对象（类似于 IndexedDB 对象存储）。此承诺通过Cache表示video-store缓存的对象来实现。然后，我们使用该Cache.addAll()方法获取一系列资产并将其响应添加到缓存中。

<hr>

## JavaScript 标准内置对象

这里的术语"全局对象"（或标准内置对象）不应与global 对象混淆。这里的"全局对象"指的是处在全局作用域里的多个对象。

global 对象可以在全局作用域里通过使用this访问到（但只有在 ECMAScript 5 的非严格模式下才可以，在严格模式下得到的是 undefined）。其实全局作用域包含全局对象中的属性，包括它可能继承来的属性。

全局作用域中的其他对象则可由用户的脚本创建，或由宿主程序提供。浏览器环境中所提供的宿主对象的说明可以在这里找到：API 参考。

## 标准内置对象分类

### 值属性

这些全局属性返回一个简单值，这些值没有自己的属性和方法。

- Infinity
- NaN
- undefined
- globalThis

### 函数属性

全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。

- eval()
- uneval()
- isFinite()
- isNaN()
- parseFloat()
- parseInt()
- decodeURI()
- decodeURIComponent()
- encodeURI()
- encodeURIComponent()

### 基本对象

顾名思义，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。

- Object
- Function
- Boolean
- Symbol

### 错误对象

错误对象是一种特殊的基本对象。它们拥有基本的 Error 类型，同时也有多种具体的错误类型。

- Error
- AggregateError
- EvalError
- InternalError
- RangeError
- ReferenceError
- SyntaxError
- TypeError
- URIError

### 数字和日期对象

用来表示数字、日期和执行数学计算的对象。

- Number
- BigInt
- Math
- Date

### 字符串

用来表示和操作字符串的对象。

- String
- RegExp

### 可索引的集合对象

这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。

- Array
- Int8Array
- Uint8Array
- Uint8ClampedArray
- Int16Array
- Uint16Array
- Int32Array
- Uint32Array
- Float32Array
- Float64Array
- BigInt64Array
- BigUint64Array

### 使用键的集合对象

这些集合对象在存储数据时会使用到键，包括可迭代的Map 和 Set，支持按照插入顺序来迭代元素。

- Map
- Set
- WeakMap
- WeakSet

### 结构化数据

这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON（JavaScript Object Notation）编码的数据。

- ArrayBuffer
- SharedArrayBuffer
- Atomics
- DataView
- JSON

### 控制抽象对象

控件抽象可以帮助构造代码，尤其是异步代码（例如，不使用深度嵌套的回调）。

- Promise
- Generator
- GeneratorFunction
- AsyncFunction

### 反射

- Reflect
- Proxy

### 国际化

ECMAScript 核心的附加功能，用于支持多语言处理。

- Intl
- Intl.Collator
- Intl.DateTimeFormat
- Intl.ListFormat
- Intl.NumberFormat
- Intl.PluralRules
- Intl.RelativeTimeFormat
- Intl.Locale

### WebAssembly- 

- WebAssembly
- WebAssembly.Module
- WebAssembly.Instance
- WebAssembly.Memory
- WebAssembly.Table
- WebAssembly.CompileError
- WebAssembly.LinkError (en-US)
- WebAssembly.RuntimeError

### 其他

- arguments

### 给定 undefined 和 null 类型使用 Object

下面的例子将一个空的 Object 对象存到 o 中：

```
var o = new Object();

var o = new Object(undefined);

var o = new Object(null);
```

### 使用 Object 生成布尔对象

下面的例子将Boolean 对象存到 o 中：

```
// 等价于 o = new Boolean(true);
var o = new Object(true);

// 等价于 o = new Boolean(false);
var o = new Object(Boolean());
```

当我们要修改现有的 Object.prototype 方法时，请你考虑一下在已经存在的逻辑之前或者之后通过包装扩展代码的方式来注入代码。 比如说，有一段代码将会在执行内部逻辑或者是其他扩展之前，有条件的执行一段自定义的逻辑。

当一个函数被调用时，调用的参数被保存在一个类似数组的“变量” arguments。 比如说：在调用 myFn(a, b, c) 时，myFunc 函数体中的 arguments 将会包含三个类似数组的元素，对应 (a, b , c)

当使用钩子修改原型时，通过调用函数的apply()将此参数和参数(调用状态)传递给当前行为。此模式可用于任何原型，例如Node。原型,函数。原型等。

<hr>

## Object() 构造函数

Object 构造函数将给定的值包装为一个新对象。

- 如果给定的值是 null 或 undefined, 它会创建并返回一个空对象。
- 否则，它将返回一个和给定的值相对应的类型的对象。
- 如果给定值是一个已经存在的对象，则会返回这个已经存在的值（相同地址）。

在非构造函数上下文中调用时， Object 和 new Object()表现一致。

### 语法

```
new Object()
new Object(value)
```

### 参数

value

任意值

### Object.prototype.constructor

constructor 属性返回 Object 的构造函数（用于创建实例对象）。注意，此属性的值是对函数本身的引用，而不是一个包含函数名称的字符串。

原始类型的值是只读的，如：1、true 和 "test"。

### 描述

所有对象（使用 Object.create(null) 创建的对象除外）都将具有 constructor 属性。在没有显式使用构造函数的情况下，创建的对象（例如对象和数组文本）将具有 constructor 属性，这个属性指向该对象的基本对象构造函数类型。

```
const o = {}
o.constructor === Object // true

const o = new Object
o.constructor === Object // true

const a = []
a.constructor === Array // true

const a = new Array
a.constructor === Array // true

const n = new Number(3)
n.constructor === Number // true
```

### 示例

打印对象的构造函数

下面这个示例创建一个构造函数（Tree），以及该类型的对象（theTree）。然后打印了 theTree 对象的 constructor 属性。

```
function Tree(name) {
  this.name = name
}

const theTree = new Tree('Redwood')
console.log('theTree.constructor is ' + theTree.constructor)
```

打印输出：

```
theTree.constructor is function Tree(name) {
  this.name = name
}
```

### Object.assign()

Object.assign() 方法将所有可枚举（Object.propertyIsEnumerable() 返回 true）的自有（Object.hasOwnProperty() 返回 true）属性从一个或多个源对象复制到目标对象，返回修改后的对象。

```
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// Expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget === target);
// Expected output: true
```

### 语法

```
Object.assign(target, ...sources)
```

### 参数

target
目标对象，接收源对象属性的对象，也是修改后的返回值。

sources
源对象，包含将被合并的属性。

### 返回值

目标对象。

### 描述

如果目标对象与源对象具有相同的 key，则目标对象中的属性将被源对象中的属性覆盖，后面的源对象的属性将类似地覆盖前面的源对象的属性。

Object.assign 方法只会拷贝源对象 可枚举的 和 自身的 属性到目标对象。该方法使用源对象的 [[Get]] 和目标对象的 [[Set]]，它会调用 getters 和 setters。故它分配属性，而不仅仅是复制或定义新的属性。如果合并源包含 getters，这可能使其不适合将新属性合并到原型中。

为了将属性定义（包括其可枚举性）复制到原型，应使用 Object.getOwnPropertyDescriptor() 和 Object.defineProperty()，基本类型 String 和 Symbol 的属性会被复制。

如果赋值期间出错，例如如果属性不可写，则会抛出 TypeError；如果在抛出异常之前添加了任何属性，则会修改 target 对象（译者注：换句话说，Object.assign() 没有“回滚”之前赋值的概念，它是一个尽力而为、可能只会完成部分复制的方法）。

备注： Object.assign() 不会在 source 对象值为 null 或 undefined 时抛出错误。

```
var c = Object.assign(a, b, null, undefined)

{1: '2', 2: '22', 3: '3'}
```

### 示例

复制对象

```
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```

### 深拷贝问题

针对深拷贝, 需要使用其他办法，因为 Object.assign() 只复制属性值。

假如源对象是一个对象的引用，它仅仅会复制其引用值。

```
function test() {
  'use strict';

  let obj1 = { a: 0 , b: { c: 0}};
  let obj2 = Object.assign({}, obj1);
  console.log(JSON.stringify(obj2)); // { "a": 0, "b": { "c": 0}}

  obj1.a = 1;
  console.log(JSON.stringify(obj1)); // { "a": 1, "b": { "c": 0}}
  console.log(JSON.stringify(obj2)); // { "a": 0, "b": { "c": 0}}

  obj2.a = 2;
  console.log(JSON.stringify(obj1)); // { "a": 1, "b": { "c": 0}}
  console.log(JSON.stringify(obj2)); // { "a": 2, "b": { "c": 0}}

  obj2.b.c = 3;
  console.log(JSON.stringify(obj1)); // { "a": 1, "b": { "c": 3}}
  console.log(JSON.stringify(obj2)); // { "a": 2, "b": { "c": 3}}

  // Deep Clone
  obj1 = { a: 0 , b: { c: 0}};
  let obj3 = JSON.parse(JSON.stringify(obj1));
  obj1.a = 4;
  obj1.b.c = 4;
  console.log(JSON.stringify(obj3)); // { "a": 0, "b": { "c": 0}}
}

test();
```

### 合并对象

```
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };

const obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, target object itself is changed.
```

### 合并具有相同属性的对象

```
const o1 = { a: 1, b: 1, c: 1 };
const o2 = { b: 2, c: 2 };
const o3 = { c: 3 };

const obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
```

属性会被后续参数中具有相同属性的其他对象覆盖。

### 拷贝 Symbol 类型属性

```
const o1 = { a: 1 };
const o2 = { [Symbol('foo')]: 2 };

const obj = Object.assign({}, o1, o2);
console.log(obj); // { a : 1, [Symbol("foo")]: 2 } (cf. bug 1207182 on Firefox)
Object.getOwnPropertySymbols(obj); // [Symbol(foo)]
```

### 原型链上的属性和不可枚举属性不能被复制

```
const obj = Object.create({ foo: 1 }, { // foo is on obj's prototype chain.
  bar: {
    value: 2  // bar is a non-enumerable property.
  },
  baz: {
    value: 3,
    enumerable: true  // baz is an own enumerable property.
  }
});

const copy = Object.assign({}, obj);
console.log(copy); // { baz: 3 }
```

### 基本类型会被包装为对象

```
const v1 = 'abc';
const v2 = true;
const v3 = 10;
const v4 = Symbol('foo');

const obj = Object.assign({}, v1, null, v2, undefined, v3, v4);
// Primitives will be wrapped, null and undefined will be ignored.
// Note, only string wrappers can have own enumerable properties.
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```

### 异常会打断后续拷贝任务

```
const target = Object.defineProperty({}, 'foo', {
  value: 1,
  writable: false
}); // target.foo is a read-only property

Object.assign(target, { bar: 2 }, { foo2: 3, foo: 3, foo3: 3 }, { baz: 4 });
// TypeError: "foo" is read-only
// The Exception is thrown when assigning target.foo

console.log(target.bar);  // 2, the first source was copied successfully.
console.log(target.foo2); // 3, the first property of the second source was copied successfully.
console.log(target.foo);  // 1, exception is thrown here.
console.log(target.foo3); // undefined, assign method has finished, foo3 will not be copied.
console.log(target.baz);  // undefined, the third source will not be copied either.
```

### Object.create()

Object.create() 方法用于创建一个新对象，使用现有的对象来作为新创建对象的原型（prototype）。

```
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // Inherited properties can be overwritten

me.printIntroduction();
// Expected output: "My name is Matthew. Am I human? true"
```

![1675241745270](https://user-images.githubusercontent.com/59645426/215996490-dd5db3a3-66f9-474c-a6a0-0a5b40a6310a.png)

### 语法

```
Object.create(proto)
Object.create(proto, propertiesObject)
```

### 参数

proto

新创建对象的原型对象。


propertiesObject 可选

如果该参数被指定且不为 undefined，则该传入对象的自有可枚举属性（即其自身定义的属性，而不是其原型链上的枚举属性）将为新创建的对象添加指定的属性值和对应的属性描述符。这些属性对应于 Object.defineProperties() 的第二个参数。

### 返回值

一个新对象，带着指定的原型对象及其属性。

### 异常

proto 参数需为

- null 或
- 除基本类型包装对象以外的对象

如果 proto 不是这几类值，则抛出一个 TypeError 异常。

### 使用 null 原型的对象

以 null 为原型的对象存在不可预期的行为，因为它未从 Object.prototype 继承任何对象方法。特别是在调试时，因为常见的对象属性的转换/检测工具可能会产生错误或丢失信息（特别是在静默模式，会忽略错误的情况下）。

例如，缺少 Object.prototype.toString() 方法通常会使调试变得非常困难：

```
const normalObj = {};   // create a normal object
const nullProtoObj = Object.create(null); // create an object with "null" prototype

console.log("normalObj is: " + normalObj); // shows "normalObj is: [object Object]"
console.log("nullProtoObj is: " + nullProtoObj); // throws error: Cannot convert object to primitive value

alert(normalObj); // shows [object Object]
alert(nullProtoObj); // throws error: Cannot convert object to primitive value
```

其它方法也同样会失败。

```
normalObj.valueOf() // shows {}
nullProtoObj.valueOf() // throws error: nullProtoObj.valueOf is not a function

normalObj.hasOwnProperty("p") // shows "true"
nullProtoObj.hasOwnProperty("p") // throws error: nullProtoObj.hasOwnProperty is not a function

normalObj.constructor // shows "Object() { [native code] }"
nullProtoObj.constructor // shows "undefined"
```

我们可以为以 null 为原型的对象添加 toString 方法，类似于这样：

```
nullProtoObj.toString = Object.prototype.toString; // since new object lacks toString, add the original generic one back

console.log(nullProtoObj.toString()); // shows "[object Object]"
console.log("nullProtoObj is: " + nullProtoObj); // shows "nullProtoObj is: [object Object]"
```

与常规的对象不同，nullProtoObj 的 toString 方法是这个对象自身的属性，而非继承自对象的原型。这是因为 nullProtoObj “没有”原型（null）。

在实践中，以 null 为原型的对象通常用于作为 map 的替代。因为 Object.prototype 原型自有的属性的存在会导致一些错误：

```
const ages = { alice: 18, bob: 27 };

function hasPerson(name) {
  return name in ages;
}

function getAge(name) {
  return ages[name];
}

hasPerson("hasOwnProperty") // true
getAge("toString") // [Function: toString]
```

使用以 null 为原型的对象消除了这种潜在的问题，且不会给 hasPerson 和 getAge 函数引入太多复杂的逻辑：

```
const ages = Object.create(null, {
  alice: { value: 18, enumerable: true },
  bob: { value: 27, enumerable: true },
});

hasPerson("hasOwnProperty") // false
getAge("toString") // undefined
```

在这种情况下，应谨慎添加任何方法，因为它们可能会与存储的键值对混淆。

令你使用的对象不继承 Object.prototype 原型的方法也可以防止原型污染攻击。如果恶意脚本向 Object.prototype 添加了一个属性，这个属性将能够被程序中的每一个对象所访问，而以 null 为原型的对象则不受影响。

```
const user = {};

// A malicious script:
Object.prototype.authenticated = true;

// Unexpectedly allowing unauthenticated user to pass through
if (user.authenticated) {
  // access confidential data...
}
```

### 示例

用 Object.create() 实现类式继承

下面的例子演示了如何使用 Object.create() 来实现类式继承。这是一个所有版本 JavaScript 都支持的单继承。

```
// Shape - superclass
function Shape() {
  this.x = 0;
  this.y = 0;
}

// superclass method
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - subclass
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// subclass extends superclass
Rectangle.prototype = Object.create(Shape.prototype);

//If you don't set Rectangle.prototype.constructor to Rectangle,
//it will take the prototype.constructor of Shape (parent).
//To avoid that, we set the prototype.constructor to Rectangle (child).
Rectangle.prototype.constructor = Rectangle;

const rect = new Rectangle();

console.log('Is rect an instance of Rectangle?', rect instanceof Rectangle); // true
console.log('Is rect an instance of Shape?', rect instanceof Shape); // true
rect.move(1, 1); // Outputs, 'Shape moved.'
```

使用 Object.create() 的 propertyObject 参数

```
let o;

// create an object with null as prototype
o = Object.create(null);

o = {};
// is equivalent to:
o = Object.create(Object.prototype);

// Example where we create an object with a couple of
// sample properties. (Note that the second parameter
// maps keys to *property descriptors*.)
o = Object.create(Object.prototype, {
  // foo is a regular 'value property'
  foo: {
    writable: true,
    configurable: true,
    value: 'hello'
  },
  // bar is a getter-and-setter (accessor) property
  bar: {
    configurable: false,
    get: function() { return 10; },
    set: function(value) {
      console.log('Setting `o.bar` to', value);
    }
/* with ES2015 Accessors our code can look like this
    get() { return 10; },
    set(value) {
      console.log('Setting `o.bar` to', value);
    } */
  }
});

function Constructor() {}
o = new Constructor();
// is equivalent to:
o = Object.create(Constructor.prototype);
// Of course, if there is actual initialization code
// in the Constructor function,
// the Object.create() cannot reflect it

// Create a new object whose prototype is a new, empty
// object and add a single property 'p', with value 42.
o = Object.create({}, { p: { value: 42 } });

// by default properties ARE NOT writable,
// enumerable or configurable:
o.p = 24;
o.p;
// 42

o.q = 12;
for (const prop in o) {
  console.log(prop);
}
// 'q'

delete o.p;
// false

// to specify an ES3 property
o2 = Object.create({}, {
  p: {
    value: 42,
    writable: true,
    enumerable: true,
    configurable: true
  }
});
/* is not equivalent to:
This will create an object with prototype : {p: 42 }
o2 = Object.create({p: 42}) */
```

![1675245651094](https://user-images.githubusercontent.com/59645426/216011741-ab25770d-e1d1-44a1-986c-05fb3a22f22a.png)

### Object.defineProperties()

Object.defineProperties() 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

### 语法

```
Object.defineProperties(obj, props)
```

### 参数

obj
在其上定义或修改属性的对象。

props
  要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符（更多详情，请参阅 Object.defineProperty()）。描述符具有以下键：

  configurable
  true 只有该属性描述符的类型可以被改变并且该属性可以从对应对象中删除。 默认为 false

  enumerable
  true 只有在枚举相应对象上的属性时该属性显现。 默认为 false

  value
  与属性关联的值。可以是任何有效的 JavaScript 值（数字，对象，函数等）。 默认为 undefined.

  writable
  true只有与该属性相关联的值被assignment operator (en-US)改变时。 默认为 false

  get
  作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。 默认为 undefined

  set
  作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。 默认为 undefined

### 返回值

传递给函数的对象。

### 描述

Object.defineProperties 本质上定义了 obj 对象上 props 的可枚举属性相对应的所有属性。

### 例子

```
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```

### Object.defineProperty()

Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

备注： 应当直接在 Object 构造器对象上调用此方法，而不是在任意一个 Object 类型的实例上调用。

```
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});

object1.property1 = 77;
// Throws an error in strict mode

console.log(object1.property1);
// Expected output: 42
```

### 语法

Object.defineProperty(obj, prop, descriptor)

### 参数

obj
要定义属性的对象。

prop
要定义或修改的属性的名称或 Symbol 。

descriptor
要定义或修改的属性描述符。

### 返回值

被传递给函数的对象。

备注： 在 ES6 中，由于 Symbol 类型的特殊性，用 Symbol 类型的值来做对象的 key 与常规的定义或修改不同，而Object.defineProperty 是定义 key 为 Symbol 的属性的方法之一。

### 描述

该方法允许精确地添加或修改对象的属性。通过赋值操作添加的普通属性是可枚举的，在枚举对象属性时会被枚举到（for...in 或 Object.keys方法），可以改变这些属性的值，也可以删除这些属性。这个方法允许修改默认的额外选项（或配置）。默认情况下，使用 Object.defineProperty() 添加的属性值是不可修改（immutable）的。

对象里目前存在的属性描述符有两种主要形式：数据描述符和存取描述符。数据描述符是一个具有值的属性，该值可以是可写的，也可以是不可写的。存取描述符是由 getter 函数和 setter 函数所描述的属性。一个描述符只能是这两者其中之一；不能同时是两者。

这两种描述符都是对象。它们共享以下可选键值（默认值是指在使用 Object.defineProperty() 定义属性时的默认值）：

configurable
当且仅当该属性的 configurable 键值为 true 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。 默认为 false。

enumerable
当且仅当该属性的 enumerable 键值为 true 时，该属性才会出现在对象的枚举属性中。 默认为 false。

### 数据描述符还具有以下可选键值：

value
该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。 默认为 undefined。

writable
当且仅当该属性的 writable 键值为 true 时，属性的值，也就是上面的 value，才能被赋值运算符 (en-US)改变。 默认为 false。

### 存取描述符还具有以下可选键值：

get
属性的 getter 函数，如果没有 getter，则为 undefined。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。 默认为 undefined。

set
属性的 setter 函数，如果没有 setter，则为 undefined。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。 默认为 undefined。

### 描述符默认值汇总

- 拥有布尔值的键 configurable、enumerable 和 writable 的默认值都是 false。
- 属性值和函数的键 value、get 和 set 字段的默认值为 undefined。

### Object.entries()

Object.entries()

方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。

```
const object1 = {
  a: 'somestring',
  b: 42
};

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// Expected output:
// "a: somestring"
// "b: 42"
```

### 语法

Object.entries(obj)

### 参数

obj
可以返回其可枚举属性的键值对的对象。

### 返回值

给定对象自身可枚举属性的键值对数组。

### 描述

Object.entries()返回一个数组，其元素是与直接在object上找到的可枚举属性键值对相对应的数组。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

### 示例

```
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// array like object
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]

// array like object with random key ordering
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]

// getFoo is property which isn't enumerable
const myObj = Object.create({}, { getFoo: { value() { return this.foo; } } });
myObj.foo = 'bar';
console.log(Object.entries(myObj)); // [ ['foo', 'bar'] ]

// non-object argument will be coerced to an object
console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]

// iterate through key-value gracefully
const obj = { a: 5, b: 7, c: 9 };
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
}

// Or, using array extras
Object.entries(obj).forEach(([key, value]) => {
console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
});
```

### 将Object转换为Map

new Map() 构造函数接受一个可迭代的entries。借助Object.entries方法你可以很容易的将Object转换为Map:

```
var obj = { foo: "bar", baz: 42 };
var map = new Map(Object.entries(obj));
console.log(map); // Map { foo: "bar", baz: 42 }
```

### Object.freeze()

Object.freeze() 方法可以冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。freeze() 返回和传入的参数相同的对象。

```
const obj = {
  prop: 42
};

Object.freeze(obj);

obj.prop = 33;
// Throws an error in strict mode

console.log(obj.prop);
// Expected output: 42
```

### 语法

Object.freeze(obj)

### 参数

obj
要被冻结的对象。

### 返回值

被冻结的对象。

### 描述

被冻结对象自身的所有属性都不可能以任何方式被修改。任何修改尝试都会失败，无论是静默地还是通过抛出TypeError异常（最常见但不仅限于strict mode）。

数据属性的值不可更改，访问器属性（有 getter 和 setter）也同样（但由于是函数调用，给人的错觉是还是可以修改这个属性）。如果一个属性的值是个对象，则这个对象中的属性是可以修改的，除非它也是个冻结对象。数组作为一种对象，被冻结，其元素不能被修改。没有数组元素可以被添加或移除。

这个方法返回传递的对象，而不是创建一个被冻结的副本。

例子

冻结对象

```
var obj = {
  prop: function() {},
  foo: 'bar'
};

// 新的属性会被添加，已存在的属性可能
// 会被修改或移除
obj.foo = 'baz';
obj.lumpy = 'woof';
delete obj.prop;

// 作为参数传递的对象与返回的对象都被冻结
// 所以不必保存返回的对象（因为两个对象全等）
var o = Object.freeze(obj);

o === obj; // true
Object.isFrozen(obj); // === true

// 现在任何改变都会失效
obj.foo = 'quux'; // 静默地不做任何事
// 静默地不添加此属性
obj.quaxxor = 'the friendly duck';

// 在严格模式，如此行为将抛出 TypeErrors
function fail(){
  'use strict';
  obj.foo = 'sparky'; // throws a TypeError
  delete obj.quaxxor; // 返回 true，因为 quaxxor 属性从来未被添加
  obj.sparky = 'arf'; // throws a TypeError
}

fail();

// 试图通过 Object.defineProperty 更改属性
// 下面两个语句都会抛出 TypeError.
Object.defineProperty(obj, 'ohai', { value: 17 });
Object.defineProperty(obj, 'foo', { value: 'eit' });

// 也不能更改原型
// 下面两个语句都会抛出 TypeError.
Object.setPrototypeOf(obj, { x: 20 })
obj.__proto__ = { x: 20 }
```

冻结数组

```
let a = [0];
Object.freeze(a); // 现在数组不能被修改了。

a[0]=1; // fails silently
a.push(2); // fails silently

// In strict mode such attempts will throw TypeErrors
function fail() {
  "use strict"
  a[0] = 1;
  a.push(2);
}

fail();
Copy to Clipboard
被冻结的对象是不可变的。但也不总是这样。下例展示了冻结对象不是常量对象（浅冻结）。

obj1 = {
  internal: {}
};

Object.freeze(obj1);
obj1.internal.a = 'aValue';

obj1.internal.a // 'aValue'
```

对于一个常量对象，整个引用图（直接和间接引用其他对象）只能引用不可变的冻结对象。冻结的对象被认为是不可变的，因为整个对象中的整个对象状态（对其他对象的值和引用）是固定的。注意，字符串，数字和布尔总是不可变的，而函数和数组是对象。

要使对象不可变，需要递归冻结每个类型为对象的属性（深冻结）。当你知道对象在引用图中不包含任何 _环 _(循环引用) 时，将根据你的设计逐个使用该模式，否则将触发无限循环。对 deepFreeze() 的增强将是具有接收路径（例如 Array）参数的内部函数，以便当对象进入不变时，可以递归地调用 deepFreeze() 。你仍然有冻结不应冻结的对象的风险，例如 [window]。

```
// 深冻结函数。
function deepFreeze(obj) {

  // 取回定义在 obj 上的属性名
  var propNames = Object.getOwnPropertyNames(obj);

  // 在冻结自身之前冻结属性
  propNames.forEach(function(name) {
    var prop = obj[name];

    // 如果 prop 是个对象，冻结它
    if (typeof prop == 'object' && prop !== null)
      deepFreeze(prop);
  });

  // 冻结自身 (no-op if already frozen)
  return Object.freeze(obj);
}

obj2 = {
  internal: {}
};

deepFreeze(obj2);
obj2.internal.a = 'anotherValue';
obj2.internal.a; // undefined
```

### Notes

在 ES5 中，如果这个方法的参数不是一个对象（一个原始值），那么它会导致 TypeError。在 ES2015 中，非对象参数将被视为要被冻结的普通对象，并被简单地返回。

```
> Object.freeze(1)
TypeError: 1 is not an object // ES5 code

> Object.freeze(1)
1                             // ES2015 code
```

### 对比 Object.seal()

用Object.seal()密封的对象可以改变它们现有的属性。使用Object.freeze() 冻结的对象中现有属性是不可变的。

### Object.fromEntries()

Object.fromEntries() 方法把键值对列表转换为一个对象。

```
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

const obj = Object.fromEntries(entries);

console.log(obj);
// Expected output: Object { foo: "bar", baz: 42 }
```

### 语法

Object.fromEntries(iterable);

### 参数

iterable
类似 Array 、 Map 或者其它实现了可迭代协议的可迭代对象。

### 返回值

一个由该迭代对象条目提供对应属性的新对象。

### 描述

Object.fromEntries() 方法接收一个键值对的列表参数，并返回一个带有这些键值对的新对象。这个迭代参数应该是一个能够实现@@iterator方法的的对象，返回一个迭代器对象。它生成一个具有两个元素的类数组的对象，第一个元素是将用作属性键的值，第二个元素是与该属性键关联的值。

Object.fromEntries() 执行与 Object.entries 互逆的操作。

### 示例

Map 转化为 Object

通过 Object.fromEntries，可以将 Map 转换为 Object:

```
const map = new Map([ ['foo', 'bar'], ['baz', 42] ]);
const obj = Object.fromEntries(map);
console.log(obj); // { foo: "bar", baz: 42 }
```

Array 转化为 Object

通过 Object.fromEntries，可以将 Array 转换为 Object:

```
const arr = [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ];
const obj = Object.fromEntries(arr);
console.log(obj); // { 0: "a", 1: "b", 2: "c" }
```

### 对象转换

Object.fromEntries 是与 Object.entries() 相反的方法，用 数组处理函数 可以像下面这样转换对象：

```
const object1 = { a: 1, b: 2, c: 3 };

const object2 = Object.fromEntries(
  Object.entries(object1)
  .map(([ key, val ]) => [ key, val * 2 ])
);

console.log(object2);
// { a: 2, b: 4, c: 6 }
```

### Object.getOwnPropertyDescriptor()

Object.getOwnPropertyDescriptor() 方法返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

```
const object1 = {
  property1: 42
};

const descriptor1 = Object.getOwnPropertyDescriptor(object1, 'property1');

console.log(descriptor1.configurable);
// Expected output: true

console.log(descriptor1.value);
// Expected output: 42
```

### 语法

Object.getOwnPropertyDescriptor(obj, prop)

### 参数

obj
需要查找的目标对象

prop
目标对象内属性名称

### 返回值

如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。

### 描述

该方法允许对一个属性的描述进行检索。在 Javascript 中，属性 由一个字符串类型的“名字”（name）和一个“属性描述符”（property descriptor）对象构成。更多关于属性描述符类型以及他们属性的信息可以查看：Object.defineProperty.

一个属性描述符是一个记录，由下面属性当中的某些组成的：

value
该属性的值 (仅针对数据属性描述符有效)

writable
当且仅当属性的值可以被改变时为 true。(仅针对数据属性描述有效)

get
获取该属性的访问器函数（getter）。如果没有访问器，该值为 undefined。(仅针对包含访问器或设置器的属性描述有效)

set
获取该属性的设置器函数（setter）。如果没有设置器，该值为 undefined。(仅针对包含访问器或设置器的属性描述有效)

configurable
当且仅当指定对象的属性描述可以被改变或者属性可被删除时，为 true。

enumerable
当且仅当指定对象的属性可以被枚举出时，为 true。

示例

```
var o, d;

o = { get foo() { return 17; } };
d = Object.getOwnPropertyDescriptor(o, "foo");
// d {
//   configurable: true,
//   enumerable: true,
//   get: /*the getter function*/,
//   set: undefined
// }

o = { bar: 42 };
d = Object.getOwnPropertyDescriptor(o, "bar");
// d {
//   configurable: true,
//   enumerable: true,
//   value: 42,
//   writable: true
// }

o = {};
Object.defineProperty(o, "baz", {
  value: 8675309,
  writable: false,
  enumerable: false
});
d = Object.getOwnPropertyDescriptor(o, "baz");
// d {
//   value: 8675309,
//   writable: false,
//   enumerable: false,
//   configurable: false
// }
```

### 注意事项

在 ES5 中，如果该方法的第一个参数不是对象（而是原始类型），那么就会产生出现 TypeError。而在 ES2015，第一个的参数不是对象的话就会被强制转换为对象。

```
Object.getOwnPropertyDescriptor('foo', 0);
// 类型错误："foo" 不是一个对象  // ES5 code

Object.getOwnPropertyDescriptor('foo', 0);
// Object returned by ES2015 code: {
//   configurable: false,
//   enumerable: true,
//   value: "f",
//   writable: false
// }
```

### Object.getOwnPropertyDescriptors()

Object.getOwnPropertyDescriptors() 方法用来获取一个对象的所有自身属性的描述符。

### 语法

Object.getOwnPropertyDescriptors(obj)

### 参数

obj
任意对象

### 返回值

所指定对象的所有自身属性的描述符，如果没有任何自身属性，则返回空对象。

### 示例

浅拷贝一个对象

Object.assign() 方法只能拷贝源对象的可枚举的自身属性，同时拷贝时无法拷贝属性的特性们，而且访问器属性会被转换成数据属性，也无法拷贝源对象的原型，该方法配合 Object.create() 方法可以实现上面说的这些。

```
Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
);
```

### 创建子类

创建子类的典型方法是定义子类，将其原型设置为超类的实例，然后在该实例上定义属性。这么写很不优雅，特别是对于 getters 和 setter 而言。相反，您可以使用此代码设置原型：

```
function superclass() {}
superclass.prototype = {
  // 在这里定义方法和属性
};
function subclass() {}
subclass.prototype = Object.create(superclass.prototype, Object.getOwnPropertyDescriptors({
  // 在这里定义方法和属性
}));
```

### Object.getOwnPropertyNames()

Object.getOwnPropertyNames()方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括 Symbol 值作为名称的属性）组成的数组。

### 语法

Object.getOwnPropertyNames(obj)

### 参数

obj
一个对象，其自身的可枚举和不可枚举属性的名称被返回。

### 返回值

在给定对象上找到的自身属性对应的字符串数组。

### 描述

Object.getOwnPropertyNames() 返回一个数组，该数组对元素是 obj自身拥有的枚举或不可枚举属性名称字符串。数组中枚举属性的顺序与通过 for...in 循环（或 Object.keys）迭代该对象属性时一致。数组中不可枚举属性的顺序未定义。

### 示例

使用 Object.getOwnPropertyNames()

```
var arr = ["a", "b", "c"];
console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]

// 类数组对象
var obj = { 0: "a", 1: "b", 2: "c"};
console.log(Object.getOwnPropertyNames(obj).sort()); // ["0", "1", "2"]

// 使用 Array.forEach 输出属性名和属性值
Object.getOwnPropertyNames(obj).forEach(function(val, idx, array) {
  console.log(val + " -> " + obj[val]);
});
// 输出
// 0 -> a
// 1 -> b
// 2 -> c

//不可枚举属性
var my_obj = Object.create({}, {
  getFoo: {
    value: function() { return this.foo; },
    enumerable: false
  }
});
my_obj.foo = 1;

console.log(Object.getOwnPropertyNames(my_obj).sort()); // ["foo", "getFoo"]
```

如果你只要获取到可枚举属性，查看Object.keys或用for...in循环（还会获取到原型链上的可枚举属性，不过可以使用hasOwnProperty()方法过滤掉）。

下面的例子演示了该方法不会获取到原型链上的属性：

```
function ParentClass() {}
ParentClass.prototype.inheritedMethod = function() {};

function ChildClass() {
  this.prop = 5;
  this.method = function() {};
}

ChildClass.prototype = new ParentClass;
ChildClass.prototype.prototypeMethod = function() {};

console.log(
  Object.getOwnPropertyNames(
    new ChildClass()  // ["prop", "method"]
  )
);
```

### Object.getOwnPropertySymbols()

Object.getOwnPropertySymbols() 方法返回一个给定对象自身的所有 Symbol 属性的数组。

### 语法

Object.getOwnPropertySymbols(obj)

### 参数

obj
要返回 Symbol 属性的对象。

### 返回值

在给定对象自身上找到的所有 Symbol 属性的数组。

### 描述

与Object.getOwnPropertyNames()类似，您可以将给定对象的所有符号属性作为 Symbol 数组获取。请注意，Object.getOwnPropertyNames()本身不包含对象的 Symbol 属性，只包含字符串属性。

因为所有的对象在初始化的时候不会包含任何的 Symbol，除非你在对象上赋值了 Symbol 否则Object.getOwnPropertySymbols()只会返回一个空的数组。

### 示例

```
var obj = {};
var a = Symbol("a");
var b = Symbol.for("b");

obj[a] = "localSymbol";
obj[b] = "globalSymbol";

var objectSymbols = Object.getOwnPropertySymbols(obj);

console.log(objectSymbols.length); // 2
console.log(objectSymbols)         // [Symbol(a), Symbol(b)]
console.log(objectSymbols[0])      // Symbol(a)
```

### Object.getPrototypeOf()

Object.getPrototypeOf() 方法返回指定对象的原型（内部[[Prototype]]属性的值）。

### 语法

Object.getPrototypeOf(object)

### 参数obj

要返回其原型的对象。

### 返回值

给定对象的原型。如果没有继承属性，则返回 null 。

```
const prototype1 = {};
const object1 = Object.create(prototype1);

console.log(Object.getPrototypeOf(object1) === prototype1);
// Expected output: true
```

### 示例

```
var proto = {};
var obj = Object.create(proto);
Object.getPrototypeOf(obj) === proto; // true

var reg = /a/;
Object.getPrototypeOf(reg) === RegExp.prototype; // true
```

### 说明

JavaScript 中的 Object 是构造函数（创建对象的包装器）。

```
// 一般用法是：
var obj = new Object();

所以：
Object.getPrototypeOf( Object );               // ƒ () { [native code] }
Object.getPrototypeOf( Function );             // ƒ () { [native code] }

Object.getPrototypeOf( Object ) === Function.prototype;        // true

Object.getPrototypeOf( Object ) 是把 Object 这一构造函数看作对象，
返回的当然是函数对象的原型，也就是 Function.prototype。

正确的方法是，Object.prototype 是构造出来的对象的原型。
var obj = new Object();
Object.prototype === Object.getPrototypeOf( obj );              // true

Object.prototype === Object.getPrototypeOf( {} );               // true
```

### Notes

在 ES5 中，如果参数不是一个对象类型，将抛出一个TypeError异常。在 ES2015 中，参数会被强制转换为一个 Object。

```
Object.getPrototypeOf('foo');
// TypeError: "foo" is not an object (ES5 code)
Object.getPrototypeOf('foo');
// String.prototype                  (ES2015 code)
```

### Object.hasOwn()

如果指定的对象自身有指定的属性，则静态方法 Object.hasOwn() 返回 true。如果属性是继承的或者不存在，该方法返回 false。

备注： Object.hasOwn() 旨在取代 Object.hasOwnProperty()。

```
const object1 = {
  prop: 'exists'
};

console.log(Object.hasOwn(object1, 'prop'));
// Expected output: true

console.log(Object.hasOwn(object1, 'toString'));
// Expected output: false

console.log(Object.hasOwn(object1, 'undeclaredPropertyValue'));
// Expected output: false
```

### 语法

hasOwn(instance, prop)

### 参数

instance
要测试的 JavaScript 实例对象。

prop
要测试属性的 String 类型的名称或者 Symbol。

### 返回值

如果指定的对象中直接定义了指定的属性，则返回 true；否则返回 false。

### 描述

如果指定的属性是该对象的直接属性——Object.hasOwn() 方法返回 true，即使属性值是 null 或 undefined。如果属性是继承的或者不存在，该方法返回 false。它不像 in 运算符，这个方法不检查对象的原型链中的指定属性。

建议使用此方法替代 Object.hasOwnProperty()，因为它适用于使用 Object.create(null) 创建的对象以及覆盖了继承的 hasOwnProperty() 方法的对象。尽管可以通过在外部对象上调用 Object.prototype.hasOwnProperty() 解决这些问题，但是 Object.hasOwn() 更加直观。

### 示例

使用 hasOwn 去测试属性是否存在

以下编码展示了如何确定 example 对象中是否包含名为 prop 的属性。

```
const example = {};
Object.hasOwn(example, 'prop');   // false - 'prop' has not been defined

example.prop = 'exists';
Object.hasOwn(example, 'prop');   // true - 'prop' has been defined

example.prop = null;
Object.hasOwn(example, 'prop');   // true - own property exists with value of null

example.prop = undefined;
Object.hasOwn(example, 'prop');   // true - own property exists with value of undefined
```

直接属性和继承属性

以下示例区分了直接属性和通过原型链继承的属性：

```
const example = {};
example.prop = 'exists';

// `hasOwn` will only return true for direct properties:
Object.hasOwn(example, 'prop');             // returns true
Object.hasOwn(example, 'toString');         // returns false
Object.hasOwn(example, 'hasOwnProperty');   // returns false

// The `in` operator will return true for direct or inherited properties:
'prop' in example;                          // returns true
'toString' in example;                      // returns true
'hasOwnProperty' in example;                // returns true
```

### 迭代对象的属性

要迭代对象的可枚举属性，你应该这样使用：

```
const example = { foo: true, bar: true };
for (const name of Object.keys(example)) {
  // …
}
```

但是如果你使用 for...in，你应该使用 Object.hasOwn() 跳过继承属性：

```
const example = { foo: true, bar: true };
for (const name in example) {
  if (Object.hasOwn(example, name)) {
    // …
  }
}
```

### 检查数组索引是否存在

Array 中的元素被定义为直接属性，所以你可以使用 hasOwn() 方法去检查一个指定的索引是否存在：

```
const fruits = ['Apple', 'Banana','Watermelon', 'Orange'];
Object.hasOwn(fruits, 3);   // true ('Orange')
Object.hasOwn(fruits, 4);   // false - not defined
```

### hasOwnProperty 的问题案例

本部分证明了影响 hasOwnProperty 的问题对 hasOwn() 是免疫的。首先，它可以与重新实现的 hasOwnProperty() 一起使用：

```
const foo = {
  hasOwnProperty() {
    return false;
  },
  bar: 'The dragons be out of office',
};

if (Object.hasOwn(foo, 'bar')) {
  console.log(foo.bar); //true - reimplementation of hasOwnProperty() does not affect Object
}
```

它也可以用于测试使用 Object.create(null) 创建的对象。这些都不继承自 Object.prototype，因此 hasOwnProperty() 无法访问。

```
const foo = Object.create(null);
foo.prop = 'exists';
if (Object.hasOwn(foo, 'prop')) {
  console.log(foo.prop); //true - works irrespective of how the object is created.
}
```

### Object.prototype.hasOwnProperty()

hasOwnProperty() 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。

```
const object1 = {};
object1.property1 = 42;

console.log(object1.hasOwnProperty('property1'));
// Expected output: true

console.log(object1.hasOwnProperty('toString'));
// Expected output: false

console.log(object1.hasOwnProperty('hasOwnProperty'));
// Expected output: false
```

### 语法

obj.hasOwnProperty(prop)

### 参数

prop
要检测的属性的 String 字符串形式表示的名称，或者 Symbol。

### 返回值

用来判断某个对象是否含有指定的属性的布尔值 Boolean。

### 描述

所有继承了 Object 的对象都会继承到 hasOwnProperty 方法。这个方法可以用来检测一个对象是否含有特定的自身属性；和 in 运算符不同，该方法会忽略掉那些从原型链上继承到的属性。

### 备注

即使属性的值是 null 或 undefined，只要属性存在，hasOwnProperty 依旧会返回 true。

```
o = new Object();
o.propOne = null;
o.hasOwnProperty('propOne'); // 返回 true
o.propTwo = undefined;
o.hasOwnProperty('propTwo'); // 返回 true
```

### 示例

使用 hasOwnProperty 方法判断属性是否存在

下面的例子检测了对象 o 是否含有自身属性 prop：

```
o = new Object();
o.hasOwnProperty('prop'); // 返回 false
o.prop = 'exists';
o.hasOwnProperty('prop'); // 返回 true
delete o.prop;
o.hasOwnProperty('prop'); // 返回 false
```

### 自身属性与继承属性

下面的例子演示了 hasOwnProperty 方法对待自身属性和继承属性的区别：

```
o = new Object();
o.prop = 'exists';
o.hasOwnProperty('prop');             // 返回 true
o.hasOwnProperty('toString');         // 返回 false
o.hasOwnProperty('hasOwnProperty');   // 返回 false
```

### 遍历一个对象的所有自身属性

下面的例子演示了如何在遍历一个对象的所有属性时忽略掉继承属性，注意这里 for...in 循环只会遍历可枚举属性，所以不应该基于这个循环中没有不可枚举的属性而得出 hasOwnProperty 是严格限制于可枚举项目的（如同 Object.getOwnPropertyNames()）。

```
var buz = {
  fog: 'stack'
};

for (var name in buz) {
  if (buz.hasOwnProperty(name)) {
    console.log('this is fog (' +
      name + ') for sure. Value: ' + buz[name]);
  }
  else {
    console.log(name); // toString or something else
  }
}
```

### 使用 hasOwnProperty 作为属性名

JavaScript 并没有保护 hasOwnProperty 这个属性名，因此，当某个对象可能自有一个占用该属性名的属性时，就需要使用外部的 hasOwnProperty 获得正确的结果：

```
var foo = {
  hasOwnProperty: function() {
    return false;
  },
  bar: 'Here be dragons'
};

foo.hasOwnProperty('bar'); // 始终返回 false

// 如果担心这种情况，
// 可以直接使用原型链上真正的 hasOwnProperty 方法
({}).hasOwnProperty.call(foo, 'bar'); // true

// 也可以使用 Object 原型上的 hasOwnProperty 属性
Object.prototype.hasOwnProperty.call(foo, 'bar'); // true
```

注意，只有在最后一种情况下，才不会新建任何对象。

### Object.is()

Object.is() 方法判断两个值是否为同一个值。

### 语法

Object.is(value1, value2);

### 参数

value1
被比较的第一个值。

value2
被比较的第二个值。

### 返回值

一个布尔值，表示两个参数是否是同一个值。

### 描述

Object.is() 方法判断两个值是否为同一个值，如果满足以下任意条件则两个值相等：

      都是 undefined
      都是 null
      都是 true 或都是 false
      都是相同长度、相同字符、按相同顺序排列的字符串
      都是相同对象（意味着都是同一个对象的值引用）
      都是数字且
      都是 +0
      都是 -0
      都是 NaN
      都是同一个值，非零且都不是 NaN

Object.is() 与 == 不同。== 运算符在判断相等前对两边的变量（如果它们不是同一类型）进行强制转换（这种行为将 "" == false 判断为 true），而 Object.is 不会强制转换两边的值。

Object.is() 与 === 也不相同。差别是它们对待有符号的零和 NaN 不同，例如，=== 运算符（也包括 == 运算符）将数字 -0 和 +0 视为相等，而将 Number.NaN 与 NaN 视为不相等。

### 示例

使用 Object.is

```
// Case 1: Evaluation result is the same as using ===
Object.is(25, 25);                // true
Object.is('foo', 'foo');          // true
Object.is('foo', 'bar');          // false
Object.is(null, null);            // true
Object.is(undefined, undefined);  // true
Object.is(window, window);        // true
Object.is([], []);                // false
var foo = { a: 1 };
var bar = { a: 1 };
Object.is(foo, foo);              // true
Object.is(foo, bar);              // false

// Case 2: Signed zero
Object.is(0, -0);                 // false
Object.is(+0, -0);                // false
Object.is(-0, -0);                // true
Object.is(0n, -0n);               // true

// Case 3: NaN
Object.is(NaN, 0/0);              // true
Object.is(NaN, Number.NaN)        // true
```

### Object.isExtensible()

### 概述

Object.isExtensible() 方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。

### 语法

Object.isExtensible(obj)

### 参数

obj
需要检测的对象

### 返回值

表示给定对象是否可扩展的一个 Boolean。

### 描述

默认情况下，对象是可扩展的：即可以为他们添加新的属性。以及它们的 Object.prototype.__proto__ 已弃用 属性可以被更改。Object.preventExtensions，Object.seal 或 Object.freeze 方法都可以标记一个对象为不可扩展（non-extensible）。

### 例子

```
// 新对象默认是可扩展的。
var empty = {};
Object.isExtensible(empty); // === true

// ...可以变的不可扩展。
Object.preventExtensions(empty);
Object.isExtensible(empty); // === false

// 密封对象是不可扩展的。
var sealed = Object.seal({});
Object.isExtensible(sealed); // === false

// 冻结对象也是不可扩展。
var frozen = Object.freeze({});
Object.isExtensible(frozen); // === false
```

### 注意

在 ES5 中，如果参数不是一个对象类型，将抛出一个 TypeError 异常。在 ES6 中，non-object 参数将被视为一个不可扩展的普通对象，因此会返回 false。

```
Object.isExtensible(1);
// TypeError: 1 is not an object (ES5 code)

Object.isExtensible(1);
// false                         (ES6 code)
```

### Object.isFrozen()

Object.isFrozen()方法判断一个对象是否被冻结。

### 语法

Object.isFrozen(obj)

### 参数

obj
被检测的对象。

### 返回值

表示给定对象是否被冻结的Boolean。

### 描述

一个对象是冻结的是指它不可扩展，所有属性都是不可配置的，且所有数据属性（即没有 getter 或 setter 组件的访问器的属性）都是不可写的。

### 例子

```
// 一个对象默认是可扩展的，所以它也是非冻结的。
Object.isFrozen({}); // === false

// 一个不可扩展的空对象同时也是一个冻结对象。
var vacuouslyFrozen = Object.preventExtensions({});
Object.isFrozen(vacuouslyFrozen) //=== true;

// 一个非空对象默认也是非冻结的。
var oneProp = { p: 42 };
Object.isFrozen(oneProp) //=== false

// 让这个对象变的不可扩展，并不意味着这个对象变成了冻结对象，
// 因为 p 属性仍然是可以配置的 (而且可写的).
Object.preventExtensions(oneProp);
Object.isFrozen(oneProp) //=== false

// 此时，如果删除了这个属性，则它会成为一个冻结对象。
delete oneProp.p;
Object.isFrozen(oneProp) //=== true

// 一个不可扩展的对象，拥有一个不可写但可配置的属性，则它仍然是非冻结的。
var nonWritable = { e: "plep" };
Object.preventExtensions(nonWritable);
Object.defineProperty(nonWritable, "e", { writable: false }); // 变得不可写
Object.isFrozen(nonWritable) //=== false

// 把这个属性改为不可配置，会让这个对象成为冻结对象。
Object.defineProperty(nonWritable, "e", { configurable: false }); // 变得不可配置
Object.isFrozen(nonWritable) //=== true

// 一个不可扩展的对象，拥有一个不可配置但可写的属性，则它仍然是非冻结的。
var nonConfigurable = { release: "the kraken!" };
Object.preventExtensions(nonConfigurable);
Object.defineProperty(nonConfigurable, "release", { configurable: false });
Object.isFrozen(nonConfigurable) //=== false

// 把这个属性改为不可写，会让这个对象成为冻结对象。
Object.defineProperty(nonConfigurable, "release", { writable: false });
Object.isFrozen(nonConfigurable) //=== true

// 一个不可扩展的对象，值拥有一个访问器属性，则它仍然是非冻结的。
var accessor = { get food() { return "yum"; } };
Object.preventExtensions(accessor);
Object.isFrozen(accessor) //=== false

// ...但把这个属性改为不可配置，会让这个对象成为冻结对象。
Object.defineProperty(accessor, "food", { configurable: false });
Object.isFrozen(accessor) //=== true

// 使用 Object.freeze 是冻结一个对象最方便的方法。
var frozen = { 1: 81 };
Object.isFrozen(frozen) //=== false
Object.freeze(frozen);
Object.isFrozen(frozen) //=== true

// 一个冻结对象也是一个密封对象。
Object.isSealed(frozen) //=== true

// 当然，更是一个不可扩展的对象。
Object.isExtensible(frozen) //=== false
```

### 注意

在 ES5 中，如果参数不是一个对象类型，将抛出一个TypeError异常。在 ES2015 中，非对象参数将被视为一个冻结的普通对象，因此会返回true。

```
Object.isFrozen(1);
// TypeError: 1 is not an object (ES5 code)

Object.isFrozen(1);
// true                          (ES2015 code)
```

### Object.prototype.isPrototypeOf()

isPrototypeOf() 方法用于测试一个对象是否存在于另一个对象的原型链上。

备注： isPrototypeOf() 与 instanceof 运算符不同。在表达式 "object instanceof AFunction"中，object 的原型链是针对 AFunction.prototype 进行检查的，而不是针对 AFunction 本身。

### 语法

prototypeObj.isPrototypeOf(object)

### 参数

object
在该对象的原型链上搜寻

### 返回值

Boolean，表示调用对象是否在另一个对象的原型链上。

### 报错

TypeError
如果 prototypeObj 为 undefined 或 null，会抛出 TypeError。

### 描述

isPrototypeOf() 方法允许你检查一个对象是否存在于另一个对象的原型链上。

### 示例

本示例展示了 Baz.prototype, Bar.prototype, Foo.prototype 和 Object.prototype 在 baz 对象的原型链上：

```
function Foo() {}
function Bar() {}
function Baz() {}

Bar.prototype = Object.create(Foo.prototype);
Baz.prototype = Object.create(Bar.prototype);

var baz = new Baz();

console.log(Baz.prototype.isPrototypeOf(baz)); // true
console.log(Bar.prototype.isPrototypeOf(baz)); // true
console.log(Foo.prototype.isPrototypeOf(baz)); // true
console.log(Object.prototype.isPrototypeOf(baz)); // true
```

如果你有段代码只在需要操作继承自一个特定的原型链的对象的情况下执行，同 instanceof 操作符一样 isPrototypeOf() 方法就会派上用场，例如，为了确保某些方法或属性将位于对象上。

例如，检查 baz 对象是否继承自 Foo.prototype：

```
if (Foo.prototype.isPrototypeOf(baz)) {
  // do something safe
}
```

### Object.isSealed()

Object.isSealed() 方法判断一个对象是否被密封。

### 语法

Object.isSealed(obj)

### 参数

obj
要被检查的对象。

### 返回值

表示给定对象是否被密封的一个Boolean 。

### 描述

如果这个对象是密封的，则返回 true，否则返回 false。密封对象是指那些不可 扩展 的，且所有自身属性都不可配置且因此不可删除（但不一定是不可写）的对象。

### 例子

```
// 新建的对象默认不是密封的。
var empty = {};
Object.isSealed(empty); // === false

// 如果你把一个空对象变的不可扩展，则它同时也会变成个密封对象。
Object.preventExtensions(empty);
Object.isSealed(empty); // === true

// 但如果这个对象不是空对象，则它不会变成密封对象，因为密封对象的所有自身属性必须是不可配置的。
var hasProp = { fee: "fie foe fum" };
Object.preventExtensions(hasProp);
Object.isSealed(hasProp); // === false

// 如果把这个属性变的不可配置，则这个属性也就成了密封对象。
Object.defineProperty(hasProp, 'fee', {
  configurable: false
});
Object.isSealed(hasProp); // === true

// 最简单的方法来生成一个密封对象，当然是使用 Object.seal.
var sealed = {};
Object.seal(sealed);
Object.isSealed(sealed); // === true

// 一个密封对象同时也是不可扩展的。
Object.isExtensible(sealed); // === false

// 一个密封对象也可以是一个冻结对象，但不是必须的。
Object.isFrozen(sealed); // === true，所有的属性都是不可写的
var s2 = Object.seal({ p: 3 });
Object.isFrozen(s2); // === false，属性"p"可写

var s3 = Object.seal({ get p() { return 0; } });
Object.isFrozen(s3); // === true，访问器属性不考虑可写不可写，只考虑是否可配置
```

### 注意

在 ES5 中，如果这个方法的参数不是一个对象（一个原始类型），那么它会导致TypeError。在 ES2015 中，非对象参数将被视为是一个密封的普通对象，只返回true。

```
Object.isSealed(1);
// TypeError: 1 is not an object (ES5 code)

Object.isSealed(1);
// true                          (ES2015 code)
```

### Object.keys()

Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致。

```
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.keys(object1));
// Expected output: Array ["a", "b", "c"]
```

### 语法

Object.keys(obj)

### 参数

obj
要返回其枚举自身属性的对象。

### 返回值

一个表示给定对象的所有可枚举属性的字符串数组。

### 描述

Object.keys() 返回一个所有元素为字符串的数组，其元素来自给定的 object 上面可直接枚举的属性。这些属性的顺序与手动遍历该对象属性时的一致。

### 例子

```
// 简单数组
const arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// 类数组对象
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// 具有随机键顺序的类数组对象
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']

// getFoo 是一个不可枚举的属性
const myObj = Object.create({}, {
  getFoo: {
    value() { return this.foo; }
  }
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```

如果你想获取一个对象的所有属性，甚至包括不可枚举的，请查看 Object.getOwnPropertyNames。

### 注意

在 ES5 里，如果此方法的参数不是对象（而是一个原始值），那么它会抛出 TypeError。

在 ES2015 中，非对象的参数将被强制转换为一个对象。

```
// In ES5
Object.keys('foo');  // TypeError: "foo" is not an object

// In ES2015+
Object.keys('foo');  // ["0", "1", "2"]
```

### Object.preventExtensions()

Object.preventExtensions()方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。

```
const object1 = {};

Object.preventExtensions(object1);

try {
  Object.defineProperty(object1, 'property1', {
    value: 42
  });
} catch (e) {
  console.log(e);
  // Expected output: TypeError: Cannot define property property1, object is not extensible
}
```

### 语法

Object.preventExtensions(obj)

### 参数

obj
将要变得不可扩展的对象。

### 返回值

已经不可扩展的对象。

### 描述

如果一个对象可以添加新的属性，则这个对象是可扩展的。Object.preventExtensions()将对象标记为不再可扩展，这样它将永远不会具有它被标记为不可扩展时持有的属性之外的属性。注意，一般来说，不可扩展对象的属性可能仍然可被删除。尝试将新属性添加到不可扩展对象将静默失败或抛出TypeError（最常见的情况是strict mode (en-US)中，但不排除其他情况）。

Object.preventExtensions()仅阻止添加自身的属性。但其对象类型的原型依然可以添加新的属性。

该方法使得目标对象的 [[prototype]] 不可变；任何重新赋值 [[prototype]] 操作都会抛出 TypeError 。这种行为只针对内部的 [[prototype]] 属性，目标对象的其它属性将保持可变。

一旦将对象变为不可扩展的对象，就再也不能使其可扩展。

### 例子

```
// Object.preventExtensions 将原对象变的不可扩展，并且返回原对象。
var obj = {};
var obj2 = Object.preventExtensions(obj);
obj === obj2;  // true

// 字面量方式定义的对象默认是可扩展的。
var empty = {};
Object.isExtensible(empty) //=== true

// ...但可以改变。
Object.preventExtensions(empty);
Object.isExtensible(empty) //=== false

// 使用 Object.defineProperty 方法为一个不可扩展的对象添加新属性会抛出异常。
var nonExtensible = { removable: true };
Object.preventExtensions(nonExtensible);
Object.defineProperty(nonExtensible, "new", { value: 8675309 }); // 抛出 TypeError 异常

// 在严格模式中，为一个不可扩展对象的新属性赋值会抛出 TypeError 异常。
function fail()
{
  "use strict";
  nonExtensible.newProperty = "FAIL"; // throws a TypeError
}
fail();
```

### 不可扩展对象的原型是不可变的：

```
var fixed = Object.preventExtensions({});
// throws a 'TypeError'.
fixed.__proto__ = { oh: 'hai' };
```

### Notes

在 ES5 中，如果参数不是一个对象类型（而是原始类型），将抛出一个TypeError异常。在 ES2015 中，非对象参数将被视为一个不可扩展的普通对象，因此会被直接返回。

```
Object.preventExtensions(1);
// TypeError: 1 is not an object (ES5 code)

Object.preventExtensions(1);
// 1                             (ES2015 code)
```

### Object.prototype.propertyIsEnumerable()

propertyIsEnumerable() 方法返回一个布尔值，表示指定的属性是否可枚举。

```
const object1 = {};
const array1 = [];
object1.property1 = 42;
array1[0] = 42;

console.log(object1.propertyIsEnumerable('property1'));
// Expected output: true

console.log(array1.propertyIsEnumerable(0));
// Expected output: true

console.log(array1.propertyIsEnumerable('length'));
// Expected output: false
```

### 语法

obj.propertyIsEnumerable(prop)

### 参数

prop
需要测试的属性名。

### 返回值

用来表示指定的属性名是否可枚举的布尔值。

### 描述

每个对象都有一个 propertyIsEnumerable 方法。此方法可以确定对象中指定的属性是否可以被 for...in 循环枚举，但是通过原型链继承的属性除外。如果对象没有指定的属性，则此方法返回 false。

### 例子

propertyIsEnumerable 方法的基本用法

下面的例子演示了 propertyIsEnumerable 方法在普通对象和数组上的基本用法：

```
var o = {};
var a = [];
o.prop = 'is enumerable';
a[0] = 'is enumerable';

o.propertyIsEnumerable('prop'); // 返回 true
a.propertyIsEnumerable(0);      // 返回 true
```

### 用户自定义对象和内置对象

下面的例子演示了用户自定义对象和内置对象上属性可枚举性的区别。

```
var a = ['is enumerable'];

a.propertyIsEnumerable(0);        // 返回 true
a.propertyIsEnumerable('length'); // 返回 false

Math.propertyIsEnumerable('random'); // 返回 false
this.propertyIsEnumerable('Math');   // 返回 false
```

### 自身属性和继承属性

```
var a = [];
a.propertyIsEnumerable('constructor'); // 返回 false

function firstConstructor() {
  this.property = 'is not enumerable';
}

firstConstructor.prototype.firstMethod = function() {};

function secondConstructor() {
  this.method = function method() { return 'is enumerable'; };
}

secondConstructor.prototype = new firstConstructor;
secondConstructor.prototype.constructor = secondConstructor;

var o = new secondConstructor();
o.arbitraryProperty = 'is enumerable';

o.propertyIsEnumerable('arbitraryProperty'); // 返回 true
o.propertyIsEnumerable('method');            // 返回 true
o.propertyIsEnumerable('property');          // 返回 false

o.property = 'is enumerable';

o.propertyIsEnumerable('property');          // 返回 true

// 之所以这些会返回 false，是因为，在原型链上 propertyIsEnumerable 不被考虑
// (尽管最后两个在 for-in 循环中可以被循环出来)。
o.propertyIsEnumerable('prototype');   // 返回 false (根据 JS 1.8.1/FF3.6)
o.propertyIsEnumerable('constructor'); // 返回 false
o.propertyIsEnumerable('firstMethod'); // 返回 false
```

### Object.seal()

Object.seal() 方法封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。当前属性的值只要原来是可写的就可以改变。

```
const object1 = {
  property1: 42
};

Object.seal(object1);
object1.property1 = 33;
console.log(object1.property1);
// Expected output: 33

delete object1.property1; // Cannot delete when sealed
console.log(object1.property1);
// Expected output: 33
```

### 语法

Object.seal(obj)

### 参数

obj
将要被密封的对象。

### 返回值

被密封的对象。

### 描述

通常，一个对象是可扩展的（可以添加新的属性）。密封一个对象会让这个对象变的不能添加新属性，且所有已有属性会变的不可配置。属性不可配置的效果就是属性变的不可删除，以及一个数据属性不能被重新定义成为访问器属性，或者反之。但属性的值仍然可以修改。尝试删除一个密封对象的属性或者将某个密封对象的属性从数据属性转换成访问器属性，结果会静默失败或抛出TypeError（在严格模式 中最常见的，但不唯一）。

不会影响从原型链上继承的属性。但 Object.prototype.__proto__ ( 已弃用 ) 属性的值也会不能修改。

返回被密封对象的引用。

### 例子

```
var obj = {
  prop: function() {},
  foo: 'bar'
};

// 可以添加新的属性
// 可以更改或删除现有的属性
obj.foo = 'baz';
obj.lumpy = 'woof';
delete obj.prop;

var o = Object.seal(obj);

o === obj; // true
Object.isSealed(obj); // === true

// 仍然可以修改密封对象的属性值
obj.foo = 'quux';


// 但是你不能将属性重新定义成为访问器属性
// 反之亦然
Object.defineProperty(obj, 'foo', {
  get: function() { return 'g'; }
}); // throws a TypeError

// 除了属性值以外的任何变化，都会失败。
obj.quaxxor = 'the friendly duck';
// 添加属性将会失败
delete obj.foo;
// 删除属性将会失败

// 在严格模式下，这样的尝试将会抛出错误
function fail() {
  'use strict';
  delete obj.foo; // throws a TypeError
  obj.sparky = 'arf'; // throws a TypeError
}
fail();

// 通过 Object.defineProperty 添加属性将会报错
Object.defineProperty(obj, 'ohai', {
  value: 17
}); // throws a TypeError
Object.defineProperty(obj, 'foo', {
  value: 'eit'
}); // 通过 Object.defineProperty 修改属性值
```

### 注意

在 ES5 中，如果这个方法的参数不是一个（原始）对象，那么它将导致TypeError。在 ES2015 中，非对象参数将被视为已被密封的普通对象，会直接返回它。

```
Object.seal(1);
// TypeError: 1 is not an object (ES5 code)

Object.seal(1);
// 1                             (ES2015 code)
```

### 对比 Object.freeze()

使用Object.freeze()冻结的对象中的现有属性值是不可变的。用Object.seal()密封的对象可以改变其现有属性值。

### Object.setPrototypeOf()

Object.setPrototypeOf() 方法设置一个指定的对象的原型（即，内部 [[Prototype]] 属性）到另一个对象或 null。

警告： 由于现代 JavaScript 引擎优化属性访问所带来的特性的关系，更改对象的 [[Prototype]] 在各个浏览器和 JavaScript 引擎上都是一个很慢的操作。其在更改继承的性能上的影响是微妙而又广泛的，这不仅仅限于 Object.setPrototypeOf(...) 语句上的时间花费，而且可能会延伸到**任何代码，那些可以访问任何** [[Prototype]] 已被更改的对象的代码。参阅JavaScript 引擎基础知识：原型优化以了解更多。

由于此特性是语言的一部分，因此引擎开发人员仍需要高效地（理想地）实现该特性。在引擎开发人员解决此问题之前，如果你担心性能问题，则应该避免设置对象的 [[Prototype]]。相反，你应该使用 Object.create() 来创建带有你想要的 [[Prototype]] 的新对象。

### 语法

Object.setPrototypeOf(obj, prototype)

### 参数

obj
要设置其原型的对象。

prototype
该对象的新原型（一个对象或 null）。

### 返回值

指定的对象。

### 异常

TypeError
如果发生以下情况中的任何一个，则抛出该异常：

  obj 参数是不可扩展的，或者它是一个不可修改原型的特异对象（exotic object），例如 Object.prototype 或 window.
  prototype 参数不是对象或 null。
  
### 描述

通常，应该使用 Object.setPrototypeOf() 方法来设置对象的原型。你应该使用使用它，因为 Object.prototype.__proto__ 访问器已被弃用。

如果 obj 参数不是一个对象（例如，数字、字符串，等），该方法将什么也不做。

出于安全考虑，某些内置对象的原型被设计为是不可变的。这可以防止原型污染攻击，尤其是与代理（proxy）相关的。核心语言仅指定 Object.prototype 是不可变原型的特异对象，其原型始终为 null。而在浏览器中，window 和 location 也是（常见的）不可变原型的特异对象。

Object.setPrototypeOf() 是 ECMAScript 6 最新草案中的方法，相对于 Object.prototype.__proto__，它被认为是修改对象原型更合适的方法

```
Object.isExtensible(Object.prototype); // true; you can add more properties
Object.setPrototypeOf(Object.prototype, {}); // TypeError: Immutable prototype object '#<Object>' cannot have their prototype set
```

### 示例

使用 Object.setPrototypeOf 实现伪类继承

JS 中可以这样实现类继承。

```
class Human {}
class SuperHero extends Human {}

const superMan = new SuperHero();
```

但是，如果我们想要在不使用 class 的情况下实现子类，我们可以这么做：

```
function Human(name, level) {
  this.name = name;
  this.level = level;
}

function SuperHero(name, level) {
  Human.call(this, name, level);
}

Object.setPrototypeOf(SuperHero.prototype, Human.prototype);

// Set the `[[Prototype]]` of `SuperHero.prototype`
// to `Human.prototype`
// To set the prototypal inheritance chain

Human.prototype.speak = function () {
  return `${this.name} says hello.`;
}

SuperHero.prototype.fly = function () {
  return `${this.name} is flying.`;
}

const superMan = new SuperHero('Clark Kent', 1);

console.log(superMan.fly());
console.log(superMan.speak())
```

上面的类继承（使用 class）和伪类继承（使用带有 prototype 属性的构造函数）的相似性已在继承与原型链中提到。

在下面的示例中，也使用了类，SuperHero 使用 setPrototypeOf 而不是 extends 来继承 Human。

警告： 由于性能和可读性的原因，不建议使用 setPrototypeOf 来代替 extends。

```
class Human {}
class SuperHero {}

// Set the instance properties
Object.setPrototypeOf(SuperHero.prototype, Human.prototype);

// Hook up the static properties
Object.setPrototypeOf(SuperHero, Human);

const superMan = new SuperHero();
```

ES-6 子类派生中提到了不使用 extends 的子类派生方法。

### Object.prototype.toLocaleString()

toLocaleString() 方法返回一个该对象的字符串表示。此方法被用于派生对象为了特定语言环境的目的（locale-specific purposes）而重载使用。

### 语法

obj.toLocaleString();

### 返回值

表示对象的字符串。

### 描述

Object toLocaleString 返回调用 toString() 的结果。

该函数提供给对象一个通用的toLocaleString 方法，即使不是全部都可以使用它。见下面的列表。

### 覆盖 toLocaleString 的对象

- Array：Array.prototype.toLocaleString()
- Number：Number.prototype.toLocaleString()
- Date：Date.prototype.toLocaleString()

### Object.prototype.toString()

toString() 方法返回一个表示该对象的字符串。该方法旨在重写（自定义）派生类对象的类型转换的逻辑。

```
function Dog(name) {
  this.name = name;
}

const dog1 = new Dog('Gabby');

Dog.prototype.toString = function dogToString() {
  return `${this.name}`;
};

console.log(dog1.toString());
// Expected output: "Gabby"
```

### 语法

toString()

### 参数

默认情况下，toString() 不接受任何参数。然而，继承自 Object 的对象可能用它们自己的实现重写它，这些实现可以接受参数。例如，Number.prototype.toString() 和 BigInt.prototype.toString() 方法接受一个可选的 radix 参数。

### 返回值

一个表示该对象的字符串。

### 描述

JavaScript 调用 toString 方法将对象转换为一个原始值。你很少需要自己去调用 toString 方法；当遇到需要原始值的对象时，JavaScript 会自己调用它。

该方法由字符串转换优先调用，但是数字的强制转换和原始值的强制转换会优先调用 valueOf()。然而，因为基本的 valueOf() 方法返回一个对象，toString() 方法通常在结束时调用，除非对象重写了 valueOf()。例如，+[1] 返回 1，因为它的 toString() 方法返回 "1"，然后将其转换为数字。

所有继承自 Object.prototype 的对象（即，除了 null-prototype 对象之外的对象）都继承 toString() 方法。当你创建一个自定义对象时，你可以重写 toString() 以调用自定义方法，以便将自定义对象转换为一个字符串。或者，你可以增加一个 @@toPrimitive 方法，该方法允许对转换过程有更多的控制，并且对于任意的类型转换，且总是优先于 valueOf 或 toString。

要将基本的 Object.prototype.toString() 用于重写的对象（或者在 null 或 undefined 上调用它），你需要在它上面调用 Function.prototype.call() 或者 Function.prototype.apply()，将要检查的对象作为第一个参数传递（称为 thisArg）。

```
const arr = [1, 2, 3];

arr.toString(); // "1,2,3"
Object.prototype.toString.call(arr); // "[object Array]"
```

Object.prototype.toString() 返回 "[object Type]"，这里的 Type 是对象的类型。如果对象有 Symbol.toStringTag 属性，其值是一个字符串，则它的值将被用作 Type。许多内置的对象，包括 Map 和 Symbol，都有 Symbol.toStringTag。一些早于 ES6 的对象没有 Symbol.toStringTag，但仍然有一个特殊的标签。它们包括（标签与下面给出的类型名相同）：

```
Array
Function（它的 typeof 返回 "function"）
Error
Boolean
Number
String
Date
RegExp
```

arguments 对象返回 "[object Arguments]"。其它所有内容，包括用户自定义的类，除非有一个自定义的 Symbol.toStringTag，否则都将返回 "[object Object]"。

在 null 和 undefined 上调用 Object.prototype.toString() 分别返回 [object Null] 和 [object Undefined]。

### 示例

为自定义对象重写 toString

你可以创建一个要调用的函数来代替默认的 toString() 方法。你创建的 toString() 函数应该返回一个字符串值。如果它返回一个对象，并且在类型转换期间隐式调用它，那么忽略它的结果，并使用相关方法 valueOf() 的值，或者这些方法都不返回原始值，则抛出 TypeError。

以下代码定义了 Dog 类。

```
class Dog {
  constructor(name, breed, color, sex) {
    this.name = name;
    this.breed = breed;
    this.color = color;
    this.sex = sex;
  }
}
```

如果你在 Dog 实例上显示或者隐式的调用 toString() 方法，它将返回从 Object 继承的默认值：

```
const theDog = new Dog("Gabby", "Lab", "chocolate", "female");

theDog.toString(); // "[object Object]"
`${theDog}`; // "[object Object]"
```

以下代码重写了默认的 toString() 方法。这个方法生成一个包含该对象的 name、breed、color 和 sex 的字符串。

```
class Dog {
  constructor(name, breed, color, sex) {
    this.name = name;
    this.breed = breed;
    this.color = color;
    this.sex = sex;
  }
  toString() {
    return `Dog ${this.name} is a ${this.sex} ${this.color} ${this.breed}`;
  }
}
```

有了前面的代码，每当 Dog 实例在字符串的上下文中使用，JavaScript 会自动调用 toString() 方法。

```
const theDog = new Dog("Gabby", "Lab", "chocolate", "female");

`${theDog}`; // "Dog Gabby is a female chocolate Lab"
```

### 使用 toString() 去检查对象类

toString() 可以与每个对象一起使用，并且（默认情况下）允许你获得它的类。

```
const toString = Object.prototype.toString;

toString.call(new Date()); // [object Date]
toString.call(new String()); // [object String]
// Math has its Symbol.toStringTag
toString.call(Math); // [object Math]

toString.call(undefined); // [object Undefined]
toString.call(null); // [object Null]
```

以这种方式使用 toString() 是不可靠的；对象可以通过定义 Symbol.toStringTag 属性来更改 Object.prototype.toString() 的行为，从而导致意想不到的结果。例如：

```
const myDate = new Date();
Object.prototype.toString.call(myDate); // [object Date]

myDate[Symbol.toStringTag] = "myDate";
Object.prototype.toString.call(myDate); // [object myDate]

Date.prototype[Symbol.toStringTag] = "prototype polluted";
Object.prototype.toString.call(new Date()); // [object prototype polluted]
```

### Object.prototype.valueOf()

Object 的 valueOf() 方法将 this 值转换为一个对象。此方法旨在用于自定义类型转换的逻辑时，重写派生类对象。

```
function MyNumberType(n) {
  this.number = n;
}

MyNumberType.prototype.valueOf = function() {
  return this.number;
};

const object1 = new MyNumberType(4);

console.log(object1 + 3);
// Expected output: 7
```

### 语法

valueOf()

### 参数

无。

### 返回值

this 值，将其转换为一个对象。

备注： 为了使 valueOf 在类型转换期间有用，它必须返回一个原始值。因为所有原始类型都有它们自己的 valueOf() 方法，调用 aPrimitiveValue.valueOf() 通常不会调用 Object.prototype.valueOf()。

### 描述

JavaScript 调用 valueOf 方法将对象转换为一个原始值。你很少需要自己去调用 valueOf 方法；当遇到需要原始值的对象时，JavaScript 会自己调用它。

该方法由数字转换和原始值转换优先调用，但是字符串转换会优先调用 toString()，并且 toString() 非常可能返回一个字符串类型（即使原始实现了 Object.prototype.toString()），所以 valueOf() 在这种情况下通常不会被调用。

所有继承自 Object.prototype 的对象（当然，除了 null-prototype 对象之外）都继承 toString() 方法。对于这些对象，Object.prototype.valueOf() 基本的实现是没有效果的：它返回对象自身，它的返回值将永远不会被任何原始值转换算法使用。许多内置对象返回一个适当的原始值覆盖这个方法。当你创建一个自定义对象，你可以调用一个自定义方法去覆盖 valueOf()，以至于你自定义的对象可以转换为一个原始值。总的来说，valueOf() 用于返回对象最有意义的值——与 toString() 不同，它不需要是字符串。或者，你可以增加一个 @@toPrimitive 方法，该方法允许对转换过程有更多的控制，并且对于任意的类型转换，且总是优先于 valueOf 或 toString。

### 示例

使用 valueOf()

原始的 valueOf() 方法会返回 this 值本身，如果尚未转换为对象，则转换为对象。因此它的返回值将从不会被任何原始值转换算法使用。

```
const obj = { foo: 1 };
console.log(obj.valueOf() === obj); // true

console.log(Object.prototype.valueOf.call("primitive"));
// [String: 'primitive'] (a wrapper object)
```

### Object.values()

Object.values() 方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用 for...in 循环的顺序相同（区别在于 for-in 循环枚举原型链中的属性）。

### 语法

Object.values(obj)

### 参数

obj
被返回可枚举属性值的对象。

### 返回值

一个包含对象自身的所有可枚举属性值的数组。

### 描述

Object.values()返回一个数组，其元素是在对象上找到的可枚举属性值。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

### 示例

```
var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']

// array like object with random key ordering
// when we use numeric keys, the value returned in a numerical order according to the keys
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']

// getFoo is property which isn't enumerable
var my_obj = Object.create({}, { getFoo: { value: function() { return this.foo; } } });
my_obj.foo = 'bar';
console.log(Object.values(my_obj)); // ['bar']

// non-object argument will be coerced to an object
console.log(Object.values('foo')); // ['f', 'o', 'o']
```

<hr>

## Object

Object 是 JavaScript 的一种 数据类型 。它用于存储各种键值集合和更复杂的实体。Objects 可以通过 Object() 构造函数或者使用 对象字面量 的方式创建

### 描述

在 JavaScript 中，几乎所有的对象都是 Object 类型的实例，它们都会从 Object.prototype 继承属性和方法，虽然大部分属性都会被覆盖（shadowed）或者说被重写了（overridden）。 除此之外，Object 还可以被故意的创建，但是这个对象并不是一个“真正的对象”（例如：通过 Object.create(null)），或者通过一些手段改变对象，使其不再是一个“真正的对象”（比如说：Object.setPrototypeOf）。

通过原型链，所有的 object 都能观察到 Object 原型对象（Object prototype object）的改变，除非这些受到改变影响的属性和方法沿着原型链被进一步的重写。尽管有潜在的危险，但这为覆盖或扩展对象的行为提供了一个非常强大的机制。

Object 构造函数为给定的参数创建一个包装类对象（object wrapper），具体有以下情况：

- 如果给定值是 null 或 undefined，将会创建并返回一个空对象
- 如果传进去的是一个基本类型的值，则会构造其包装类型的对象
- 如果传进去的是引用类型的值，仍然会返回这个值，经他们复制的变量保有和源对象相同的引用地址

当以非构造函数形式被调用时，Object 的行为等同于 new Object()。

### 从一个对象上删除一个属性

Object 自身没有提供方法删除其自身属性（Map 中的 Map.prototype.delete() 可以删除自身属性）为了删除对象上的属性，必须使用 delete 操作符

### 构造函数

Object()

创建一个新的 Object 对象。该对象将会包裹（wrapper）传入的参数

### 静态方法

Object.assign()
通过复制一个或多个对象来创建一个新的对象。

Object.create()
使用指定的原型对象和属性创建一个新对象。

Object.defineProperty()
给对象添加一个属性并指定该属性的配置。

Object.defineProperties()
给对象添加多个属性并分别指定它们的配置。

Object.entries()
返回给定对象自身可枚举属性的 [key, value] 数组。

Object.freeze()
冻结对象：其他代码不能删除或更改任何属性。

Object.getOwnPropertyDescriptor()
返回对象指定的属性配置。

Object.getOwnPropertyNames()
返回一个数组，它包含了指定对象所有的可枚举或不可枚举的属性名。

Object.getOwnPropertySymbols()
返回一个数组，它包含了指定对象自身所有的符号属性。

Object.getPrototypeOf()
返回指定对象的原型对象。

Object.is()
比较两个值是否相同。所有 NaN 值都相等（这与==和===不同）。

Object.isExtensible()
判断对象是否可扩展。

Object.isFrozen()
判断对象是否已经冻结。

Object.isSealed()
判断对象是否已经密封。

Object.keys()
返回一个包含所有给定对象自身可枚举属性名称的数组。

Object.preventExtensions()
防止对象的任何扩展。

Object.seal()
防止其他代码删除对象的属性。

Object.setPrototypeOf()
设置对象的原型（即内部 [[Prototype]] 属性）。

Object.values()
返回给定对象自身可枚举值的数组。

### 实例属性

Object.prototype.constructor
一个引用值，指向 Object 构造函数

Object.prototype.__proto__
指向一个对象，当一个 object 实例化时，使用该对象作为实例化对象的原型

### 实例方法

Object.prototype.__defineGetter__()
将一个属性与一个函数相关联，当该属性被访问时，执行该函数，并且返回函数的返回值。

Object.prototype.__defineSetter__()
将一个属性与一个函数相关联，当该属性被设置时，执行该函数，执行该函数去修改某个属性。

Object.prototype.__lookupGetter__()
返回一个函数，该函数通过给定属性的 Object.prototype.__defineGetter__() 得出。

Object.prototype.__lookupSetter__()
返回一个函数，该函数通过给定属性的 Object.prototype.__defineSetter__() 得出。

Object.prototype.hasOwnProperty()
返回一个布尔值，用于表示一个对象自身是否包含指定的属性，该方法并不会查找原型链上继承来的属性。

Object.prototype.isPrototypeOf()
返回一个布尔值，用于表示该方法所调用的对象是否在指定对象的原型链中。

Object.prototype.propertyIsEnumerable()
返回一个布尔值，用于表示内部属性 ECMAScript [[Enumerable]] attribute 是否被设置。

Object.prototype.toLocaleString()
调用 toString()。

Object.prototype.toString()
返回一个代表该对象的字符串。

Object.prototype.valueOf()
返回指定对象的原始值。

<hr>

## Array

JavaScript 的 Array 对象是用于构造数组的全局对象，数组是类似于列表的高阶对象。

1. 构造器： `Array()` 创建一个新的Array对象
2. 静态属性：`get Array[@@species]` 返回Array的构造函数
3. 静态方法：
   - `Array.from()` 从类数组对象或者可迭代对象中创建一个新的数组实例
   - `Array.isArray()` 用来判断某个变量是否是一个数组对象
   - `Array.of()` 根据一组参数来创建新的数组实例，支持任意的参数数量和类型 - `Array(1, 2, 3);    // [1, 2, 3]`
4. 实例属性：
   - `Array.prototype.length`: 数组中的元素个数
   - `Array.prototype[@@unscopables]`: 包含了所有 ES2015 (ES6) 中新定义的、且并未被更早的 ECMAScript 标准收纳的属性名。这些属性被排除在由 with 语句绑定的环境中
5. 实例方法：
   - `Array.prototype.at()`: at() 方法接收一个整数值并返回该索引的项目，允许正数和负数。负整数从数组中的最后一个项目开始倒数。(返回给定索引处的数组项。接受负整数，从最后一项开始计数。)
   - `Array.prototype.concat()`: 用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组
   - `Array.prototype.copyWithin()`: 浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度
   - `Array.prototype.entries()`: 返回一个新的 Array Iterator 对象，该对象包含数组中每个索引的键/值对
   - `Array.prototype.every()`: 测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值
   - `Array.prototype.fill()`: 用一个固定值填充一个数组中从起始索引到终止索引内的全部元素
   - `Array.prototype.filter()`: 创建一个新数组，其包含通过所提供函数实现的测试的所有元素
   - `Array.prototype.find()`: 返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined
   - `Array.prototype.findIndex()`: 返回数组中满足提供的测试函数的第一个元素的索引。若没有找到对应元素则返回 -1
   - `Array.prototype.flat()`: 按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回
   - `Array.prototype.flatMap()`: 使用映射函数映射每个元素，然后将结果压缩成一个新数组
   - `Array.prototype.forEach()`: 对数组的每个元素执行一次给定的函数
   - `Array.prototype.includes()`: 判断一个数组是否包含一个指定的值，如果包含则返回 true，否则返回 false
   - `Array.prototype.indexOf()`: 返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回 -1
   - `Array.prototype.join()`: 将一个数组的所有元素连接成一个字符串并返回这个字符串
   - `Array.prototype.keys()`: 返回一个包含数组中每个索引键的 Array Iterator 对象
   - `Array.prototype.lastIndexOf()`: 返回指定元素在数组中的最后一个的索引，如果不存在则返回 -1
   - `Array.prototype.map()`: 返回一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值
   - `Array.prototype.pop()`: 从数组中删除最后一个元素，并返回该元素的值
   - `Array.prototype.push()`: 将一个或多个元素添加到数组的末尾，并返回该数组的新长度
   - `Array.prototype.reduce()`: 对数组中的每个元素执行一个由您提供的 reducer 函数（升序执行），将其结果汇总为单个返回值
   - `Array.prototype.reduceRight()`: 接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值
   - `Array.prototype.reverse()`: 将数组中元素的位置颠倒，并返回该数组。该方法会改变原数组
   - `Array.prototype.shift()`: 从数组中删除第一个元素，并返回该元素的值
   - `Array.prototype.slice()`: 提取源数组的一部分并返回一个新数组
   - `Array.prototype.some()`: 测试数组中是不是至少有一个元素通过了被提供的函数测试
   - `Array.prototype.sort()`: 对数组元素进行原地排序并返回此数组
   - `Array.prototype.splice()`: 通过删除或替换现有元素或者原地添加新的元素来修改数组，并以数组形式返回被修改的内容
   - `Array.prototype.toLocaleString()`: 返回一个字符串表示数组中的元素。数组中的元素将使用各自的 Object.prototype.toLocaleString() 方法转成字符串
   - `Array.prototype.toString()`: 返回一个字符串表示指定的数组及其元素。数组中的元素将使用各自的 Object.prototype.toString() 方法转成字符串
   - `Array.prototype.unshift()`: 将一个或多个元素添加到数组的头部，并返回该数组的新长度
   - `Array.prototype.values()`: 返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值
   - `Array.prototype[@@iterator]()`: 返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值

### 属性

> `Array.prototype[@@unscopables]`

Symbol 属性 @@unscopable 包含了所有 ES2015 (ES6) 中新定义的、且并未被更早的 ECMAScript 标准收纳的属性名。这些属性被排除在由 with 语句绑定的环境中。

语法：arr[Symbol.unscopables]

描述：

with 绑定中未包含的数组默认属性有：

- copyWithin()
- entries()
- fill()
- find()
- findIndex()
- includes()
- keys()
- values()

`Array.prototype[@@unscopables]` 属性的属性特性：

- writable: false
- enumerable: false
- configurable: true

🌰：

```js
var keys = [];

with(Array.prototype) {
 keys.push("something");
}

Object.keys(Array.prototype[Symbol.unscopables]);

// Object.keys(Array.prototype[Symbol.unscopables]);
// (13) ['at', 'copyWithin', 'entries', 'fill', 'find', 'findIndex', 'flat', 'flatMap', 'includes', 'keys', 'values', 'findLast', 'findLastIndex']
```

> Array.prototype.length

length 是 Array 的实例属性。返回或设置一个数组中的元素个数。该值是一个无符号 32-bit 整数，并且总是大于数组最高项的下标。

- Writable：如果设置为 false，该属性值将不能被修改。
- Configurable：如果设置为 false，删除或更改任何属性都将会失败。
- Enumerable: 如果设置为 true，属性可以通过迭代器for或for...in进行迭代。

**Array.length 属性的属性特性：**

- writable	true
- enumerable	false
- configurable	false

### 方法

1. 构造函数
2. 静态方法 - Array.isArray()
3. 实例方法
   - valueOf(), toString()
   - push(), pop()
   - shift(), unshift()
   - join()
   - concat()
   - reverse()
   - slice()
   - splice()
   - sort()
   - map()
   - forEach()
   - filter()
   - some(), every()
   - reduce(), reduceRight()
   - indexOf(), lastIndexOf()
   - 链式使用

### 构造函数

Array是 JavaScript 的原生对象，同时也是一个构造函数，可以用它生成新的数组。

```js
var arr = new Array(2);
arr.length // 2
arr // [ empty x 2 ]
```

Array()构造函数的参数2，表示生成一个两个成员的数组，每个位置都是空值。

如果没有使用new关键字，运行结果也是一样的。

```js
var arr = Array(2);
// 等同于
var arr = new Array(2);
```

考虑到语义性，以及与其他构造函数用法保持一致，建议总是加上new。

```js
// 无参数时，返回一个空数组
new Array() // []

// 单个正整数参数，表示返回的新数组的长度
new Array(1) // [ empty ]
new Array(2) // [ empty x 2 ]

// 非正整数的数值作为参数，会报错
new Array(3.2) // RangeError: Invalid array length
new Array(-3) // RangeError: Invalid array length

// 单个非数值（比如字符串、布尔值、对象等）作为参数，
// 则该参数是返回的新数组的成员
new Array('abc') // ['abc']
new Array([1]) // [Array[1]]

// 多参数时，所有参数都是返回的新数组的成员
new Array(1, 2) // [1, 2]
new Array('a', 'b', 'c') // ['a', 'b', 'c']
```

可以看到，Array()作为构造函数，行为很不一致。因此，不建议使用它生成新数组，直接使用数组字面量是更好的做法。

```js
// bad
var arr = new Array(1, 2);

// good
var arr = [1, 2];
```

注意，如果参数是一个正整数，返回数组的成员都是空位。虽然读取的时候返回undefined，但实际上该位置没有任何值。虽然这时可以读取到length属性，但是取不到键名。

```js
var a = new Array(3);
var b = [undefined, undefined, undefined];

a.length // 3
b.length // 3

a[0] // undefined
b[0] // undefined

0 in a // false
0 in b // true
```

上面代码中，a是Array()生成的一个长度为3的空数组，b是一个三个成员都是undefined的数组，这两个数组是不一样的。读取键值的时候，a和b都返回undefined，但是a的键名（成员的序号）都是空的，b的键名是有值的。

### 静态方法

#### Array.isArray()

Array.isArray方法返回一个布尔值，表示参数是否为数组。它可以弥补typeof运算符的不足。

```js
var arr = [1, 2, 3];

typeof arr // "object"
Array.isArray(arr) // true
```

上面代码中，typeof运算符只能显示数组的类型是Object，而Array.isArray方法可以识别数组。

### 实例方法

- valueOf(), toString()

valueOf方法是一个所有对象都拥有的方法，表示对该对象求值。不同对象的valueOf方法不尽一致，数组的valueOf方法返回数组本身。

```js
var arr = [1, 2, 3];
arr.valueOf() // [1, 2, 3]

Array.isArray([1,2,3].valueOf())
true
```

toString方法也是对象的通用方法，数组的toString方法返回数组的字符串形式。

```js
[1,2,3].valueOf()
(3) [1, 2, 3]
[1,2,3].toString()
'1,2,3'

var arr = [1, 2, 3];
arr.toString() // "1,2,3"

var arr = [1, 2, 3, [4, 5, 6]];
arr.toString() // "1,2,3,4,5,6"

[1,[2,[3,4],3],4].toString()
'1,2,3,4,3,4'
```

- push(), pop()

push方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

```js
var arr = [];

arr.push(1) // 1
arr.push('a') // 2
arr.push(true, {}) // 4
arr // [1, 'a', true, {}]
```

上面代码使用push方法，往数组中添加了四个成员。

pop方法用于删除数组的最后一个元素，并返回该元素。注意，该方法会改变原数组。

```js
var arr = ['a', 'b', 'c'];

arr.pop() // 'c'
arr // ['a', 'b']
```

对空数组使用pop方法，不会报错，而是返回undefined。

```js
[].pop() // undefined
```

push和pop结合使用，就构成了“后进先出”的栈结构（stack）。

```js
var arr = [];
arr.push(1, 2);
arr.push(3);
arr.pop();
arr // [1, 2]
```

- shift(), unshift()

shift()方法用于删除数组的第一个元素，并返回该元素。注意，该方法会改变原数组。

```js
var a = ['a', 'b', 'c'];

a.shift() // 'a'
a // ['b', 'c']
```

上面代码中，使用shift()方法以后，原数组就变了。

shift()方法可以遍历并清空一个数组。

```js
var list = [1, 2, 3, 4];
var item;

while (item = list.shift()) {
  console.log(item);
}

list // []
```

上面代码通过list.shift()方法每次取出一个元素，从而遍历数组。它的前提是数组元素不能是0或任何布尔值等于false的元素，因此这样的遍历不是很可靠。

push()和shift()结合使用，就构成了“先进先出”的队列结构（queue）。

unshift()方法用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

```js
var a = ['a', 'b', 'c'];

a.unshift('x'); // 4
a // ['x', 'a', 'b', 'c']
```

unshift()方法可以接受多个参数，这些参数都会添加到目标数组头部。

```js
var arr = [ 'c', 'd' ];
arr.unshift('a', 'b') // 4
arr // [ 'a', 'b', 'c', 'd' ]
```

- join()

join()方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。如果不提供参数，默认用逗号分隔。

```js
var a = [1, 2, 3, 4];

a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"
a.join() // "1,2,3,4"
```

如果数组成员是undefined或null或空位，会被转成空字符串。

```js
[undefined, null].join('#')
// '#'

['a',, 'b'].join('-')
// 'a--b'
```

通过call方法，这个方法也可以用于字符串或类似数组的对象。

```js
Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"

var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')
// 'a-b'
```

```js
var a = [1, 2, 3, 4];
a.join()
'1,2,3,4'
a
(4) [1, 2, 3, 4]
```

- concat()

concat方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组，原数组不变。

```js
['hello'].concat(['world'])
// ["hello", "world"]

['hello'].concat(['world'], ['!'])
// ["hello", "world", "!"]

[].concat({a: 1}, {b: 2})
// [{ a: 1 }, { b: 2 }]

[2].concat({a: 1})
// [2, {a: 1}]
```

除了数组作为参数，concat也接受其他类型的值作为参数，添加到目标数组尾部。

```js
[1, 2, 3].concat(4, 5, 6)
// [1, 2, 3, 4, 5, 6]
```

如果数组成员包括对象，concat方法返回当前数组的一个浅拷贝。所谓“浅拷贝”，指的是新数组拷贝的是对象的引用。

```js
var obj = { a: 1 };
var oldArray = [obj];

var newArray = oldArray.concat();

obj.a = 2;
newArray[0].a // 2
```

上面代码中，原数组包含一个对象，concat方法生成的新数组包含这个对象的引用。所以，改变原对象以后，新数组跟着改变。

- reverse()

reverse方法用于颠倒排列数组元素，返回改变后的数组。注意，该方法将改变原数组。

```js
var a = ['a', 'b', 'c'];

a.reverse() // ["c", "b", "a"]
a // ["c", "b", "a"]
```

- slice()

slice()方法用于提取目标数组的一部分，返回一个新数组，原数组不变。

```js
arr.slice(start, end);
```

它的第一个参数为起始位置（从0开始，会包括在返回的新数组之中），第二个参数为终止位置（但该位置的元素本身不包括在内）。如果省略第二个参数，则一直返回到原数组的最后一个成员。

```js
var a = ['a', 'b', 'c'];

a.slice(0) // ["a", "b", "c"]
a.slice(1) // ["b", "c"]
a.slice(1, 2) // ["b"]
a.slice(2, 6) // ["c"]
a.slice() // ["a", "b", "c"]
```

上面代码中，最后一个例子slice()没有参数，实际上等于返回一个原数组的拷贝。

如果slice()方法的参数是负数，则表示倒数计算的位置。

```js
var a = ['a', 'b', 'c'];
a.slice(-2) // ["b", "c"]
a.slice(-2, -1) // ["b"]
```

上面代码中，-2表示倒数计算的第二个位置，-1表示倒数计算的第一个位置。

如果第一个参数大于等于数组长度，或者第二个参数小于第一个参数，则返回空数组。

```js
var a = ['a', 'b', 'c'];
a.slice(4) // []
a.slice(2, 1) // []
```

slice()方法的一个重要应用，是将类似数组的对象转为真正的数组。

```js
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```

上面代码的参数都不是数组，但是通过call方法，在它们上面调用slice()方法，就可以把它们转为真正的数组。

- splice()

splice()方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。注意，该方法会改变原数组。

```js
arr.splice(start, count, addElement1, addElement2, ...);
```

splice的第一个参数是删除的起始位置（从0开始），第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2) // ["e", "f"]
a // ["a", "b", "c", "d"]
```

上面代码从原数组4号位置，删除了两个数组成员。

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2, 1, 2) // ["e", "f"]
a // ["a", "b", "c", "d", 1, 2]
```

上面代码除了删除成员，还插入了两个新成员。

起始位置如果是负数，就表示从倒数位置开始删除。

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(-4, 2) // ["c", "d"]
```

上面代码表示，从倒数第四个位置c开始删除两个成员。

如果只是单纯地插入元素，splice方法的第二个参数可以设为0。

```js
var a = [1, 1, 1];

a.splice(1, 0, 2) // []
a // [1, 2, 1, 1]
```

如果只提供第一个参数，等同于将原数组在指定位置拆分成两个数组。

```js
var a = [1, 2, 3, 4];
a.splice(2) // [3, 4]
a // [1, 2]
```

- sort()

sort方法对数组成员进行排序，默认是按照字典顺序排序。排序后，原数组将被改变。

```js
['d', 'c', 'b', 'a'].sort()
// ['a', 'b', 'c', 'd']

[4, 3, 2, 1].sort()
// [1, 2, 3, 4]

[11, 101].sort()
// [101, 11]

[10111, 1101, 111].sort()
// [10111, 1101, 111]
```

上面代码的最后两个例子，需要特殊注意。sort()方法不是按照大小排序，而是按照字典顺序。也就是说，数值会被先转成字符串，再按照字典顺序进行比较，所以101排在11的前面。

如果想让sort方法按照自定义方式排序，可以传入一个函数作为参数。

```js
[10111, 1101, 111].sort(function (a, b) {
  return a - b;
})
// [111, 1101, 10111]
```

上面代码中，sort的参数函数本身接受两个参数，表示进行比较的两个数组成员。如果该函数的返回值大于0，表示第一个成员排在第二个成员后面；其他情况下，都是第一个元素排在第二个元素前面。

```js
[
  { name: "张三", age: 30 },
  { name: "李四", age: 24 },
  { name: "王五", age: 28  }
].sort(function (o1, o2) {
  return o1.age - o2.age;
})
// [
//   { name: "李四", age: 24 },
//   { name: "王五", age: 28  },
//   { name: "张三", age: 30 }
// ]
```

注意，自定义的排序函数应该返回数值，否则不同的浏览器可能有不同的实现，不能保证结果都一致。

```js
// bad
[1, 4, 2, 6, 0, 6, 2, 6].sort((a, b) => a > b)

// good
[1, 4, 2, 6, 0, 6, 2, 6].sort((a, b) => a - b)
```

上面代码中，前一种排序算法返回的是布尔值，这是不推荐使用的。后一种是数值，才是更好的写法。

- map()

map()方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回。

```js
var numbers = [1, 2, 3];

numbers.map(function (n) {
  return n + 1;
});
// [2, 3, 4]

numbers
// [1, 2, 3]
```

上面代码中，numbers数组的所有成员依次执行参数函数，运行结果组成一个新数组返回，原数组没有变化。

map()方法接受一个函数作为参数。该函数调用时，map()方法向它传入三个参数：当前成员、当前位置和数组本身。

```js
[1, 2, 3].map(function(elem, index, arr) {
  return elem * index;
});
// [0, 2, 6]
```

上面代码中，map()方法的回调函数有三个参数，elem为当前成员的值，index为当前成员的位置，arr为原数组（[1, 2, 3]）。

map()方法还可以接受第二个参数，用来绑定回调函数内部的this变量（详见《this 变量》一章）。

```js
var arr = ['a', 'b', 'c'];

[1, 2].map(function (e) {
  return this[e];
}, arr)
// ['b', 'c']
```

上面代码通过map()方法的第二个参数，将回调函数内部的this对象，指向arr数组。

如果数组有空位，map()方法的回调函数在这个位置不会执行，会跳过数组的空位。

```js
var f = function (n) { return 'a' };

[1, undefined, 2].map(f) // ["a", "a", "a"]
[1, null, 2].map(f) // ["a", "a", "a"]
[1, , 2].map(f) // ["a", , "a"]
```

上面代码中，map()方法不会跳过undefined和null，但是会跳过空位。

- forEach()

forEach()方法与map()方法很相似，也是对数组的所有成员依次执行参数函数。但是，forEach()方法不返回值，只用来操作数据。这就是说，如果数组遍历的目的是为了得到返回值，那么使用map()方法，否则使用forEach()方法。

forEach()的用法与map()方法一致，参数是一个函数，该函数同样接受三个参数：当前值、当前位置、整个数组。

```js
function log(element, index, array) {
  console.log('[' + index + '] = ' + element);
}

[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```

上面代码中，forEach()遍历数组不是为了得到返回值，而是为了在屏幕输出内容，所以不必使用map()方法。

forEach()方法也可以接受第二个参数，绑定参数函数的this变量。

```js
var out = [];

[1, 2, 3].forEach(function(elem) {
  this.push(elem * elem);
}, out);

out // [1, 4, 9]
```

上面代码中，空数组out是forEach()方法的第二个参数，结果，回调函数内部的this关键字就指向out。

注意，forEach()方法无法中断执行，总是会将所有成员遍历完。如果希望符合某种条件时，就中断遍历，要使用for循环。

```js
var arr = [1, 2, 3];

for (var i = 0; i < arr.length; i++) {
  if (arr[i] === 2) break;
  console.log(arr[i]);
}
// 1
```

上面代码中，执行到数组的第二个成员时，就会中断执行。forEach()方法做不到这一点。

forEach()方法也会跳过数组的空位。

```js
var log = function (n) {
  console.log(n + 1);
};

[1, undefined, 2].forEach(log)
// 2
// NaN
// 3

[1, null, 2].forEach(log)
// 2
// 1
// 3

[1, , 2].forEach(log)
// 2
// 3
```

上面代码中，forEach()方法不会跳过undefined和null，但会跳过空位。

- filter()

filter()方法用于过滤数组成员，满足条件的成员组成一个新数组返回。

它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。

```js
[1, 2, 3, 4, 5].filter(function (elem) {
  return (elem > 3);
})
// [4, 5]
```

上面代码将大于3的数组成员，作为一个新数组返回。

```js
var arr = [0, 1, 'a', false];

arr.filter(Boolean)
// [1, "a"]
```

上面代码中，filter()方法返回数组arr里面所有布尔值为true的成员。

filter()方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组。

```js
[1, 2, 3, 4, 5].filter(function (elem, index, arr) {
  return index % 2 === 0;
});
// [1, 3, 5]
```

上面代码返回偶数位置的成员组成的新数组。

filter()方法还可以接受第二个参数，用来绑定参数函数内部的this变量。

```js
var obj = { MAX: 3 };
var myFilter = function (item) {
  if (item > this.MAX) return true;
};

var arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, obj) // [8, 4, 9]
```

上面代码中，过滤器myFilter()内部有this变量，它可以被filter()方法的第二个参数obj绑定，返回大于3的成员。

- some(), every()

这两个方法类似“断言”（assert），返回一个布尔值，表示判断数组成员是否符合某种条件。

它们接受一个函数作为参数，所有数组成员依次执行该函数。该函数接受三个参数：当前成员、当前位置和整个数组，然后返回一个布尔值。

some方法是只要一个成员的返回值是true，则整个some方法的返回值就是true，否则返回false。

```js
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
// true
```

上面代码中，如果数组arr有一个成员大于等于3，some方法就返回true。

every方法是所有成员的返回值都是true，整个every方法才返回true，否则返回false。

```js
var arr = [1, 2, 3, 4, 5];
arr.every(function (elem, index, arr) {
  return elem >= 3;
});
// false
```

上面代码中，数组arr并非所有成员大于等于3，所以返回false。

注意，对于空数组，some方法返回false，every方法返回true，回调函数都不会执行。

```js
function isEven(x) { return x % 2 === 0 }

[].some(isEven) // false
[].every(isEven) // true
```

some和every方法还可以接受第二个参数，用来绑定参数函数内部的this变量。

- reduce(), reduceRight()

reduce()方法和reduceRight()方法依次处理数组的每个成员，最终累计为一个值。它们的差别是，reduce()是从左到右处理（从第一个成员到最后一个成员），reduceRight()则是从右到左（从最后一个成员到第一个成员），其他完全一样。

由于这两个方法会遍历数组，所以实际上可以用来做一些遍历相关的操作。比如，找出字符长度最长的数组成员。

```js
function findLongest(entries) {
  return entries.reduce(function (longest, entry) {
    return entry.length > longest.length ? entry : longest;
  }, '');
}

findLongest(['aaa', 'bb', 'c']) // "aaa"
```

上面代码中，reduce()的参数函数会将字符长度较长的那个数组成员，作为累积值。这导致遍历所有成员之后，累积值就是字符长度最长的那个成员。

- indexOf(), lastIndexOf()

indexOf方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。

```js
var a = ['a', 'b', 'c'];

a.indexOf('b') // 1
a.indexOf('y') // -1
```

indexOf方法还可以接受第二个参数，表示搜索的开始位置。

```js
['a', 'b', 'c'].indexOf('a', 1) // -1
```

上面代码从1号位置开始搜索字符a，结果为-1，表示没有搜索到。

lastIndexOf方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。

```js
var a = [2, 5, 9, 2];
a.lastIndexOf(2) // 3
a.lastIndexOf(7) // -1
```

注意，这两个方法不能用来搜索NaN的位置，即它们无法确定数组成员是否包含NaN。

```js
[NaN].indexOf(NaN) // -1
[NaN].lastIndexOf(NaN) // -1
```

这是因为这两个方法内部，使用严格相等运算符（===）进行比较，而NaN是唯一一个不等于自身的值。

### 链式使用

```js
var users = [
  {name: 'tom', email: 'tom@example.com'},
  {name: 'peter', email: 'peter@example.com'}
];

users
.map(function (user) {
  return user.email;
})
.filter(function (email) {
  return /^t/.test(email);
})
.forEach(function (email) {
  console.log(email);
});
// "tom@example.com"
```

上面代码中，先产生一个所有 Email 地址组成的数组，然后再过滤出以t开头的 Email 地址，最后将它打印出来。

## Array

与其他编程语言中的数组一样，Array 对象支持在单个变量名下存储多个元素，并具有执行常见数组操作的成员。

### 描述

在 JavaScript 中，数组不是基本类型，而是具有以下核心特征的 Array 对象：

- JavaScript 数组是可调整大小的，并且可以包含不同的数据类型。（当不需要这些特征时，可以使用类型化数组。）
- JavaScript 数组不是关联数组，因此，不能使用任意字符串作为索引访问数组元素，但必须使用非负整数（或它们各自的字符串形式）作为索引访问。
- JavaScript 数组的索引从 0 开始：数组的第一个元素在索引 0 处，第二个在索引 1 处，以此类推，最后一个元素是数组的 length 属性减去 1 的值。
- JavaScript 数组复制操作创建浅拷贝。（所有 JavaScript 对象的标准内置复制操作都会创建浅拷贝，而不是深拷贝）。

### 数组下标

Array 对象不能使用任意字符串作为元素索引（如关联数组），必须使用非负整数（或它们的字符串形式）。通过非整数设置或访问不会设置或从数组列表本身检索元素， 但会设置或访问与该数组的对象属性集合相关的变量。数组的对象属性和数组元素列表是分开的，数组的遍历和修改操作不能应用于这些命名属性。

数组元素是对象属性，就像 toString 是属性一样（具体来说，toString() 是一种方法）。然而，尝试按以下方式访问数组的元素会抛出语法错误，因为属性名无效：

`console.log(arr.0); // a syntax error`

JavaScript 语法要求使用方括号表示法而不是点号表示法来访问以数字开头的属性。也可以用引号包裹数组下标（例如，years['2'] 而不是 years[2]），尽管通常没有必要。

JavaScript 引擎通过隐式的 toString，将 years[2] 中的 2 强制转换为字符串。因此，'2' 和 '02' 将指向 years 对象上的两个不同的槽位，下面的例子可能是 true：

`console.log(years['2'] !== years['02']);`

只有 years['2'] 是一个实际的数组索引。years['02'] 是一个在数组迭代中不会被访问的任意字符串属性。

### 数组方法和空槽

稀疏数组中的空槽在数组方法之间的行为不一致。通常，旧方法会跳过空槽，而新方法将它们视为 undefined。

在遍历多个元素的方法中，下面的方法在访问索引之前执行 in 检查，并且不将空槽与 undefined 合并：

- concat()
- copyWithin()
- every()
- filter()
- flat()
- flatMap()
- forEach()
- indexOf()
- lastIndexOf()
- map()
- reduce()
- reduceRight()
- reverse()
- slice()
- some()
- sort()
- splice()

这些方法将空槽视为 undefined：

- entries()
- fill()
- find()
- findIndex()
- findLast()
- findLastIndex()
- group()
- groupToMap()
- includes()
- join()
- keys()
- toLocaleString()
- values()

### 复制方法和修改方法

有些方法不改变调用该方法的现有数组，而是返回一个新数组。它们首先访问 this.constructor[Symbol.species] 来确定用于新数组的构造函数。然后用元素填充新构造的数组。复制总是触发浅拷贝——该方法从不复制初始创建数组以外的任何内容。原数组的元素按如下方式复制到新数组中：

- 对象：对象引用被复制到新数组中。原数组和新数组都引用同一个对象。也就是说，如果一个被引用的对象被修改，新数组和原数组都可以看到更改。
- 基本类型，如字符串，数字和布尔值（不是 String、Number 和 Boolean 对象）：它们的值被复制到新数组中。

其他方法会改变调用该方法的数组，在这种情况下，它们的返回值根据方法的不同而不同：有时是对相同数组的引用，有时是新数组的长度。

以下方法使用 @@species 创建新数组：

- concat()
- filter()
- flat()
- flatMap()
- map()
- slice()
- splice() (构造返回的已删除元素数组）

注意，group() (en-US) 和 groupToMap() (en-US) 不使用 @@species 为每个组条目创建新数组，而是始终使用普通的 Array 构造函数。从概念上讲，它们也不是复制方法。

以下方法可以对原数组进行修改：

- copyWithin()
- fill()
- pop()
- push()
- reverse()
- shift()
- sort()
- splice()
- unshift()

### 通用数组方法

数组方法总是通用的——它们不访问数组对象的任何内部数据。它们只通过 length 属性和索引访问数组元素。这意味着它们也可以在类数组对象上调用。

```
const arrayLike = {
  0: "a",
  1: "b",
  length: 2,
};
console.log(Array.prototype.join.call(arrayLike, "+")); // 'a+b'
```

### 长度属性的规范化

length 属性被转换为一个数字，被截断为一个整数，然后固定为 0 到 253 - 1 之间的范围。NaN 变成 0，所以即使 length 没有出现或 undefined，它也会表现得好像它的值是 0。

`Array.prototype.flat.call({}); // []`

一些数组方法会设置数组对象的 length 属性。它们总是在规范化后设置值，因此 length 总是以整数结尾。

```
const a = { length: 0.7 };
Array.prototype.push.call(a);
console.log(a.length); // 0
```

### 类数组对象

术语类数组对象指的是在上面描述的 length 转换过程中不抛出的任何对象。在实践中，这样的对象应该实际具有 length 属性，并且索引元素的范围在 0 到 length - 1 之间。（如果它没有所有的索引，它将在功能上等同于稀疏数组。）

许多 DOM 对象都是类数组对象——例如 NodeList 和 HTMLCollection。arguments 对象也是类数组对象。你可以在它们上调用数组方法，即使它们本身没有这些方法。

```
function f() {
  console.log(Array.prototype.join.call(arguments, "+"));
}

f("a", "b"); // 'a+b'
```

### 构造函数

Array()

创建一个新的 Array 对象。

### 静态属性

get Array[@@species]

返回 Array 构造函数。

### 静态方法

`Array.from()`
从数组类对象或可迭代对象创建一个新的 Array 实例。

`Array.isArray()`
如果参数是数组则返回 true ，否则返回 false 。

`Array.of()`
创建一个新的 Array 实例，具有可变数量的参数，而不管参数的数量或类型。

### 实例属性

`Array.prototype.length`
反映数组中元素的数量。

`Array.prototype[@@unscopables]`
包含 ES2015 版本之前 ECMAScript 标准中没有包含的属性名，在使用 with 绑定语句时会被忽略。

### 实例方法

Array.prototype.at()
返回给定索引处的数组元素。接受从最后一项往回计算的负整数。

Array.prototype.concat()
返回一个新数组，该数组由被调用的数组与其它数组或值连接形成。

Array.prototype.copyWithin()
在数组内复制数组元素序列。

Array.prototype.entries()
返回一个新的数组迭代器对象，其中包含数组中每个索引的键/值对。

Array.prototype.every()
如果调用数组中的每个元素都满足测试函数，则返回 true。

Array.prototype.fill()
用静态值填充数组中从开始索引到结束索引的所有元素。

Array.prototype.filter()
返回一个新数组，其中包含调用所提供的筛选函数返回为 true 的所有数组元素。

Array.prototype.find()
返回数组中满足提供的测试函数的第一个元素的值，如果没有找到合适的元素，则返回 undefined。

Array.prototype.findIndex()
返回数组中满足提供的测试函数的第一个元素的索引，如果没有找到合适的元素，则返回 -1。

Array.prototype.findLast()
返回数组中满足提供的测试函数的最后一个元素的值，如果没有找到合适的元素，则返回 undefined。

Array.prototype.findLastIndex()
返回数组中满足所提供测试函数的最后一个元素的索引，如果没有找到合适的元素，则返回 -1。

Array.prototype.flat()
返回一个新数组，所有子数组元素递归地连接到其中，直到指定的深度。

Array.prototype.flatMap()
对调用数组的每个元素调用给定的回调函数，然后将结果平展一层，返回一个新数组。

Array.prototype.forEach()
对调用数组中的每个元素调用函数。

Array.prototype.group() (en-US) 实验性
根据测试函数返回的字符串，将数组的元素分组到一个对象中。

Array.prototype.groupToMap() (en-US) 实验性
根据测试函数返回的值，将数组的元素分组到 Map 中。

Array.prototype.includes()
确定调用数组是否包含一个值，根据情况返回 true 或 false。

Array.prototype.indexOf()
返回在调用数组中可以找到给定元素的第一个（最小）索引。

Array.prototype.join()
将数组的所有元素连接为字符串。

Array.prototype.keys()
返回一个新的数组迭代器，其中包含调用数组中每个索引的键。

Array.prototype.lastIndexOf()
返回在调用数组中可以找到给定元素的最后一个（最大）索引，如果找不到则返回 -1。

Array.prototype.map()
返回一个新数组，其中包含对调用数组中的每个元素调用函数的结果。

Array.prototype.pop()
从数组中移除最后一个元素并返回该元素。

Array.prototype.push()
在数组末尾添加一个或多个元素，并返回数组新的 length。

Array.prototype.reduce()
对数组的每个元素（从左到右）执行用户提供的 “reducer” 回调函数，将其简化为单个值。

Array.prototype.reduceRight()
对数组的每个元素（从右到左）执行用户提供的 “reducer” 回调函数，将其简化为单个值。

Array.prototype.reverse()
反转数组中元素的顺序。（前面变成后面，后面变成前面。）

Array.prototype.shift()
从数组中移除第一个元素并返回该元素。

Array.prototype.slice()
提取调用数组的一部分并返回一个新数组。

Array.prototype.some()
如果调用数组中至少有一个元素满足提供的测试函数，则返回 true。

Array.prototype.sort()
对数组的元素进行排序并返回该数组。

Array.prototype.splice()
从数组中添加和/或删除元素。

Array.prototype.toLocaleString()
返回一个表示调用数组及其元素的本地化字符串。重写 Object.prototype.toLocaleString() 方法。

Array.prototype.toString()
返回一个表示调用数组及其元素的字符串。重写 Object.prototype.toString() 方法。

Array.prototype.unshift()
在数组的前面添加一个或多个元素，并返回数组新的 length。

Array.prototype.values()
返回一个新的数组迭代器对象，该对象包含数组中每个索引的值。

Array.prototype[@@iterator]()
默认情况下，该方法为 values() 方法的别名。

### 遍历数组

下面的例子使用 for...of 循环遍历 fruits 数组，将每一个元素打印到控制台。

```
const fruits = ['Apple', 'Mango', 'Cherry'];
for (const fruit of fruits) {
  console.log(fruit);
}
// Apple
// Mango
// Cherry
```

### 对数组中的每个元素调用函数

下面的例子使用 forEach() 方法在 fruits 数组中的每个元素上调用一个函数；该函数将每个元素以及元素的索引号打印到控制台。

```
const fruits = ['Apple', 'Mango', 'Cherry'];
fruits.forEach((item, index, array) => {
  console.log(item, index);
});
// Apple 0
// Mango 1
// Cherry 2
```

### 复制数组

下面的例子展示了从现有的 fruits 数组创建新数组的三种方法：首先使用展开语法，然后使用 from() 方法，然后使用 slice() 方法。

```
const fruits = ['Strawberry', 'Mango'];

// Create a copy using spread syntax.
const fruitsCopy = [...fruits];
// ["Strawberry", "Mango"]

// Create a copy using the from() method.
const fruitsCopy2 = Array.from(fruits);
// ["Strawberry", "Mango"]

// Create a copy using the slice() method.
const fruitsCopy3 = fruits.slice();
// ["Strawberry", "Mango"]
```

# 收集与分享

excalidraw.com，一个在线的手绘白板工具

www.grabient.com，很棒的 UI 工具，用于生成线性渐变色。

shadows.brumm.af，你可以设置模糊、透明度、位置和其他参数设置阴影，帮我们生成阴影代码。

keyframes.app/animate，使用可视时间轴编辑器创建 CSS @keyframe 动画。

undraw.co，一款国际范的免费开源插图网站

headlessui.com，一款漂亮的UI 组件，可以在使用 Vue 和 React 项目中很方便调用， 并能与Tailwind CSS 完美集成

生成纯CSS模糊动画背景。
官方网站：https://wweb.dev/resources/animated-css-background-generator/

构建半透明、模糊的玻璃状背景，类似于ui.glass/generator。
官方网站：https://hype4.academy/tools/glassmorphism-generator

生成支持跨浏览器的纯CSS发光效果。
官方网站：https://cssbud.com/css-generator/css-glow-generator/

可以通过border-radius属性生成超酷的图形对象，与BlobMaker类似。
官方网站：https://9elements.github.io/fancy-border-radius/

使用内设的阴影生成Soft-UI CSS样式
官方网站：https://neumorphism.io/
