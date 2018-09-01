RegexpMe
=======

一个JavaScript正则表达式可视化工具

线上 : http://regexp.me.solomox.cn/

### 特点:
- 用纯JavaScript编写的，不需要后端。
- 您可以通过html iframe元素将图表嵌入到您自己的站点中。
- 详细的错误消息。在大多数情况下，它可以指出精确的语法错误位置。
- 不支持八进制，是的，这是一个特点。 ECMAScript严格模式不支持字符串中的八进制转义，但是许多浏览器仍然支持正则表达式中的八进制转义。我让事情变得更加容易。在RegexpMe，  十进制将永远被当作后参考。如果后面的引用是无效的， e.g. `/\1/`、`/(\1)/`、`/(a)\2/`，或者在charset中出现了十进制转义（因为在这种情况下它不能被解释为后引用， e.g. `/(ab)[\1]/`），RegexpMe总是会犯错误。

### 多语言

中文，English

### 构造

全局安装requirejs
```bash
npm install requirejs -g
```


### API

#### Parse to AST
```javascript
var parse = require('regulex').parse;
var re = /var\s+([a-zA-Z_]\w*);/ ;
console.log(parse(re.source));
```

#### Visualize
```javascript
var parse = require('regulex').parse;
var visualize = require('regulex').visualize;
var Raphael = require('regulex').Raphael;
var re = /var\s+([a-zA-Z_]\w*);/;
var paper = Raphael('yourSvgContainer',0,0);
try {
  visualize(parse(re.source),getRegexFlags(re),paper);
} catch(e) {
  if (e instanceof parse.RegexSyntaxError) {
    logError(re,e);
  } else throw e;
}

function logError(re,err) {
  var msg=["Error:"+err.message,""];
  if (typeof err.lastIndex==='number') {
    msg.push(re);
    msg.push(new Array(err.lastIndex).join('-')+"^");
  }
  console.log(msg.join("\n"));
}


function getRegexFlags(re) {
  var flags='';
  flags+=re.ignoreCase?'i':'';
  flags+=re.global?'g':'';
  flags+=re.multiline?'m':'';
  return flags;
}
```
