# 少编码多思考：代码越多 问题越多

* 摘要：本文作者Ed Finkler是一名PHP、Python、JavaScript程序员。有许多产品开发经验，例如Spaz，一个开源微博客户端桌面和WebOS。
他在编码时总结了一些非常益用的编码守则，分享给大家。

大约一年前，我曾编写过一些PHP Web编程守则——MicroPHP Manifesto。但我发现各个语言之间有一些共同的编程/编码规则，这或许是我在熟悉
各种类型的编程语言后的一些收获吧。下面是我总结出来的一些规则，并且在实际中应该牢记于心。

1. 学习语言而不是框架
我喜欢PHP、Python和JavaScript，喜欢用他们做些东西。但我却不是Symfony、Django、jQuery开发人员。我认为这有很大的区别。
一个人很有可能成为一名jQuery程序员而非JavaScript，也有可能成为Django程序员而不是Python。在实际应用中，的确存在许多有价值且
非常实用的工具和框架，但如果我仅知道如何使用一个框架，我想表达的观点是在工作上只使用合适的工具其实会给任务带来一些限制，你可以查看
一下，看看你用的工具，看看你用的框架。以我的经验来看，一些复杂的全栈（full-stack）框架并不是非常合适的工具，尤其在灵活性和性能方
面都不是太好。集中精力学习一门语言会让程序员变的更加灵活。全栈式复杂框架可以帮助我快速的构建某个产品，但当我需要一个不属于框架范围
内的解决方案时，它反而会变成一种伤害。我经常会采用“plug和pray”方法进行开发，当我发现某个库或插件可以满足需求时，我就会把它们应用
到产品里。这样可能会使应用程序快速推出，但在以后的道路上会留下很多障碍。此外，学习全栈框架和学习新语言一样复杂。它们通常都有复杂的
体系结构和术语，并且有些部分并不适用于其他框架和工具上。当然框架也有许多好处，但前提是你必须要懂这门语言，然后才能理解其真正的工作
原理，所以我宁愿花时间学习更多关于语言本身的东西，并且把所学的技能应用到其他语言或者库上。

2. 构建小模块
有些小型的单元代码是很好很讨程序员们喜欢的，因为越小越容易理解且很难把它弄的很糟，所以限制编写冗长复杂的代码是非常重要的。
所以有目的的构建一些小模块——尽可能的接近需求目标。它们应该是独立的块，单纯地解决某方面问题，但是把它们结合起来时，就可以解决
许多大型的、复杂的问题。像这些简单的模块代码修复起bug来也会非常容易。因为这些单独的块通俗易懂，一看就会知道其用途。如果模块是
自我包含的，那么测试起来会更加简单。

3. 代码越少越好
套用Biggie Smalls的一句话：“代码越多，问题也就越多”。谁都喜欢管理少的代码。估计大家都有过这样的体会，当审查一个功能模块的
代码时，如果代码很多很乱，第一印象肯定不好，相反，如果该模块代码简洁明了，你会非常愉悦。更通俗点讲就是代码越多，管理起来也就越困难：
搜索代码库的时间会变长、查看文件导航也需要较长的时间、跟踪执行也会变的困难等。你是否发现，代码审查、还有你使用的工具，很大程度上都
是用来减少代码量的。那些庞大的库和长代码似乎会溢出人们的大脑缓冲区。当我在追踪一段较长的源码或执行跳跃好几个源文件的功能时，我会感
到很苦恼。这就是为什么我会喜欢给语法进行着色的编辑器，并且保持一致的空格对我也非常有帮助。除了喜欢管理较少的代码外，我还支持开发者
们尽量简化代码。程序员要为应用程序所使用的代码，不仅仅是自己编写的部分负责——甚至是这些应用里的每行代码。这也就意味着要替这些应用里
出现的bug或者安全漏洞负责。你会在程序中使用自己不理解的代码吗？这并不表示我从不使用他人的代码——坦白说，世上有许多优秀的程序员，但
是在应用他人代码的时候，你必须理解代码，因为应用程序里的每行代码都很重要。在编码时千万不要忘记思考，编写最少代码的背后应该是多思考，
这样就不会给自己带来不必要的麻烦。

4. 编写简单、有用可读的代码
编写容易理解的代码，少编码多思考，这样完成一个功能就会很快，生产力就会得到提高。当然，我也希望代码是可验证的。并且我一直认为简单、
模块化的代码是更容易被测试。代码应具备的另一特征就是可读。代码应简洁明了，语义清楚。在编写代码时，我会思考其他程序员在第一眼看到
它的时候会花多长时间来理解。或者一两个月后我自己能一目了然吗？正如大家熟知的那句编程谚语：任何一个傻瓜都会写出能够让机器理解的代码，
只有好的程序员才能写出人类可以理解的代码。当我试图发现它们工作原理的时间越少，做的事情就会越多。但是很少有人能坚持这些规则，如果我
说是，那么我肯定是在撒谎。有时候我也会很懒惰，甚至由于时间限制，我会编写一些复杂的、难以理解的代码或者使用没有审查的库来实现某个功
能。想要在短期内编写简单、清晰的代码会很困难——它需要更多的纪律和不断的技术评估。特别是那种对时间敏感的项目，实行起来将会更难。但是，
当你花时间和精力去做的时候，你会发现功夫不负有心人——不仅仅对自己有帮助，还会给其他团队成员带来很多益处。
