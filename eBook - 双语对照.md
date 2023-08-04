# Level Up Your Code With Game Progamming SimplifiedChinesePatterns-Sim

[toc]

## 介绍设计模式
When working in Unity, you don’t have to reinvent the wheel . It’s likely someone has already invented one for you .
当在Unity中工作时，你不需要重新发明轮子。很可能有人已经为你发明了一个。

For every software design issue you encounter, a thousand developers have been there before . While you can’t always ask them directly for advice,  you can learn from their decisions through design patterns . 

对于你遇到的每一个软件设计问题，都有成千上万的开发者在你之前遇到过。虽然你不能总是直接向他们寻求建议，但你可以通过设计模式来学习他们的决策。

Design patterns are general solutions to common problems found in software engineering . These aren’t finished solutions that you can copy and paste into your code, but you can think of design patterns as extra tools in your toolbox .Some are more obvious than others .

设计模式是解决软件工程中常见问题的通用解决方案。这些并不是你可以复制粘贴到代码中的完成的解决方案，但你可以把设计模式看作是工具箱中的额外工具。有些比其他的更明显。

This guide assembles well-known design patterns in Unity development .  The examples in this guide have been simplified and technical jargon reduced, to make them more accessible, though you should have a working knowledge  of C# basics before starting with them .

这份指南汇集了Unity开发中的知名设计模式。本指南中的示例已经简化，技术术语也已经减少，使得它们更易于接触，尽管你在开始使用它们之前应该具备一些C#基础知识。

If you’re still new to design patterns or need a quick refresher, the guide also provides common scenarios where you can apply them in game development .For those switching from another object-oriented language (Java, C++, etc .)  to C#, these samples will show you how to adapt patterns specifically to Unity .

如果你对设计模式还很新，或者需要快速复习，本指南还提供了一些你可以在游戏开发中应用它们的常见场景。对于那些从其他面向对象语言（如Java、C++等）转到C#的人，这些样例将展示如何将模式特别地适应到Unity中。

At the core of it, design patterns are just ideas . They won’t apply in all situations . But they can help you build larger applications that scale when used correctly . Integrate them into your project to improve code readability and make your codebase cleaner . As you gain experience with patterns, you’ll recognize when they can speed up your development process .

归根结底，设计模式只是一些想法。它们并不适用于所有情况。但是，如果正确使用，它们可以帮助你构建规模更大的应用程序。将它们整合到你的项目中，可以提高代码的可读性，使你的代码库更加清晰。当你对模式有了更多的经验，你会认识到它们何时可以加快你的开发过程。

Then you can stop reinventing the wheel and, well, start working on something new .

然后，你就可以停止重新发明轮子，开始着手于一些新的事情。

**Contributors 贡献者**

This guide was written by Wilmer Lin, a 3D and visual effects artist with over 15 years of industry experience in film and television, who now works as an independent game developer and educator . Significant contributions were also made by senior technical content marketing manager Thomas Krogh-Jacobsen and senior Unity engineers Peter Andreasen and Scott Bilas .

本指南由Wilmer Lin撰写，他是一位有超过15年电影和电视行业经验的3D和视觉效果艺术家，现在作为一名独立游戏开发者和教育者工作。高级技术内容营销经理Thomas Krogh-Jacobsen和高级Unity工程师Peter Andreasen和Scott Bilas也作出了重要贡献。  
  
<br>  
 
>**Using this guide and the KISS principle 使用本指南和KISS原则**  
>
>
>This guide aims to present you with new ways of thinking about and organizing your code . Several patterns for software design highlighted in this guide are adapted to Unity development .  
>本指南旨在向你展示关于思考和组织你的代码的新方法。本指南中突出显示的几种软件设计模式都已适应于Unity开发。  
>
>The included [sample project](https://github.com/Unity-Technologies/game-programming-patterns-demo/) shows some of the code in context .  Use the corresponding scenes to explore these design patterns and their underpinning principles .  
>包含的[示例项目](https://github.com/Unity-Technologies/game-programming-patterns-demo/)显示了一些上下文中的代码。使用相应的场景来探索这些设计模式及其基础原则。  
>   
>However, when reviewing these examples, remember there isn’t a blanket “right way” to approach a problem . The sample code is one solution among many .   
>然而，在审查这些例子时，请记住并没有一种全面的“正确方式”来处理问题。示例代码只是众多解决方案中的一种。   
>
>When in doubt, filter everything in this guide through the [KISS principle](https://en.wikipedia.org/wiki/KISS_principle): “Keep it simple, stupid .” Only add complexity if necessary .  
>在有疑问的时候，通过[KISS原则](https://en.wikipedia.org/wiki/KISS_principle)来过滤本指南中的所有内容：“保持简单、愚蠢。”只有在必要的时候才增加复杂性.   
>
>Every design pattern comes with tradeoffs, whether that means additional structures to maintain or more setup at the beginning .  Decide if the benefit justifies extra work before implementing it .  
>每种设计模式都带有权衡，无论是意味着需要维护额外的结构，还是在开始时需要更多的设置。在实施之前，决定是否利益能够证明额外的工作是值得的。    
> 
>If you’re unsure if a pattern applies to your specific problem, you might be better off waiting for a situation where it feels like a more natural fit . Don’t use a pattern because it’s new or novel to you; use it when you need it .  
>如果你不确定某个模式是否适用于你的特定问题，你可能最好等待一个情况，让它感觉更自然地适合。不要因为一个模式对你来说是新的或者是新颖的就使用它；只有在你需要的时候才使用它。  
> 
>Then, the design pattern will serve its intended purpose: to help you develop better software .   
 >然后，设计模式将发挥其预期的目的：帮助你开发更好的软件。  
>
>Let’s get started .  
>让我们开始吧。
  
  <br>  
  Before charging into the patterns themselves, let’s look at some design principles that influence how they work .  

 在深入设计模式本身之前，让我们看一下一些影响它们工作方式的设计原则.   

 SOLID is a mnemonic acronym for five core fundamentals of software design:  
 SOLID是五个软件设计的核心基础的助记符：  
+ [Single responsibility 单一职责](https://en.wikipedia.org/wiki/Single-responsibility_principle)  
+ [Open-closed 开闭原则](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)  
+ [Liskov substitution 里氏替换](https://en.wikipedia.org/wiki/Liskov_substitution_principle)  
+ [Interface segregation 接口隔离](https://en.wikipedia.org/wiki/Interface_segregation_principle)  
+ [Dependency inversion 依赖反转](https://en.wikipedia.org/wiki/Dependency_inversion_principle)  

Let’s examine each concept and see how they help you make your code more understandable, flexible, and maintainable .  
让我们分别检查每个概念，看看它们如何帮助你让你的代码更易于理解、更灵活和更易于维护。

**Single-responsibility principle 单一职责原则**

A class should have one reason to change, just its single responsibility .  
一个类应该只有一个变化的理由，也就是其单一的职责。

The first and most important SOLID principle is the single-responsibility principle (SRP), which states that each module, class, or function is responsible for one thing and encapsulates only that part of the logic .  
最重要的SOLID原则是[单一职责原则(SRP)](https://en.wikipedia.org/wiki/Single-responsibility_principle)，它规定每个模块、类或函数只负责一件事，并且只封装那部分逻辑。

Assemble your projects from many smaller ones instead of building monolithic classes . Shorter classes and methods are easier to explain, understand, and implement .  
你应该把你的项目从许多小的项目中组合起来，而不是构建庞大的类。较短的类和方法更容易解释、理解和实现。

If you’ve worked in Unity for a while, you’re likely already familiar with this concept . When you create a GameObject, it holds a variety of smaller components . For example, it might come with:  
如果你在Unity中工作了一段时间，你可能已经熟悉了这个概念。当你创建一个GameObject时，它会包含各种小的组件。例如，它可能会带有：

+ A MeshFilter that stores a reference to the 3D model  
一个MeshFilter，存储对3D模型的引用
+ A Renderer that controls how the model surface appears onscreen  
一个Renderer，控制模型表面在屏幕上的显示方式
+ A Transform component that stores scale, rotation, and position  
一个Transform组件，存储缩放、旋转和位置
+ A Rigidbody if it needs to interact with the physics simulation  
如果需要与物理模拟交互，还会有一个Rigidbody

Each component does one thing and does it well . You build an entire scene from GameObjects . The interaction between their components is what makes a game possible .  
每个组件只做一件事，并且做得很好。你从GameObject构建整个场景。它们的组件之间的互动是游戏可能性的体现。

You’ll construct your scripted components in the same way . Design them so each one can be clearly understood . Then have them work in concert to make complex behavior .   
你将以同样的方式构建你的脚本组件。设计它们使得每一个都可以清楚地被理解。然后让它们协同工作，产生复杂的行为。  
