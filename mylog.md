# 配置记录

主要根据Volantis主题<a href="https://volantis.js.org/v4/getting-started/">说明文档</a>进行相关配置

当前主题<a href="https://github.com/celestezj/hexo-theme-volantis/releases/tag/4.3.1">版本</a>存在的三个问题（bug）：

1. 主题首页的箭头bug，如果`height_scheme`置为`full`、`layout_scheme`置为`featured`，首页底部会显示一个下拉的箭头，如果切换到归档页面，这个箭头还是会存在（别人的博客没有啊，难不成只有我一个人遇到？），且无法点击，目前我是将`height_scheme`置为`half`、`layout_scheme`置为`dock`以避免该问题（就眼不见心不烦呗）

2. 初始时刻打开博客网站时，下滑出现眉头导航栏，这没毛病，但是上滑却有很大概率不能隐藏（就好像导航栏卡住不动似的），必须重新刷新网页才变得正常，如下图所示（此时仅仅将`_config.yml`中的`theme`置为`volantis`，其他设置还没有做任何改动）：

   <img src="https://raw.githubusercontent.com/celestezj/ImageHosting/master/img/20210209160917.PNG" style="zoom: 50%;" />

   经过测试发现，每次导航栏卡住，只要点击一下导航栏里的“博客”按键，就能恢复正常，所以我尝试通过jQuery模拟一次点击操作，于是添加script代码，写入`$(".title.flat-box")[0].click();`并添加在`_config.yml`文件中（<a href="https://volantis.js.org/v4/site-settings/">使用Import导入外部文件</a>），但是不起作用，然后我发现需要<a href="https://www.cnblogs.com/mq0036/p/6278498.html">等待页面加载完毕</a>之后再执行模拟点击才有效，于是新增完整代码为：

   ```yml
   import:
     script:
       - <script>window.onload =function(){$(".title.flat-box")[0].click();}</script>
   ```

3. 在iPad设备上归档页面的时间线无法图示无法正常显示

   <img src="https://raw.githubusercontent.com/celestezj/ImageHosting/master/img/20210209160949.png" style="zoom: 33%;" />

   这个暂时解决不了啊

关于博客的部署，遇到一个莫名其妙的问题，`hexo deploy`无法推送代码至远程仓库，deploy插件似乎就没有执行最后一步`push -f`操作，仅仅执行到“将`public/`目录下的文件拷贝到`.deploy_git/`目录下”，所以现在我只能手动推送：`cd .deploy_git`，`git add -A`，`git push -f origin master`。一开始如果没有`.deploy_git/`文件夹，先从远程仓库克隆一份，即在博客根目录下执行`git clone 博客仓库地址 .deploy_git`

还有一个应该不算bug，因为文档明确说了首页文档的摘要不支持latex渲染，但是下面这个（katex）实在有点奇怪，大部分不渲染，但还有一小部分渲染：

<img src="https://raw.githubusercontent.com/celestezj/ImageHosting/master/img/20210209173734.PNG" style="zoom: 80%;" />

其他配置：

- <a href="https://blog.csdn.net/qq_45428737/article/details/106299049">添加动态滚动字体</a>，这个很好弄，添加一个div，再通过js往里面填入字符串，并通过js实现滚动特效，代码为：

  ```javascript
  <!-- 封面滚动字体 -->
  <div id="binft"></div> 
  <script>
      var binft = function (r) {
        function t() {
          return b[Math.floor(Math.random() * b.length)]
        }  
        function e() {
          return String.fromCharCode(94 * Math.random() + 33)
        }
        function n(r) {
          for (var n = document.createDocumentFragment(), i = 0; r > i; i++) {
            var l = document.createElement("span");
            l.textContent = e(), l.style.color = t(), n.appendChild(l)
          }
          return n
        }
        function i() {
          var t = o[c.skillI];
          c.step ? c.step-- : (c.step = g, c.prefixP < l.length ? (c.prefixP >= 0 && (c.text += l[c.prefixP]), c.prefixP++) : "forward" === c.direction ? c.skillP < t.length ? (c.text += t[c.skillP], c.skillP++) : c.delay ? c.delay-- : (c.direction = "backward", c.delay = a) : c.skillP > 0 ? (c.text = c.text.slice(0, -1), c.skillP--) : (c.skillI = (c.skillI + 1) % o.length, c.direction = "forward")), r.textContent = c.text, r.appendChild(n(c.prefixP < l.length ? Math.min(s, s + c.prefixP) : Math.min(s, t.length - c.skillP))), setTimeout(i, d)
        }
        var l = "",
        o = ["山有木兮木有枝，心悦君兮君不知。", "人生若只如初见，何事秋风悲画扇。","十年生死两茫茫，不思量，自难忘。", "曾经沧海难为水，除却巫山不是云。","洛中何郁郁，冠带自相索。","只愿君心似我心，定不负相思意。","愿得一心人，白头不相离。","入我相思门，知我相思苦。"].map(function (r) {
        return r + ""
        }),
        a = 2,
        g = 1,
        s = 5,
        d = 75,
        b = ["rgb(110,64,170)", "rgb(150,61,179)", "rgb(191,60,175)", "rgb(228,65,157)", "rgb(254,75,131)", "rgb(255,94,99)", "rgb(255,120,71)", "rgb(251,150,51)", "rgb(226,183,47)", "rgb(198,214,60)", "rgb(175,240,91)", "rgb(127,246,88)", "rgb(82,246,103)", "rgb(48,239,130)", "rgb(29,223,163)", "rgb(26,199,194)", "rgb(35,171,216)", "rgb(54,140,225)", "rgb(76,110,219)", "rgb(96,84,200)"],
        c = {
          text: "",
          prefixP: -s,
          skillI: 0,
          skillP: 0,
          direction: "forward",
          delay: a,
          step: g
        };
        i()
        };
        binft(document.getElementById('binft'));
    </script>
  ```

  将上述代码直接复制粘贴到volantis/layout/_cover/dock.ejs指定位置处：

  ```ejs
  <div class='top'>
      <% if (theme.cover.logo) { %>
        <img class='logo' src='<%- url_for(theme.cover.logo) %>'/>
      <% } %>
      <% if (theme.cover.title) { %>
        <p class="title"><%- theme.cover.title ? theme.cover.title : config.title %></p>
      <% } %>
      <% if (theme.cover.subtitle) { %>
        <p class="subtitle"><%- theme.cover.subtitle%></p>
      <% } %>
  +     此处添加代码
  </div>
  ```

  我是用这个动态效果来展示网站的`subtitle`的（具体的逻辑是，如果`subtitle`不为空，则显示无滚动的`subtitle`，否则以动态效果展示`colourful_subtitle`），为了方便，我将要滚动的字符串定义在主题配置文件中，修改上述js代码为`binft(document.getElementById('binft'),<%- JSON.stringify(theme.cover.colourful_subtitle)%>);`（替代`binft(document.getElementById('binft'));`），第二个参数就是要往div中填入的字符串，通过模板变量传入，还是为了方便，我将上述js代码单独放在一个ejs文件中，见`volantis/layout/_cover/subtitle.ejs`，最后还要修改`volantis/layout/_cover/dock.ejs`的逻辑：

  ```ejs
  <div class='top'>
      <% if (theme.cover.logo) { %>
        <img no-lazy class='logo' src='<%- url_for(theme.cover.logo) %>'/>
      <% } %>
      <% if (theme.cover.title) { %>
        <p class="title"><%- theme.cover.title ? theme.cover.title : config.title %></p>
      <% } %>
      <% if (theme.cover.subtitle) { %>
        <p class="subtitle"><%- theme.cover.subtitle%></p>
      <% } else if (theme.cover.colourful_subtitle) { %>
  	  <%- partial('subtitle') %>
  	<% } %>
    </div>
  ```

- 增加页面中卡片的边框阴影效果，这个要改好几处，就举一个例子，首页文章卡片，修改`volantis/source/css/_layout/main.styl`，找到`.post-wrapper`属性，修改如下（要添加的内容以加号标注）：

  ```stylus
  .post-wrapper
      column-break-inside: avoid
      break-inside: avoid-column
  +   box-shadow: 0 1px 20px -6px rgba(0,0,0,0.5)
  +   border-radius: 8px
  +   transition: box-shadow .5s ease
  +   &:hover
  +     box-shadow:0px 1px 20px 1px rgba(0,0,0,0.5)
  ```

  至于其他文件修改都是相同的，都是添加上面这5行内容，哪些是要修改的？就鼠标右键那些还没有阴影的卡片，点击“检查”，找属性名，然后通过`findstr /M /S "属性名" *.*`在主题文件夹下搜寻即可，很容易就不多说了

- 电脑版对latex公式的渲染是很好的，但是到了手机端，那些太长的公式并不会自动换行，而是超出了div边界，超出边界的部分就看不到了，为此卸载了原始的渲染器hexo-renderer-marked，转而安装hexo-renderer-markdown-it-plus，即使用katex渲染：

  ```shell
  npm uninstall hexo-renderer-marked --save
  npm install hexo-renderer-markdown-it-plus --save
  ```

  安装好后，在`_config.yml`中添加配置：

  ```yaml
  markdown_it_plus:
      highlight: true
      html: true
      xhtmlOut: true
      breaks: true
      langPrefix:
      linkify: true
      typographer:
      quotes: “”‘’
      plugins:
          - plugin:
              name: markdown-it-mark
              enable: false
  ```

  如果还不行，继续修改`volantis/layout/_partial/head.ejs`，在最后一行添加：

  ```ejs
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.css" integrity="sha384-zB1R0rpPzHqg7Kpt0Aljp8JPLqbXI3bhnPWROx27a9N0Ll6ZP/+DiW/UqRcLbRjq" crossorigin="anonymous">
  
      <!-- The loading of KaTeX is deferred to speed up page rendering -->
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.js" integrity="sha384-y23I5Q6l+B6vatafAwxRu/0oK/79VlbSz7Q9aiSZUvyWYIYsd+qj+o24G5ZU2zJz" crossorigin="anonymous"></script>
  
      <!-- To automatically render math in text elements, include the auto-render extension: -->
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous"
          onload="renderMathInElement(document.body);"></script>
  ```

  现在手机端长公式就能够自动换行了

- 由于我的图片等资源是放在GitHub仓库里的，国内访问需要翻墙，很不友好，所以这里借助<a href="https://www.bilibili.com/read/cv4297993">jsDelivr白嫖全球超高速的静态资源访问服务</a>，简单来说，jsDelivr会镜像备份GitHub的数据在全球服务器节点，包括中国区，这样我们相当于访问国内的服务器，加载速度就很快，其使用方式异常简单，只需要重构资源链接即可，形式为`https://cdn.jsdelivr.net/gh/user/repo/file`，这也是最简单的方式，主要用于不会被修改的文件，另一种是文件可能会被修改，那么最好是先在GitHub仓库上发布release版本，然后通过版本号，构建链接地址，形式为`https://cdn.jsdelivr.net/gh/user/repo@version/file`

- 添加微信二维码弹窗特效，这个动画效果通过css就可以完成，效果如下图所示：

  <img src="https://raw.githubusercontent.com/celestezj/ImageHosting/master/img/20210209172444.PNG" style="zoom:80%;" />

  原本微信图标是一个超链接，为了实现特效需要在超链接标签里添加一个img标签用于显示二维码，为了使代码更加灵活不僵硬，我并没有直接在微信图标处强行写入img标签，而是通过jQuery寻找微信图标，假设其存在的话（上图展示了GitHub、邮箱、微信二维码、和rss四个图标，这个是在配置文件中可选择的），然后再在其中新增img标签，这段script还是在`_config.yml`中配置：

  ```yaml
  import:
    script:
      - <script>$('.fa-weixin').html('<img class="qrcode" src="https://cdn.jsdelivr.net/gh/celestezj/ImageHosting/img/20210209044239.jpg" alt="微信二维码"/>');</script>
  ```

  而用于动画的css则添加在`volantis/source/css/first.styl`中：

  ```stylus
  #safearea{
    display: none;
    .fa-weixin {
      position: relative;
    }
    .fa-weixin img.qrcode {
      position: absolute;
      z-index: 9;
      top: -135px;
      right: -43px;
      width: 7.5rem;
      max-width: none;
      height: 7.5rem;
      transform: scale(0);
      transform-origin: bottom;
      opacity: 0;
      //border: .1rem solid #0085ba;
      border-radius: .25rem;
      -webkit-transition: all .4s ease-in-out;
      -o-transition: all .4s ease-in-out;
      transition: all .4s ease-in-out;
    }
    .fa-weixin:hover img.qrcode {
      transform: scale(1);
      opacity: 1;
    }
  }
  ```

- 开启暗黑模式，点击后，将暗黑模式变成白天模式，很简单，只要通过jQuery修改节点文本即可：

  <img src="https://raw.githubusercontent.com/celestezj/ImageHosting/master/img/20210210114452.png" style="zoom:100%;" />

  将jQuery代码添加到`volantis/layout/_partial/scripts/darkmode.ejs`中：

  ```ejs
  function bindToggleButton() {
      btn.on('click',(e) => {
        const mode = toggleCustomDarkMode();
  	  if(mode=="light"){
            $(".menuitem.flat-box.header.toggle-mode-btn").eq(0).children().eq(0).attr('class','fas fa-moon fa-fw');
            $(".menuitem.flat-box.header.toggle-mode-btn").eq(0).contents().last()[0].textContent='暗黑模式';
  	  }else if(mode=="dark"){
            $(".menuitem.flat-box.header.toggle-mode-btn").eq(0).children().eq(0).attr('class','fas fa-sun fa-fw');
            $(".menuitem.flat-box.header.toggle-mode-btn").eq(0).contents().last()[0].textContent='白天模式';
  	  };
        applyCustomDarkModeSettings(mode);
      });
  }
  ```

- 添加代码一键复制功能，首先安装`npm install clipboard --save`，然后在`volantis/source/css/first.styl`中添加按钮样式：

  ```stylus
  .highlight{
    //方便copy代码按钮（btn-copy）的定位
    position: relative;
  }
  .btn-copy {
      display: inline-block;
      cursor: pointer;
      background-color: #eee;
      background-image: linear-gradient(#fcfcfc,#eee);
      border: 1px solid #d5d5d5;
      border-radius: 3px;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
      -webkit-appearance: none;
      font-size: 13px;
      font-weight: 700;
      line-height: 20px;
      color: #333;
      -webkit-transition: opacity .3s ease-in-out;
      -o-transition: opacity .3s ease-in-out;
      transition: opacity .3s ease-in-out;
      padding: 2px 6px;
      position: absolute;
      right: 5px;
      top: 5px;
      opacity: 0;
  }
  .btn-copy span {
      margin-left: 5px;
  }
  .highlight:hover .btn-copy{
    opacity: 1;
  }
  ```

  在主题的js目录下创建文件`clipboard-use.js`用于创建复制按钮：

  ```js
  function create_copy_buttons(e, t, a) { 
    /* code */
    var initCopyCode = function(){
      var copyHtml = '';
      copyHtml += '<button class="btn-copy" data-clipboard-snippet="">';
      copyHtml += '  <i class="fa fa-clipboard"></i><span>copy</span>';
      copyHtml += '</button>';
      $(".highlight .code pre").before(copyHtml);
      new ClipboardJS('.btn-copy', {
          target: function(trigger) {
              return trigger.nextElementSibling;
          }
      });
    }
    initCopyCode();
  };
  create_copy_buttons(window, document);
  ```

  在`_config.yml`中添加对上述js代码的引用：

  ```yaml
  import:
    script:
      - <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/2.0.4/clipboard.min.js"></script>
      - <script type="text/javascript" src="/js/clipboard-use.js"></script>
  ```

- 

2021.2.7