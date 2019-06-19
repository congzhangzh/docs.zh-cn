---
title: 匿名的记录
description: 了解如何使用构造和使用一种语言功能，可帮助与操作数据的匿名记录。
ms.date: 06/12/2019
ms.openlocfilehash: e576210d4fb76ccfd09f8feb157ef4542aa94ccf
ms.sourcegitcommit: c4dfe37032c64a1fba2cc3d5947550d79f95e3b5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "67041801"
---
# <a name="anonymous-records"></a><span data-ttu-id="40b4a-103">匿名的记录</span><span class="sxs-lookup"><span data-stu-id="40b4a-103">Anonymous Records</span></span>

<span data-ttu-id="40b4a-104">匿名记录是无需使用之前声明的命名值的简单聚合。</span><span class="sxs-lookup"><span data-stu-id="40b4a-104">Anonymous records are simple aggregates of named values that don't need to be declared before use.</span></span> <span data-ttu-id="40b4a-105">可以将它们声明为结构或引用类型。</span><span class="sxs-lookup"><span data-stu-id="40b4a-105">You can declare them as either structs or reference types.</span></span> <span data-ttu-id="40b4a-106">它们是默认情况下引用类型。</span><span class="sxs-lookup"><span data-stu-id="40b4a-106">They're reference types by default.</span></span>

## <a name="syntax"></a><span data-ttu-id="40b4a-107">语法</span><span class="sxs-lookup"><span data-stu-id="40b4a-107">Syntax</span></span>

<span data-ttu-id="40b4a-108">下面的示例演示的匿名记录语法。</span><span class="sxs-lookup"><span data-stu-id="40b4a-108">The following examples demonstrate the anonymous record syntax.</span></span> <span data-ttu-id="40b4a-109">项分隔为`[item]`都是可选的。</span><span class="sxs-lookup"><span data-stu-id="40b4a-109">Items delimited as `[item]` are optional.</span></span>

```fsharp
// Construct an anonymous record
let value-name = [struct] {| Label1: Type1; Label2: Type2; ...|}

// Use an anonymous record as a type parameter
let value-name = Type-Name<[struct] {| Label1: Type1; Label2: Type2; ...|}>

// Define a parameter with an anonymous record as input
let function-name (arg-name: [struct] {| Label1: Type1; Label2: Type2; ...|}) ...
```

## <a name="basic-usage"></a><span data-ttu-id="40b4a-110">基本用法</span><span class="sxs-lookup"><span data-stu-id="40b4a-110">Basic usage</span></span>

<span data-ttu-id="40b4a-111">匿名记录最佳的想法作为F#记录不需要在实例化之前声明的类型。</span><span class="sxs-lookup"><span data-stu-id="40b4a-111">Anonymous records are best thought of as F# record types that don't need to be declared before instantiation.</span></span>

<span data-ttu-id="40b4a-112">例如，此处您可以使用函数进行交互的方式，生成的匿名记录：</span><span class="sxs-lookup"><span data-stu-id="40b4a-112">For example, here how you can interact with a function that produces an anonymous record:</span></span>

```fsharp

open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

<span data-ttu-id="40b4a-113">下面的示例与前一个扩展`printCircleStats`采用作为输入的匿名记录函数：</span><span class="sxs-lookup"><span data-stu-id="40b4a-113">The following example expands on the previous one with a `printCircleStats` function that takes an anonymous record as input:</span></span>

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let printCircleStats r (stats: {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

let r = 2.0
let stats = getCircleStats r
printCircleStats r stats
```

<span data-ttu-id="40b4a-114">调用`printCircleStats`与任何不具有相同的"形状"，如输入的类型将无法进行编译的匿名记录类型：</span><span class="sxs-lookup"><span data-stu-id="40b4a-114">Calling `printCircleStats` with any anonymous record type that doesn't have the same "shape" as the input type will fail to compile:</span></span>

```fsharp
printCircleStats r {| Diameter = 2.0; Area = 4.0; MyCircumference = 12.566371 |}
// Two anonymous record types have mismatched sets of field names
// '["Area"; "Circumference"; "Diameter"]' and '["Area"; "Diameter"; "MyCircumference"]'
```

## <a name="struct-anonymous-records"></a><span data-ttu-id="40b4a-115">结构的匿名记录</span><span class="sxs-lookup"><span data-stu-id="40b4a-115">Struct anonymous records</span></span>

<span data-ttu-id="40b4a-116">此外可以定义作为结构，它有可选的匿名记录`struct`关键字。</span><span class="sxs-lookup"><span data-stu-id="40b4a-116">Anonymous records can also be defined as struct with the optional `struct` keyword.</span></span> <span data-ttu-id="40b4a-117">下面的示例生成和使用结构的匿名记录与前一增加的内容：</span><span class="sxs-lookup"><span data-stu-id="40b4a-117">The following example augments the previous one by producing and consuming a struct anonymous record:</span></span>

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    // Note that the keyword comes before the '{| |}' brace pair
    struct {| Area = a; Circumference = c; Diameter = d |}

// the 'struct' keyword also comes before the '{| |}' brace pair when declaring the parameter type
let printCircleStats r (stats: struct {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

let r = 2.0
let stats = getCircleStats r
printCircleStats r stats
```

### <a name="structness-inference"></a><span data-ttu-id="40b4a-118">Structness 推理</span><span class="sxs-lookup"><span data-stu-id="40b4a-118">Structness inference</span></span>

<span data-ttu-id="40b4a-119">结构匿名记录还允许为"structness 推理"不需要指定`struct`调用站点上的关键字。</span><span class="sxs-lookup"><span data-stu-id="40b4a-119">Struct anonymous records also allow for "structness inference" where you do not need to specify the `struct` keyword at the call site.</span></span> <span data-ttu-id="40b4a-120">在此示例中，您 elide`struct`关键字调用时`printCircleStats`:</span><span class="sxs-lookup"><span data-stu-id="40b4a-120">In this example, you elide the `struct` keyword when calling `printCircleStats`:</span></span>

```fsharp

let printCircleStats r (stats: struct {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

printCircleStats r {| Area = 4.0; Circumference = 12.6; Diameter = 12.6 |}
```

<span data-ttu-id="40b4a-121">反向模式-指定`struct`时输入的类型不是结构的匿名记录-将无法进行编译。</span><span class="sxs-lookup"><span data-stu-id="40b4a-121">The reverse pattern - specifying `struct` when the input type is not a struct anonymous record - will fail to compile.</span></span>

## <a name="embedding-anonymous-records-within-other-types"></a><span data-ttu-id="40b4a-122">嵌入内其他类型的匿名记录</span><span class="sxs-lookup"><span data-stu-id="40b4a-122">Embedding anonymous records within other types</span></span>

<span data-ttu-id="40b4a-123">可用于声明[可区分联合](discriminated-unions.md)的记录。</span><span class="sxs-lookup"><span data-stu-id="40b4a-123">It's useful to declare [discriminated unions](discriminated-unions.md) whose cases are records.</span></span> <span data-ttu-id="40b4a-124">但是，在记录中的数据是否可区分联合类型相同，必须将所有类型都定义为相互递归的。</span><span class="sxs-lookup"><span data-stu-id="40b4a-124">But if the data in the records is the same type as the discriminated union, you must define all types as mutually recursive.</span></span> <span data-ttu-id="40b4a-125">使用匿名记录可避免此限制。</span><span class="sxs-lookup"><span data-stu-id="40b4a-125">Using anonymous records avoids this restriction.</span></span> <span data-ttu-id="40b4a-126">下面是一个示例通过其类型和函数匹配该模式：</span><span class="sxs-lookup"><span data-stu-id="40b4a-126">What follows is an example type and function that pattern matches over it:</span></span>

```fsharp
type FullName = { FirstName: string; LastName: string }

// Note that using a named for Manager and Executive would require mutually recursive definitions.
type Employee =
    | Engineer of FullName
    | Manager of {| Name: FullName; Reports: Employee list |}
    | Executive of {| Name: FullName; Reports: Employee list; Assistant: Employee |}

let getFirstName e =
    match e with
    | Engineer fullName -> fullName.FirstName
    | Manager m -> m.Name.FirstName
    | Executive ex -> ex.Name.FirstName
```

## <a name="copy-and-update-expressions"></a><span data-ttu-id="40b4a-127">复制和更新表达式</span><span class="sxs-lookup"><span data-stu-id="40b4a-127">Copy and update expressions</span></span>

<span data-ttu-id="40b4a-128">匿名记录支持与构造[复制和更新表达式](copy-and-update-record-expressions.md)。</span><span class="sxs-lookup"><span data-stu-id="40b4a-128">Anonymous records support construction with [copy and update expressions](copy-and-update-record-expressions.md).</span></span> <span data-ttu-id="40b4a-129">例如，下面是如何构造的新实例将复制的一个现有的匿名记录数据：</span><span class="sxs-lookup"><span data-stu-id="40b4a-129">For example, here's how you can construct a new instance of an anonymous record that copies an existing one's data:</span></span>

```fsharp
let data = {| X = 1; Y = 2 |}
let data' = {| data with Y = 3 |}
```

<span data-ttu-id="40b4a-130">但是，命名记录与匿名记录，可构造副本完全不同的窗体和更新表达式。</span><span class="sxs-lookup"><span data-stu-id="40b4a-130">However, unlike named records, anonymous records allow you to construct entirely different forms with copy and update expressions.</span></span> <span data-ttu-id="40b4a-131">下面的示例采用相同的匿名记录上一示例中，并将其扩展到新的匿名记录：</span><span class="sxs-lookup"><span data-stu-id="40b4a-131">The follow example takes the same anonymous record from the previous example and expands it into a new anonymous record:</span></span>

```fsharp
let data = {| X = 1; Y = 2 |}
let expandedData = {| data with Z = 3 |} // Gives {| X=1; Y=2; Z=3 |}
```

<span data-ttu-id="40b4a-132">还有可能构造命名记录的实例中的匿名记录：</span><span class="sxs-lookup"><span data-stu-id="40b4a-132">It is also possible to construct anonymous records from instances of named records:</span></span>

```fsharp
type R = { X: int }
let data = { X = 1 }
let data' = {| data with Y = 2 |} // Gives {| X=1; Y=2 |}
```

<span data-ttu-id="40b4a-133">此外可以将数据复制到和从引用和结构的匿名记录：</span><span class="sxs-lookup"><span data-stu-id="40b4a-133">You can also copy data to and from reference and struct anonymous records:</span></span>

```fsharp
// Copy data from a reference record into a struct anonymous record
type R1 = { X: int }
let r1 = { X = 1 }

let data1 = struct {| r1 with Y = 1 |}

// Copy data from a struct record into a reference anonymous record
[<Struct>]
type R2 = { X: int }
let r2 = { X = 1 }

let data2 = {| r1 with Y = 1 |}

// Copy the reference anonymous record data into a struct anonymous record
let data3 = struct {| data2 with Z = r2.X |}
```

## <a name="properties-of-anonymous-records"></a><span data-ttu-id="40b4a-134">匿名记录的属性</span><span class="sxs-lookup"><span data-stu-id="40b4a-134">Properties of anonymous records</span></span>

<span data-ttu-id="40b4a-135">匿名记录具有大量特征对完全了解如何使用它们至关重要的。</span><span class="sxs-lookup"><span data-stu-id="40b4a-135">Anonymous records have a number of characteristics that are essential to fully understanding how they can be used.</span></span>

### <a name="anonymous-records-are-nominal"></a><span data-ttu-id="40b4a-136">匿名记录是名义上</span><span class="sxs-lookup"><span data-stu-id="40b4a-136">Anonymous records are nominal</span></span>

<span data-ttu-id="40b4a-137">记录匿名[名义类型](https://en.wikipedia.org/wiki/Nominal_type_system)。</span><span class="sxs-lookup"><span data-stu-id="40b4a-137">Anonymous records are [nominal types](https://en.wikipedia.org/wiki/Nominal_type_system).</span></span> <span data-ttu-id="40b4a-138">它们是最佳的思想，如名为[记录](records.md)类型 （这也是名义上的） 不需要预先声明的。</span><span class="sxs-lookup"><span data-stu-id="40b4a-138">They are best thought as named [record](records.md) types (which are also nominal) that do not require an up-front declaration.</span></span>

<span data-ttu-id="40b4a-139">请考虑下面的示例使用两个匿名记录声明：</span><span class="sxs-lookup"><span data-stu-id="40b4a-139">Consider the following example with two anonymous record declarations:</span></span>

```fsharp
let x = {| X = 1 |}
let y = {| Y = 1 |}
```

<span data-ttu-id="40b4a-140">`x`和`y`值具有不同的类型和与另一个不兼容。</span><span class="sxs-lookup"><span data-stu-id="40b4a-140">The `x` and `y` values have different types and are not compatible with one another.</span></span> <span data-ttu-id="40b4a-141">它们不是可以相等和不可比较。</span><span class="sxs-lookup"><span data-stu-id="40b4a-141">They are not equatable and they are not comparable.</span></span> <span data-ttu-id="40b4a-142">为了说明这一点，请考虑命名的记录等效:</span><span class="sxs-lookup"><span data-stu-id="40b4a-142">To illustrate this, consider a named record equivalent:</span></span>

```fsharp
type X = { X: int }
type Y = { Y: int }

let x = { X = 1 }
let y = { Y = 1 }
```

<span data-ttu-id="40b4a-143">没有任何与它们命名的记录的等效项时，就类型等效性或比较的匿名记录有关本质上是不同的内容。</span><span class="sxs-lookup"><span data-stu-id="40b4a-143">There isn't anything inherently different about anonymous records when compared with their named record equivalents when concerning type equivalency or comparison.</span></span>

### <a name="anonymous-records-use-structural-equality-and-comparison"></a><span data-ttu-id="40b4a-144">匿名的记录，请使用结构相等和比较</span><span class="sxs-lookup"><span data-stu-id="40b4a-144">Anonymous records use structural equality and comparison</span></span>

<span data-ttu-id="40b4a-145">记录类型，如匿名记录是结构是可以相等和比较。</span><span class="sxs-lookup"><span data-stu-id="40b4a-145">Like record types, anonymous records are structurally equatable and comparable.</span></span> <span data-ttu-id="40b4a-146">这只是如果所有构成类型都支持相等性和比较，如与记录类型，则返回 true。</span><span class="sxs-lookup"><span data-stu-id="40b4a-146">This is only true if all constituent types support equality and comparison, like with record types.</span></span> <span data-ttu-id="40b4a-147">若要支持相等或比较，两个匿名记录必须具有相同的"形状"。</span><span class="sxs-lookup"><span data-stu-id="40b4a-147">To support equality or comparison, two anonymous records must have the same "shape".</span></span>

```fsharp
{| a = 1+1 |} = {| a = 2 |} // true
{| a = 1+1 |} > {| a = 1 |} // true

// error FS0001: Two anonymous record types have mismatched sets of field names '["a"]' and '["a"; "b"]'
{| a = 1 + 1 |} = {| a = 2;  b = 1|}
```

### <a name="anonymous-records-are-serializable"></a><span data-ttu-id="40b4a-148">匿名记录是可序列化</span><span class="sxs-lookup"><span data-stu-id="40b4a-148">Anonymous records are serializable</span></span>

<span data-ttu-id="40b4a-149">就像可以使用命名的记录一样，可以序列化匿名记录。</span><span class="sxs-lookup"><span data-stu-id="40b4a-149">You can serialize anonymous records just as you can with named records.</span></span> <span data-ttu-id="40b4a-150">下面是一个示例使用[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/):</span><span class="sxs-lookup"><span data-stu-id="40b4a-150">Here is an example using [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/):</span></span>

```fsharp
open Newtonsoft.Json

let phillip = {| name="Phillip"; age=28 |}
JsonConvert.SerializeObject(phillip)

let phillip = JsonConvert.DeserializeObject<{|name: string; age: int|}>(str)
printfn "Name: %s Age: %d" phillip.name phillip.age
```

<span data-ttu-id="40b4a-151">匿名记录可用于通过网络而无需定义用于序列化/反序列化类型预先域发送轻型的数据。</span><span class="sxs-lookup"><span data-stu-id="40b4a-151">Anonymous records are useful for sending lightweight data over a network without the need to define a domain for your serialized/deserialized types up front.</span></span>

### <a name="anonymous-records-interoperate-with-c-anonymous-types"></a><span data-ttu-id="40b4a-152">与的匿名记录进行互操作C#匿名类型</span><span class="sxs-lookup"><span data-stu-id="40b4a-152">Anonymous records interoperate with C# anonymous types</span></span>

<span data-ttu-id="40b4a-153">可以使用需要使用.NET API [ C#匿名类型](../../csharp/programming-guide/classes-and-structs/anonymous-types.md)。</span><span class="sxs-lookup"><span data-stu-id="40b4a-153">It is possible to use a .NET API that requires the use of [C# anonymous types](../../csharp/programming-guide/classes-and-structs/anonymous-types.md).</span></span> <span data-ttu-id="40b4a-154">C#匿名类型是普通与进行互操作使用的匿名记录。</span><span class="sxs-lookup"><span data-stu-id="40b4a-154">C# anonymous types are trivial to interoperate with by using anonymous records.</span></span> <span data-ttu-id="40b4a-155">下面的示例演示如何使用匿名记录，以调用[LINQ](../../csharp/programming-guide/concepts/linq/index.md)需要匿名类型的重载：</span><span class="sxs-lookup"><span data-stu-id="40b4a-155">The following example shows how to use anonymous records to call a [LINQ](../../csharp/programming-guide/concepts/linq/index.md) overload that requires an anonymous type:</span></span>

```fsharp
open System.Linq

let names = [ "Ana"; "Felipe"; "Emilia"]
let nameGrouping = names.Select(fun n -> {| Name = n; FirstLetter = n.[0] |})
for ng in nameGrouping do
    printfn "%s has first letter %c" ng.Name ng.FirstLetter
```

<span data-ttu-id="40b4a-156">有多种其他需要传递一个匿名类型中使用的 Api 在.NET 中使用。</span><span class="sxs-lookup"><span data-stu-id="40b4a-156">There are a multitude of other APIs used throughout .NET that require the use of passing in an anonymous type.</span></span> <span data-ttu-id="40b4a-157">匿名记录是用于使用这些工具。</span><span class="sxs-lookup"><span data-stu-id="40b4a-157">Anonymous records are your tool for working with them.</span></span>

## <a name="limitations"></a><span data-ttu-id="40b4a-158">限制</span><span class="sxs-lookup"><span data-stu-id="40b4a-158">Limitations</span></span>

<span data-ttu-id="40b4a-159">匿名记录其使用情况中具有一些限制。</span><span class="sxs-lookup"><span data-stu-id="40b4a-159">Anonymous records have some restrictions in their usage.</span></span> <span data-ttu-id="40b4a-160">某些可归结于它们的设计，但有些则是易于更改。</span><span class="sxs-lookup"><span data-stu-id="40b4a-160">Some are inherent to their design, but others are amenable to change.</span></span>

### <a name="limitations-with-pattern-matching"></a><span data-ttu-id="40b4a-161">模式匹配的限制</span><span class="sxs-lookup"><span data-stu-id="40b4a-161">Limitations with pattern matching</span></span>

<span data-ttu-id="40b4a-162">模式匹配，命名记录与不支持匿名记录。</span><span class="sxs-lookup"><span data-stu-id="40b4a-162">Anonymous records do not support pattern matching, unlike named records.</span></span> <span data-ttu-id="40b4a-163">有三个原因：</span><span class="sxs-lookup"><span data-stu-id="40b4a-163">There are three reasons:</span></span>

1. <span data-ttu-id="40b4a-164">一种模式必须为每个字段的匿名记录，与名为的记录类型不同的帐户。</span><span class="sxs-lookup"><span data-stu-id="40b4a-164">A pattern would have to account for every field of an anonymous record, unlike named record types.</span></span> <span data-ttu-id="40b4a-165">这是因为匿名记录不支持结构子类型化的 – 名义类型。</span><span class="sxs-lookup"><span data-stu-id="40b4a-165">This is because anonymous records do not support structural subtyping – they are nominal types.</span></span>
2. <span data-ttu-id="40b4a-166">(1)，因此没有在模式匹配表达式中，有其他模式能为每个非重复模式的含义不同的匿名记录类型。</span><span class="sxs-lookup"><span data-stu-id="40b4a-166">Because of (1), there is no ability to have additional patterns in a pattern match expression, as each distinct pattern would imply a different anonymous record type.</span></span>
3. <span data-ttu-id="40b4a-167">由于 (3)，任何匿名记录模式是使用"dot"表示法相比更详细。</span><span class="sxs-lookup"><span data-stu-id="40b4a-167">Because of (3), any anonymous record pattern would be more verbose than the use of “dot” notation.</span></span>

<span data-ttu-id="40b4a-168">没有到打开语言建议[允许在模式匹配有限上下文](https://github.com/fsharp/fslang-suggestions/issues/713)。</span><span class="sxs-lookup"><span data-stu-id="40b4a-168">There is an open language suggestion to [allow pattern matching in limited contexts](https://github.com/fsharp/fslang-suggestions/issues/713).</span></span>

### <a name="limitations-with-mutability"></a><span data-ttu-id="40b4a-169">可变性的限制</span><span class="sxs-lookup"><span data-stu-id="40b4a-169">Limitations with mutability</span></span>

<span data-ttu-id="40b4a-170">它不是当前无法定义使用的匿名记录`mutable`数据。</span><span class="sxs-lookup"><span data-stu-id="40b4a-170">It is not currently possible to define an anonymous record with `mutable` data.</span></span> <span data-ttu-id="40b4a-171">没有[打开语言建议](https://github.com/fsharp/fslang-suggestions/issues/732)允许可变的数据。</span><span class="sxs-lookup"><span data-stu-id="40b4a-171">There is an [open language suggestion](https://github.com/fsharp/fslang-suggestions/issues/732) to allow mutable data.</span></span>

### <a name="limitations-with-struct-anonymous-records"></a><span data-ttu-id="40b4a-172">结构的匿名记录的限制</span><span class="sxs-lookup"><span data-stu-id="40b4a-172">Limitations with struct anonymous records</span></span>

<span data-ttu-id="40b4a-173">不能声明为匿名记录的结构`IsByRefLike`或`IsReadOnly`。</span><span class="sxs-lookup"><span data-stu-id="40b4a-173">It is not possible to declare struct anonymous records as `IsByRefLike` or `IsReadOnly`.</span></span> <span data-ttu-id="40b4a-174">没有[打开语言建议](https://github.com/fsharp/fslang-suggestions/issues/712)到有关`IsByRefLike`和`IsReadOnly`匿名记录。</span><span class="sxs-lookup"><span data-stu-id="40b4a-174">There is an [open language suggestion](https://github.com/fsharp/fslang-suggestions/issues/712) to for `IsByRefLike` and `IsReadOnly` anonymous records.</span></span>