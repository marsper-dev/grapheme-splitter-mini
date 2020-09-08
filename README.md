# 背景

在JavaScript中，字符串字符与用户称为单独的可视“字母”之间并不总是一对一的关系。一些符号由几个字符表示。当分割字符串并无意地将多字符字母切成两半时，或者当您需要字符串中的实际字母数时，这可能会导致问题。

例如，诸如“🌷“，”🎁“，”💩“，”😜“和”👍”分别由两个JavaScript字符表示（高替代和低替代）。也就是说，

```javascript
"🌷".length == 2
```
组合的表情符号甚至更长：

```javascript
"🏳️‍🌈".length == 6
```

此外，某些语言通常包含组合标记-用于修改其前面字母的字符。常见的例子是德语字母ü和西班牙语字母ñ。有时，它们可以交替表示为单个字符和字母+组合标记，两种形式均有效：
    
```javascript
var two = "ñ"; // unnormalized two-char n+◌̃  , i.e. "\u006E\u0303";
var one = "ñ"; // normalized single-char, i.e. "\u00F1"
console.log(one!=two); // prints 'true'
```

由流行的punycode.js库或ECMAScript 6的String.normalize执行的Unicode标准化有时可以修复这些差异并将两字符序列转换为单个字符。但这并不是在所有情况下都足够。诸如印地语之类的某些语言在其字母上大量使用了组合标记，由于可能的组合数量众多，它们没有专用的单码点Unicode序列。例如，印地语单词“अनुच्छेद”由5个字母和3个组合标记组成：

अ + न + ु + च + ् + छ + े + द

实际上，这只是5个用户可感知的字母：

अ + नु + च् + छे + द

以及哪种Unicode规范化无法正确组合。还有一些不常见的字母+组合标记组合，它们没有专用的Unicode代码点。字符串Z͑ͫ̓ͪ̂ͫ̽͏̴̙̤̞͉͚̯̞̠͍A̴̵̜̰͔ͫ͗͢L̠ͨͧͩ͘G̴̻͈͍͔̹̑͗̎̅͛́Ǫ̵̹̻̝̳͂̌̌͘显然具有5个单独的字母，但实际上由58个JavaScript字符组成，其中大多数是组合标记。

输入grapheme-splitter.js库。它可以用于将JavaScript字符串正确地拆分为人类用户称为单独字母（或Unicode术语中的“扩展字素簇”）的内容，无论其内部表示形式是什么。它是在一个实现默认字形簇边界的UAX＃29。

# 用例

```

var splitter = require("grapheme-splitter-mini-min.js");

splitter.splitGraphemes("abcd"); // 返回 ["a", "b", "c", "d"]

splitter.splitGraphemes("🌷🎁💩😜👍🏳️‍🌈"); // 返回 ["🌷","🎁","💩","😜","👍","🏳️‍🌈"]

splitter.splitGraphemes("Ĺo͂řȩm̅"); // 返回 ["Ĺ","o͂","ř","ȩ","m̅"]

splitter.splitGraphemes("뎌쉐"); // 返回 ["뎌","쉐"]

splitter.splitGraphemes("अनुच्छेद"); // 返回 ["अ","नु","च्","छे","द"]

splitter.splitGraphemes("Z͑ͫ̓ͪ̂ͫ̽͏̴̙̤̞͉͚̯̞̠͍A̴̵̜̰͔ͫ͗͢L̠ͨͧͩ͘G̴̻͈͍͔̹̑͗̎̅͛́Ǫ̵̹̻̝̳͂̌̌͘!͖̬̰̙̗̿̋ͥͥ̂ͣ̐́́͜͞"); // 返回 ["Z͑ͫ̓ͪ̂ͫ̽͏̴̙̤̞͉͚̯̞̠͍","A̴̵̜̰͔ͫ͗͢","L̠ͨͧͩ͘","G̴̻͈͍͔̹̑͗̎̅͛́","Ǫ̵̹̻̝̳͂̌̌͘","!͖̬̰̙̗̿̋ͥͥ̂ͣ̐́́͜͞"]


