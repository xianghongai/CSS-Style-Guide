# CSS 开发规范手册

## 1. 规则声明

```
selectorlist {
  property: value;
  [more property:value; pairs]
}
```

`selector[:pseudo-class] [::pseudo-element] [, more selectorlists]`

```
选择器 {
  属性: 值;
}
```


## 2. 准则与格式

- 不使用 ID 选择器
- 避免使用后代选择器
- 避免使用 `!important`
- 使用两个空格作为缩进
- 类名小写使用连字符(`kebab-case`)代替驼峰法(`camelCase `)
- 在一个规则声明中应用了多个选择器时，每个选择器独占一行
- 在规则声明的左大括号 `{` 前加上一个空格
- 在属性的冒号 `:` 后面加上一个空格，前面不加空格
- 规则声明的右大括号 `}` 独占一行
- 规则声明之间用空行分隔开

Bad

```css bad
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

Good

```css good
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```


## 3. 注释

- 使用行注释 (在 Sass 中是 `//`) 代替块注释。
- 注释独占一行。避免行末注释。
- 给没有自注释的代码写上详细说明，比如：
    + 为什么用到了 z-index
    + 兼容性处理或者针对特定浏览器的 hack


## 4. 构建 CSS

组合使用 [SMACSS](https://smacss.com/)、[OOCSS](https://github.com/stubbornella/oocss/wiki) 和 [BEM](http://getbem.com/naming/)：

- 理清 CSS 和 HTML 之间清晰且严谨的关系
- 对样式中不同性质部分进行分离
- 创建出可重用、易装配的组件
- 减少嵌套，降低特定性
- 创建可扩展的样式表


### 4.1 SMACSS

Scalable and Modular Architecture for CSS，

构建可扩展和模块化的 CSS，其核心是从页面及交互中的 “Base, Layout, Module, State, Theme” 等角度分离样式。

- `Base` ，最基础的单个元素选择器，设置段落、标题、链接、字体、body 背景等样式；
- `Layout` ，划分页面的主干结构、栅格结构；
- `Module` ，设定可重复使用的模块化组件；
- `State` ，描述布局或模块在特定状态下的特征；
- `Theme` ，描述布局或模块的视觉特征。

### 4.2 OOCSS

Object Oriented CSS，

面向对象的CSS，旨在编写可复用、低耦合和可扩展的 CSS 代码。

两个主要原则：

- 结构型样式和视觉特征型样式分离
- 容器型样式和内容型样式分离

Bad

```css bad
.sidebar h3 {
  font-family: Arial, Helvetica, sans-serif;
  font-size: .8em;
  line-height: 1;
  color: #777;
  text-shadow: rgba(0, 0, 0, .3) 3px 3px 6px;
}

.footer h3 {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 2em;
  line-height: 1;
  color: #777;
  text-shadow: rgba(0, 0, 0, .3) 3px 3px 6px;
}
```

Good

```css good
.title {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 2em;
  line-height: 1;
  color: #777;
  text-shadow: rgba(0, 0, 0, .3) 3px 3px 6px;
}

.footer .title {
  font-size: 1.5em;
  text-shadow: rgba(0, 0, 0, .3) 2px 2px 4px;
}
```

### 4.3 BEM

Block-Element-Modifier，

命名约定，使前端代码易于阅读和理解，易于使用，容易扩展，更强大和更明确，更严格。

#### 4.3.1 结构拆分

- `Block` 可独立模块
- `Element` 子元素：俩下划线
- `Modifier` 模块状态：一个下划线
- `Block` 下的所有 `Element` 不考虑层级嵌套关系，扁平化处理都属于 `Block`，即连续的 `__` 下划线只能出现一次
- 复合词、断词使用连字符(中横线) `-`

#### 4.3.2 实现

- 模块：`.Block`
- 模块_状态：`.Block_Modifier`
- 模块__子元素：`.Block__Element`
- 模块__子元素_状态：`.Block__Element_Modifier`

```html good
<article class="listing-card listing-card_featured">
  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>
  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>
</article>
```

```css good
.listing-card { }
.listing-card_featured { }
.listing-card__title { }
.listing-card__content { }
```

- `listing-card` 是一个块（block），表示高层次的组件。
- `listing-card__title` 是一个元素（element），它属于 `.listing-card` 的一部分，因此块是由元素组成的。
- `listing-card_featured` 是一个修饰符（modifier），表示这个块与 `.listing-card` 有着不同的状态或者变化。


## 5. 命名

- 使用功能性的或通用的命名方式
- 避免使用类型选择器限定 Class 名
- 使用名词
- 修饰使用“尺寸等级/颜色层级/形态/状态”之类的形容词

不用**表象的**具体词汇!

如：某模块在当前 Theme 下主色是蓝色，字体大小 14px，内外间距 10px，在右边，右对齐，不能以 `.blue`, `.f14`, `.m10`, `.p10`, `.right`, `.text-right` 命名；其它 Theme 中可能是别的颜色、尺寸规格、方位和对齐方式。

## 6. JavaScript 钩子

避免在 CSS 和 JavaScript 中绑定相同的类。否则开发者在重构时通常会出现以下情况：

- 浪费时间在 HTML、CSS、JS 三类文件中对照查找每个要改变的类
- 担心破坏功能而不利于作出更改

推荐在创建用于特定 JavaScript 的类名时，添加 `.js_` 前缀，采用 `.js_lowerCamelCase` 命名规则。

```html
<button class="btn btn-primary js_requestToBook">Request to Book</button>
```


## 7. SCSS

### 7.1 SCSS变量

变量名应使用破折号（例如 `$my-variable`），对于仅用在当前文件的变量，在变量名之前添加下划线前缀（例如 `$_my-variable`）。

### 7.2 嵌套选择器

不要让嵌套选择器的深度超过 3 层，经 BEM 扁平化处理之后的层级大部分无需嵌套。

```css
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```


## 8. 常用命名

| 用途     | ClassName                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 布局     | header, container, footer, aside, row, column, section, wrapper, inner                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 形态     | default, primary, secondary, tertiary, accent, muted, light, dark, success, danger, warning, info, sans-serif, serif, monospace                                                                                                                                                                                                                                                                                                                                                               |
| 等级[^1] | base, small, normal, regular, medium, large, xlarge, xxlarge, xxxlarge                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 方位[^2] | top, right, bottom, left, center, vertical, horizontal, reverse, start, end, between, around, baseline, stretch, prev, next, forward, back                                                                                                                                                                                                                                                                                                                                                    |
| 状态     | active, enable, disabled, ready, wait, not started, activate, running, in progress, tested, failed, checked, suspend, resume, delayed, aborted, amending, blocked, canceling, mounted, destroyed, completed                                                                                                                                                                                                                                                                                   |
| 操作     | create, read, update, delete, insert, edit, remove, verify, switch, off, on, cancel, close, hover, focus                                                                                                                                                                                                                                                                                                                                                                                      |
| 组件     | module, title, alert, form, button, table, dialog, nav, menu, breadcrumb, pagination, pager, popover, modal, dropdown, tooltip, list, accordion, collapse, tab, panel, carousel, slide, media, card, colorpicker, timepicker, datepicker, datetimepicker, calendar, switch, select, radio, checkbox, transfer, upload, cascader, tree-select, label, badge, avatar, backtop, loading, spin, message, notification, overlay, steps, timeline, progress, scroll, rate, divider, spacer, gallery |
| 页面     | index, archive, detail, special, contact, search, about, forum                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 其它     | timestamp, uniques, item, more, icon                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

[^1]: “等级”名称仅用于 SCSS 变量。
[^2]: 不用“方位”命名，极特殊的情况除外。