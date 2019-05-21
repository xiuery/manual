### xpath
xpath的使用

#### 如何获取节点名称？？？

| expression    | desc														|
| ------------- | -----------------------------------------------			|
| NodeName      | 选取此节点的所有子节点									|
| /             | 从根节点选取												|
| //            | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置	|
| .             | 选取当前节点												|
| ..            | 选取当前节点的父节点										|
| @             | 选取属性													|

#### 1.获取节点内容or属性

- 获取节点内容
```
/html/head/title/text()
```

- 获取节点指定属性
```
/html/body/title/@href
or
/html/body/title/attribute::href
```

- 获取节点所有属性
```
/html/body/title/@*
or
/html/body/title/attribute::*
```

#### 2.谓语（Predicates）
谓语用来查找某个特定的节点或者包含某个指定的值的节点
谓语被嵌在方括号中

- 选取属于bookstore子元素的第一个book元素
```
/bookstore/book[1]
```

- 选取属于bookstore子元素的最后一个book元素
```
/bookstore/book[last()]
```

- 选取属于bookstore子元素的倒数第二个book元素
```
/bookstore/book[last()-1]
```

- 选取最前面的两个属于bookstore元素的子元素的book元素
```
/bookstore/book[position()<3]
```

- 选取所有拥有名为lang的属性的title元素
```
//title[@lang]
```

- 选取所有title元素，且这些元素拥有值为eng的lang属性
```
//title[@lang='eng']
```

- 选取bookstore元素的所有book元素，且其中的price元素的值须大于35.00
```
/bookstore/book[price>35.00]
```

- 选取bookstore元素中的book元素的所有title元素，且其中的price元素的值须大于35.00
```
/bookstore/book[price>35.00]/title
```

#### 3.选取未知节点

- 匹配任何元素节点
```
*
```
- 匹配任何属性节点
```
@*
example：//title[@*]     #选取所有带有属性的title元素
```

- 匹配任何类型的节点
```
node()
```

#### 4.选取若干路径
通过在路径表达式中使用“|”运算符，您可以选取若干个路径

- 选取book元素的所有title和price元素
```
//book/title | //book/price
```

#### 5.轴
轴可定义相对于当前节点的节点集

- 选取当前节点的所有先辈（父、祖父等）
```
ancestor
exampl：
ancestor::book	        #选择当前节点的所有book先辈。
```

- 选取当前节点的所有先辈（父、祖父等）以及当前节点本身
```
ancestor-or-self
example：
ancestor-or-self::book  #选取当前节点的所有book先辈以及当前节点（如果此节点是book节点）
```

- 选取当前节点的所有属性
```
attribute
example：
attribute::lang	        #选取当前节点的lang属性
attribute::*	        #选取当前节点的所有属性
```

- 选取当前节点的所有子元素
```
child
example：
child::book	            #选取所有属于当前节点的子元素的book节点
child::*	            #选取当前节点的所有子元素
child::text()	        #选取当前节点的所有文本子节点
child::node()	        #选取当前节点的所有子节点
child::*/child::price	#选取当前节点的所有price孙节点。
```

- 选取当前节点的所有后代元素（子、孙等）
```
descendant
example：
descendant::book	    #选取当前节点的所有book后代
```

- 选取当前节点的所有后代元素（子、孙等）以及当前节点本身
```
descendant-or-self
```

- 选取文档中当前节点的结束标签之后的所有节点
```
following
```

- 选取当前节点的所有命名空间节点
```
namespace
```

- 选取当前节点的父节点
```
parent
```

- 选取文档中当前节点的开始标签之前的所有节点
```
preceding
```

- 选取当前节点之前的所有同级节点
```
preceding-sibling
```

- 选取当前节点
```
self
```
