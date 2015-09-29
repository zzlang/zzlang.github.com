---
layout: post
title: "JavaScript Regex"
categroies: JavaScript
---

闲着没事把JS中关于正则的一些方法和注意事项给总结了一下，内容全都放在代码里面了，注释包含一切。

<script src="https://gist.github.com/1986554.js?file=regex.js">
</script>

总结中发现了一点问题，注意76行代码中的greg.lastIndex = 0，这里手动把greg的lastIndex属性置为了0,因为在此之前用过grep.test(str1),而最后输出的是grep.lastIndex=2。
但是此后又用到match = str2.match(greg)，于是后面的greg.lastIndex开始不淡定了，下面代码可以看的更明白：

<script src="https://gist.github.com/1986559.js?file=reg.js">
</script>

代码在chrome中输出为2，而在FF和IE9下输出为0,意味着match方法对greg的lastIndex属性有影响，而在chrome下却没有影响，opera和其他的IE版本还没有测试。

在ECMA上面找到了答案。 样式太难搞了，直接放位置了。

ECMA-262 3rd中第113页，15.5.4.10 String.prototype.match (regexp)。
ECMA-262 5rd中的155页。

`Else, global is true, Call the [[Put]] internal method of rx with arguments “lastIndex” and 0.`

rx指代正则对象。 根据ECMA上面的描述，如果greg有globle属性的话，会先把greg.lastIndex的属性置为0, 所以应该说chrome是遵循规范的。
