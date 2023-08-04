# Level Up Your Code With Game Progamming SimplifiedChinesePatterns-Sim

[toc]

## 介绍设计模式
当在Unity中工作时，你不需要重新发明轮子。很可能有人已经为你发明了一个。

对于你遇到的每一个软件设计问题，都有成千上万的开发者在你之前遇到过。虽然你不能总是直接向他们寻求建议，但你可以通过设计模式来学习他们的决策。

设计模式是解决软件工程中常见问题的通用解决方案。这些并不是你可以复制粘贴到代码中的完成的解决方案，但你可以把设计模式看作是工具箱中的额外工具。有些比其他的更明显。

这份指南汇集了Unity开发中的知名设计模式。本指南中的示例已经简化，技术术语也已经减少，使得它们更易于接触，尽管你在开始使用它们之前应该具备一些C#基础知识。

如果你对设计模式还很新，或者需要快速复习，本指南还提供了一些你可以在游戏开发中应用它们的常见场景。对于那些从其他面向对象语言（如Java、C++等）转到C#的人，这些样例将展示如何将模式特别地适应到Unity中。

归根结底，设计模式只是一些想法。它们并不适用于所有情况。但是，如果正确使用，它们可以帮助你构建规模更大的应用程序。将它们整合到你的项目中，可以提高代码的可读性，使你的代码库更加清晰。当你对模式有了更多的经验，你会认识到它们何时可以加快你的开发过程。

然后，你就可以停止重新发明轮子，开始着手于一些新的事情。

**贡献者**

本指南由Wilmer Lin撰写，他是一位有超过15年电影和电视行业经验的3D和视觉效果艺术家，现在作为一名独立游戏开发者和教育者工作。高级技术内容营销经理Thomas Krogh-Jacobsen和高级Unity工程师Peter Andreasen和Scott Bilas也作出了重要贡献。  
  
<br>  
 
>**使用本指南和KISS原则**  
>
>本指南旨在向你展示关于思考和组织你的代码的新方法。本指南中突出显示的几种软件设计模式都已适应于Unity开发。  
>包含的[示例项目](https://github.com/Unity-Technologies/game-programming-patterns-demo/)显示了一些上下文中的代码。使用相应的场景来探索这些设计模式及其基础原则。  
>然而，在审查这些例子时，请记住并没有一种全面的“正确方式”来处理问题。示例代码只是众多解决方案中的一种。  
>在有疑问的时候，通过KISS原则来过滤本指南中的所有内容：“保持简单、愚蠢。”只有在必要的时候才增加复杂性.  
>每种设计模式都带有权衡，无论是意味着需要维护额外的结构，还是在开始时需要更多的设置。在实施之前，决定是否利益能够证明额外的工作是值得的。  
>如果你不确定某个模式是否适用于你的特定问题，你可能最好等待一个情况，让它感觉更自然地适合。不要因为一个模式对你来说是新的或者是新颖的就使用它；只有在你需要的时候才使用它。  
>然后，设计模式将发挥其预期的目的：帮助你开发更好的软件。  
>
>让我们开始吧。

在深入设计模式本身之前，让我们看一下一些影响它们工作方式的设计原则。  
SOLID是五个软件设计的核心基础的助记符：  
+ [单一职责](https://en.wikipedia.org/wiki/Single-responsibility_principle)  
+ [开闭原则](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)  
+ [里氏替换](https://en.wikipedia.org/wiki/Liskov_substitution_principle)  
+ [接口隔离](https://en.wikipedia.org/wiki/Interface_segregation_principle)  
+ [依赖反转](https://en.wikipedia.org/wiki/Dependency_inversion_principle)  

让我们分别检查每个概念，看看它们如何帮助你让你的代码更易于理解、更灵活和更易于维护。

**单一职责原则**

一个类应该只有一个变化的理由，也就是其单一的职责。

最重要的SOLID原则是[单一职责原则(SRP)](https://en.wikipedia.org/wiki/Single-responsibility_principle)，它规定每个模块、类或函数只负责一件事，并且只封装那部分逻辑。

你应该把你的项目从许多小的项目中组合起来，而不是构建庞大的类。较短的类和方法更容易解释、理解和实现。

如果你在Unity中工作了一段时间，你可能已经熟悉了这个概念。当你创建一个GameObject时，它会包含各种小的组件。例如，它可能会带有：

+ 一个MeshFilter，存储对3D模型的引用
+ 一个Renderer，控制模型表面在屏幕上的显示方式
+ 一个Transform组件，存储缩放、旋转和位置
+ 如果需要与物理模拟交互，还会有一个Rigidbody

每个组件只做一件事，并且做得很好。你从GameObject构建整个场景。它们的组件之间的互动是游戏可能性的体现。

你将以同样的方式构建你的脚本组件。设计它们使得每一个都可以清楚地被理解。然后让它们协同工作，产生复杂的行为。  

如果你忽视了单一职责，你可能会创建一个自定义组件，如下所示：  

![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)

```
public class UnrefactoredPlayer : MonoBehaviour
{
    [SerializeField] private string inputAxisName;
    [SerializeField] private float positionMultiplier;
    private float yPosition;
    private AudioSource bounceSfx;

    private void Start()
    {
        bounceSfx = GetComponent<AudioSource>();
    }

    private void Update()
    {
        float delta = Input.GetAxis(inputAxisName) * Time.deltaTime;
        yPosition = Mathf.Clamp(yPosition + delta, -1, 1);
        transform.position = new Vector3(transform.position.x, yPosition * positionMultiplier, transform.position.z);
    }

    private void OnTriggerEnter(Collider other)
    {
        bounceSfx.Play();
    }
}

```

这个未重构的Player类的职责混杂。它在玩家与某物碰撞时播放声音，管理输入，并处理移动。即使这个类目前相对较短，但随着你的项目的发展，它将变得难以维护。考虑将Player类拆分为更小的类。
![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)
```
[RequireComponent(typeof(PlayerAudio), typeof(PlayerInput), typeof(PlayerMovement))]
public class Player : MonoBehaviour
{
    [SerializeField] private PlayerAudio playerAudio;
    [SerializeField] private PlayerInput playerInput;
    [SerializeField] private PlayerMovement playerMovement;

    private void Start()
    {
        playerAudio = GetComponent<PlayerAudio>();
        playerInput = GetComponent<PlayerInput>();
        playerMovement = GetComponent<PlayerMovement>();
    }
}

public class PlayerAudio : MonoBehaviour
{
    // ...
}

public class PlayerInput : MonoBehaviour
{
    // ...
}

public class PlayerMovement : MonoBehaviour
{
    // ...
}

```
Player脚本仍然可以管理其他的脚本组件，但每个类只做一件事。这种设计使得修改代码变得更容易，特别是随着你的项目需求随时间的变化。另一方面，你需要用一定的常识来平衡单一职责原则。不要过度简化到创建只有一个方法的类的极端。

在使用单一职责原则时，要牢记以下目标：

+ 可读性：短的类更容易阅读。没有硬性规定，但许多开发者设置了200-300行的限制。自己或作为一个团队确定什么构成“短”。当超过这个阈值时，决定是否可以将其重构成更小的部分。
+ 可扩展性：你可以更容易地从小的类中继承。修改或替换它们，而不用担心破坏意外的功能。
+ 可重用性：设计你的类使其小而模块化，这样你可以在游戏的其他部分重用它们。  

在重构时，考虑重新排列代码将如何改善自己或其他团队成员的生活质量。一开始的一些额外努力可以在后来为你节省很多麻烦。  
<br> 

**开闭原则**

在SOLID设计中的开闭原则（OCP）规定类必须对扩展开放，但对修改封闭。组织你的类，使得你可以在不修改原始代码的情况下创建新的行为。

这方面的经典例子是计算形状的面积。你可以创建一个名为AreaCalculator的类，其中包含返回矩形和圆形面积的方法。为了计算面积，Rectangle类有Width和Height。

而Circle只需要Radius和pi的值。
```
public class AreaCalculator 
{
    public float GetRectangleArea(Rectangle rectangle)
    {
        return rectangle.width * rectangle.height;
    }

    public float GetCircleArea(Circle circle)
    {
        return circle.radius * circle.radius * Mathf.PI;
    }
}

public class Rectangle
{
    public float width;
    public float height;
}

public class Circle
{
    public float radius;
}

```
这种方式运行得很好，但是如果你想在你的AreaCalculator中添加更多的形状，你需要为每一个新的形状创建一个新的方法。

假设你以后想要传递一个五边形或者八边形呢？如果你需要20个更多的形状呢？AreaCalculator类将会迅速失控。

你可以创建一个名为Shape的基类，并创建一个方法来处理形状。然而，这样做将需要在逻辑内部有多个if语句来处理每一种形状。这样的扩展性不好。

你希望在不修改原始代码（AreaCalculator的内部）的情况下，打开程序进行扩展（使用新的形状的能力）。虽然它是功能性的，但当前的AreaCalculator违反了开闭原则。  

![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)  
  
  
相反，考虑定义一个抽象的Shape类：
```
public abstract class Shape
{
    public abstract float CalculateArea();
}
```
这包括一个名为CalculateArea的抽象方法。如果你让Rectangle和Circle从Shape继承，每个形状都可以计算自己的面积，并返回以下结果：  
```
public class Rectangle : Shape
{
    public float width;
    public float height;

    public override float CalculateArea()
    {
        return width * height;
    }
}

public class Circle : Shape
{
    public float radius;

    public override float CalculateArea()
    {
        return radius * radius * Mathf.PI;
    }
}
```
AreaCalculator可以简化为这样：
```

public class AreaCalculator 
{
    public float GetArea(Shape shape)
    {
        return shape.CalculateArea();
    }
}
```
修订后的AreaCalculator类现在可以获取任何正确实现抽象Shape类的形状的面积。你可以扩展AreaCalculator的功能，而无需改变任何原始源代码。  
![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)  

每当你需要一个新的多边形，只需定义一个从Shape继承的新类。每个子类的形状然后覆盖CalculateArea方法来返回正确的面积。  
这种新的设计使得调试更容易。如果新形状引入了错误，你不必重新访问AreaCalculator。旧代码保持不变，所以你只需要检查新代码中是否有任何逻辑错误。  
在Unity中创建新的类时，利用接口和抽象。这有助于避免在你的逻辑中出现难以后期扩展的繁琐的switch或if语句。一旦你习惯了设置你的类来尊重OCP，长期添加新的代码变得更简单。  
<br>  

**里氏替换原则**

里氏替换原则（LSP）规定，派生类必须能够替换它们的基类。面向对象编程中的继承允许你通过子类增加功能。然而，如果你不小心，这可能导致不必要的复杂性。

里氏替换原则，SOLID的第三个支柱，告诉你如何应用继承，使你的子类更加健壮和灵活。

想象一下，你的游戏需要一个叫做Vehicle的类。这将是你为应用创建的车辆子类的基类。例如，你可能需要一辆汽车或卡车。   
![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)

在你可以使用基类（Vehicle）的任何地方，你应该能够使用像Car或Truck这样的子类，而不会破坏应用程序。

你的Vehicle类可能看起来像这样：
```
public class Vehicle
{
    public float speed = 100;
    public Vector3 direction;

    public void GoForward()
    {
        // ...
    }

    public void Reverse()
    {
        // ...
    }

    public void TurnRight()
    {
        // ...
    }

    public void TurnLeft()
    {
        // ...
    }
}

```

假设你正在构建一个回合制的游戏，你在棋盘上移动车辆。    
![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)

你可以有另一个叫做Navigator的类，来沿着预设的路径驾驶车辆：  
```
public class Navigator
{
    public void Move(Vehicle vehicle)
    {
        vehicle.GoForward();
        vehicle.TurnLeft();
        vehicle.GoForward();
        vehicle.TurnRight();
        vehicle.GoForward();
    }
}

```

有了这个类，你期望能够将任何车辆传递给Navigator的Move方法，这对于汽车和卡车都会很好地工作。但是，当你想实现一个叫做Train的类时，会发生什么？  
![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)


TurnLeft和TurnRight方法在Train类中不会工作，因为火车不能离开它的轨道。如果你确实将一辆火车传递给Navigator的Move方法，那么当你到达那些行时，它将抛出一个未实现的异常（或者什么也不做）。如果你不能用子类型替代类型，你就违反了里氏替换原则。

由于Train是Vehicle的子类型，你会期望在任何接受Vehicle类的地方使用它。否则，可能会使你的代码行为不可预测。

**考虑一些提示以更加符合里氏替换原则：**

+ **如果你在子类化时移除了功能，你可能正在破坏里氏替换：** NotImplementedException是你违反了这个原则的明显迹象。留下一个空的方法也是这样。如果子类的行为不像基类，那么你就没有遵循LSP——即使没有明显的错误或异常。
+ **保持抽象简单：** 你在基类中放入的逻辑越多，就越可能破坏LSP。基类应该只表达派生子类的公共功能。
+ **子类需要拥有与基类相同的公有成员：** 调用它们时，这些成员也需要具有相同的签名和行为。
+ **在建立类层次结构之前考虑类API：** 尽管你把它们都看作是车辆，但是Car和Train继承自不同的父类可能更有意义。现实中的分类并不总是转化为类层次结构。
+ **倾向于组合而不是继承：** 而不是试图通过继承传递功能，创建一个接口或单独的类来封装特定的行为。然后通过混合和匹配来构建不同功能的“组合”。

![image error](https://www.markdown.xyz/assets/images/san-juan-mountains.jpg)


