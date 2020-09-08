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

# 用例

```

var splitter = require("grapheme-splitter-mini-min.js");

splitter.splitGraphemes("abcd"); // 返回 ["a", "b", "c", "d"]

splitter.splitGraphemes("🌷🎁💩😜👍🏳️‍🌈"); // 返回 ["🌷","🎁","💩","😜","👍","🏳️‍🌈"]

splitter.splitGraphemes("Ĺo͂řȩm̅"); // 返回 ["Ĺ","o͂","ř","ȩ","m̅"]

splitter.splitGraphemes("뎌쉐"); // 返回 ["뎌","쉐"]

splitter.splitGraphemes("अनुच्छेद"); // 返回 ["अ","नु","च्","छे","द"]

splitter.splitGraphemes("Z͑ͫ̓ͪ̂ͫ̽͏̴̙̤̞͉͚̯̞̠͍A̴̵̜̰͔ͫ͗͢L̠ͨͧͩ͘G̴̻͈͍͔̹̑͗̎̅͛́Ǫ̵̹̻̝̳͂̌̌͘!͖̬̰̙̗̿̋ͥͥ̂ͣ̐́́͜͞"); // 返回 ["Z͑ͫ̓ͪ̂ͫ̽͏̴̙̤̞͉͚̯̞̠͍","A̴̵̜̰͔ͫ͗͢","L̠ͨͧͩ͘","G̴̻͈͍͔̹̑͗̎̅͛́","Ǫ̵̹̻̝̳͂̌̌͘","!͖̬̰̙̗̿̋ͥͥ̂ͣ̐́́͜͞"]


