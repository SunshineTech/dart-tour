# Dart 库教程

翻译自 Dart 官网，[查看原文](https://www.dartlang.org/guides/libraries/library-tour)。

当前版本：2.0.0 (stable)

完成度：20%

该教程为你展示了下面这些库的主要特性，这些库被包含在所有的 Dart 平台中。

[dart:core](#)

内置类型、集合和其他核心功能。该库在每一个 Dart 程序中被自动引入。

[dart:async](#)

支持异步编程，包括 Future 和 Stream 等类。

[dart:math](#)

数学常数和函数，外加随机生成器。

[dart:convert](#)

用于在不同的数据表示之间进行转换的编码器和解码器，包括 JSON 和 UTF-8。

该篇文章只是一个概览；它只覆盖 dart:* 中的一小部分库而且不包含第三方库。平台相关的 dart:io 和 dart:html 库分别包含在 [dart:io 教程](#) 和 [dart:html 教程](#) 中。

其他查找库信息的地方是 [pub.dartlang.org](#) 和 [Dart web 开发者库指引](#)。 你可以在 [Dart API 参考](#) 中找到所有的 dart:* 库；而如果你在使用 Flutter，则是 [Flutter API 参考](#)。

> DartPad 小提示：你可以通过将其拷贝到 [DartPad](#) 来尝试本页的代码。

## dart:core —— 数值、集合、字符串和其他

库 dart:core（[API 参考](#)）提供了一组小而重要的内置功能。这个库在所有的 Dart 程序中被自动引入。

### 打印到控制台

顶级方法 **print()** 接受单个参数（任意类型）并将这个对象的字符串值（通过 **toString()** 返回）显示在控制台上。

```dart
print(anObject);
print('I drink $tea.');
```

要了解更多关于基本的字符串和 **toString()** 的信息，请参阅语言教程中的 [String](#) 章节。

### 数值

库 dart:core 定义了 num、int 和 double 类，它们拥有操作数值的一些基本方法。

你可以通过 int 和 double 的 **parse()** 方法将字符串分别转换为整数或浮点数：

```dart
assert(int.parse('42') == 42);
assert(int.parse('0x42') == 66);
assert(double.parse('0.50') == 0.5);
```

或者使用 num 的 parse() 方法，它在可能的情况下创建一个整数，否则创建一个浮点数：

```dart
assert(num.parse('42') is int);
assert(num.parse('0x42') is int);
assert(num.parse('0.50') is double);
```

要指定一个整数的进制，添加 **radix** 参数：

```dart
assert(int.parse('42', radix: 16) == 66);
```

使用 **toString()** 方法将一个 int 或者 double 装换成字符串。要指定小数点右边的位数，请使用 [toStringAsFixed()](#)。要指定字符串中的有效位数，请使用 [toStringAsPrecision()](#) ：

```dart
// 转换 int 为字符串
assert(42.toString() == '42');

// 转换 double 为字符串
assert(123.456.toString() == '123.456');

// 指定小数点后的位数
assert(123.456.toStringAsFixed(2) == '123.46');

// 指定数值的有效位数
assert(123.456.toStringAsPrecision(2) == '1.2e+2');
assert(double.parse('1.2e+2') == 120.0);
```

要了解更多信息，请参阅 [int](#)、[double](#) 和 [num](#) 的 API 文档。也请参阅 [dart:math](#) 章节。

### 字符串和正则表达式

字符串在 Dart 中表示 UTF-16 编码单元组成的一个不可变序列。语言教程中有关于 [字符串](#) 的更详细信息。你可以使用正则表达式（RegExp 对象）在字符串中查找和替换部分字符串。

字符串类定义了诸如 **split()**、**contains()**、**startsWith()**、**endsWith** 这样的以及其他更多的方法。

#### 在字符串中查找

你可以在字符串中查找一个指定模式的位置，或者检查字符串是否以其开头或结尾。比如：

```dart
// 检查一个字符串是否包含另一个字符串
assert('Never odd or even'.contains('odd'));

// 检查一个字符串是否开始于另一个字符串
assert('Never odd or even'.startsWith('Never'));

// 检查一个字符串是否结束于另一个字符串
assert('Never odd or even'.endsWith('even'));

// 查找一个字符串在另一个字符串中的位置
assert('Never odd or even'.indexOf('odd') == 6);
```

#### 从字符串中提取数据

你可以从一个字符串中获取独立的字符分别作为字符串或者整数。准确来说，你得到的其实是独立的 UTF-16 编码单元；像高音音符这样的高位字符 (‘\u{1D11E}’) 为在一块的两个编码单元。

你也可以提取一个子字符串或者拆分一个字符串为一个子字符串列表：

```dart
// 抓取一个子字符串
assert('Never odd or even'.substring(6, 9) == 'odd');

// 使用一个字符串模式拆分字符串
var parts = 'structured web apps'.split(' ');
assert(parts.length == 3);
assert(parts[0] == 'structured');

// 使用索引获取 UTF-16 编码单元（作为字符串）
assert('Never odd or even'[0] == 'N');

// 使用空字符串作为 split() 的参数来获得
// 所有字符（作为字符串）的列表；便于迭代
for (var char in 'hello'.split('')) {
  print(char);
}

// 获取字符串中所有的 UTF-16 编码单元
var codeUnitList =
    'Never odd or even'.codeUnits.toList();
assert(codeUnitList[0] == 78);
```

#### 大小写转换

你可以很容易地转换一个字符串为它的大写或小写形式：

```dart
// 转换为大写
assert('structured web apps'.toUpperCase() ==
    'STRUCTURED WEB APPS');

// 转换为小写
assert('STRUCTURED WEB APPS'.toLowerCase() ==dart
    'structured web apps');
```

> 说明：这些方法并不适用于所有语言。比如，土耳其字母的 *I* 转换就是错误的。

#### 修剪和空字符串

使用 **trim()** 移除前面和后面的所有空白。要检查一个字符串是否是空的（长度为0），使用 **isEmpty**。

```dart
// 修剪字符串
assert('  hello  '.trim() == 'hello');

// 检查字符串是否是空的
assert(''.isEmpty);

// 只有空白符的字符串不是空的
assert('  '.isNotEmpty);
```

#### 替换字符串的部分内容

字符串是不可变的对象，意味着你可以创建它们但是不可以修改它们。如果你仔细观察 [String API 索引](#)，你会发现没有一个方法真正改变了一个字符串的状态。比如，方法 **replaceAll()** 不会修改原来的字符串：

```dart
var greetingTemplate = 'Hello, NAME!';
var greeting =
    greetingTemplate.replaceAll(RegExp('NAME'), 'Bob');

// greetingTemplate 没有改变
assert(greeting != greetingTemplate);
```

#### 构建一个字符

要程序化的创建一个字符串，你可以使用 StringBuffer。一个 StringBuffer 不生成新的字符串除非 **toString()** 方法被调用。方法 **writeAll()** 有一个可选的第二个参数让你可以指定一个分隔符——在这里是一个空格。

```dart
var sb = StringBuffer();
sb
  ..write('Use a StringBuffer for ')
  ..writeAll(['efficient', 'string', 'creation'], ' ')
  ..write('.');

var fullString = sb.toString();

assert(fullString ==
    'Use a StringBuffer for efficient string creation.');
```

#### 正则表达式

RegExp 类提供了与 JavaScript 中正则表达式相同的功能。使用正则表达式来高效地查找和匹配字符串中的模式。

```dart
// 这是一个表示一个或多个数字的正则表达式
var numbers = RegExp(r'\d+');

var allCharacters = 'llamas live fifteen to twenty years';
var someDigits = 'llamas live 15 to 20 years';

// contains() 可以使用正则表达式
assert(!allCharacters.contains(numbers));
assert(someDigits.contains(numbers));

// 替换所有匹配的部分为另一个字符串
var exedOut = someDigits.replaceAll(numbers, 'XX');
assert(exedOut == 'llamas live XX to XX years');
```

你也可以直接使用 RegExp 类。Match 类提供了对于正则表达式匹配结果的访问方法。

```dart
var numbers = RegExp(r'\d+');
var someDigits = 'llamas live 15 to 20 years';

// 检查正则表达式在字符串中是否有一个匹配
assert(numbers.hasMatch(someDigits));

// 循环遍历所有匹配结果
for (var match in numbers.allMatches(someDigits)) {
  print(match.group(0)); // 15，然后是 20
}
```

#### 更多信息

参考 [String API 索引](#) 来获取方法的完整列表。另请参阅 [StringBuffer](#)、[Pattern](#)、[RegExp](#) 和 [Match](#) 的 API 索引。

### 集合

Dart 附带了核心的集合 API，包含了与列表、Set 和 Map 相关的类。

#### 列表

就如语言教程所展示的，你可以使用字面量来创建和初始化 [列表](#)。除此以外，你还可以使用 List 的构造函数。List 类同时定义了一些往列表添加和从列表移除项目的方法。

```dart
// 使用一个集合的构造函数
var vegetables = List();

// 或者简单地使用字面量
var fruits = ['apples', 'oranges'];

// 往列表添加
fruits.add('kiwis');

// 往列表添加多个项目
fruits.addAll(['grapes', 'bananas']);

// 获取列表的长度
assert(fruits.length == 5);

// 移除单个项目
var appleIndex = fruits.indexOf('apples');
fruits.removeAt(appleIndex);
assert(fruits.length == 4);

// 从列表中移除所有项目
fruits.clear();
assert(fruits.length == 0);
```

使用 **indexOf()** 来获取一个对象在列表中的索引：

```dart
var fruits = ['apples', 'oranges'];

// 使用索引访问项目
assert(fruits[0] == 'apples');

// 在列表中查找一个项目
assert(fruits.indexOf('apples') == 0);
```

使用 **sort()** 方法对列表进行排序。你可以提供一个排序函数用于比较两个对象。该排序函数必须返回 < 0 的值来表示“更小”、0 表示相等、> 0 的值表示“更大”。下面的例子使用 **compareTo()**，该方法定义在 [Compareable](#) 类中并且被字符串所实现。

```dart
var fruits = ['bananas', 'apples', 'oranges'];

// 对列表进行排序
fruits.sort((a, b) => a.compareTo(b));
assert(fruits[0] == 'apples');
```

列表是参数化类型的，所以你可以指定一个列表应该包含的类型：

```dart
// 这个列表应该只包含字符串
var fruits = List<String>();

fruits.add('apples');
var fruit = fruits[0];
assert(fruit is String);
```

```dart
fruits.add(5); // 错误：'int' 不可以赋值给 'String'
```

参见 [List API 索引](#) 获取列表的完整方法列表。

#### Set

Dart 中的 set 是独特元素的无序集合。因为 set 是无序的，你不可以使用索引（为值）获取项目。

```dart
var ingredients = Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);
assert(ingredients.length == 3);

// 添加重复项目是无效的
ingredients.add('gold');
assert(ingredients.length == 3);

// 从 set 中移除项目
ingredients.remove('gold');
assert(ingredients.length == 2);
```

使用 **contains()** 和 **containsAll()** 来检查一个或多个对象是否在一个 set 中：

```dart
var ingredients = Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// 检查一个项目是否在 set 中
assert(ingredients.contains('titanium'));

// 检查所有项目是否都在 set 中
assert(ingredients.containsAll(['titanium', 'xenon']));
```

交集是一个所有项目同时在两个其他 set 中的 set。

```dart
var ingredients = Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// 创建两个 set 的交集
var nobleGases = Set.from(['xenon', 'argon']);
var intersection = ingredients.intersection(nobleGases);
assert(intersection.length == 1);
assert(intersection.contains('xenon'));
```

参见 [Set API 索引](#) 获取 set 的完整方法列表。

#### Maps

一个 map，通常被称作“字典”或者“映射”，是键值对的无序集合。Map 关联一个键到一些值为了便于取回。不像 JavaScript，Dart 对象不是 map。

你可以使用字面量声明一个 map，或者使用一个传统的构造函数：

```dart
// Map 经常使用字符串作为键
var hawaiianBeaches = {
  'Oahu': ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai': ['Hanalei', 'Poipu']
};

// Map 可以由一个构造函数构造
var searchTerms = Map();

// Map 是参数类型的，你可以指定
// 哪些类型可以作为键和值
var nobleGases = Map<int, String>();
```

使用中括号语法来添加、获取和设置 map 的项目。使用 **remove()** 方法从 map 中移除一个键和它对应的值。

```dart
var nobleGases = {54: 'xenon'};

// 使用一个键取回一个值
assert(nobleGases[54] == 'xenon');

// 检查 map 是否包含一个键
assert(nobleGases.containsKey(54));

// 移除一个键和它对应的值
nobleGases.remove(54);
assert(!nobleGases.containsKey(54));
```

你可以从一个 map 中取回所有的值或所有的键：

```dart
var hawaiianBeaches = {
  'Oahu': ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai': ['Hanalei', 'Poipu']
};

// 获取所有的键作为一个无序集合
// （一个 Iterable）
var keys = hawaiianBeaches.keys;

assert(keys.length == 3);
assert(Set.from(keys).contains('Oahu'));

// 获取所有的值作为一个无序集合
// (一个列表组成的 Iterable)
var values = hawaiianBeaches.values;
assert(values.length == 3);
assert(values.any((v) => v.contains('Waikiki')));
```

要检查一个 map 是否包含一个键，使用 **containsKey()**。因为 map 的值可以为空，你不能简单地依赖获取并判断值是否为空来决定一个键是否存在。

```dart
var hawaiianBeaches = {
  'Oahu': ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai': ['Hanalei', 'Poipu']
};

assert(hawaiianBeaches.containsKey('Oahu'));
assert(!hawaiianBeaches.containsKey('Florida'));
```

当你仅想当一个键不存在于一个 map 中时才指定一个值到该键，使用 **putIfAbsent()** 方法。你必须指定一个方法返回该值。

```dart
var teamAssignments = {};
teamAssignments.putIfAbsent(
    'Catcher', () => pickToughestKid());
assert(teamAssignments['Catcher'] != null);
```

参见 [Map API 索引](#) 获取 map 的完整方法列表。

#### 通用集合方法

列表、Set 和 Map 共享了一些在许多集合类型中都可找到的通用方法。一些通用方法定义在 Iterable 类中，该类被列表和 Set 所实现。

> 说明：尽管 Map 没有实现 Iterable，你仍可以使用 **keys** 和 **values** 属性从 map 中获取 Iterable。

使用 **isEmpty** 或 **isNotEmpty** 来检查一个列表、set 或 map 是否拥有项目：

```dart
var coffees = [];
var teas = ['green', 'black', 'chamomile', 'earl grey'];
assert(coffees.isEmpty);
assert(teas.isNotEmpty);
```

要应用一个函数到一个列表、set 或 map 的每一个元素上，你可以使用 **forEach()**：

```dart
var teas = ['green', 'black', 'chamomile', 'earl grey'];

teas.forEach((tea) => print('I drink $tea'));
```

当你在一个 map 上调用 **forEach()**  时，你的函数必须接受两个参数（键和值）。

```dart
hawaiianBeaches.forEach((k, v) {
  print('I want to visit $k and swim at $v');
  // I want to visit Oahu and swim at
  // [Waikiki, Kailua, Waimanalo], 等等
});
```

Iterable 提供了 **map()** 方法，它以单个对象给你所有的结果：

```dart
var teas = ['green', 'black', 'chamomile', 'earl grey'];

var loudTeas = teas.map((tea) => tea.toUpperCase());
loudTeas.forEach(print);
```

> 说明：由 **map()** 返回的对象是一个“懒求值”的 Iterable：直到你请求返回的对象时你的函数才会被调用。

要强制你的方法在每一个项目上被立即调用，使用 **map().toList()** 或 **map().toSet()**：

```dart
var loudTeas =
    teas.map((tea) => tea.toUpperCase()).toList();
```

使用 Iterable 的 **where()** 方法来获取所有符合某个条件的元素。使用 Iterable 的 **any()** 方法和 **every()** 方法来检查是否有一些元素或所有元素符合某个条件。

```dart
var teas = ['green', 'black', 'chamomile', 'earl grey'];

// Chamomile（黄春菊）是不含咖啡因的
bool isDecaffeinated(String teaName) =>
    teaName == 'chamomile';

// 使用 where() 来查找只有
// 从提供的函数返回 true 的项目
var decaffeinatedTeas =
    teas.where((tea) => isDecaffeinated(tea));
// 或者 teas.where(isDecaffeinated)

// 使用 any() 来检查是否至少有一个
// 集合中的项目满足条件
assert(teas.any(isDecaffeinated));

// 使用 every() 来检查是否集合中
// 所有的项目都满足条件
assert(!teas.every(isDecaffeinated));
```

要获取完整的方法列表，请参见 [Iterable API 索引](#)，以及 [列表](#)、[Set](#) 和 [Map](#) 中的这些方法。

### URI

[Uri 类](#) 提供编码和解码字符串用来作为 URI（就是你可能知道的 URL）的函数。这些函数处理 URI 中的特殊字符，比如 **&** 和 **=**。Uri 类也包含解析和获取 URI 的各个部件的方法——host、port、schema 等等。

#### 编码和解码完全限定的 URI

要编码和解码 URI 中除去那些有特殊意义（比如 **/**、**:**、**&**、**#**）之外的字符，使用 **encodeFull()** 和 **decodeFull()** 方法。这些方法对于编码和解码完全限定的 URI 是非常好的，它们会留下 URI 中完整的特殊字符。

```dart
var uri = 'http://example.org/api?foo=some message';

var encoded = Uri.encodeFull(uri);
assert(encoded ==
    'http://example.org/api?foo=some%20message');

var decoded = Uri.decodeFull(encoded);
assert(uri == decoded);
```

注意只有 **some** 和 **message** 之间的空格被编码了。

#### 编码和解码 URI 部件

要编码和解码一个字符串在 URI 中有特殊意义的所有字符，包括（但不限于）**/**、**&** 和 **:**，使用 **encodeComponent()** 和 **decodeComponent** 方法。

```dart
var uri = 'http://example.org/api?foo=some message';

var encoded = Uri.encodeComponent(uri);
assert(encoded ==
    'http%3A%2F%2Fexample.org%2Fapi%3Ffoo%3Dsome%20message');

var decoded = Uri.decodeComponent(encoded);
assert(uri == decoded);
```

#### 解析 URI

如果你有一个 Uri 对象或者一个 URI 字符串，你可以使用 Uri 的属性获取它的部件比如 **path**。要从一个字符串创建一个 Uri，使用静态方法 **parse()**：

```dart
var uri =
    Uri.parse('http://example.org:8080/foo/bar#frag');

assert(uri.scheme == 'http');
assert(uri.host == 'example.org');
assert(uri.path == '/foo/bar');
assert(uri.fragment == 'frag');
assert(uri.origin == 'http://example.org:8080');
```

参见 [Uri API 索引](#) 了解所有你可以获取的部件。

#### 构建 URI

你可以使用 **Uri()** 构造函数从独立的部件构建 URL：

```dart
var uri = Uri(
    scheme: 'http',
    host: 'example.org',
    path: '/foo/bar',
    fragment: 'frag');
assert(
    uri.toString() == 'http://example.org/foo/bar#frag');
```

### 日期和时间

一个 DateTime 对象代表一个时间点。时区可以是 UTC 或者本地时区。

你可以使用多个构造函数创建 DateTime 对象：

```dart
// 获取现在的日期和时间
var now = DateTime.now();

// 使用本地时区创建一个新的 DateTime 对象
var y2k = DateTime(2000); // 2000年1月1日

// 指定月和日
y2k = DateTime(2000, 1, 2); // 2000年1月2日

// 指定日期为 UTC 时间
y2k = DateTime.utc(2000); // 2000.01.01, UTC

// 使用 Unix 时间戳指定一个日期和时间
y2k = DateTime.fromMillisecondsSinceEpoch(946684800000,
    isUtc: true);

// 解析一个 ISO 8601 格式的日期
y2k = DateTime.parse('2000-01-01T00:00:00Z');
```

一个日期的 **millisecondsSinceEpoch** 属性返回距“Unix 纪元”——UTC 时间1970年1月1日 ——的毫秒数：

```dart
// 2000.01.01, UTC
var y2k = DateTime.utc(2000);
assert(y2k.millisecondsSinceEpoch == 946684800000);

// 1970.01.01, UTC
var unixEpoch = DateTime.utc(1970);
assert(unixEpoch.millisecondsSinceEpoch == 0);
```

使用 Duration 类来计算两个日期的差异，并向前或向后偏移日期：

```dart
var y2k = DateTime.utc(2000);

// 增加一年
var y2001 = y2k.add(Duration(days: 366));
assert(y2001.year == 2001);

// 减去30天
var december2000 = y2001.subtract(Duration(days: 30));
assert(december2000.year == 2000);
assert(december2000.month == 12);

// 计算两个日期之间的差异
// 返回一个 Duration 对象
var duration = y2001.difference(y2k);
assert(duration.inDays == 366); // y2k 是一个闰年
```

> 警告：使用 Duration 来偏移一个 DateTime 的天数可能会出问题，因为时间转换（比如夏令时）。如果你一定要偏移天数，可以使用 UTC 日期。

要获取完整的方法列表，请参见 [DateTime](#) 和 [Duration](#) 的 API 索引。

### 实用工具类

核心库包含各种各样的实用工具类，可以用来排序、映射值和迭代。

#### 比较对象

实现 [Comparable](#) 接口来表明一个对象可以与其他对象进行比较，通常用来排序。方法 **compareTo()** 返回 < 0 的值来表示“更小”、0 表示相等、> 0 的值表示“更大”。

```dart
class Line implements Comparable<Line> {
  final int length;
  const Line(this.length);

  @override
  int compareTo(Line other) => length - other.length;
}

void main() {
  var short = const Line(1);
  var long = const Line(100);
  assert(short.compareTo(long) < 0);
}
```

#### 实现 map 的键

Dart 中的每一个对象都提供了一个整数类型的 hash 码，因此可以用来作为一个 map 的键。然而，你可以重载 **hashCode** 的 getter 来生成一个自定义的 hash 码。如果你这样做，你必须同时重载 **==** 操作符。相等的对象（使用 ==）必须拥有相同的 hash 码。一个 hash 码不一定要是唯一的，但是应该合理地分发。

```dart
class Person {
  final String firstName, lastName;

  Person(this.firstName, this.lastName);

  // 使用 Effective Java 第11章中的策略重载 hash 码
  @override
  int get hashCode {
    int result = 17;
    result = 37 * result + firstName.hashCode;
    result = 37 * result + lastName.hashCode;
    return result;
  }

  // 如果你重载 hash 码，你通常要重载 == 运算符
  @override
  bool operator ==(dynamic other) {
    if (other is! Person) return false;
    Person person = other;
    return (person.firstName == firstName &&
        person.lastName == lastName);
  }
}

void main() {
  var p1 = Person('Bob', 'Smith');
  var p2 = Person('Bob', 'Smith');
  var p3 = 'not a person';
  assert(p1.hashCode == p2.hashCode);
  assert(p1 == p2);
  assert(p1 != p3);
}
```

#### 迭代

[Iterable 类](#) 和 [Iterator 类](#) 提供了 for-in 循环。当你想要创建一个提供 for-in 循环的迭代器类时，继承（如果可以）或者实现 Iterable。实现 Iterator 来定义真正的迭代能力。

```dart
class Process {
  // 代表一个处理...
}

class ProcessIterator implements Iterator<Process> {
  @override
  Process get current => ...
  @override
  bool moveNext() => ...
}

// 一个虚拟的类，它继承了 Iterable 的子类，使你可以
// 从所有的处理中迭代
class Processes extends IterableBase<Process> {
  @override
  final Iterator<Process> iterator = ProcessIterator();
}

void main() {
  // Iterable 对象可以使用 for-in
  for (var process in Processes()) {
    // 对 process 做一些处理
  }
}
```

### 异常

Dart 核心库定义了许多通用的异常和错误。异常是指你计划要面对和处理的情况。错误是指你不期望或计划的情况。

几个最常见的错误为：

[NoSuchMethodError](#)

当接收对象（可能是 null）未实现一个方法时抛出。

[ArgumentError](#)

当方法遇到一个不期望的参数时可以抛出。

抛出一个应用级别的异常是表明错误发生了的一个常用的方法。你可以通过实现 Exception 接口自定义一个异常:

```dart
class FooException implements Exception {
  final String msg;

  const FooException([this.msg]);

  @override
  String toString() => msg ?? 'FooException';
}
```

要了解更多信息，请参阅语言教程中的 [异常](#) 和 [Exception API 索引](#)。