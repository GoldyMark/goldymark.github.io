---
title: 翻译：UiBinder with I18N
comments: true
date: 2013-09-14 13:26:00
updated: 2013-09-14 13:26:00
tags:
  - java
  - gwt
categories:
  - program
  - java
---

> ## 原文地址
> http://www.gwtproject.org/doc/latest/DevGuideUiBinderI18n.html

## 翻译时间
2013年9月10日至2013年9月14日

> 译者注
> 这篇文章是我边看边译的，方便自己的同时也方便他人。内容不完全直译，未必100%准确，只能翻译出大概意思，本人之前毫无翻译经验，如有错漏之处，敬请指教。

<!-- more -->

## 译文
本文主要探讨`UiBinder`模板的`I18N`功能，想了解更多`UiBinder`的功能，请浏览[Declarative Layout with UiBinder](http://www.gwtproject.org/doc/latest/DevGuideUiBinder.html)（译者注：该文章的中文版，[click here](http://gwt-note.psmonkey.org/official/buildUI/With-UiBinder)）

## 目录
1. 背景；
2. Bonjour, Tout Le Monde（Hello,world）！
3. Message内的简易HTML标记
4. 隐藏（保护）部分消息
5. Messages在运行期的值运算
6. 将Widgets植入message当中（只适用于HTMLPanel）
7. 翻译HTML属性
8. 具有多重含义的词组

### 背景

`UiBinder`模板可以用于本地化标记，使用`<ui:msg>`和`<ui:attribute>`元素表示该模板的哪些部分应进行翻译，在构建应用程序时提供版本化本地化消息的properties文件。
> 原文：then provide properties files with localized versions of the messages when building your app.

前文中（[click here](http://gwt-note.psmonkey.org/official/buildUI/With-UiBinder)）通过一系列的典型应用例子部分介绍了如何本地化你的UI模板。
注意：你将看到UiBinder的本地化与`GWT`的[Messages](http://www.gwtproject.org/doc/latest/DevGuideI18nMessages.html)有很多相似之处，原因是：`UiBinder`的I18N功能是通过为每个模板生成一个隐藏的[com.google.gwt.i18n.client.Messages](http://www.gwtproject.org/javadoc/latest/com/google/gwt/i18n/client/Messages.html)接口而实现的。除了复数形式（原文：plural forms），你能通过模板实现[Messages](http://www.gwtproject.org/doc/latest/DevGuideI18nMessages.html)所实现的。

### Bonjour, Tout Le Monde（Hello,world）

这里将展示如何开启这个功能。

**Original：**
```xml
<ui:UiBinder xmlns:ui='urn:ui:com.google.gwt.uibinder'>
  <div>Hello, world.</div>
</ui:UiBinder>
```

**Tagged：**
```xml
<ui:UiBinder xmlns:ui='urn:ui:com.google.gwt.uibinder'
    ui:generateFormat='com.google.gwt.i18n.rebind.format.PropertiesFormat'
    ui:generateKeys="com.google.gwt.i18n.rebind.keygen.MD5KeyGenerator"
    ui:generateLocales="default">
  <div><ui:msg description="Greeting">Hello, world.</ui:msg></div>
</ui:UiBinder>
```
在这里我们做了两件事。一、通过在根元素`<ui:UiBinder>`里面添加一些属性来配置`UiBinder`的I18N功能；二、单独标记需要实现本地化的文本。
先来看看`“Hello，World”`这个文本。我们将放进`<ui:msg>`的`“Hello，World”`标记为一条可翻译的文本，一个`Message.description`属性是用来描述该消息的用途。虽然只是个可选属性，但可能是翻译者在上下文中能看到的为数不多的信息，所以你应该习惯性地为消息添加描述，就像为代码添加注释一样。
现在我们有了需要翻译的东西，我们通过一些配置在`<ui:UiBinder>`里面的属性告诉`UiBinder`如何完成翻译工作。这些`ui:generate*`属性相当于`LocalizableResource`的[@Generate](http://www.gwtproject.org/javadoc/latest/com/google/gwt/i18n/client/LocalizableResource.Generate.html)标注。详见如下：

+ `ui:generateFormat`
使用java properties格式。

+ `ui:generateKeys`
使用消息内容的MD5哈希值作为键集合，因此消息翻译才能在模板内生效。（如果我们留意一下properties是如何被管理的，你会发现在多个模板使用同一个message只需要翻译一次）

+ `ui:generateLocales`
将生成的properties作为`default locale`的一部分。（关于`default locale`和`fallback properties`的更多内容，请浏览[Locales in GWT](http://www.gwtproject.org/doc/latest/DevGuideI18nLocale.html)）
编译应用程序时，给`gwt compiler`传递一个`-extras`参数，告诉编译器生成应用程序的附属文件。然后就会为每个模板生成一个包含所有message入口标记位置的properties文件。
```properties
# Generated from my.app.HelloWorldMyBinderImplGenMessages
# for locale default

# Description: Greeting
022A824F26735ED0582324BE34F3CAE1=Hello, world.
```

生成的文件名是根据你定义的UiBinder接口而定的，有点惨不忍睹。
举个例子：
模板名称：`com.example.app.client.Hello.Binder`；
module名称：`App`；
运行带有`-extra /tmp/gwt-extras`参数的`gwt compiler`；
然后你会发现文件的路径是这个样子：`/tmp/gwt-extras/com.example.app.App/com.example.app.client.HelloBinderImplGenMessages.properties`

就如包大人所说的：这，并不好笑嘛。希望在GWT以后的发行版本里面能有更简洁的解决方案。

然后，你可以在`ui.xml`文件的同一目录下创建该文件的本地化版本。继续上面的例子，一个`Mexican Spanish`版本的本地化properties应命名为`HelloBinderImplGenMessages_es_MX.properties`（注意将`com.example...`前缀咔擦掉）。
你不一定需要使用`MD5 keys`。如果你想自定义键名，可以在`<ui:msg>`里面添加一个`key`属性并指定你想要设置的键名。另一方面，如果你坚持使用`MD5 keys`，你可以将应用程序所有的翻译集合成一个指定特定语言环境的文件，而不是让他们遍布你的代码。这个properties文件的文件名格式是：`LocalizableResource_<locale>.properties`，而且你必须把这个文件放在`com/google/gwt/i18n/client`包下面。（原理：i18n properties文件会沿着类继承树往上搜寻，而所有`Messages`接口都继承于`LocalizableResource`）

> 译者注：这个功能看上去挺有趣，有兴趣的可以去了解以下。

有些项目会使用脚本来维护这些统一的翻译文件。这里有个来自`puzzlebazar`项目的例子（[wiki entry](http://code.google.com/p/puzzlebazar/wiki/UiBinderInternationalisation)）。

还有其他I18N属性可以配置在`<ui:UiBinder>`元素下，其实相当于`Localizable`的其他注解，但你很少会改变他们的默认值。

+ `ui:defaultLocale`
See @DefaultLocale for details

+ `ui:generateFilename`
See @Generate(fileName = "...") for details

+ `ui:baseMessagesInterface`
设置用于生成`Messages`的基类接口，值必须是全路径类名，且该类需继承于`Messages`接口。你可以添加任何注解，以便公司及项目范围内的设置可以在一个地方修改即可。如有必要，你也可以覆盖该接口其他属性的默认值。

### Message内的简易HTML标记

`<ui:msg>`可以适当地使用HTML标记。

**Original：**
```html
We <b>strongly</b> urge you to reconsider.
```

**Tagged：**
```xml
<ui:msg>We <b>strongly</b> urge you to reconsider.</ui:msg>
```

`UiBinder`中的`Messages`支持`HTML`，不只是文本。

### 隐藏（保护）部分消息

> 译者注：这部分内容翻译起来有点难，可能看代码反而更容易理解。

有时候你需要将message的某些部分隐藏起来，以防翻译者破坏你的应用程序。

**Original：**
```html
<!-- Uh oh, don't want translator to mess up brand CSS or the trademark span -->

<div><span class="brand">Colgate</span>, with MFP!<span class="tm">TM</span></div>
```

**Tagged：**
```xml
<div>
  <ui:msg description="blurb"><span class="brand" ui:ph="brandedSpan">Colgate</span>,
  with MFP!<ui:ph name="trademark"><span class="tm">TM</span></ui:ph></ui:msg>
</div>
Generated：
# Description: blurb
# 0=arg0 (Example: <span class='brand'>), 1=arg1 (Example: </span>), 2=arg2 (Example: <span class='tm'>TM</span>)
6E8B421C6A7C1FEAE23FAA9D43C90D5E={0}Colgate{1}, with MFP\!{2}
```

在这里例子中有两个地方。首先，你可以在`<ui:msg>`里面的任何子元素添加`ui:ph`属性，表明这里应该创建占位符免受翻译。在`element`的开始和结束标记位置会创建两个占位符（在这个例子里，分别是`brandedSpanOpen`和`brandedSpanClose`）。
然后，我们同样看到一个名为`ui:ph`的子元素，可以将任何被保护的元素置于其包围之中（在这个例子里，就是`tradermark`占位符）。
所以，你既可以将被保护的元素置于`<ui:ph>`包围之下，就像`<ui:ph>don't translate</ui:ph>`，也可以使用`ui:ph`属性隐藏元素的开始和结束标记（相对于翻译者而言），只保留元素的内容允许翻译，就像`<span ui:ph>attribute</span>`。注意：你可以将`ui:ph`属性置于任何`DOM element`，而不只是`<span>`。

> 译者注：其实上面这段话的意思就是：`ui:ph`既可以作为`element`的属性使用，也可以将`<ui:ph>`作为`element`封装需要被保护的元素。
> **用法1：**
> `<span class="brand" ui:ph="brandedSpan">Colgate</span>`

> **用法2：**
> `<ui:ph name="trademark"><span class="tm">TM</span></ui:ph>`

### Messages在运行期的值运算

当你想在运行期间修改模板的某一部分，你通常会将`ui:field`放入一个`span`或者其他`element`并且置于其`HTML`当中。当你在`<ui:msg>`里面这样做，它会自动保护免受翻译。

**Original：**
```html
<!-- Java code will make calls like getClosingDate().setInnerText(closingDate()) -->
(closed <span ui:field="closingDate" /> through <span ui:field="reopeningDate"/>)
```

**Tagged：**
```xml
<ui:msg description='closed for business message'>
  (closed <span ui:field='closingDate' /> through <span ui:field='reopeningDate'/>)
</ui:msg>
```

***Generated：***
```
# Description: closed for business message
# 0=arg0 (Example: <span id=''>), 1=arg1 (Example: </span>), 2=arg2 (Example: <span id=''>), 3=arg3 (Example: </span>)
E30D43242E1AD2AC2EFA1AEEEFDFCC33=(closed {0}{1} through {2}{3})
```

这里有一个好消息和坏消息。好消息就是你不需要在标记有`ui:filed`属性的`span`里额外添加任何`ui:ph`属性或用`<ui:ph>`包围去保护开始和结束标记。坏消息就是无法限制翻译者在这些开始和结束标记之间添加任何东西。`Notice：{0}{1} and {2}{3}`
如果这是个问题，你需要将`spans`放入`<ui:ph>`里面以被保护，就像这样：

**Tagged：**
```xml
<ui:msg>
  (closed <ui:ph name='closingDate' example="7/12/2008"><span ui:field="closingDate"/></ui:ph>
  through <ui:ph name='reopeningDate' example="7/12/2008"><span ui:field="reopeningDate"/></ui:ph>)
</ui:msg>
```

**Generated：**
```
# 0=arg0 (Example: 7/12/2008), 1=arg1 (Example: 7/12/2008)
53B9CF65553DFAA091435791E5C731E7=(closed {0} through {1})
```

`example`属性是可选的，这个属性允许你提供翻译者更多关于占位符的值和用途用法的信息，方便翻译者。

> 译者注：这一节同样看代码能更易理解，对比两段`tagged`和`generated`就很容易明白，大概意思就是说如果你需要在`<ui:msg>`里面使用`ui:field`的话，需要使用`<ui:ph>`包围封装带有`ui:field`属性的`element`。

### 将Widgets植入message当中（只适用于[HTMLPanel](http://www.gwtproject.org/javadoc/latest/com/google/gwt/user/client/ui/HTMLPanel.html)）

当使用`<g:HTMLPanel>`时，你可能会想将`widgets`植入`messages`当中。这个可以有！

**Original：**
```xml
<g:HTMLPanel>
  Meeting starts at
    <my:TimePicker ui:field="startPicker"/>
  and ends at
    <my:TimePicker ui:field="endPicker"/>.
</g:HTMLPanel>
```

**Tagged：**
```xml
<g:HTMLPanel>
  <ui:msg>Meeting starts at
    <my:TimePicker ui:field="startPicker"/>
  and ends at
    <my:TimePicker ui:field="endPicker"/>.
  </ui:msg>
</g:HTMLPanel>
```

**Generated：**
```
# 0=arg0 (Example: <span>), 1=arg1 (Example: </span>), 2=arg2 (Example: <span>), 3=arg3 (Example: </span>)
23CBEA252C9901BF84D757FAD4968289=Meeting starts at {0}{1} and ends at {2}{3}.
```

注意：`widgets`是没有`ui:ph`属性的，因为不需要。当一个`widget`出现在`message`中的时候，`widget`需要做什么是毫无歧义的。需要留意的是你只能在`HTMLPanel`里面做这种事情（widgets in messages），因为`HTMLPanel`是`GWT`里面唯一一个`widget`能混合html标记和子widget。

更多：你可能注意到在生成的properties文件里每个`widget`拥有太多的占位符，这意味着翻译者可能会引入一些不需要的文本到在运行期被`widgets`取代的`spans`里面。出现这样的事情，除了浪费翻译者的时间，不会造成任何危害。`widgets`还在那里，而文本则会丢失——你的用户不会看到这些文本。

当你把一个包含在message文本主体里的widget放入HTMLPanel，事情会变得更加有趣（widget implements [HasText](http://www.gwtproject.org/javadoc/latest/com/google/gwt/user/client/ui/HasText.html), [HasHTML](http://www.gwtproject.org/javadoc/latest/com/google/gwt/user/client/ui/HasHTML.html)）。

**Original：**
```xml
<g:HTMLPanel>
  To do the thing, <g:Hyperlink targetHistoryToken="/doThe#thing">click here</g:Hyperlink>
  and massage vigorously.
</g:HTMLPanel>
```

**Tagged：**
```xml
<g:HTMLPanel>
  <ui:msg>
    To do the thing, <g:Hyperlink targetHistoryToken="/doThe#thing">click here</g:Hyperlink>
    and massage vigorously.
  </ui:msg>
</g:HTMLPanel>
```

**Generated：**
```
# 0=arg0 (Example: <span>), 1=arg1 (Example: </span>)
8EFBF967A3FEFE78C41C8A298562A094=To do the thing, {0}click here{1} and massage vigorously.
```

> 译者注：但我觉得这种做法很恶心……

### 翻译HTML属性
`
可以翻译的并不只是主体文本——属性同样也可以。比较常见的有`title`属性（`tool-tip`文本）和`<img>`的`alt`标记。

**Original：**
```html
<th title="Gross recipts">Gross</th>
```

**Tagged：**
```xml
<th title="Gross receipts">
  <ui:attribute ui:name='title' ui:description='Tooltip text for gross column'/>
  <ui:msg description='name of gross column'>Gross</ui:msg>
</th>
```

### 具有多重含义的词组

需要注意的是，`words`在不同的上下文中有时候会表达不同的意思。

**Original：**
```
Favorite Color:
  <ui:RadioButton name="color">Red</ui:RadioButton>
  <ui:RadioButton name="color">Orange</ui:RadioButton>
```

```
Favorite Fruit:
  <ui:RadioButton name="fruit">Apple</ui:RadioButton>
  <ui:RadioButton name="fruit">Orange</ui:RadioButton>
```

**Tagged：**
```
Favorite Color:
  <ui:RadioButton name="color"><ui:msg>Red</ui:msg></ui:RadioButton>
  <ui:RadioButton name="color"><ui:msg meaning="the color"/>Orange</ui:msg></ui:RadioButton>
```

```
Favorite Fruit:
  <ui:RadioButton name="fruit"><ui:msg>Apple</ui:msg></ui:RadioButton>
  <ui:RadioButton name="fruit"><ui:msg meaning="the fruit">Orange<ui:msg></ui:RadioButton>
```

**Generated：**
```
# Meaning: the color
4404BE8C34552617D633271BBC1FAB07=Orange

# Meaning: the fruit
7A6DCA1ACC86B4A7D7574CD6BDD4E0C1=Orange

9F6290F4436E5A2351F12E03B6433C3C=Apple

EE38E4D5DD68C4E440825018D549CB47=Red
```

The punchline here is that a translator may well be working with no more context than the attributes you set on an individual message. （译者注：这句不懂得翻译，但也不大影响对本节内容的理解）

如果你将所有的翻译都放入一个大池当中使用，而它们之间仅仅通过MD5哈希值来区别区分的话，你在这里就不能提供“两种口味”的“Organge”了。

所以，你可以通过使用`meaning`属性来解决这个问题。不同`description`的是，`meaning`属性是会影响生成的哈希值的。

> 译者注：这一节大意就是说当一个properties里有同一个值的时候，需要增加一个`meaning`属性来区分彼此。

## 结尾语
自己在做翻译的过程中也觉得翻译不好做，也不知道自己的理解和表达是否准确，希望对学习`uibinder`的人有一点点帮助吧。
这里附上一个有关`UiBinder`的PDF下载地址。[gwt-ui-overhaul](http://203.208.46.147/url?sa=t&rct=j&q=gwt-ui-overhaul&source=web&cd=4&ved=0CD0QFjAD&url=%68%74%74%70%3a%2f%2f%64%6c%2e%67%6f%6f%67%6c%65%2e%63%6f%6d%2f%67%6f%6f%67%6c%65%69%6f%2f%32%30%31%30%2f%67%77%74%2d%75%69%2d%6f%76%65%72%68%61%75%6c%2e%70%64%66&ei=XvMzUvmrJseriAe13oC4DQ&usg=AFQjCNGT7DoLG0Snur8w615nTIdfNxt_fw&bvm=bv.52164340,d.aGc&cad=rjt)
