# Pattern Atoms and Pattern operators
Pattern是通过原子事件和操作符组合在一起构成模板。原子事件有3类，操作符有4类，具体如下：

## 原子事件：
- 普通事件：包括POJO，Map，Array，XML
- 时间事件：包括间隔n个时间单位、crontab
- 自定义插件：用于观察特定事件的发生

## 操作符：
- 重复操作符：every, every-distinct, [num] and until
- 逻辑操作符：and, or, not
- 顺序操作符：->（Followed by）
- 事件生命周期操作符：timer:within, timer:withinmax, while-expression, 自定义插件

## 关于操作符，自然会有优先级，具体如下：
Precedence Operator Description Example
- guard postfix where timer:within and while (expression) (incl. withinmax and plug-in pattern guard) MyEvent where timer:within(1 sec)
- unary every, not every, distinct every MyEventtimer:interval(5 min) and not MyEvent
- repeat [num], until [5] MyEvent[1..3] MyEvent until MyOtherEvent
- and and every (MyEvent and MyOtherEvent)
- or or every (MyEvent or MyOtherEvent)
- followed by -> every (MyEvent -> MyOtherEvent)

# Pattern Filter Expression
Pattern的Filter表达式和普通的表达式没有区别，我就不展开讲解了，各位看看下面几个例子就好，除了Filter之外的东西暂时不用关心是什么意思。
- every e1=RfidEvent -> e2=RfidEvent(assetId=e1.assetId)
- every e1=RfidEvent -> e2=RfidEvent(MyLib.isInRadius(e1.x, e1.y, x, y) and zone in (1, e1.zone))
- every (RfidEvent(zone > 1) and RfidEvent(zone < 10))

# Controlling Event Consumption
上面说到了Filter，因为Pattern可以由多个原子事件组成，那么Filter自然也会有多个，正常情况下，所有的Filter都会对进入引擎的事件进行判定，但是我们也有只需要判定一次的时候，只要满足了某个Filter，那么其他的Filter就不用管这个事件了。Esper考虑到了这个需求，我们只需要在Filter表达式后面加个@consume注解即可，此注解可以跟随数字，表示过滤的优先级。默认优先级为1，数值越大优先级越高。

# Followed-by
如果各位有看过官方文档，应该会发现Followed-by的讲解是在比较靠后的位置，而放在第一的是Every关键字。我把它提前主要是因为其他的关键字结合Followed-by能更好的说明那个关键字的特点。如果不习惯我这样的顺序硬要跟着文档学习的朋友，可以跳过这一节先看后面的内容。
Followed-by，顾名思义就是“紧跟，跟随”的意思，通常用于事件之间的关联。举个现实生活中的例子：比如下班了，我用钥匙把家门打开，这是一个事件，紧接着我打开了浴室的灯，这也是一个事件，由于我之前在中央控制系统里设定了一个规则：打开家门后如果开了浴室的灯，热水就会放好，我一会儿就能洗澡了。所以我之前的一系列操作就触发了放热水这个动作。可能这个例子不是比较实际，但是应该能很清楚的说明这个关键字的含义吧。
Followed-by的操作符用减号和大于号组成，即->，和C语言里的指针一模一样。操作符两边是事件名称，表示右边的事件跟随左边的事件发生之后发生，即进入引擎。如果只是简单的事件follow，在操作符的两边写上事先定义的事件名或者类全名。例如： AppleEvent -> BananaEvent 
