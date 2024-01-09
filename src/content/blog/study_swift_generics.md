---
pubDatetime: 2024-01-09T10:00:00.0Z
title: Swift之泛型（Generics）
featured: true
draft: false
tags:
  - Swift
description: Swift关于泛型的部分
---

# 泛型

## Table of contents

## 简介

<u>**_泛型_**</u>代码让你能根据自定义的需求，编写出适用于任意类型的、灵活可复用的函数及类型。

### 优点

- 避免编写重复的代码
- 使用清晰抽象的方式来表达代码的意图

## 解决的问题

> 🌰

```Swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
	let temp = a
	a = b
	b = temp
}
```

```Swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temp = a
    a = b
    b = temp
}
```

```Swift
func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temp = a
    a = b
    b = temp
}
```

上面三个函数使用输入输出参数（`inout`）来叫唤`a`和`b`的值

三个函数只有入参类型不同，函数实现的功能是相同的！

如何提高代码的质量呢？请看下面👇

## 泛型函数

泛型函数可适用于任意类型，下面是函数`swapTwoInts(_:_:)`的泛型版本

```Swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

泛型版本的函数使用`占位符`类型名`T`，而不是*实际*类型名（`Int`、`Double`或`String`），`占位符`类型名并不关心`T`具体的类型，但是要求`a`和`b`必须是相同的类型，`T`的实际类型由每次函数调用来决定。

### 与非泛型函数不同之处

- 函数写法 函数名后<T>
- 入参类型不关心 调用后根据入参值做类型推断

## 类型参数

> Tips:

> 大写单字母T，U，V或驼峰式命名MyTypeParameter （为了区分类型和值）

## 泛型类型

除了泛型函数，Swift还可以自定义**_泛型类型_**

这些自定义类、结构体和枚举可以适用于*任意类型*，类似于`Array`和`Dictionary`

> 🌰👉🏻 `Int`型栈

```Swift
struct IntStack {
    var items: [Int] = []
    mutating func push(_ item: Int) {
        items.append(item)
    }

    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

> 泛型写法 👇🏻

```Swift
struct Stack<Element> {
    var items: [Element] = []
    mutating func push(_ item: Element) {
        items.append(item)
    }

    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

> 🏷 👇🏻

```Swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("Hello ")
stackOfStrings.push("World")
stackOfStrings.pop()
```

## 泛型扩展

当对泛型类型进行扩展时，并不需要提供类型参数列表作为定义的一部分。
原始类型定义中声明的类型参数列表在扩展中可以直接使用，并且这些来自原始类型中的参数名称会被用作原始定义中类型参数的引用。

> 🌰

```Swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

> 🏷 👇🏻

```Swift
if let topItem = stackOfStrings.topItem {
    print("The top item is \(topItem)")
}
```

## 类型约束

可以在泛型函数或泛型类型中添加特定的**_类型约束_**

类型约束指定类型参数必须继承自指定类、遵循特定的协议或协议组合

### 类型约束语法

```Swift
func someFunc<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    //函数体
}
```

- `T`必须是`SomeClass`的子类
- `U`必须遵循`SomeProtocol`协议

### 实践

> 🌰查找`String`数组中给定`String`的索引

```
func findIndex(ofString valueToFind: String, in arr: [String]) -> Int? {
    for (i, value) in arr.enumerated() {
        if value == valueToFind {
            return i
        }
    }
    return nil
}
```

> 泛型版本👇🏻

```
func findIndex<T: Equatable>(ofString valueToFind: T, in arr: [T]) -> Int? {
    for (i, value) in arr.enumerated() {
        if value == valueToFind {
            return i
        }
    }
    return nil
}
```

## 关联类型

定义一个协议时，声明一个或多个**_关联类型_**

关联类型为协议中的某个类型提供了一个占位名称，代表的实际类型在协议被遵循时才会被指定。

关联类型通过`associatedType`关键字来指定。

### 实践

> 🌰定义`Container`协议，该协议定义了一个关联类型`Item`

```
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

> `IntStack`遵循`Container`协议：

```
struct IntStack: Container {
    //IntStack
    var items: [Int] = []
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    //Container
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

> 泛型`Stack`遵循`Container`协议：

```
struct Stack<Element>: Container{
    //Stack<Element>
    var items: [Element] = []
    mutating func push(_ item: Element) {
        items.append(item)
    }

    mutating func pop() -> Element {
        return items.removeLast()
    }
    //Container
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

### 扩展现有类型来指定关联类型

扩展一个已经存在的类型遵循一个协议，包括使用了关联类型的协议。

```
extension Array: Container {}
```

### 给关联类型添加约束

> 🌰给`Container `中的`Item`添加`Equatable`协议约束

```
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

### 在关联类型约束里使用协议

协议可以作为它自身的要求出现。

> 🌰&nbsp;&nbsp;有一个协议细化了`Container`协议，添加了一个`suffix(_:)`方法

```
protocol SuffixableContainer: Container {
    associatedtype Suffix: SuffixableContainer where Suffix.Item == Item
    func suffix(_ size: Int) -> Suffix
}
```

在`SuffixableContainer `协议里，`Suffix`是一个关联类型，拥有两个约束：

- 必须遵循`SuffixableContainer `协议
- 它的`Item`类型必须和容器里的`Item`类型相同 使用`where`分句

> `Stack`遵循`SuffixableContainer `协议

```
extension Stack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack {
        var result = Stack()
        for i in (count-size)..<count {
            result.append(self[i])
        }
        return result
    }
}

var stackOfInts = Stack<Int>()
stackOfInts.append(10)
stackOfInts.append(20)
stackOfInts.append(30)
let suffix = stackOfInts.suffix(2)
//suffix 包含 20 和 30
```

## 泛型Where语句

类型约束能够为泛型函数、下标、类型的类型参数定义一些强制要求。

常见使用场景

- 通过泛型`where`子句让关联类型遵循某个特定协议
- 保持`某个特定的类型参数` 和`关联类型`类型相同

### 语法

通过将`where`关键字紧跟在类型参数列表后面来定义`where`子句，`where`子句后面跟一个或多个针对关联类型的约束，以及一个或多个类型参数和关联类型间的相等关系。

可以在函数体或类型的大括号之前添加`where`子句

> 🌰&nbsp;&nbsp; 定义一个名为`allItemsMatch`的泛型函数，用来检查两个`Container`实例是否包含相同顺序的相同元素。如果所以元素能够匹配，返回`true`，否则返回`false`

被检查的两个`Container`可以不是相同类型的容器（虽然他们可以相同），但它们必须拥有相同类型的元素。这个要求同个一个类型约束以及一个`where`子句来表示：

```
func allItemsMatch<C1: Container, C2: Container>(_ someContainer: C1, _ anotherContainer: C2) -> Bool where C1.Item == C2.Item, C1.Item: Equatable {

    // 检查两个容器含有相同数量的元素
    if someContainer.count != anotherContainer.count {
        return false
    }

    // 检查每一对元素是否相等
    for i in 0..<someContainer.count {
        if someContainer[i] != anotherContainer[i] {
            return false
        }
    }

    // 所有元素都匹配，返回true
    return true
}
```

`C1`和`C2`是容器的两个占位类型参数，函数被调用时才能确定他们的具体类型。

这个函数的类型参数列表还定义了对这两个类型参数的要求：

1. `C1`必须符合`Container`协议（`C1: Container`）
2. `C2`必须符合`Container`协议（`C2: Container`）
3. `C1`的`Item`必须和`C2`的`Item`类型相同（`C1.Item == C2.Item`）
4. `C1`的`Item`必须符合`Equatable`协议（`C1.Item: Equatable`）

`1``2`要求定义在函数的类型形参列表里，`3``4`要求定义在了函数的泛型`where`分句中。

这些要求意味着：

- `someContainer`是一个`C1`类型的容器
- `anotherContainer`是一个`C2`类型的容器
- `someContainer`和`anotherContainer`包含相同类型的元素
- `someContainer`中的元素可以通过不等于操作符（`!=`）来检查它们是否相同

第三和第四个要求结合起来意味着`anotherContainer`中的元素也可以通过`!=`操作符来比较，因为他们和`someContainer`中的元素类型相同。

这些要求让`allItemsMatch(_:_:)`函数能够比较两个容器，即使他们的容器类型不同。

## 具有泛型Where子句的扩展

可以使用泛型`where`子句作为扩展的一部分

> 🌰&nbsp;&nbsp;扩展`Stack`结构体，添加`isTop(_:)`方法。

```
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

`isTop(_:)`方法首先检查这个栈是否为空，然后比较给定元素与栈顶元素。

如果不用泛型`where`子句，会有一个问题：在`isTop(_:)`里面使用了`==`运算符，但是`Stack`的定义没有要求它的元素是符合`Equatable`协议的，所以使用`==`运算符导致编译时错误。

使用泛型`where`子句可以为扩展添加新的条件约束，因此只有当栈中的元素符合`Equatable`协议时，扩展才会添加`isTop(_:)`方法。

> 🌰&nbsp;&nbsp;可以使用泛型`where`子句扩展`Container`协议，添加`startsWith(_:)`方法。

```
extension Container where Item: Equatable {
    func startsWith(_ item: Item) -> Bool {
        return count >= 1 && self[0] == item
    }
}
```

> 🏷调用👇🏻

```
if [9, 9, 9].startsWith(42) {
    print("Starts with 42.")
} else {
    print("Starts with something else.")
}
```

> 再来一个🌰

```
extension Container where Item == Double {
    func anerage() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += self[index]
        }
        return sum / Double(count)
    }
}

print([1234.0, 4321.0].anerage())
```

## 包含上下文关系的Where分句

当使用泛型是，可以为没有独立类型约束的声明添加`where`分句。

例如，可以使用`where`分句为泛型添加下标，或为扩展添加泛型约束。

> 🌰&nbsp;&nbsp;`Container`结构体是个泛型，通过`where`分句让新的方法声明其调用所需要满足的类型约束。

```
extension Container  {
    func anerage() -> Double where Item == Double {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }

    func endsWith(_ item: Item) -> Bool where Item: Equatable {
        return count >= 1 && self[count-1] == item
    }
}

let nums = [1234.0, 4321.0, 37]
print(nums.anerage())
print(nums.endsWith(37))
```

效果同下👇🏻

```
extension Container where Item == Double {
    func anerage() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
}

extension Container where Item: Equatable {
    func endsWith(_ item: Item) -> Bool {
        return count >= 1 && self[count-1] == item
    }
}
```

## 具有泛型Where子句的关联类型

可以在关联类型后面加上泛型`where`子句

> 🌰&nbsp;&nbsp;建立一个包含迭代器（`Iterator`）的容器

```
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }

    associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
    func makeInerator() -> Iterator
}
```

迭代器（`Iterator`）的泛型`where`子句要求：

- 无论迭代器是什么类型，迭代器中的元素类型，必须和容器项目的类型保持一致。
- `makeIterator()`则提供了容器的迭代器的访问接口。

一个协议继承了另一个协议，在协议声明的时候通过泛型`where`子句来添加一个约束到被继承协议的关联类型。

> 🌰&nbsp;&nbsp;声明`ComparableContainer`协议，要求所有的`Item`必须遵循`Comparable`协议。

```
protocol ComparableContainer: Container where Item: Comparable {}
```

## 泛型下标

下标可以是泛型，可以包含泛型`where`子句

可以在`subscript`后用尖括号来写占位符类型，还可以在下标代码块花括号前写`where`子句。

> 🌰&nbsp;&nbsp;👇🏻

```
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result: [Item] = []
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```

这个`Container`协议的扩展添加下标方法，接收一个索引的集合，返回每一个索引所在的值的数组。
这个泛型下标的约束如下：

- 在尖括号的泛型参数`Indices`，必须是符合标准库中的`Sequence`协议的类型。
- 下标使用的单一参数`Indices`必须是`Indices`的实例
- 泛型`where`子句要求`Sequence（Indices）`的迭代器，其所有的元素都是`Int`类型。这样就能确保在序列（`Sequence`）中的索引和容器（`Container`）里面的索引类型是一致的。

综合以上约束得到：传入到`indices`下标，是一个整型的序列。

以上内容来自[SwiftGG](https://gitbook.swiftgg.team/swift/swift-jiao-cheng/22_generics)
