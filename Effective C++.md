[https://blog.csdn.net/fatalerror99/category_163286.html?spm=1001.2014.3001.5482](https://blog.csdn.net/fatalerror99/category_163286.html?spm=1001.2014.3001.5482)



#### <font style="background-color:#74B602;">Item 1: 将 C++ 视为 federation of languages（语言联合体）</font>
<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">C++是由四个部分组成的：</font>

1. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">C子系统</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：这部分和C语言很像，涉及基本的语法、数据类型、数组和指针等。在这个部分，编程规则比较简单，但功能有限。</font>
2. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">面向对象层</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：这部分引入了类、继承、多态等面向对象的特性。它使得代码更易于管理和扩展，但规则也变得更复杂一些。</font>
3. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">模板元编程层</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：模板允许我们编写通用的代码，可以在编译时进行类型检查和优化。这部分比较高级，适合需要高性能的场景。</font>
4. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">STL生态层</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：STL提供了一套标准的数据结构和算法，比如向量、列表和排序算法。使用STL可以大大提高开发效率，但也有一些特定的使用规则。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在编程时，我们需要根据具体情况选择使用哪个部分。例如，处理性能敏感的代码时，可能会用到C子系统；设计复杂的系统时，可能会用到面向对象层和模板元编程层。</font>

#### <font style="color:rgb(51, 51, 51);">Item 2: 用 consts, enums 和 inlines 取代 #defines</font>
<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">问题的引入</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在C++编程中，</font>`<font style="background-color:rgb(252, 252, 252);">#define</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">预处理器指令常被用于定义常量和实现类似函数的宏。然而，这种做法存在多个问题：</font>

1. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">类型安全性缺失</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：</font>`<font style="background-color:rgb(252, 252, 252);">#define</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">仅仅是文本替换，编译器无法对其进行类型检查，容易导致类型错误。</font>
2. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">作用域控制困难</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：宏定义在整个编译单元中有效，可能导致命名冲突和意外行为。</font>
3. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">调试困难</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：宏在预处理阶段被替换，调试器难以追踪宏的执行过程，错误信息中常出现替换后的值，增加了调试难度。</font>
4. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">代码膨胀</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：宏展开可能导致代码重复，增加目标文件大小，影响程序性能。</font>**<font style="background-color:rgb(252, 252, 252);"></font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">为了解决上述问题，文档提出了以下替代方案：</font>

1. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">使用常量（const）或枚举（enum）替代宏定义</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：</font>
    - **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">常量</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：使用</font>`<font style="background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">关键字定义常量，如</font>`<font style="background-color:rgb(252, 252, 252);">const double AspectRatio = 1.653;</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。常量被编译器识别并加入符号表，便于调试，且不会导致代码膨胀。</font>
    - **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">枚举</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：对于整型常量，可以使用枚举类型，如</font>`<font style="background-color:rgb(252, 252, 252);">enum { NumTurns = 5 };</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。枚举类型具有类型安全性和作用域控制，且不会引入额外的内存开销。</font>**<font style="background-color:rgb(252, 252, 252);">1</font>****<font style="background-color:rgb(252, 252, 252);">6</font>****<font style="background-color:rgb(252, 252, 252);">7</font>**
2. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">使用内联函数（inline function）替代函数式宏</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：</font>
    - **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">内联函数</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：使用</font>`<font style="background-color:rgb(252, 252, 252);">inline</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">关键字定义函数，如</font>`<font style="background-color:rgb(252, 252, 252);">inline int max(int a, int b) { return a > b ? a : b; }</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。内联函数在编译时展开，避免了函数调用的开销，同时具有类型安全和作用域控制。</font>**<font style="background-color:rgb(252, 252, 252);">1</font>****<font style="background-color:rgb(252, 252, 252);">6</font>****<font style="background-color:rgb(252, 252, 252);">8</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:#FBDE28;">实施步骤</font>

1. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">识别宏定义的使用场景</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：检查代码中所有使用</font>`<font style="background-color:rgb(252, 252, 252);">#define</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">的地方，确定是否可以替换为常量或内联函数。</font>
2. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">替换宏定义为常量或枚举</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：将简单的宏定义替换为</font>`<font style="background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">对象或枚举类型，确保类型安全和作用域控制。</font>
3. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">替换函数式宏为内联函数</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：将复杂的宏定义替换为内联函数，提供类型检查和优化的同时，避免宏展开带来的问题。</font>





#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 3: 只要可能就用 const</font>
在这段内容中，Scott Meyers 详细讨论了 C++ 中 `const` 关键字的使用，尤其是在函数声明、指针、迭代器、成员函数等方面的应用。以下是其中提到的主要问题及其解决方案的整理：

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">1. </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>` 的语义约束

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">  • </font>**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">:</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 关键字用于指定某个对象不应该被修改，但如何确保编译器和其他程序员都能理解并遵守这一约束？  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">  • </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：在代码中明确使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 来标识不应该被修改的对象，编译器会强制执行这一约束。这有助于减少错误，并提高代码的可读性和可维护性。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">2. </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>` 在指针中的应用

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">  • </font>**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：指针的 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 用法可能会让人困惑，如何区分指针本身是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 还是指针指向的数据是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">？  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">    ◦ </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 出现在星号左边时，表示指针指向的数据是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">    ◦ </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 出现在星号右边时，表示指针本身是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">    ◦ </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 出现在星号两边时，表示指针本身和指向的数据都是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">3. </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>` 在迭代器中的应用

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：STL 迭代器的 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 用法与指针类似，如何区分迭代器本身是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 还是迭代器指向的数据是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">？  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">     ◦ 使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const_iterator</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 来表示迭代器指向的数据是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">     ◦ 使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 修饰迭代器本身时，表示迭代器本身是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">4. </font>**`**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>**`** 在函数返回值中的应用**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：为什么函数的返回值应该声明为 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">？如何避免客户代码中对返回值进行非法操作？  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：将函数的返回值声明为 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，可以防止客户代码对返回值进行非法操作，例如赋值操作。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">5. </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`** 成员函数**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：如何区分成员函数是否会修改对象的状态？</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员函数的作用是什么？  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   ◦ 将成员函数声明为 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，表示该函数不会修改对象的状态。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   ◦</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员函数可以被 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 对象调用，并且可以用于重载非 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员函数。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">6. </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">bitwise constness</font>` 与 `<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">logical constness</font>`

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">bitwise constness</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 要求成员函数不改变对象的任何二进制位，但有时需要在不改变对象逻辑状态的情况下修改某些数据成员。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">   • </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">mutable</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 关键字来修饰那些在 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员函数中需要修改的数据成员。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">7. </font>**避免 **`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`** 和 **`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">non-const</font>`** 成员函数的代码重复**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> </font>**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">non-const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员函数的实现可能非常相似，如何避免代码重复？</font>

**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">non-const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员函数调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员函数，并通过 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const_cast</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 去掉 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 修饰。</font>

```cpp
class TextBlock {
     public:
         const char& operator[](std::size_t position) const;
         char& operator[](std::size_t position) {
             return const_cast<char&>(
                 static_cast<const TextBlock&>(*this)[position]
             );
         }
     };
```

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">如何最大化利用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 来提高代码的安全性和可维护性？  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：尽可能使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，包括在函数参数、返回值、局部变量、成员函数等地方。</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">const</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 可以帮助编译器发现潜在的错误，并提高代码的可读性。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 4: 确保 objects（对象）在使用前被初始化</font>
<font style="color:rgba(0, 0, 0, 0.9);">C++对象使用未初始化对象的风险、不同类型对象的初始化规则以及初始化顺序问题，并给出了相应的解决办法。</font>

<font style="color:rgba(0, 0, 0, 0.9);">问题1：C++对象值初始化情况复杂</font>

<font style="color:rgba(0, 0, 0, 0.9);">在C++中，内置类型和自定义类对象的值初始化结果不确定，读取未初始化值会导致未定义行为，如程序中止或不可预测的程序行为。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">解决办法</font>**<font style="color:rgba(0, 0, 0, 0.9);">：</font>

+ <font style="color:rgba(0, 0, 0, 0.9);">对于内置类型的非成员对象，手动进行初始化。例如：</font>

```cpp
int x = 0; 
const char * text = "A C-style string";
```

+ <font style="color:rgba(0, 0, 0, 0.9);">对于其他情况，由构造函数负责初始化，确保所有构造函数都初始化对象中的每一项。</font>

<font style="color:rgba(0, 0, 0, 0.9);">问题2：赋值和初始化混淆</font>

<font style="color:rgba(0, 0, 0, 0.9);">在构造函数中使用赋值操作而非初始化列表，会导致先调用默认构造函数，再进行赋值，造成效率浪费。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">解决办法</font>**<font style="color:rgba(0, 0, 0, 0.9);">：</font>

+ <font style="color:rgba(0, 0, 0, 0.9);">使用成员初始化列表代替赋值操作。例如：</font>

```cpp
ABEntry::ABEntry(const std::string& name, const std::string& address, 
const std::list<PhoneNumber>& phones) 
: theName(name), 
theAddress(address), 
thePhones(phones), 
numTimesConsulted(0) 
{}
```

+ <font style="color:rgba(0, 0, 0, 0.9);">成员初始化列表中的成员排列顺序应与它们在类中声明的顺序一致。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">问题3：不同转换单元中非局部静态对象的初始化顺序不确定</font>**

<font style="color:rgba(0, 0, 0, 0.9);">当一个转换单元内的非局部静态对象的初始化依赖于另一个转换单元内的非局部静态对象时，可能会出现使用未初始化对象的情况。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">解决办法</font>**<font style="color:rgba(0, 0, 0, 0.9);">：</font>

+ <font style="color:rgba(0, 0, 0, 0.9);">将每个非局部静态对象移到它自己的函数中，并声明为静态。函数返回该对象的引用。例如：</font>

```cpp
FileSystem& tfs() { 
    static FileSystem fs; 
    return fs; 
} 

Directory& tempDir() { 
    static Directory td; 
    return td; 
}
```

<font style="color:rgba(0, 0, 0, 0.9);">在多线程系统中，可在程序的单线程启动部分手动调用所有返回引用的函数，以避免初始化相关的问题。</font>

<font style="color:rgba(0, 0, 0, 0.9);">（单例模式）</font>

<font style="color:rgba(0, 0, 0, 0.9);"></font>

#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 5: 了解 C++ 为你偷偷地加上和调用了什么函数</font>
**<font style="color:rgba(0, 0, 0, 0.9);">编译器隐式生成的函数</font>**

+ **<font style="color:rgba(0, 0, 0, 0.9);">条件</font>**<font style="color:rgba(0, 0, 0, 0.9);">：</font><u><font style="color:rgba(0, 0, 0, 0.9);">若用户未声明拷贝构造函数、拷贝赋值运算符和析构函数，编译器会自行声明；若未声明任何构造函数，编译器会声明一个缺省构造函数。这些函数默认是 public 和 inline 的。</font></u>

**<font style="color:rgba(0, 0, 0, 0.9);">示例</font>**<font style="color:rgba(0, 0, 0, 0.9);">：空类 </font>`<font style="color:rgba(0, 0, 0, 0.9);">class Empty {};</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 本质上等同于声明了缺省构造函数、拷贝构造函数、析构函数和拷贝赋值运算符的类。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">各函数作用</font>**

+ **<font style="color:rgba(0, 0, 0, 0.9);">缺省构造函数和析构函数</font>**<font style="color:rgba(0, 0, 0, 0.9);">：主要用于放置“幕后”代码，如调用基类和非静态数据成员的构造函数与析构函数。生成的析构函数通常是非虚拟的，除非所在类继承自声明了虚拟析构函数的基类。</font>
+ **<font style="color:rgba(0, 0, 0, 0.9);">拷贝构造函数和拷贝赋值运算符</font>**<font style="color:rgba(0, 0, 0, 0.9);">：编译器生成的版本会将源对象的每个非静态数据成员复制到目标对象。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">特殊情况</font>**

+ **<font style="color:rgba(0, 0, 0, 0.9);">已有构造函数</font>**<font style="color:rgba(0, 0, 0, 0.9);">：若类中声明了构造函数，编译器不会生成缺省构造函数。</font>
+ **<font style="color:rgba(0, 0, 0, 0.9);">引用成员和 const 成员</font>**<font style="color:rgba(0, 0, 0, 0.9);">：若类包含引用成员或 const 成员，编译器拒绝生成拷贝赋值运算符，用户需自行定义。</font>
+ **<font style="color:rgba(0, 0, 0, 0.9);">基类私有拷贝赋值运算符</font>**<font style="color:rgba(0, 0, 0, 0.9);">：若基类将拷贝赋值运算符声明为私有，编译器拒绝为派生类生成隐式拷贝赋值运算符。</font>

<font style="color:rgba(0, 0, 0, 0.9);"></font>

<font style="color:rgba(0, 0, 0, 0.9);"></font>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 6: 如果你不想使用 compiler-generated functions（编译器生成函数），就明确拒绝</font>
C++可以直接使用=deleted表示编译器不提供默认的实现。

**<font style="color:rgba(0, 0, 0, 0.9);">问题背景：编译器默认生成拷贝函数可能导致逻辑错误</font>**

<font style="color:rgba(0, 0, 0, 0.9);">某些类的对象不应允许拷贝（如表示唯一实体的</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">HomeForSale</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">房产类）。但若未显式声明拷贝构造函数和拷贝赋值运算符，编译器会自动生成公有版本，导致对象被意外拷贝。</font>

```cpp
HomeForSale h1;
HomeForSale h2(h1);  // 若不阻止，编译器生成拷贝构造函数，逻辑不合理
```

**<font style="color:rgba(0, 0, 0, 0.9);">阻止拷贝的方法一：私有声明 + 不实现</font>**

+ **<font style="color:rgba(0, 0, 0, 0.9);">原理</font>**<font style="color:rgba(0, 0, 0, 0.9);">：将拷贝函数声明为</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">private</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">并</font>**<font style="color:rgba(0, 0, 0, 0.9);">不提供实现</font>**<font style="color:rgba(0, 0, 0, 0.9);">，防止外部调用；若成员或友元误调用，则引发</font>**<font style="color:rgba(0, 0, 0, 0.9);">链接时错误</font>**<font style="color:rgba(0, 0, 0, 0.9);">（因找不到函数定义）。</font>
+ **<font style="color:rgba(0, 0, 0, 0.9);">代码示例</font>**<font style="color:rgba(0, 0, 0, 0.9);">：</font>

```cpp
class HomeForSale {
public:
// 公有函数...
private:
HomeForSale(const HomeForSale&);  // 仅声明，不实现 
HomeForSale& operator=(const HomeForSale&);
};
```

+ **<font style="color:rgba(0, 0, 0, 0.9);">局限性</font>**<font style="color:rgba(0, 0, 0, 0.9);">：错误仅在链接阶段暴露，无法在编译时发现；需依赖开发者不实现私有函数。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">阻止拷贝的方法二：继承 </font>**`**<font style="color:rgba(0, 0, 0, 0.9);">Uncopyable</font>**`**<font style="color:rgba(0, 0, 0, 0.9);"> 基类</font>**

+ **<font style="color:rgba(0, 0, 0, 0.9);">原理</font>**<font style="color:rgba(0, 0, 0, 0.9);">：通过继承一个禁止拷贝的基类（如</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">Uncopyable</font>`<font style="color:rgba(0, 0, 0, 0.9);">），子类在尝试拷贝时，编译器会尝试调用基类的私有拷贝函数，直接触发</font>**<font style="color:rgba(0, 0, 0, 0.9);">编译错误</font>**<font style="color:rgba(0, 0, 0, 0.9);">（而非链接错误）。</font>
+ **<font style="color:rgba(0, 0, 0, 0.9);">基类设计</font>**<font style="color:rgba(0, 0, 0, 0.9);">：</font>

```cpp
class Uncopyable {
protected:
Uncopyable() = default;
~Uncopyable() = default;
private:
Uncopyable(const Uncopyable&);  // 禁止拷贝 
Uncopyable& operator=(const Uncopyable&);
};

class HomeForSale : private Uncopyable {  // 私有继承 
// 无需声明拷贝函数 
};
```

+ **<font style="color:rgba(0, 0, 0, 0.9);">优势</font>**<font style="color:rgba(0, 0, 0, 0.9);">：错误提前至编译阶段，更易排查；代码更简洁（子类无需重复声明）。</font>

| **<font style="color:rgba(0, 0, 0, 0.9);">方法</font>** | **<font style="color:rgba(0, 0, 0, 0.9);">错误阶段</font>** | **<font style="color:rgba(0, 0, 0, 0.9);">适用场景</font>** |
| --- | --- | --- |
| <font style="color:rgba(0, 0, 0, 0.9);">私有声明 + 不实现</font> | <font style="color:rgba(0, 0, 0, 0.9);">链接时</font> | <font style="color:rgba(0, 0, 0, 0.9);">简单类，无需外部继承</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">继承</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">Uncopyable</font>`<br/><font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">基类</font> | <font style="color:rgba(0, 0, 0, 0.9);">编译时</font> | <font style="color:rgba(0, 0, 0, 0.9);">复杂类或需代码复用，兼容 Boost 库</font> |


<font style="color:rgba(0, 0, 0, 0.9);">相关实践与扩展</font>

+ **<font style="color:rgba(0, 0, 0, 0.9);">Boost 库的</font>****<font style="color:rgba(0, 0, 0, 0.9);"> </font>**`**<font style="color:rgba(0, 0, 0, 0.9);">noncopyable</font>**`<font style="color:rgba(0, 0, 0, 0.9);">：提供现成实现，类名为</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::noncopyable</font>`<font style="color:rgba(0, 0, 0, 0.9);">，语义更直观。</font>
+ **<font style="color:rgba(0, 0, 0, 0.9);">注意事项</font>**<font style="color:rgba(0, 0, 0, 0.9);">：</font>
    - `<font style="color:rgba(0, 0, 0, 0.9);">Uncopyable</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">基类无需虚析构函数（因不涉及多态删除）。</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">可使用空基类优化（EBCO）避免内存开销（见 Item 39）。</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">避免多重继承冲突（Item 40）。</font>

<font style="color:rgba(0, 0, 0, 0.9);"></font>

<font style="color:rgba(0, 0, 0, 0.9);">通过显式声明私有拷贝函数（不实现）或继承禁止拷贝的基类，阻止编译器生成默认拷贝逻辑。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">选择策略</font>**<font style="color:rgba(0, 0, 0, 0.9);">：优先使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">Uncopyable</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 基类（或 Boost 的 </font>`<font style="color:rgba(0, 0, 0, 0.9);">noncopyable</font>`<font style="color:rgba(0, 0, 0, 0.9);">），以实现编译期错误检查，提升代码安全性与可维护性。</font>





#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 7:在 多态基类中将析构函数声明为 virtual</font>
**<font style="color:rgba(0, 0, 0, 0.9);">问题</font>**

<font style="color:rgba(0, 0, 0, 0.9);">当一个派生类对象通过指向带有非虚拟析构函数的基类指针被删除时，结果是未定义的。运行时典型后果是对象的派生部分不会被析构，仅基类部分可能被析构，导致“部分被析构”对象，造成资源泄漏、数据结构破坏等问题。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">解决方法</font>**

+ **<font style="color:rgba(0, 0, 0, 0.9);">为多态基类声明虚拟析构函数</font>**<font style="color:rgba(0, 0, 0, 0.9);">：给基类一个虚拟析构函数，这样删除派生类对象时能正确析构整个对象，包括所有派生类部分。</font>
+ **<font style="color:rgba(0, 0, 0, 0.9);">声明纯虚拟析构函数</font>**<font style="color:rgba(0, 0, 0, 0.9);">：对于想成为抽象类但没有纯虚拟函数的类，可声明纯虚拟析构函数使其成为抽象类，同时要为纯虚拟析构函数提供定义。</font>

<font style="color:rgba(0, 0, 0, 0.9);">问题示例</font>

```cpp
class TimeKeeper { 
public: 
TimeKeeper(); 
~TimeKeeper(); 
... 
}; 

class AtomicClock: public TimeKeeper { ... }; 

TimeKeeper* getTimeKeeper(); 

TimeKeeper *ptk = getTimeKeeper(); 
delete ptk; // 可能导致 AtomicClock 部分未被析构
```



```cpp
class SpecialString: public std::string { ... }; 
SpecialString *pss = new SpecialString("Impending Doom"); 
std::string *ps = pss; 
delete ps; // 未定义行为，SpecialString 资源可能泄漏
```

<font style="color:rgba(0, 0, 0, 0.9);">解决方法示例</font>

```cpp
class TimeKeeper { 
public: 
TimeKeeper(); 
virtual ~TimeKeeper(); 
... 
}; 

TimeKeeper *ptk = getTimeKeeper(); 
delete ptk; // 现在行为正确
```



```cpp
class AWOV { 
public: 
virtual ~AWOV() = 0; // 声明纯虚拟析构函数 
}; 

AWOV::~AWOV() {} // 提供纯虚拟析构函数的定义
```

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

#### <font style="color:rgb(51, 51, 51);">Item 8: 防止因为 exceptions（异常）而离开 destructors（析构函数）</font>
<font style="color:rgba(0, 0, 0, 0.9);">问题描述</font>

<font style="color:rgba(0, 0, 0, 0.9);">C++ 虽然不禁止从析构函数中引发异常，但这种做法会带来严重问题。当多个对象析构时，若某个对象析构函数抛出异常，后续对象仍需析构，若在此过程中又有析构函数抛出异常，会导致同时有两个活动异常，程序可能会终止或出现未定义行为。即使不使用容器和数组，析构函数引发异常也可能导致程序过早终止或出现未定义行为。</font>

<font style="color:rgba(0, 0, 0, 0.9);">案例分析</font>

**<font style="color:rgba(0, 0, 0, 0.9);">案例一：容器对象析构</font>**

```cpp
class Widget { 
public: 
~Widget() { ... } // 假设此析构函数可能抛出异常 
}; 

void doSomething() 
{ 
    std::vector<Widget> v; 
    ... 
        } // v 在此自动销毁
```

<font style="color:rgba(0, 0, 0, 0.9);">当</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">vector v</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">被析构时，它会依次析构包含的所有</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">Widget</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">对象。若第一个</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">Widget</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">析构时抛出异常，后续</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">Widget</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">仍需析构，若第二个</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);">Widget</font>`<font style="color:rgba(0, 0, 0, 0.9);"> </font><font style="color:rgba(0, 0, 0, 0.9);">析构时又抛出异常，程序会出现未定义行为。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">案例二：数据库连接类析构</font>**

```cpp
class DBConnection { 
public: 
static DBConnection create(); 
void close(); // 关闭连接，若失败抛出异常 
}; 

class DBConn { 
public: 
~DBConn() { 
    db.close();  
} 
private: 
DBConnection db; 
};
```

<font style="color:rgba(0, 0, 0, 0.9);">若 </font>`<font style="color:rgba(0, 0, 0, 0.9);">DBConnection</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的 </font>`<font style="color:rgba(0, 0, 0, 0.9);">close</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 方法在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">DBConn</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的析构函数中调用时抛出异常，</font>`<font style="color:rgba(0, 0, 0, 0.9);">DBConn</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的析构函数会传播该异常，从而引发问题。通过上述三种解决方法可以避免这种情况。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:#FBDE28;">解决方法</font>**

**<font style="color:rgba(0, 0, 0, 0.9);">终止程序</font>**<font style="color:rgba(0, 0, 0, 0.9);">：如果 </font>`<font style="color:rgba(0, 0, 0, 0.9);">close</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 抛出异常，通过调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">abort</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 终止程序。这样能防止因异常传播导致的未定义行为，适用于程序在析构出错后无法继续运行的情况。</font>

```cpp
DBConn::~DBConn() 
{ 
    try { db.close();  } 
    catch (...) { 
        make log entry that the call to close failed; 
        std::abort(); 
    } 
}
```

**<font style="color:rgba(0, 0, 0, 0.9);">抑制异常</font>**<font style="color:rgba(0, 0, 0, 0.9);">：捕获 </font>`<font style="color:rgba(0, 0, 0, 0.9);">close</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 调用产生的异常并记录日志，但不传播异常。虽然抑制异常可能会隐瞒重要信息，但在某些情况下，比程序过早终止或出现未定义行为更可取，前提是程序在忽略错误后仍能可靠运行。</font>

```cpp
DBConn::~DBConn() 
{ 
    try { db.close();  } 
    catch (...) { 
        make log entry that the call to close failed; 
    } 
}
```

**<font style="color:rgba(0, 0, 0, 0.9);">让客户处理异常</font>**<font style="color:rgba(0, 0, 0, 0.9);">：设计类的接口，提供一个非析构的常规函数（如 </font>`<font style="color:rgba(0, 0, 0, 0.9);">close</font>`<font style="color:rgba(0, 0, 0, 0.9);">）让客户调用，使客户有机会处理可能发生的异常。同时，在析构函数中包含一个“候补”调用，以防止资源泄漏。若析构函数中的调用失败，再考虑终止或抑制异常。</font>

```cpp
class DBConn { 
public: 
void close() { 
    db.close();  
    closed = true; 
} 

~DBConn() { 
    if (!closed) { 
        try { 
            db.close();  
        } 
        catch (...) { 
            make log entry that call to close failed; 
            ... 
                } 
    } 
} 

private: 
DBConnection db; 
bool closed; 
};
```



<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

#### <font style="color:rgb(34, 34, 38);background-color:#FBDE28;">Item 9: 绝不要在构造或析构期间调用虚函数</font>
这个item没有说到本质。

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">问题描述</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在 C++ 中，</font>**在构造函数或析构函数期间调用虚函数**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">可能会导致意外的行为。当基类的构造函数或析构函数调用虚函数时，</font>**虚函数机制不会按照通常的多态行为工作**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，而是会调用当前类的版本，而不是派生类的版本。这是因为在基类构造或析构期间，派生类的部分尚未完全构造或已经被销毁，因此虚函数机制无法正确地向下匹配到派生类。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">代码案例</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">以下是一个模拟股票交易的类继承体系，展示了在构造函数中调用虚函数的问题：</font>

```cpp
class Transaction {                               // 基类
public:
    Transaction();
    virtual void logTransaction() const = 0;      // 纯虚函数，用于记录交易日志
    ...
};

Transaction::Transaction()                        // 基类构造函数
{
    ...
    logTransaction();                               // 在构造函数中调用虚函数
}

class BuyTransaction : public Transaction {       // 派生类
public:
    virtual void logTransaction() const;          // 派生类的日志记录实现
    ...
};

class SellTransaction : public Transaction {      // 派生类
public:
    virtual void logTransaction() const;          // 派生类的日志记录实现
    ...
};
```

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">当执行以下代码时：</font>

```cpp
BuyTransaction b;
```

`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">BuyTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 的构造函数会被调用，但首先会调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Transaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 的构造函数。在 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Transaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 的构造函数中，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">logTransaction()</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 被调用，但此时 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">BuyTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 的构造函数尚未执行，因此 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">logTransaction()</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 调用的是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Transaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 中的版本，而不是 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">BuyTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 中的版本。这显然不是预期的行为。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">问题原因</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在基类构造函数执行期间，派生类的成员尚未初始化，因此虚函数机制无法安全地调用派生类的实现。C++ 的设计是为了避免未定义行为，因此在基类构造期间，虚函数调用会被限制在当前类（基类）中。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">同样的问题也适用于析构函数。在派生类析构函数执行期间，派生类的成员已经被销毁，因此虚函数调用也会被限制在当前类（基类）中。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">解决方案</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">为了避免在构造函数或析构函数中调用虚函数，可以采用以下方法：</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">方法 1：将虚函数改为非虚函数，并通过参数传递信息</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">将 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">logTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 改为非虚函数，并在派生类的构造函数中通过参数将日志信息传递给基类构造函数。基类构造函数可以安全地调用非虚函数。</font>

```cpp
class Transaction {
public:
    explicit Transaction(const std::string& logInfo);  // 非虚函数
    void logTransaction(const std::string& logInfo) const;  // 非虚函数
    ...
};

Transaction::Transaction(const std::string& logInfo)
{
    ...
    logTransaction(logInfo);  // 调用非虚函数
}

class BuyTransaction : public Transaction {
public:
    BuyTransaction(parameters)
        : Transaction(createLogString(parameters))  // 将日志信息传递给基类构造函数
    { ... }

private:
    static std::string createLogString(parameters);  // 辅助函数，用于生成日志信息
};
```

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在这个解决方案中，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">BuyTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 通过静态辅助函数 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">createLogString</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 生成日志信息，并将其传递给 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Transaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 的构造函数。基类构造函数可以安全地调用非虚函数 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">logTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，从而避免了在构造函数中调用虚函数的问题。</font>

****

**在构造函数或析构函数中调用虚函数**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">会导致虚函数机制无法正确向下匹配到派生类，从而引发意外行为。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• </font>**解决方案**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">是将虚函数改为非虚函数，并通过参数传递必要的信息，或者使用其他设计模式来避免在构造或析构期间调用虚函数。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• </font>**静态辅助函数**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">可以在派生类中生成必要的信息，并将其传递给基类构造函数，从而避免直接调用虚函数。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

#### <font style="color:rgb(255, 0, 0);background-color:#FBDE28;">Item 10: 让赋值运算符返回一个 引向 *this 的引用</font>
**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">问题描述</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在 C++ 中，赋值操作（assignment）具有</font>**右结合性**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">（right-associative），这意味着多个赋值操作可以从右向左依次执行。例如：</font>

```cpp
x = y = z = 15;  // 等价于 x = (y = (z = 15));
```

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">为了实现这种链式赋值，赋值运算符需要返回一个</font>**引向左侧操作数的引用**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">（reference to the left-hand operand）。这是 C++ 中赋值运算符的标准行为，并且被所有内建类型和标准库类型所遵循。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">解决方案</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">当你为自定义类实现赋值运算符时，应该遵循这一惯例，即让赋值运算符返回当前对象的引用（</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*this</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">）。这不仅适用于标准的赋值运算符，还适用于其他类似的运算符（如 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">+=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">-=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, 等）。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">代码案例</font>**

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">标准赋值运算符</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">以下是一个简单的 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Widget</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 类，展示了如何实现标准的赋值运算符：</font>

```cpp
class Widget {
public:
    ...
    Widget& operator=(const Widget& rhs)  // 返回当前对象的引用
    {
        // 执行赋值操作
        ...
        return *this;  // 返回左侧操作数的引用
    }
    ...
};
```

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在这个例子中，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">operator=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 返回 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*this</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，即当前对象的引用。这使得链式赋值成为可能：</font>

```cpp
Widget a, b, c;
a = b = c;  // 链式赋值
```

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">其他运算符</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">除了标准的赋值运算符，其他类似的运算符（如 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">+=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">-=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, 等）也应该遵循这一惯例。例如：</font>

```cpp
class Widget {
public:
    ...
    Widget& operator+=(const Widget& rhs)  // 返回当前对象的引用
    {
        // 执行 += 操作
        ...
        return *this;  // 返回左侧操作数的引用
    }

    Widget& operator=(int rhs)  // 即使参数类型非类类型，也应返回引用
    {
        // 执行赋值操作
        ...
        return *this;  // 返回左侧操作数的引用
    }
    ...
};
```

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在这个例子中，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">operator+=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">operator=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 都返回 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*this</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，这使得链式操作成为可能：</font>

```cpp
Widget a, b, c;
a += b += c;  // 链式 += 操作
a = 10;       // 赋值为 int 类型
```

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">为什么遵循这一惯例？</font>**

1. **一致性**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：所有内建类型和标准库类型都遵循这一惯例。例如，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">int</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">std::string</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">std::vector</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 等类型的赋值运算符都返回引向左侧操作数的引用。遵循这一惯例可以使你的代码与这些类型的行为保持一致。</font>
2. **链式操作**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：返回引向左侧操作数的引用使得链式赋值成为可能。这是 C++ 中常见的用法，破坏这一惯例会导致代码的可读性和可维护性下降。</font>
3. **兼容性**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：许多标准库算法和容器操作依赖于赋值运算符的这种行为。如果你的类不遵循这一惯例，可能会导致与这些算法和容器的兼容性问题。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">不遵循惯例的后果</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">虽然不遵循这一惯例的代码仍然可以通过编译，但它可能会导致以下问题：</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• </font>**无法链式赋值**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：如果赋值运算符不返回引用，链式赋值将无法正常工作。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• </font>**与标准库不兼容**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：某些标准库算法或容器可能依赖于赋值运算符的返回值类型，不遵循惯例可能导致不可预期的行为。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• </font>**代码可读性差**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：链式操作是 C++ 中常见的编程风格，破坏这一惯例会使代码难以理解和维护。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• </font>**让赋值运算符返回引向 **`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*this</font>`** 的引用**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，即 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">T& operator=(const T& rhs)</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• 这一惯例适用于所有赋值运算符，包括 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">+=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">-=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*=</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">, 等。  
</font><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">• 遵循这一惯例可以确保代码的一致性、可读性和兼容性。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

#### **<font style="color:#ff0000;background-color:#FBDE28;">Item 11: 在 operator= 中处理 assignment to self（自赋值）</font>**
这个需要重点看。

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">问题描述</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在 C++ 中，</font>**自赋值**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">（assignment to self）是指一个对象被赋值给自己，例如 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">w = w;</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。虽然这种情况看起来很愚蠢，但它是合法的，客户可能会这样做。此外，自赋值并不总是显而易见的，例如 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">a[i] = a[j];</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 或 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">*px = *py;</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，在这些情况下，如果 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">i == j</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 或 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">px == py</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，就会发生自赋值。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">自赋值可能导致以下问题：</font>

1. **资源管理问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：如果对象管理动态分配的资源（如指针），在自赋值的情况下，资源可能会被错误地释放或重复释放。</font>
2. **异常安全问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：如果在赋值过程中抛出异常，对象可能会处于不一致的状态。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">解决方案</font>**

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">方法 1：使用身份测试（Identity Test）</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在赋值运算符的开头添加一个身份测试，检查源对象和目标对象是否相同。如果相同，则直接返回目标对象。</font>

```cpp
Widget& Widget::operator=(const Widget& rhs)
{
    if (this == &rhs) return *this;  // 身份测试：如果是自赋值，直接返回
    delete pb;                       // 释放当前资源
    pb = new Bitmap(*rhs.pb);        // 复制新资源
    return *this;
}
```

**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：这种方法虽然解决了自赋值问题，但仍然不是异常安全的。如果在 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">new Bitmap(*rhs.pb)</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 时抛出异常，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">pb</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 将指向一个无效的内存。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">方法 2：调整语句顺序以确保异常安全</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">通过调整语句顺序，确保在释放旧资源之前先复制新资源，从而避免自赋值和异常安全问题。</font>

```cpp
Widget& Widget::operator=(const Widget& rhs)
{
    Bitmap *pOrig = pb;               // 保存原始指针
    pb = new Bitmap(*rhs.pb);         // 复制新资源
    delete pOrig;                     // 释放旧资源
    return *this;
}
```

**优点**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：这种方法不仅解决了自赋值问题，还确保了异常安全。如果在 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">new Bitmap(*rhs.pb)</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 时抛出异常，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">pb</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 仍然指向原始资源，对象状态保持一致。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">方法 3：使用 "Copy and Swap" 惯用法</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">"Copy and Swap" 是一种更通用的方法，它通过创建一个临时副本并交换其内容来确保异常安全和自赋值安全。</font>

```cpp
class Widget {
public:
    void swap(Widget& rhs);  // 交换 *this 和 rhs 的数据
    ...
};

Widget& Widget::operator=(const Widget& rhs)
{
    Widget temp(rhs);  // 创建 rhs 的副本
    swap(temp);        // 交换 *this 和副本的数据
    return *this;
}
```

**优点**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：这种方法不仅解决了自赋值问题，还确保了异常安全。此外，它通常比手动调整语句顺序更清晰，并且可以利用传值语义让编译器生成更高效的代码。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">方法 4：通过传值方式传递参数</font>**

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">利用 C++ 的传值语义，将参数作为副本传递，并在函数体内交换数据。这种方法与 "Copy and Swap" 类似，但更简洁。</font>

```cpp
Widget& Widget::operator=(Widget rhs)  // rhs 是传入对象的副本
{
    swap(rhs);  // 交换 *this 和 rhs 的数据
    return *this;
}
```

**优点**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：这种方法简洁且异常安全，但可能会牺牲一些代码清晰度。</font>

**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">自赋值问题的示例</font>**

```cpp
class Bitmap { ... };

class Widget {
public:
    Widget& operator=(const Widget& rhs);
private:
    Bitmap *pb;  // 指向动态分配的位图
};

Widget& Widget::operator=(const Widget& rhs)
{
    delete pb;  // 释放当前位图
    pb = new Bitmap(*rhs.pb);  // 复制新位图
    return *this;
}
```

**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：如果 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">this == &rhs</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">delete pb;</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 会释放 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">rhs.pb</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 指向的资源，导致后续 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">pb = new Bitmap(*rhs.pb);</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 出现未定义行为。</font>

1. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">使用身份测试的解决方案</font>**

```cpp
Widget& Widget::operator=(const Widget& rhs)
{
    if (this == &rhs) return *this;  // 身份测试
    delete pb;
    pb = new Bitmap(*rhs.pb);
    return *this;
}
```

**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：仍然不是异常安全的。</font>

2. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">调整语句顺序的解决方案</font>**

```cpp
Widget& Widget::operator=(const Widget& rhs)
{
    Bitmap *pOrig = pb;               // 保存原始指针
    pb = new Bitmap(*rhs.pb);         // 复制新资源
    delete pOrig;                     // 释放旧资源
    return *this;
}
```

**优点**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：异常安全和自赋值安全。</font>

3. **<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">使用 "Copy and Swap" 的解决方案</font>**

```cpp
class Widget {
public:
    void swap(Widget& rhs);
    ...
};

Widget& Widget::operator=(const Widget& rhs)
{
    Widget temp(rhs);  // 创建副本
    swap(temp);        // 交换数据
    return *this;
}
```

**优点**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：异常安全和自赋值安全，代码清晰。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">4</font>**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">.通过传值方式传递参数的解决方案</font>**

```cpp
Widget& Widget::operator=(Widget rhs)  // rhs 是副本
{
    swap(rhs);  // 交换数据
    return *this;
}
```

**优点**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：简洁且异常安全，但可能牺牲一些清晰度。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> </font>**自赋值问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：在赋值运算符中，必须处理自赋值的情况，以避免资源管理问题和异常安全问题。  
</font>**推荐方法**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：使用 "Copy and Swap" 或通过传值方式传递参数的方法，因为它们不仅解决了自赋值问题，还确保了异常安全，并且代码更清晰。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 12: 拷贝一个对象的所有组成部分</font>
**新增成员变量后，拷贝函数没有及时更新出现问题**

**问题**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：</font>

1. <u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">当为类增加新的数据成员后，若未更新已有的拷贝函数（拷贝构造函数和拷贝赋值运算符），已有的拷贝函数会只进行部分拷贝，而编译器大多情况下不会对此给出警告，</font></u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">即便在最高警告级别。例如在 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Customer</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 类中增加 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Date lastTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 数据成员后，原有的拷贝函数只拷贝了 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">name</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员，未拷贝 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">lastTransaction</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 成员。</font>
2. <u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">在派生类中，当自行编写拷贝函数时，若不注意拷贝从基类继承来的数据成员，会导致派生类对象的基类部分未被正确拷贝。</font></u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">如 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">PriorityCustomer</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 类的拷贝函数只拷贝了自身声明的数据成员 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">priority</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">，未拷贝从 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Customer</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 类继承来的数据成员。</font>
3. <u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">拷贝构造函数和拷贝赋值运算符代码存在相似性时，</font></u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">若试图通过一个拷贝函数调用另一个来避免代码重复，会出现错误。用拷贝赋值运算符调用拷贝构造函数是试图构造一个已存在的对象，没有意义且无合适语法支持；用拷贝构造函数调用拷贝赋值运算符是对未初始化对象进行只有已初始化对象才能进行的赋值操作，也是不合理的。</font>

**解决方法**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">：</font>

1. <u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">当为类增加新的数据成员时，务必更新拷贝函数（拷贝构造函数和拷贝赋值运算符），同时还需更新类中的所有构造函数以及任何非标准形式的 </font></u>`<u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">operator=</font></u>`<u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。</font></u>
2. <u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">对于派生类，在编写拷贝函数时，必须调用基类对应的拷贝函数来拷贝基类部分</font></u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。如在 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">PriorityCustomer</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 类的拷贝构造函数中通过 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Customer(rhs)</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 调用基类的拷贝构造函数，在拷贝赋值运算符中通过 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">Customer::operator=(rhs)</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"> 来赋值基类部分。</font>
3. <u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">当拷贝构造函数和拷贝赋值运算符有相似代码时，通过创建一个私有的第三个成员函数（常命名为 </font></u>`<u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">init</font></u>`<u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">）供两者调用，以此消除代码重复</font></u><font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);">。</font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 13: 使用对象管理资源</font>
**问题**：**手动释放资源存在泄漏风险**

```cpp
void f()
{
  Investment *pInv = createInvestment();         // call factory function
  // 假设这里可能会有提前的 return、continue、goto 或异常抛出
  // 导致控制流程无法到达下面的 delete 语句
  if(/* 某种条件 */) return;
  delete pInv;                                   // release object，可能无法执行
}
```

    - 当使用`createInvestment`函数获取`Investment`对象后，手动调用`delete`释放资源存在风险。如在函数执行过程中遇到提前`return`、`continue`、`goto`或异常抛出等情况，可能导致控制流程无法到达`delete`语句，从而造成资源泄漏，不仅泄漏了对象的内存，还可能泄漏对象持有的其他资源。
    - `createInvestment`函数返回裸指针，调用者容易忘记调用`delete`释放资源。

**解决方法**：

    - 使用资源管理对象（`RAII`对象）来管理资源，在资源管理对象的构造函数中获取资源，在析构函数中释放资源。这样无论控制流程如何离开作用域，资源都能被正确释放。
    - 可以使用unique_ptr或者shared_ptr来接受一个动态分配的对象。
    - 如果需要处理动态分配的数组，由于`vector`和`string`几乎总是能代替动态分配数组，可优先考虑使用它们。
    - 从根本上解决`createInvestment`函数返回裸指针导致的问题，需要改变`createInvestment`的接口（这是`Item 18`的主题）。

```cpp
void f()
{
  std::tr1::shared_ptr<Investment> pInv(createInvestment());  // call factory function
  // 使用 pInv
  // 当离开作用域时，自动通过 shared_ptr 的析构函数删除 pInv
}
```





#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 14: 谨慎考虑资源管理类的拷贝行为</font>
自定义RAII类需要注意的问题

1. 问题分析

+ <u>并非所有资源都基于堆，对于非堆资源</u>，`std::unique_ptr` 和 `std::shared_ptr` 这类智能指针可能不适用，可能需要创建自定义的资源管理类。
+ 当自定义的资源管理类（如 `Lock` 类管理互斥体）的对象被拷贝时，需要考虑合适的拷贝行为，因为不同的资源管理场景对拷贝行为有不同的需求，且默认的拷贝行为可能不符合预期。
+ 使用 `std::shared_ptr` 实现引用计数的拷贝行为时，其默认行为是在引用计数为 0 时删除所指向的对象，但对于某些资源（如互斥体），我们希望在使用完毕后解锁而不是删除，存在默认行为与实际需求不符的问题。
+ 编译器生成的拷贝函数（拷贝构造函数和拷贝赋值运算符）的行为可能并非总是符合我们对资源管理类的需求，需要根据具体情况决定是否自行编写。

2. 解决方法

对于自定义资源管理类，根据资源管理的实际需求选择合适的拷贝行为，常见的拷贝行为有以下几种：

    - **禁止拷贝**：当拷贝一个资源管理类没有意义时（如 `Lock` 类管理互斥体，互斥体的“副本”通常无意义），可以使用 `= delete` 语法来禁止拷贝。例如：

```cpp
class Lock {
public:
    Lock(const Lock&) = delete;
    Lock& operator=(const Lock&) = delete;
    // 其他成员函数和数据成员
};
```

对底层的资源引用计数：当需要保持一个资源直到最后一个使用它的对象被销毁时，使用引用计数的方式。通常在资源管理类中包含一个 std::shared_ptr 数据成员来实现。对于 std::shared_ptr 默认在引用计数为 0 时删除对象的问题，可以通过指定 deleter（当引用计数变为 0 时调用的函数或函数对象）来解决。

对于拷贝函数，除非编译器生成的版本符合需求，否则应自行编写拷贝构造函数和拷贝赋值运算符，有时还需支持这些函数的泛型化版本（参考 C++ 模板相关知识）。

```cpp

// 假设的 lock 和 unlock 函数
void lock(std::mutex* pm) {
    pm->lock();
}

void unlock(std::mutex* pm) {
    pm->unlock();
}

// 使用自定义 Lock 类管理互斥体（基于 RAII 原则）的示例
class Lock {
public:
    explicit Lock(std::mutex* pm) : mutexPtr(pm) {
        lock(mutexPtr);
    }
    ~Lock() {
        unlock(mutexPtr);
    }
private:
    std::mutex* mutexPtr;
};

// 禁止 Lock 类对象拷贝的示例
class LockNoCopy {
public:
    LockNoCopy(const LockNoCopy&) = delete;
    LockNoCopy& operator=(const LockNoCopy&) = delete;

    explicit LockNoCopy(std::mutex* pm) : mutexPtr(pm) {
        lock(mutexPtr);
    }
    ~LockNoCopy() {
        unlock(mutexPtr);
    }
private:
    std::mutex* mutexPtr;
};

// Lock 类使用 std::shared_ptr 实现引用计数并指定 deleter 的示例
class LockShared {
public:
    explicit LockShared(std::mutex* pm) : mutexPtr(pm, unlock) {
        lock(mutexPtr.get());
    }
private:
    std::shared_ptr<std::mutex> mutexPtr;
};
```



<font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(252, 252, 252);"></font>





### 
RAII获取裸资源：

很多时候我们需要与传统的 C API 进行交互，这些 C API 通常需要直接访问裸资源，因此 RAII 类需要提供一种方式来获取其所管理的资源。

**问题**

当 RAII 类与 C API 交互时，需要将 RAII 类对象转换为 C API 所需的裸资源类型。有两种转换方式可供选择：显式转换和隐式转换。显式转换需要用户手动调用转换函数，相对安全但使用起来不够方便；隐式转换则允许编译器自动进行转换，使用起来更自然，但可能会增加错误发生的机会。

**解决方法**

+ **显式转换函数**：RAII 类提供一个显式的转换函数，如 `get()`，用户需要手动调用该函数来获取裸资源。这种方式可以避免意外的转换，提高代码的安全性。
+ **隐式转换函数**：RAII 类提供一个隐式转换运算符，如 `operator ResourceType()`，编译器会在需要时自动调用该运算符进行转换。这种方式使代码更简洁，但可能会导致一些难以察觉的错误。

**显式转换**

写一个get成员函数获取RAII类包裹的资源。

```cpp
#include <iostream>
// 假设 FontHandle 是某种字体资源句柄类型
using FontHandle = int;
// 模拟 C API 函数
FontHandle getFont() {
    // 这里简单返回一个值，实际中会从系统获取字体资源
    return 1; 
}
void releaseFont(FontHandle fh) {
  ...
}
void changeFontSize(FontHandle f, int newSize) {
   ...
}

// RAII 类
class Font {
public:
    // 构造函数，获取资源
    explicit Font(FontHandle fh) : f(fh) {}
    // 析构函数，释放资源
    ~Font() {
        releaseFont(f);
    }
    // 显式转换函数
    FontHandle get() const {
        return f;
    }
private:
    FontHandle f; // 原始字体资源
};

int main() {
    Font f(getFont());
    int newFontSize = 12;
    // 显式将 Font 转换为 FontHandle
    changeFontSize(f.get(), newFontSize);
    return 0;
}    
```

**隐式转换函数**

重<u>载()运算符，把RAII类进行隐式转换。</u>

```cpp
#include <iostream>

using FontHandle = int;
FontHandle getFont() {
    // 这里简单返回一个值，实际中会从系统获取字体资源
    return 1; 
}

void releaseFont(FontHandle fh) {
    std::cout << "Releasing font with handle: " << fh << std::endl;
}

void changeFontSize(FontHandle f, int newSize) {
    std::cout << "Changing font size of handle " << f << " to " << newSize << std::endl;
}

// RAII 类
class Font {
public:
    // 构造函数，获取资源
    explicit Font(FontHandle fh) : f(fh) {}

    // 析构函数，释放资源
    ~Font() {
        releaseFont(f);
    }
    // 隐式转换函数
    operator FontHandle() const {
        return f;
    }

private:
    FontHandle f; // 原始字体资源
};

int main() {
    Font f(getFont());
    int newFontSize = 12;

    // 隐式将 Font 转换为 FontHandle
    changeFontSize(f, newFontSize);

    Font f1(getFont());
    // 可能的错误：本意是复制 Font 对象，但实际上隐式转换为了 FontHandle 并复制
    FontHandle f2 = f1; 

    return 0;
}    
```

**🥐****思考：为什么重载的是FontHandle()而不是其他运算符？**

在C++里，隐式转换可以从构造函数和类型转换运算符。

构造函数引发的隐式转换

构造函数触发的隐式转换是把参数类型转换为类类型。

当构造函数只有一个参数或者除了第一个参数外其他参数都有默认值时，它就可以将参数类型隐式转换为类类型。

```cpp
#include <iostream>
class Distance {
private:
    double meters;
public:
    // 构造函数，支持隐式转换
    Distance(double m) : meters(m) {}
    void print() const {
        std::cout << "Distance: " << meters << " meters" << std::endl;
    }
};
void showDistance(const Distance& d) {
    d.print();
}
int main() {
    double value = 10.5;
    // 隐式转换：double 转换为 Distance
    showDistance(value);
    return 0;
}
```

`Distance`类的构造函数`Distance(double m)`能把`double` 类型隐式转换为 `Distance` 类型，返回值类型就是`Distance`。

**类型转换运算符引发的隐式转换**

类型转换运算符实现的是将类类型转换为其他类型。其返回值类型就是类型转换运算符指定的目标类型。

类型转换运算符的语法格式为 `operator TargetType() const`，其中 `TargetType` 就是返回值类型。

```cpp
#include <iostream>
class Fraction {
private:
    int numerator;
    int denominator;

public:
    Fraction(int num, int den) : numerator(num), denominator(den) {}
    // 类型转换运算符，将 Fraction 转换为 double
    operator double() const {
        return static_cast<double>(numerator) / denominator;
    }
};

int main() {
    Fraction f(3, 4);
    // 隐式转换：Fraction 转换为 double
    double result = f + 1.0;
    std::cout << "Result: " << result << std::endl;
    return 0;
}
```

在这个例子中，`Fraction` 类的类型转换运算符 `operator double() const` 把 `Fraction` 类型隐式转换为 `double` 类型，返回值类型就是 `double`。



#### <font style="color:rgb(51, 51, 51);">Item 16: 使用相同形式的 new 和 delete</font>
C++中`new`和`delete`使用

**背景**：在C++中，使用`new`动态创建对象时，会先分配内存（通过`operator new`函数），再调用构造函数；使用`delete`删除对象时，会先调用析构函数，再回收内存（通过`operator delete`函数）。`delete`操作需要知道要删除的内存中驻留对象的数量，以决定调用析构函数的个数。

**问题**：使用`new`和`delete`时，如果形式不匹配，会导致未定义行为。例如`std::string *stringArray = new std::string[100];` 后使用 `delete stringArray;` ，此时`stringArray`指向的100个`string`对象中的99个可能无法完全销毁，因为它们的析构函数或许根本没有被调用。原因是单一对象和数组的内存布局不同，数组内存布局通常包含数组大小信息，而单一对象内存中缺乏该信息，`delete`需要根据`delete`表达式中是否有`[]`来判断指针指向的是单一对象还是对象数组。

**解决方法**：如果在`new`表达式中使用了`[]`，那么在相应的`delete`表达式中也必须使用`[]`；如果在`new`表达式中没有使用`[]`，那么在匹配的`delete`表达式中也不要使用`[]`。



```cpp
std::string *stringPtr1 = new std::string;
std::string *stringPtr2 = new std::string[100];
...
delete stringPtr1;                     // delete an object
delete [] stringPtr2;                  //  delete an array of objects
```

```cpp
typedef std::string AddressLines[4];   // a person's address has 4 lines,
                                       // each of which is a string
std::string *pal = new AddressLines;   // note that "new AddressLines"
                                       // returns a string*, just like
                                       // "new string[4]" would
delete pal; // undefined!
delete [] pal; // fine
```







#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 17: 在一个独立的语句中将 new 出来的对象存入智能指针</font>
在 C++ 编程中，为了避免资源泄漏，通常采用对象来管理资源。`processWidget` 函数接收一个智能指针 `std::shared_ptr<Widget>` 和一个表示优先级的整数作为参数，用于处理动态分配的 `Widget` 对象。

以下代码可能会导致资源泄漏：

```cpp
processWidget(std::shared_ptr<Widget>(new Widget), priority());
```

在调用 `processWidget` 函数之前，编译器需要完成三件事：

1. 调用 `priority` 函数。
2. 执行 `new Widget` 表达式。
3. 调用 `std::shared_ptr` 的构造函数。



C++ 编译器在决定这三件事的执行顺序上有较大的自由度。虽然 `new Widget` 必须在 `std::shared_ptr` 的构造函数调用之前执行，但 `priority` 的调用顺序不固定。如果编译器选择在 `new Widget` 之后、`std::shared_ptr` 构造函数调用之前调用 `priority`，并且 `priority` 的调用引发异常，那么 `new Widget` 返回的指针就无法存入 `std::shared_ptr`，从而造成资源泄漏。

使用单独的语句创建 `Widget` 对象并将其存入智能指针，然后将该智能指针传递给 `processWidget` 函数。这样做是因为编译器在不同语句间重新安排操作顺序的余地较小，能避免 `priority` 的调用插入 `new Widget` 和 `std::shared_ptr` 构造函数调用之间。

```cpp
#include <memory>
// 假设的 Widget 类
class Widget {
    // 类的具体实现
};
// 取得处理优先级的函数
int priority() {
    // 函数的具体实现
    return 1; 
}
// 处理动态分配的 Widget 的函数
void processWidget(std::shared_ptr<Widget> pw, int priority) {
    // 函数的具体实现
}
int main() {
    // 在独立语句中将 new 出来的对象存入智能指针
    std::shared_ptr<Widget> pw = std::make_shared<Widget>();
    // 调用 processWidget 函数，不会发生资源泄漏
    processWidget(pw, priority());
    return 0;
}    
```

上述代码先在单独的语句里创建了`Widget`对象并将其存入`std::shared_ptr`，再把该智能指针传递给 `processWidget` 函数，避免了资源泄漏的风险。同时使用了 `std::make_shared` 来创建 `std::shared_ptr`，这比直接使用`new`更高效。







#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 18: 使接口易于正确使用，而难以错误使用</font>
C++ 中存在各种接口，如函数接口、类接口、模板接口等，接口是客户代码和开发者代码交互的地方。

理想情况下，一个完善的接口应使得不符合客户预期的用法无法编译，而能编译的代码就是客户想要的。

1. **构造函数参数错误**：以 `Date` 类的构造函数 `Date(int month, int day, int year);` 为例，客户可能会以错误的顺序传递参数，如 `Date d(30, 3, 1995);` （本应是 `3, 30` 而不是 `30, 3`）；也可能传递非法的代表月或日的数字，如 `Date d(2, 20, 1995);` （可能是输入错误，因为 2 在 3 旁边）。
2. **运算符使用错误**：对用户自定义类型，客户可能会犯类似 `if (a * b = c) ...` 这样的错误（本意是比较，却写成了赋值）。
3. **资源管理错误**：对于 `Investment* createInvestment();` 这样的工厂函数，客户可能会忘记删除返回的指针导致资源泄漏，或者多次删除同一个指针；客户也可能使用错误的资源析构机制（如该用 `getRidOfInvestment` 时却用了 `delete`）。
4. **接口不一致问题**：不同类型的接口在操作上存在不一致，如 Java 中数组用 `length` 属性，`String` 用 `length` 方法，`List` 用 `size` 方法；.NET 中 `Array` 有 `Length` 属性，`ArrayList` 有 `Count` 属性，这增加了开发者的记忆负担和出错概率。

**原因**

1. 接口设计不够完善，没有对参数类型和值进行足够的限制，导致客户容易传递错误的参数。
2. 用户自定义类型的运算符行为与内建类型不一致，使得客户按照对内建类型的习惯使用时出现错误。
3. 接口没有强制客户进行正确的资源管理，客户可能会忘记相关操作或使用错误的资源析构方式。
4. 接口设计缺乏一致性，不同类型的操作方式不同，增加了开发者的记忆成本，容易导致错误。

**解决方法**

1. **引入新类型**：通过引入简单的包装类型来区别日、月和年，如 `struct Day`、`struct Month`、`struct Year`，并将这些类型用于 `Date` 的构造函数，限制参数类型，防止错误参数传递。
2. **限制类型操作**：对类型的操作加上 `const` 限定，使类型的行为与内建类型保持一致，如使 `operator*` 的返回类型具有 `const` 资格，防止客户误操作。
3. **约束对象值**：以 `Month` 类为例，通过函数返回所有有效的 `Month` 值（如 `static Month Jan() { return Month(1); }` 等），预先确定合法的 `Month` 集合，避免客户传入非法的月值。
4. **消除资源管理职责**：让工厂函数返回智能指针，如 `std::shared_ptr<Investment> createInvestment();`强制客户将返回值存入智能指针，由智能指针负责资源管理，减少资源泄漏和错误析构的可能性。
5. **利用智能指针特性**：`shared_ptr` 支持自定义 `deleter`，可以将 `getRidOfInvestment` 绑定为 `deleter`，避免客户使用错误的资源析构机制；`shared_ptr` 还能自动处理 “cross-DLL 问题”，避免在不同 DLL 中创建和删除对象时的运行时错误。

**引入新类型的 **`Date`** 类示例**

```cpp
struct Day {
    explicit Day(int d) : val(d) {}
    int val;
};

struct Month {
    explicit Month(int m) : val(m) {}
    int val;
};

struct Year {
    explicit Year(int y) : val(y) {}
    int val;
};

class Date {
public:
    Date(const Month& m, const Day& d, const Year& y);
};

// 错误示例，参数类型错误
Date d1(30, 3, 1995); 
// 错误示例，参数类型错误
Date d2(Day(30), Month(3), Year(1995)); 
// 正确示例，参数类型正确
Date d3(Month(3), Day(30), Year(1995)); 
```

**返回智能指针并绑定自定义**`deleter`**的**`createInvestment`**示例**

```cpp
#include <memory>
class Investment {
    public:
        virtual ~Investment() {}
};

class Stock : public Investment {};
void getRidOfInvestment(Investment* p) {
    // 资源析构的具体实现
    delete p;
}

std::shared_ptr<Investment> createInvestment() {
    std::shared_ptr<Investment> retVal(static_cast<Investment*>(0), getRidOfInvestment);
    retVal = std::shared_ptr<Investment>(new Stock);
    return retVal;
}

```

🥐补充：<font style="color:rgb(51, 51, 51);">cross-DLL new/delete 对会引起运行时错误是什么意思？</font>

在 C++ 编程中，“cross - DLL new/delete 对会引起运行时错误” 是指在动态链接库（DLL）的使用过程中，当在一个 DLL 中使用`new`操作符分配内存，而在另一个 DLL 或者主程序中使用`delete`操作符来释放该内存时，可能会引发运行时错误。以下是具体的解释：

+ **内存管理机制差异**：不同的 DLL 可能有自己独立的内存管理机制，包括堆的分配和释放方式等。当在一个 DLL 中使用`new`分配内存时，内存是由该 DLL 的堆管理器进行管理和分配的。如果在另一个 DLL 或主程序中使用`delete`来释放这块内存，由于它们的内存管理机制可能不同，可能会导致释放操作与分配时的管理方式不匹配，从而引发错误。
+ **运行时库不一致**：如果不同的 DLL 或主程序使用的运行时库版本不一致，也可能导致`new`和`delete`的实现不同。例如，一个 DLL 使用的是旧版本的运行时库，而另一个 DLL 或主程序使用的是新版本的运行时库，那么在进行跨 DLL 的`new/delete`操作时，可能会因为运行时库的差异而产生错误。这些错误可能表现为程序崩溃、内存泄漏、数据损坏等，严重影响程序的稳定性和正确性。

为了避免这种错误，应该尽量确保`new`和`delete`操作在同一个 DLL 或同一个内存管理环境中进行。如果需要在不同的模块之间传递动态分配的内存，可以考虑使用一些其他的方式来管理内存，提供专门的内存管理接口来确保内存的正确分配和释放。

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 19: 视类设计为类型设计</font>
在C++ 这样的面向对象编程语言中，可通过定义新类来定义新类型，类设计需像语言设计者对待内建类型设计那样用心。设计良好的类具有挑战性，因为设计良好的类型本身就有难度，良好的类型需具备简单自然的语法、符合直觉的语义以及高效的实现，而缺乏计划的类设计难以达成这些目标，类成员函数的执行特性还可能受声明方式影响。

**问题**：

1. 新类型的对象应如何创建和销毁？这会影响构造函数、析构函数以及内存分配和回收函数的设计。
2. 对象的初始化和赋值有何不同？答案决定构造函数和赋值运算符的行为及差异。
3. 以值传递新类型的对象意味着什么？拷贝构造函数定义传值的实现方式。
4. 新类型合法值的限定条件是什么？合法值组合决定类的不变量，影响成员函数内的错误检查、异常抛出及异常规范。
5. 新类型是否适合放进继承图表中？从已存在类继承会受其设计约束，若允许其他类继承自身类，会影响函数是否声明为 virtual 及析构函数的设计。
6. 新类型允许哪种类型转换？涉及隐式转换和显式转换，需考虑在类中写类型转换函数或非显式构造函数等情况。
7. 对于新类型哪些运算符和函数有意义？答案决定类应声明的函数，部分是成员函数，部分不是。
8. 哪些标准函数不应该被接受？需将其声明为 private。
9. 新类型中哪些成员可以被访问？帮助决定成员的访问权限，以及友元类 / 函数的设置和类嵌套的合理性。
10. 新类型的 “undeclared interface” 是什么？它对性能、异常安全和资源使用提供何种保证？这些保证会影响类的实现。
11. 新类型有多大程度的通用性？是否要定义一个类模板来代表一个类型家族。
12. 一个新的类型真的是所需要的吗？是否可以通过定义新的继承类、非成员函数或模板来更好地达成目标。

**解决方法**：  
	在定义新类型(类)之前，要全面考虑上述所有问题，因为类设计就是类型设计，做好类设计能让用户自定义类生成的类型至少和内建类型一样好，使所有努力都有价值。



#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 20: 用 pass-by-reference-to-const（传引用给 const）取代 pass-by-value（传值）</font>
在C++中，缺省情况下以传值方式将对象传入或传出函数，这是从C继承来的特性。函数参数会以实际参数的拷贝进行初始化，函数调用者会收到函数返回值的一个拷贝，这个拷贝由对象的拷贝构造函数生成。

+ **性能问题**：传值方式传递对象会导致构造函数和析构函数的多次调用，带来较高的性能开销。例如传递一个Student对象，会引起一次Student的拷贝构造函数的调用，一次Person的拷贝构造函数的调用，以及四次string的拷贝构造函数调用，同时还有对应的析构函数调用。
+ **切断问题**：当派生类对象以传值方式作为基类对象传递时，基类的拷贝构造函数被调用，派生类对象的特殊特性被“切断”，只剩下纯粹的基类对象，无法表现出派生类的行为。

**原因**

+ **性能问题原因**：一个对象可能包含其他对象或继承自其他对象，构造该对象时需要构造其包含的对象和基类对象，析构时也有对应的操作，所以传值方式传递对象会产生较多的构造和析构函数调用。
+ **切断问题原因**：传值方式传递派生类对象给基类类型的参数时，会调用基类的拷贝构造函数，导致派生类的特殊信息丢失。

**解决方法**：使用传引用给const（pass by reference-to-const）的方式取代传值方式传递对象。这样既可以避免构造函数和析构函数的调用，提高性能，又能避免切断问题。但对于内建类型及STL中的迭代器和函数对象类型，传值通常更合适。

切断问题示例（错误写法）：

```cpp
class Window {
public:
  ...
    std::string name() const;
    virtual void display() const;
};
class WindowWithScrollBars : public Window {
public:
  ...
    virtual void display() const;
};
void printNameAndDisplay(Window w) {
    std::cout << w.name();
    w.display();
}
WindowWithScrollBars wwsb;
printNameAndDisplay(wwsb);
```

解决切断问题示例（正确写法）：

```cpp
void printNameAndDisplay(const Window& w) {
    std::cout << w.name();
    w.display();
}
```







#### **<font style="color:rgb(51, 51, 51);">Item 21: 当你必须返回一个对象时不要试图返回一个引用</font>**
**问题**：在图形绘制系统中，程序员想要实现一个函数`combineShapes`用于将两个图形对象（以代表矩形的`Rectangle`类为例）组合成一个新的图形对象。在实现`combineShapes`函数时，若试图返回引用代替返回值，会面临诸多问题。

1. <u>尝试返回栈上局部变量的引用时，局部变量（即新组合的图形对象在栈上的实例）在函数退出时被销毁，导致返回的引用指向已销毁的对象，程序会进入未定义行为，后续使用该引用会出现错误。</u>
2. <u>尝试返回堆上对象的引用时，存在内存管理问题，因为调用者很难明确知道何时应该释放这个堆上的对象，若不进行释放则会造成内存泄漏，若错误释放则可能导致程序崩溃。</u>
3. <u>尝试返回函数内部</u>`<u>static</u>`<u>对象的引用时，会出现线程安全问题</u>，当多个线程同时调用`combineShapes`函数时，可能会对`static`对象造成竞争访问。并且在比较操作中，由于多个`combineShapes`调用都返回对同一个`static`对象的引用，导致比较结果永远为真，这与实际想要比较两个不同组合图形是否相同的意图不符。

**原因**：引用本质上是实际存在对象的别名，在`combineShapes`函数中若返回引用，就需要返回一个已存在且包含组合结果的`Rectangle`对象的引用。但在调用`combineShapes`前，这样的组合图形对象不一定存在，而且通过不同方式（栈上、堆上、`static`对象等）创建对象并返回引用都存在各自的缺陷，无法满足正确的程序逻辑和内存管理需求。

**解决方法**：对于像`combineShapes`这样必须返回一个新对象的函数，正确的方法是让函数返回一个新对象，而不是试图返回引用。可以将组合后的图形对象在函数内部创建好，然后直接返回这个新对象。可以让编译器厂商去进行优化，例如通过返回值优化（RVO）等技术来降低这种返回新对象带来的性能成本。

```cpp
class Rectangle {
public:
    Rectangle(int width = 0, int height = 0);
   ...
private:
    int w;
    int h;
    friend
    const Rectangle combineShapes(const Rectangle& rect1, const Rectangle& rect2);
};
// 正确的实现方式，返回新对象
inline const Rectangle combineShapes(const Rectangle& rect1, const Rectangle& rect2)
{
    int newWidth = rect1.w > rect2.w? rect1.w : rect2.w;
    int newHeight = rect1.h > rect2.h? rect1.h : rect2.h;
    return Rectangle(newWidth, newHeight);
}
```



补充：局部static变量

<font style="color:rgba(0, 0, 0, 0.85);">C++ 保证了局部</font>`<font style="color:rgba(0, 0, 0, 0.85);">static</font>`<font style="color:rgba(0, 0, 0, 0.85);">变量的初始化在多线程环境下的安全性，但这并不意味着在多线程环境下对该变量的所有操作都是安全的。</font>







#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 22: 将数据成员声明为 private</font>
**问题**：

1. 为什么数据成员不应该声明为public？
    - 从语法一致性角度，若数据成员为public，客户访问类成员时在是否使用圆括号上存在困惑，影响使用体验。
    - 从访问控制角度，public数据成员无法精确控制可存取性，所有人都能读写访问，缺乏灵活性。
    - 从封装角度，public数据成员没有封装，一旦数据成员发生变化，所有使用它的客户代码都可能被破坏，缺乏实现弹性。
2. 为什么protected数据成员也存在问题？
    - 关于语法一致性和条分缕析的访问控制方面，protected和public一样存在问题。
    - 从封装角度，protected数据成员一旦发生变化（如被移除），所有使用它的派生类代码都可能被破坏，和public数据成员一样缺乏封装性。

**方法**：  
	将数据成员声明为private。  
    - 从语法一致性上，能让客户访问对象时无需纠结是否使用圆括号，因为此时访问类成员都通过函数。  
    - 从访问控制上，使用函数来获取和设置数据成员的值，可以实现禁止访问、只读访问、读写访问甚至只写访问等精确的访问控制。  
    - 从封装上，将数据成员隐藏在功能性接口之后，能为各种实现提供弹性，例如在读写时通报其他对象、检验类的不变量、在多线程环境中执行同步任务等，还能确保类的不变量总能被维持，同时预留以后改变实现决策的权力。

根据你提供的文章内容，按照“背景-问题-方法-代码示例”的结构整理如下：

****



#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 23: 用非成员非友元函数取代成员函数</font>
**问题**：  
对于实现`WebBrowser`类中一些组合功能（如`clearEverything`所实现的清空缓存、历史记录和移除cookies），成员函数`clearEverything`和非成员函数`clearBrowser`哪个更好？  
    - 从面向对象原则角度，传统观点认为数据和操作函数应绑定，成员函数是更好选择，但这是对面向对象的误解。  
    - 从封装角度，成员函数`clearEverything`可能导致比非成员函数`clearBrowser`更差的封装性。  
    - 从包装弹性和编译依赖角度，成员函数可能无法提供与非成员函数同样的包装弹性，且会增加不必要的编译依赖。  
    - 从扩展性角度，成员函数在类的扩展性方面存在局限，非成员函数在这方面更具优势。

**方法**：  
用非成员非友元函数取代成员函数。  
    - <u>从封装上，非成员非友元函数不会增加能访问类的</u>`<u>private</u>`<u>部分的函数数量，能获得更强的封装性</u>。因为对于`private`数据成员，只有类的成员函数和友元函数能访问，非成员非友元函数不具备这种访问权限，所以选择非成员非友元函数能更好地保护数据封装。  
    - <u>从包装弹性和编译依赖上，将非成员非友元函数放在同一个</u>`<u>namespace</u>`<u>中，并且可以将相关功能的函数声明在不同的头文件中，这样可以使客户在编译时仅依赖实际使用的那部分系统功能，提高了包装弹性，减少了编译依赖。</u>例如`WebBrowser`类的方便性函数可以按功能（书签相关、cookie相关、打印相关等）分别声明在不同头文件中。  
    -<u> 从扩展性上，客户可以通过在</u>`<u>namespace</u>`<u>中添加更多的非成员非友元函数来扩充功能集合，而类对于客户来说是扩充封闭的</u>（派生类不能访问基类的`private`成员，且不是所有类都设计为基类），所以非成员非友元函数在扩展性方面更具优势。

**代码示例**：

`WebBrowser`类的成员函数示例：

```cpp
class WebBrowser {
public:
  ...
  void clearCache();
  void clearHistory();
  void removeCookies();
  void clearEverything();               // calls clearCache, clearHistory,
                                        // and removeCookies
  ...
};
```

非成员非友元函数示例：

```cpp
void clearBrowser(WebBrowser& wb)
{
  wb.clearCache();
  wb.clearHistory();
  wb.removeCookies();
}
```

将非成员非友元函数放在`namespace`中的示例：

```cpp
namespace WebBrowserStuff {
    class WebBrowser { ... };
    void clearBrowser(WebBrowser& wb);
   ...
}
```

按功能分割`namespace`中函数声明在不同头文件的示例：

```cpp
// header "webbrowser.h" — header for class WebBrowser itself
// as well as "core" WebBrowser-related functionality
namespace WebBrowserStuff {
    class WebBrowser { ... };
   ...                                // "core" related functionality, e.g.
                                    // non-member functions almost
                                    // all clients need
}
// header "webbrowserbookmarks.h"
namespace WebBrowserStuff {
  ...                                   // bookmark-related convenience
}                                       // functions
// header "webbrowsercookies.h"
namespace WebBrowserStuff {
  ...                                   // cookie-related convenience
}                                       // functions
...
```





#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 24: 当类型转换应该用于所有参数时，声明为非成员函数</font>
在C++编程中，讨论对于自定义的数值类型（如表示有理数的`Rational`类），实现算术运算（如乘法`operator*`）时，选择成员函数、非成员函数还是非成员的友元函数来实现？

- 若将`operator*`实现为`Rational`类的成员函数，虽然能实现`Rational`对象之间的乘法运算，但在混合模式操作中，只有部分情况能工作，如`oneHalf * 2`能编译通过，而`2 * oneHalf`会报错。这是因为成员函数中，对应于成员函数被调用的那个对象的隐含参数（`this`指针指向的对象）没有资格进行隐式类型转换，只有列在参数列表中的参数才有资格进行隐式类型转换。

1. `Rational`类的定义（构造函数允许隐式类型转换）：

```cpp
class Rational {
public:
    Rational(int numerator = 0,        // ctor is deliberately not explicit;
             int denominator = 1);     // allows implicit int-to-Rational
                                       // conversions

    int numerator() const;             // accessors for numerator and
    int denominator() const;           // denominator — see Item 22

private:
  ...
};
```

2. `Rational`类的成员函数实现`operator*`（不支持混合模式操作的交换性）：

```cpp
class Rational {
public:
 ...

 const Rational operator*(const Rational& rhs) const;
};
```

使用示例：

 - 若`Rational`的构造函数是显式的，那么混合模式操作（如`oneHalf * 2`和`2 * oneHalf`）都无法编译通过，不满足支持混合运算的需求。



**方法**：  
	将`operator*`声明为非成员函数，这样可以允许隐式类型转换应用于所有参数，从而支持混合模式操作。  
    - 因为非成员函数的参数都列在参数列表中，所以所有参数都有资格进行隐式类型转换，能够满足乘法运算交换性以及混合模式操作的要求。  
    - 同时<u>，在这种情况下，</u>`<u>operator*</u>`<u>不需要作为</u>`<u>Rational</u>`<u>类的友元函数，因为它能够根据</u>`<u>Rational</u>`<u>的</u>`<u>public</u>`<u>接口完全实现，应尽量避免使用友元函数，以减少不必要的复杂性。</u>

`Rational`类的非成员函数实现`operator*`（支持混合模式操作的交换性）：

```cpp
class Rational {
  ...                                             // contains no operator*
};
const Rational operator*(const Rational& lhs,     // now a non-member
                         const Rational& rhs)     // function
{
  return Rational(lhs.numerator() * rhs.numerator(),
                  lhs.denominator() * rhs.denominator());
}
```

使用示例：

```cpp
Rational oneFourth(1, 4);
Rational result;

result = oneFourth * 2;                           // fine
result = 2 * oneFourth;                           // hooray, it works!
```





#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 25: 考虑支持不抛异常的 swap</font>


这个item需要仔细看。

**问题**

    - 缺省的`std::swap`实现对于某些类型可能效率低下

```cpp
namespace std {
  template<typename T>
  void swap(T& a, T& b)
  {
    T temp(a);
    a = b;
    b = temp;
  }
}
```

使用“pimpl idiom”（指针指向实现）设计的类型，缺省实现会进行多次不必要的拷贝。

当`Widget`和`WidgetImpl`是类模板时，无法对`std::swap`函数模板进行部分特化，且不能在`std`命名空间中增加新的模板（包括函数等）。C++ 标准规定了函数模板不能进行部分特化，只能进行全特化。`std::swap`是一个函数模板，当`Widget`和`WidgetImpl`是类模板时，如果你想为它们特化`std::swap`，只能进行全特化，而不能进行部分特化。对于`std::swap`，不能只针对模板参数的一部分进行特化，如只针对`Widget`模板的某个特定类型参数特化`swap`，而让其他参数保持通用模板的行为，这是不被允许的。









    - 调用`swap`时，不确定应调用哪个版本的`swap`函数，以及如何确保调用到最合适的版本。
    - 为类（及类模板）提供强大的异常安全保证时，`swap`的成员版本不能抛出异常。

**背景**

    - `swap`是STL的一部分，是异常安全编程的支柱和压制自赋值可能性的通用机制，其缺省实现通过拷贝构造函数和拷贝赋值运算符进行对象交换，对于某些类型效率不高。
    - C++允许为自己创建的类型完全特化标准模板（如`swap`），但不允许在`std`命名空间中增加新的模板等内容，违反规则程序行为未定义。
    - C++的名字查找规则（参数依赖查找或Koenig查找）会影响`swap`函数的调用选择。
    - `swap`的重要应用之一是为类（及类模板）提供异常安全保证，该技术基于`swap`成员版本不抛异常的假设。

**解决方法**

    - 若`std::swap`对类型低效：
        * 提供一个高效且不抛异常的`public`的`swap`成员函数。
        * 在类或模板所在的命名空间中提供一个非成员的`swap`，调用成员`swap`函数。
        * 若为类（非类模板），特化`std::swap`，并调用成员`swap`函数。
    - 调用`swap`时：
        * 在函数中使用`using std::swap;`使`std::swap`可见。
        * 调用`swap`时不使用任何命名空间限定条件。

**代码示例**

    - -使用“pimpl idiom”的`Widget`类及特化`std::swap`（最初不能编译的版本）：

```cpp
class WidgetImpl {
public:
  ...
private:
  int a, b, c;
  std::vector<double> v;
  ...
};

class Widget {
public:
  Widget(const Widget& rhs);
  Widget& operator=(const Widget& rhs)
  {
   ...
   *pImpl = *(rhs.pImpl);
   ...
  }
  ...
private:
  WidgetImpl *pImpl;
};

namespace std {
  template<>
  void swap<Widget>(Widget& a, Widget& b)
  {
    swap(a.pImpl, b.pImpl);
  }
}
```

- 修正后的`Widget`类及特化`std::swap`（增加`swap`成员函数）：

```cpp
class Widget {
public:
  ...
  void swap(Widget& other)
  {
    using std::swap;
    swap(pImpl, other.pImpl);
  }
  ...
};

namespace std {
  template<>
  void swap<Widget>(Widget& a, Widget& b)
  {
    a.swap(b);
  }
}
```

- `Widget`类模板及试图部分特化`std::swap`（非法代码）：

```cpp
template<typename T>
class WidgetImpl { ... };

template<typename T>
class Widget { ... };

namespace std {
  template<typename T>
  void swap<Widget<T> >(Widget<T>& a, Widget<T>& b)
  { a.swap(b); }
}
```

- 正确的`Widget`类模板及非成员`swap`（不在`std`命名空间）：

```cpp
namespace WidgetStuff {
  ...
  template<typename T>
  class Widget { ... };
  ...
  template<typename T>
  void swap(Widget<T>& a, Widget<T>& b)
  {
    a.swap(b);
  }
}
```

- 调用`swap`的正确示例：

```cpp
template<typename T>
void doSomething(T& obj1, T& obj2)
{
  using std::swap;
  ...
  swap(obj1, obj2);
  ...
}
```

- 调用`swap`的错误示例（使用命名空间限定）：

```cpp
template<typename T>
void doSomething(T& obj1, T& obj2)
{
  ...
  std::swap(obj1, obj2); 
  ...
}
```

### 
#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 26: 只要有可能就推迟变量定义</font>
在C++中，定义带有构造函数和析构函数的类型变量时，变量定义处会产生构造成本，变量离开作用域时会产生析构成本。

**问题**

1. 函数中可能定义无用变量，如在`encryptPassword`函数中，如果抛出异常，提前定义的`encrypted`变量就无用，但仍会产生构造和析构成本。

`encryptPassword`**函数中变量定义过早的情况**

```cpp
// this function defines the variable "encrypted" too soon
std::string encryptPassword(const std::string& password)
{
    using namespace std;
    string encrypted;
    if (password.length() < MinimumPasswordLength) {
        throw logic_error("Password is too short");
    }
  ...                        // do whatever is necessary to place an
                             // encrypted version of password in encrypted
    return encrypted;
}
```

2. 变量定义时若未进行有效初始化，先默认构造再赋值，会导致效率低下。

尽量在定义变量时就使用需要的值进行初始化，跳过默认构造然后赋值的低效操作，如在`encryptPassword`函数中，使用`password`初始化`encrypted`。

```cpp
// finally, the best way to define and initialize encrypted
std::string encryptPassword(const std::string& password)
{
  ...                                     // check length
    string encrypted(password);             // define and initialize
                                          // via copy constructor
    encrypt(encrypted);
    return encrypted;
}
```

3. 对于仅在循环内使用的变量，存在在循环外定义并每次循环赋值，还是在循环内定义的选择问题。

对于循环内使用的变量，除非确定赋值比构造函数/析构函数对成本更低且处于代码性能敏感部分，否则默认在循环内定义变量。

```cpp
// Approach A: define outside loop
Widget w;
for (int i = 0; i < n; ++i){
    w = some value dependent on i;
  ...
}

// Approach B: define inside loop
for (int i = 0; i < n; ++i) {
    Widget w(some value dependent on i);
  ...
}
```





问题

强制转型存在多种语法形式，旧风格的 C 风格和函数风格强制转型与新风格的 C++ 风格强制转型（const_cast、dynamic_cast、reinterpret_cast、static_cast）并存。

**旧风格强制转型**

    - **C 风格**：`(T) expression // cast expression to be of type T`
    - **函数风格**：`T(expression) // cast expression to be of type T`
    - **新风格强制转型**
        * `const_cast<T>(expression)`：一般用于强制消除对象的常量性。
        * `dynamic_cast<T>(expression)`：主要用于执行“安全的向下转型”，确定对象是否是继承体系中的特定类型。
        * `reinterpret_cast<T>(expression)`：用于底层的强制转型，导致实现依赖的结果。
        * `static_cast<T>(expression)`：可用于强制隐型转换及一些转换的反向转换，但不能将 const 对象转型为 non-const 对象。

dynamic_cast 实现通常较慢，见下面。

**解决方法**

1. <u>尽量避免使用强制转型，尤其是在性能敏感代码中避免使用 dynamic_casts。如果设计中需要强制转型，尝试开发无强制转型的替代方案。</u>
2. 若<u>必须使用强制转型，将其隐藏在函数内部，通过函数接口保护调用者，使其无需在自己的代码中添加强制转型。</u>
3. <u>优先使用 C++ 风格的强制转型（const_cast、dynamic_cast、reinterpret_cast、static_cast）替代旧风格的强制转型（C 风格和函数风格），因为新风格在代码中更易识别，能更精确指定强制转型目的，便于编译器诊断错误。</u>
4. <u>对于通过基类指针或引用操作派生类对象且需要执行派生类操作的情况，可采用两种方法避免使用 dynamic_cast：一是使用存储直接指向派生类对象指针（通常是智能指针）的容器，避免通过基类接口操作对象；二是在基类中提供虚函数并给出默认实现，通过基类接口调用虚函数来实现相应操作。</u>
5. <u>避免包含极联 dynamic_casts 的设计，此类代码应替换为基于虚函数调用的代码。</u>



1. **使用旧风格强制转型调用 explicit 构造函数示例**

```cpp
//例1
class Widget {
public:
    explicit Widget(int size);
  ...
};
void doSomeWork(const Widget& w);
doSomeWork(Widget(15));                   
doSomeWork(static_cast<Widget>(15));       

//例2
int x, y;
...
double d = static_cast<double>(x)/y;        
```

**避免使用 dynamic_cast 的两种方法示例**

`dynamic_cast` 的实现效率较为低下。

```cpp
#include <iostream>
#include <typeinfo>
class Animal {
public:
    virtual ~Animal() = default; // 虚析构函数，确保 RTTI 被启用
};

class Dog : public Animal {
public:
    void bark() const { std::cout << "Woof!" << std::endl; }
};

class Cat : public Animal {
public:
    void meow() const { std::cout << "Meow!" << std::endl; }
};

int main() {
    Animal* animal1 = new Dog();
    Animal* animal2 = new Cat();
    // 使用 dynamic_cast 进行向下转型
    Dog* dog1 = dynamic_cast<Dog*>(animal1); // 成功，animal1 指向 Dog
    Dog* dog2 = dynamic_cast<Dog*>(animal2); // 失败，animal2 指向 Cat
    if (dog1) {
        dog1->bark(); // 输出 "Woof!"
    }
    if (!dog2) {
        std::cout << "Not a Dog!" << std::endl; // 输出 "Not a Dog!"
    }
    delete animal1;
    delete animal2;
    return 0;
}
```

`dynamic_cast<Dog*>(animal)`为例：

**访问虚函数表**:通过 `animal` 的 `vptr`（每个对象的隐式成员）获取虚函数表地址。

**获取类型信息**:从虚函数表中提取该对象的 **类型信息指针**（指向 `type_info` 对象）。

**类型匹配检查**:比较目标类型 `Dog` 的 `type_info` 与对象的 `type_info`，判断是否匹配或存在继承关系。

**计算偏移量(若需要)**:如果是多继承或虚继承，可能需要计算指针的偏移量以正确转换。

每次`dynamic_cast`需要多次内存访问(虚函数表、类型信息对象)和类型比较,比`static_cast`(仅简单指针偏移计算)慢。



**解决方法1：使用存储派生类对象指针的容器示例**

```cpp
// 改进后
typedef std::vector<std::shared_ptr<SpecialWindow> > VPSW;
VPSW winPtrs;
...
for (VPSW::iterator iter = winPtrs.begin();        // better code: uses
     iter != winPtrs.end();                        // no dynamic_cast
     ++iter)
    (*iter)->blink();
```

**解决方法2：在基类中提供虚函数示例**

```cpp
class Window {
public:
    virtual void blink() {}                      
  ...                                           
};                                                                                           // a bad idea
class SpecialWindow: public Window {
public:
    virtual void blink() { ... }                 
  ...                                         
};
typedef std::vector<std::shared_ptr<Window> > VPW;
VPW winPtrs;                                    
                                                
...                                             
for (VPW::iterator iter = winPtrs.begin();
     iter != winPtrs.end();
     ++iter)                                    
    (*iter)->blink();                           
```





**问题**：

成员函数返回对象内部构件的句柄(引用、指针或迭代器),导致对象的封装性被破坏，如 Rectangle 类的 upperLeft 和 lowerRight 函数返回引向内部 Point 对象的引用，使原本应被封装的 private 数据成员可被外部修改。

**原因**：

+ 即使数据成员被声明为 private 等具有一定封装性的访问级别，但返回其句柄的成员函数会使这些数据成员实际上被公开，因为外部可通过句柄访问和修改这些数据。
+ const 成员函数返回指向外部数据的引用时，由于二进制位常量性的局限性，调用者可通过该引用修改数据。
+ 当函数返回一个包含内部构件的临时对象时，该对象被销毁后，返回的指向其内部构件的句柄就会变为空悬句柄。

```cpp
class Point {
public:
    Point(int x, int y);
   ...
    void setX(int newVal);
    void setY(int newVal);
   ...
};
struct RectData {
    Point ulhc;
    Point lrhc;
};
class Rectangle {
public:
   ...
    Point& upperLeft() const { return pData->ulhc; }
    Point& lowerRight() const { return pData->lrhc; }
   ...
private:
    std::shared_ptr<RectData> pData;
};
```

上面upperleft和lowerRight const虽然是const函数，但是可能导致 const 成员函数无法实现其应有的 const 效果，const 成员函数返回的引用可用于修改对象内部状态。

返回值也需要改为为const引用。

```cpp
...
const Point& upperLeft() const { return pData->ulhc; }
const Point& lowerRight() const { return pData->lrhc; }
...
```

**解决方法**：

+ <u>对于返回对象内部构件句柄的成员函数，将其返回类型声明为 const，如 Rectangle 类的 upperLeft 和 lowerRight 函数，将返回类型改为 const Point&，这样可限制客户只能读取数据而不能写入数据，一定程度上恢复了 const 成员函数的 const 效果，并在一定程度上保护了封装性。</u>
+ <u>尽量避免返回对象内部构件的句柄，除非是像 operator[] 这样的特殊情况（如 string 和 vector 的 operator[] 用于取出单独元素）。</u>

<u></u>

<u></u>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 29: 争取异常安全（exception-safe）的代码</font>
在C++编程中，尤其是在多线程环境下，需要考虑代码的异常安全问题。

<u>没有看懂</u>

**问题**

```cpp
void PrettyMenu::changeBackground(std::istream& imgSrc)
{
  lock(&mutex);                      // acquire mutex (as in Item 14)
  delete bgImage;                    // get rid of old background
  ++imageChanges;                    // update image change count
  bgImage = new Image(imgSrc);       // install new background
  unlock(&mutex);                    // release mutex
}
```

不满足异常安全的两条要求：

+ 存在资源泄露风险。当new Image(imgSrc)表达式产生异常时，对unlock的调用不会执行，互斥体将被永远挂起。
+ 可能导致数据结构恶化。若“new Image(imgSrc)”抛出异常，bgImage会指向一个被删除对象，且imageChanges已被增加（内存泄漏问题）

异常安全函数必须提供基本保证、强力保证或不抛出保证中的一种，若不提供则不是异常安全的。

很多C++遗留代码在编写时未考虑异常安全，导致整个系统可能不是异常安全的，因为只要有一个函数不是异常安全的，就可能导致资源泄漏和数据结构恶化。

**解决方案**

1. 对于资源泄露问题，使用对象管理资源的方法，如Item 13中所述，以及引入Lock类（如Item 14中所示）来确保互斥体被释放（RAII）

```cpp
void PrettyMenu::changeBackground(std::istream& imgSrc)
{
  Lock ml(&mutex);                 // from Item 14: acquire mutex and
                                   // ensure its later release
  delete bgImage;
  ++imageChanges;
  bgImage = new Image(imgSrc);
}
```

2. 对于数据结构恶化问题，可选择为函数提供基本保证、强力保证或不抛出保证中的一种。

一般应提供实际可达到的最强力的保证，若无法提供强力保证则提供基本保证，尽量避免不提供保证的情况（除非调用遗留代码且别无选择）。

```cpp
class PrettyMenu {
  ...
  std::shared_ptr<Image> bgImage;
  ...
};
void PrettyMenu::changeBackground(std::istream& imgSrc){
  Lock ml(&mutex);
  bgImage.reset(new Image(imgSrc));  // replace bgImage's internal
                                     // pointer with the result of the
                                     // "new Image" expression
  ++imageChanges;
}
```

为changeBackground函数提供强力保证的具体措施：

+ 将PrettyMenu的bgImage数据成员类型从内建的Image*指针改为智能资源管理指针（如shared_ptr），利用智能指针自动管理资源的特性，避免手动删除资源的风险。
+ 重新排列changeBackground中的语句，直到图像真正发生变化时，再增加imageChanges。
+ 采用copy-and-swap策略，将对象的全部数据放入单独的实现对象中（如PMImpl结构体），通过指向实现对象的指针来管理对象。先对拷贝对象进行修改，若修改过程中无异常，再将修改后的对象与原对象进行交换。<u>copy-and-swap策略虽然能实现强力保证，但需要对对象进行拷贝，可能耗费大量时间和空间资源，导致效率问题（为什么）</u>。

采用copy-and-swap策略（结合pimpl idiom）后的changeBackground函数实现：

```cpp
struct PMImpl {                               // PMImpl = "PrettyMenu
  std::shared_ptr<Image> bgImage;        // Impl."; see below for
  int imageChanges;                           // why it's a struct
};

class PrettyMenu {
  ...
private:
  Mutex mutex;
  std::tr1::shared_ptr<PMImpl> pImpl;
};

void PrettyMenu::changeBackground(std::istream& imgSrc)
{
  using std::swap;                            // see Item 25
  Lock ml(&mutex);                            // acquire the mutex
  std::shared_ptr<PMImpl>                // copy obj. data
    pNew(new PMImpl(*pImpl));

  pNew->bgImage.reset(new Image(imgSrc));     // modify the copy
  ++pNew->imageChanges;
  swap(pImpl, pNew);                          // swap the new
                                              // data into place
}                                             // release the mutex
```



#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 30: 理解 inline 化的介入和排除</font>
**问题：inline函数可能带来的****<font style="color:#DF2A3F;">代码膨胀问题</font>**

    - **原因**：inline函数的思想是用函数本体代替每一处对这个函数的调用，这可能会增加目标代码的大小。在有限内存的机器上，过分热衷于inline化会使得程序对于可用空间来说过于庞大。<u>即使使用虚拟内存，inline引起的代码膨胀也会导致附加</u>**<u>的分页调度</u>**<u>，</u>**<u>减少指令缓存命中率</u>**<u>，以及随之而来的性能损失。</u>
    - **解决方法**：<u>将大部分inline限制在小的，调用频繁的函数上，这样可以最小化潜在的</u>

**问题：编译器可能忽略inline请求**

    - **原因**：大多数编译器拒绝它们认为太复杂的inline函数（例如，那些包含循环或者递归的），而且，除了最细碎的以外的全部虚拟函数的调用都不会被inline化。因为虚拟意味着“等待，直到运行时才能断定哪一个函数被调用”，而inline意味着“执行之前，用被调用函数取代调用的地方”，编译器在不知道调用哪个函数时无法进行inline化。
    - **解决方法**：<u>幸运的是，大多数编译器都有一个诊断层次，在它们不能inline化一个提出的函数时，会导致一个警告（参见Item 53），可根据警告信息进行调整。</u>

**问题：构造函数和析构函数不一定适合inline化**

    - <u>看似简单甚至为空的构造函数和析构函数，实际上C++对对象创建和销毁有各种保证，编译器会插入一些代码来实现这些保证，例如调用基类和数据成员的构造函数、处理异常等，这些代码会影响它们对于inline化的吸引力</u>。如果基类或成员的构造函数也是inline的，还会进一步增加代码量。
    - **解决方法**：在决定是否inline化构造函数和析构函数时，要充分考虑上述因素，不能简单决定。

```cpp
class Base {
public:
 ...
private:
   std::string bm1, bm2; 
};
class Derived: public Base {
public:
  Derived() {} 
  ...
private:
  std::string dm1, dm2, dm3; 
};
```

**问题：库中inline函数的二进制升级问题**

    - **<u>原因</u>**<u>：库中的客户可见的inline函数，客户会将函数本体编译到他们的应用程序中。如果库的实现者后来决定修改该inline函数，所有使用了该函数的客户都必须重新编译。</u>
    - **解决方法**：库设计者必须评估声明函数为inline的影响，<u>尽量避免在库中使用客户可见的inline函数，若使用非inline函数</u>，对函数的改变只需要客户重新连接，可减轻负担。

**问题：大多数调试器会与inline函数发生冲突**

    - **原因**：inline函数在编译时会被展开，在调试时可能无法在一个“不存在”的函数中设置断点。虽然一些构建环境设法支持inline函数的调试，但多数环境还是简单地为调试构建取消了inline化。
    - **解决方法**：最初，不要inline任何东西，或者至少要将inline化的范围限制在那些必须inline的（参见Item 46）和那些实在微不足道的函数上，这样可以使调试器的使用变得容易。





#### <font style="color:rgb(51, 51, 51);">Item 31: 最小化文件之间的编译依赖</font>
1. **问题1：C++中类定义导致的编译依赖问题**
2. C++ 中一个类定义不仅指定了类的接口，还包含相当数量的实现细节。例如在定义 `Person` 类时，若不访问其实现所用到的 `string`、`Date` 和 `Address` 类的定义（一般通过 `#include` 指令提供），`Person` 类就无法编译。这样就建立了定义 `Person` 的文件和这些头文件之间的编译依赖关系，一旦这些头文件或其依赖的文件发生变化，包含 `Person` 类的文件和使用了 `Person` 的文件都必须重新编译，带来层叠编译依赖问题。

**解决方法1**：

+ 采用 `pimpl` 惯用法（Handle类），将类的实现隐藏在一个指针后面。把类分为仅提供接口的主类和实现接口的实现类，主类除了一个指向实现类的指针外不包含其他数据成员，使客户脱离实现细节，实现接口和实现的分离。

**Handle类(**`pimpl`**惯用法)**

```cpp
#include <string>
#include <memory>

class PersonImpl;
class Date;
class Address;
class Person {
public:
    Person(const std::string& name, const Date& birthday,
           const Address& addr);
    std::string name() const;
    std::string birthDate() const;
    std::string address() const;
private:
    std::tr1::shared_ptr<PersonImpl> pImpl;
};

// Person 成员函数实现
#include "Person.h"
#include "PersonImpl.h"

Person::Person(const std::string& name, const Date& birthday,
               const Address& addr)
    : pImpl(new PersonImpl(name, birthday, addr))
{}

std::string Person::name() const
{
    return pImpl->name();
}
```

**方法2：  **让类成为 `Interface` 类（特殊种类的抽象基类），为派生类指定接口。其典型特征是没有数据成员，没有构造函数，有一个虚析构函数和一组指定接口的纯虚函数，客户针对其指针或引用编程，通过 `factory` 函数（虚拟构造函数）创建对象。

+ `Person` 类被定义为抽象基类m抽象基类本身不能被实例化，其主要作用是为派生类指定接口。客户代码只能针对 `Person` 的指针或引用进行编程，而不需要知道具体派生类（如 `RealPerson`）的实现细节。
+ 对于使用 `Person` 类的客户代码来说，只需要包含 `Person` 类的声明（通常在头文件中），而不需要包含派生类 `RealPerson` 的定义。这就减少了客户代码与派生类实现之间的编译依赖。当派生类 `RealPerson` 的实现发生变化时（例如修改了成员变量或成员函数的实现），只要 `Person` 类的接口（即纯虚函数的声明）不变，客户代码就不需要重新编译。

`Person` 类中定义了静态的工厂函数 `create`，用于创建具体的派生类对象（如 `RealPerson` 对象）。客户代码通过调用 `Person::create` 函数来获取指向 `Person` 接口的智能指针，而不需要直接依赖于 `RealPerson` 类的构造函数。

```cpp
class Person {
public:
    virtual ~Person();
    virtual std::string name() const = 0;
    virtual std::string birthDate() const = 0;
    virtual std::string address() const = 0;
};

static std::shared_ptr<Person> 
    create(const std::string& name,const Date& birthday,
           const Address& addr);

class RealPerson: public Person {
public:
    RealPerson(const std::string& name, const Date& birthday,
               const Address& addr)
        : theName(name), theBirthDate(birthday), theAddress(addr)
    {}
    virtual ~RealPerson() {}
    std::string name() const;
    std::string birthDate() const;
    std::string address() const;
private:
    std::string theName;
    Date theBirthDate;
    Address theAddress;
};

std::shared_ptr<Person> Person::create(const std::string& name,
                                            const Date& birthday,
                                            const Address& addr){
    return std:::shared_ptr<Person>(new RealPerson(name, birthday, addr));
}
```

**问题：前向声明的类无法确定对象大小**

+ **原因**：当编译器遇到定义对象的语句时，需要知道对象的大小来分配空间。对于前向声明的类，编译器无法从类的声明中得知对象大小，因为省略了实现细节的类定义无法提供足够信息。
+ **解决方法**：
    - 采用 `pimpl` 惯用法（Handle类），主类中仅包含指向实现类的指针，通过指针间接访问实现类的对象，避免了直接定义实现类对象带来的大小确定问题。
    - 对于 `Interface` 类，客户针对其指针或引用编程，同样不需要在编译时确定对象的具体大小。

**问题：函数声明可能引入不必要的编译依赖**

    - **原因**：在声明使用某个类的函数时，如果直接依赖类的定义，而不是仅依赖类的声明，会引入不必要的编译依赖。例如在声明函数时，即使函数通过传值方式传递或返回某个类，也不一定要有该类的定义。
    - **解决方法**：
    - 当对象的引用和指针可以满足需求时，避免使用对象。仅需一个类型的声明，就可以定义到这个类型的引用或指针，而定义一个类型的对象则必须要有该类型的定义。
    - 只要能做到，就用对类声明的依赖替代对类定义的依赖。在声明使用类的函数时，仅依赖类的声明，将提供类定义的责任从声明函数的头文件转移到客户的包含函数调用的文件，消除客户对不需要的类型的依赖。
    - **代码示例**：

**问题：头文件管理混乱可能导致编译依赖问题**

    - **原因**：如果头文件中同时包含声明和定义，或者客户自行前向声明某些东西，可能导致头文件之间的依赖关系混乱，增加编译依赖的复杂性。
    - **解决方法**：
        * <u>为声明和定义分别提供头文件。头文件成对出现，一个用于声明，另一个用于定义，且必须保持一致。库的客户应该总是 </u>`<u>#include</u>`<u> 声明文件，而不是自己前向声明某些东西，库的作者应该提供两个头文件。</u>

**问题：Handle类和Interface类带来的运行时和内存成本问题**

    - **Handle类**：成员函数通过实现的指针访问对象数据，增加了间接层；每个对象需要额外存储实现指针的大小；实现指针需要在构造函数中初始化为指向动态分配的对象，带来动态内存分配及释放的成本和可能的 `bad_alloc` 异常。
    - **Interface类**：每个函数调用都是虚拟的，需要支付间接跳转的成本；从 `Interface` 派生的对象必须包含一个 `virtual table` 指针，可能增加存储对象所需的内存量；`Handle` 类和 `Interface` 类一般设计为隐藏实现细节，难以大量使用 `inline` 函数。
    - **解决方法**：在开发过程中，使用 `Handle` 类和 `Interface` 类来最小化实现变化对客户的影响；<u>当性能（速度和/或大小）上的差异足以证明增加类之间的耦合是值得的时候，可以用具体类取代 </u>`<u>Handle</u>`<u> 类和 </u>`<u>Interface</u>`<u> 类供产品使用。</u>







#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 32: 确保 public inheritance 模拟 "is-a"</font>
1. **问题1：企鹅继承自鸟的错误设计**
    - **原因**：用C++表达企鹅继承自鸟时，因英语表达“鸟能飞”的不严谨，错误地认为所有鸟都能飞，导致企鹅类继承了能飞的属性，而实际上企鹅不能飞。
    - **解决方法**：重新设计继承体系，区分能飞的鸟和不能飞的鸟，避免企鹅类继承能飞的属性；或者为企鹅重定义fly函数，使其产生运行时错误，但这种方法存在运行时才能发现错误的问题，不如通过编译器阻止的设计好。
    - **代码**：

```cpp
// 更好的继承体系设计
class Bird {
  ...                                       // no fly function is declared
};

class FlyingBird: public Bird {
public:
  virtual void fly();
  ...
};

class Penguin: public Bird {
  ...                                       // no fly function is declared
};

// 为企鹅重定义fly函数产生运行时错误
void error(const std::string& msg);       // defined elsewhere

class Penguin: public Bird {
public:
  virtual void fly() { error("Attempt to make a penguin fly!");}
  ...
};
```

**问题：正方形继承自矩形的错误设计**

+ **原因**：从数学角度看正方形是矩形的一种特殊情况，但在C++中，矩形的宽度和高度可以独立变化，而正方形的宽度和高度必须相等，这导致一些适用于矩形的操作（如改变宽度时高度不变）不适用于正方形，与public inheritance断言（适用于基类对象的每一件事也适用于派生类对象）相矛盾。
+ **解决方法**：认识到在C++中不能简单地用public inheritance模拟正方形和矩形的关系，在设计时加入对继承的正确理解，避免因直觉错误导致的设计问题。

```cpp
class Rectangle {
public:
  virtual void setHeight(int newHeight);
  virtual void setWidth(int newWidth);
  virtual int height() const;               // return current values
  virtual int width() const;

  ...
};
void makeBigger(Rectangle& r)               // function to increase r's area
{
  int oldHeight = r.height();
  r.setWidth(r.width() + 10);               // add 10 to r's width
  assert(r.height() == oldHeight);          // assert that r's
}                                           // height is unchanged

class Square: public Rectangle {...};
Square s;
...
assert(s.width() == s.height());           // this must be true for all squares
makeBigger(s);                             // by inheritance, s is-a Rectangle,
                                           // so we can increase its area
assert(s.width() == s.height());           // this must still be true
                                           // for all squares
```







#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 33: 避免覆盖（hiding）“通过继承得到的名字”</font>
**问题：派生类中名字覆盖基类中名字导致的意外行为**

+ **原因**<font style="color:rgb(28, 31, 35);">：C++中作用域的规则，内层作用域的名字会覆盖外层作用域的名字，当派生类继承基类时，派生类作用域嵌套在基类作用域中。在派生类中定义与基类中同名的函数（无论参数类型、函数是否为虚函数）时，基于作用域的名字覆盖规则，基类中同名函数会被派生类中的同名函数覆盖，使得从基类继承的一些函数在派生类中不可见，这违反了公有继承中基类和派生类之间“is-a”关系的基本原则。</font>

**<font style="color:#DF2A3F;">解决方法1：使用using声明</font>**

<font style="color:rgb(28, 31, 35);">在派生类中使用</font>`<font style="color:rgb(28, 31, 35);">using</font>`<font style="color:rgb(28, 31, 35);">声明，使基类中特定名字的函数在派生类作用域中可见（并且保持公有属性）。</font>

```cpp
class Base {
private:
  int x;
public:
  virtual void mf1() = 0;
  virtual void mf1(int);
  virtual void mf2();
  void mf3();
  void mf3(double);
  ...
};

class Derived: public Base {
public:
  using Base::mf1;    // 使Base中所有名为mf1的函数在Derived作用域中可见（并且公有）
  using Base::mf3;    // 使Base中所有名为mf3的函数在Derived作用域中可见（并且公有）
  virtual void mf1();
  void mf3();
  void mf4();
  ...
};
```

**补充：派生类 Derived 中的 mf3 的重写与隐藏**  
基类 Base 的 mf3有两个重载版本：  
`void mf3();（无参数）  
void mf3(double);（带 double 参数）`  
派生类 Derived 的 mf3重写了基类的无参数版本 void mf3();。使用 using Base::mf3; 将基类的所有 mf3 函数（包括 mf3() 和 mf3(double)）引入派生类的作用域。

调用 mf3() 的行为，当通过派生类对象调用 无参数的 mf3() 时，派生类的 mf3() 会覆盖基类的 mf3()：  
但通过 using Base::mf3;，基类的 mf3(double) 也被引入派生类的作用域，形成重载。  
重写不会冲突

派生类的 mf3() 与基类的 mf3() 是同名同参数的重写，这属于覆盖（Override），而非隐藏（因为基类的 mf3() 是虚函数）。基类的 mf3(double) 是非虚函数且参数不同，因此不会被覆盖，而是形成重载。

**<font style="color:#DF2A3F;">解决方法2：使用转调函数</font>**

当私有继承且只想继承基类中某个特定版本的函数时，使用转调函数。转调函数还可用于不支持using声明将继承名字引入派生类作用域的老式编译器。

```cpp
class Base {
public:
  virtual void mf1() = 0;
  virtual void mf1(int);
  ...                                    // 如之前一样
};

class Derived: private Base {
public:
  virtual void mf1()                   // 转调函数；隐式内联（见Item 30）
  { Base::mf1(); }
  ...
};
```

**补充：**<font style="color:rgb(44, 44, 54);">转调函数显式地将派生类的函数调用委托给基类的同名函数，从而解决名称隐藏的问题。</font>

**总结建议**<font style="color:rgb(28, 31, 35);"></font>

+ <font style="color:rgb(28, 31, 35);">派生类中的名字会覆盖基类中的名字，在公有继承中，这种情况通常不是我们期望的，因为它违反了公有继承中基类和派生类之间“is-a”的关系。</font>
+ <font style="color:rgb(28, 31, 35);">为了使被隐藏的名字重新可见，可以使用</font>`<font style="color:rgb(28, 31, 35);">using</font>`<font style="color:rgb(28, 31, 35);">声明或者转调函数 。当公有继承时，推荐使用</font>`<font style="color:rgb(28, 31, 35);">using</font>`<font style="color:rgb(28, 31, 35);">声明；当私有继承且有特定需求时，可使用转调函数。</font>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 34: 区分 接口继承和 实现继承</font>
**问题**

<font style="color:rgb(28, 31, 35);">在C++的继承体系中，对于成员函数的继承，缺乏经验的类设计者常出现两种错误：一是将所有函数声明为非虚拟（non - virtual），二是将所有成员函数声明为虚拟（virtual）。</font>

**解决方法及代码**

<font style="color:rgb(28, 31, 35);">理解virtual函数的作用和性能影响，在设计作为基类的类时，合理地将需要派生类有不同实现的函数声明为virtual函数。</font>

<font style="color:rgb(28, 31, 35);">例如在图形应用程序的Shape类继承体系中</font>

```cpp
class Shape {
public:
    virtual void draw() const = 0; // 纯虚函数，仅继承接口
    virtual void error(const std::string& msg); // 简单虚函数，继承接口和默认实现
    int objectID() const; // 非虚函数，继承接口和强制实现
};
```

在Shape类中，若objectID函数的计算方式不希望在派生类中被改变，就声明为non - virtual。  
声明一个纯虚函数的目的是使派生类只继承函数接口。

如Shape类中的draw函数，不同的具体派生类（如Rectangle、Ellipse）需要各自实现绘制逻辑。

```cpp
class Shape {
public:
    virtual void draw() const = 0;
};
class Rectangle : public Shape {
public:
    virtual void draw() const {
        // 矩形的绘制代码
    }
};
class Ellipse : public Shape {
public:
    virtual void draw() const {
        // 椭圆的绘制代码
    }
};
```

** 简单虚函数：**

声明一个简单虚函数是让派生类继承函数接口以及一个默认实现。为了避免派生类意外继承错误的默认实现，可以将接口和默认实现分离，如Airplane类的改进设计：

```cpp
// 原始有问题的设计
class Airplane {
public:
    virtual void fly(const Airport& destination);
};
void Airplane::fly(const Airport& destination) {
    // 默认飞行代码
}
class ModelA : public Airplane {};
class ModelB : public Airplane {};
class ModelC : public Airplane {}; // 这里ModelC可能意外继承错误的fly实现


// 改进后的设计
class Airplane {
public:
    virtual void fly(const Airport& destination) = 0;
protected:
    void defaultFly(const Airport& destination);
};
void Airplane::defaultFly(const Airport& destination) {
    // 默认飞行代码
}
class ModelA : public Airplane {
public:
    virtual void fly(const Airport& destination) {
        defaultFly(destination);
    }
};
class ModelB : public Airplane {
public:
    virtual void fly(const Airport& destination) {
        defaultFly(destination);
    }
};
class ModelC : public Airplane {
public:
    virtual void fly(const Airport& destination);
};
void ModelC::fly(const Airport& destination) {
    // ModelC的飞行代码
}
```

-非虚函数：声明一个非虚函数是使派生类继承函数接口。

如Shape类中的objectID函数，其计算方式固定，派生类不应改变。

```cpp
class Shape {
public:
    int objectID() const {
        // 固定的计算objectID的代码
    }
};
```

**总结建议**

+ <font style="color:rgb(28, 31, 35);">纯虚函数指定仅有接口被继承，用于要求派生类必须实现特定功能。</font>
+ <font style="color:rgb(28, 31, 35);">简单虚函数指定接口继承加上默认实现继承，在设计时要注意合理处理默认实现，避免派生类意外继承错误实现。</font>
+ <font style="color:rgb(28, 31, 35);">非虚函数指定接口继承加上强制实现继承，用于定义在派生类中不应被改变的行为。</font>







**问题**：





























#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 35: 考虑可选的 virtual functions（虚拟函数）的替代方法</font>
需要仔细看。

以下是几种替代虚函数的设计方法

**1. NVI 惯用法(非虚接口)**

****基类控制调用流程，派生类定制具体实现。  
**适用场景**：需要在调用前后添加统一逻辑（如日志、验证）

```cpp
class GameCharacter {
public:
    // 非虚公有函数，控制流程
    int healthValue() const {
        // 调用前处理（如加锁、日志）
        int health = doHealthValue();
        // 调用后处理（如解锁）
        return health;
    }
private:
    // 私有虚函数，派生类可重写
    virtual int doHealthValue() const {
        // 默认健康值计算
        return 100;
    }
};
class EvilBadGuy : public GameCharacter {
private:
    virtual int doHealthValue() const override {
        // 派生类自定义实现
        return 75;
    }
};
```

**2. 策略模式 (函数指针,lambda表达式，仿函数)**

**特点**：解耦计算逻辑，运行时切换策略  
**适用场景**：需要动态更换算法，但计算逻辑较简单

```cpp
class GameCharacter;

// 计算健康值的函数类型
int defaultHealthCalc(const GameCharacter&);

class GameCharacter {
public:
    using HealthCalcFunc = std::function<int(const GameCharacter&)>;
    explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc):healthFunc(hcf){}
    int healthValue() const {
        return healthFunc(*this); // 调用外部函数
    }
private:
    HealthCalcFunc healthFunc;
};
// 具体计算函数
int defaultHealthCalc(const GameCharacter&) {
    return 100;
}
int customHealthCalc(const GameCharacter&) {
    return 50;
}
// 使用
EvilBadGuy badGuy(&customHealthCalc); // 传入自定义函数
```

所以策略模式的本质可以根据不同情景选择不同的算法。function是最简单的实现方式。

###### 补充：什么是NVI?
NVI（Non-Virtual Interface，非虚接口）是一种C++设计惯用法（Idiom）。

通过将虚函数设为私有或保护成员，并通过公有非虚函数作为接口，实现接口与实现的解耦。核心思想是**确保虚函数仅通过基类定义的公共接口调用**，从而增强封装性、控制行为流程，并避免设计缺陷。

****

**接口与实现分离**：

+ 基类接口可以统一添加通用操作（如锁、日志、参数校验），无需在每个派生类中重复编写。
+ 私有/保护虚函数（实现）负责具体逻辑，由派生类重写。

**增强封装性**：

    - 虚函数被隐藏为私有/保护成员，外部无法直接调用，防止滥用或误用。

```cpp
class GameCharacter {
public:
    // 公有非虚接口（接口）
    int healthValue() const {
        // 前置操作（如日志、锁）
        int retVal = doHealthValue(); // 调用私有虚函数
        // 后置操作（如解锁、校验）
        return retVal;
    }

private:
    // 私有虚函数（实现）
    virtual int doHealthValue() const {
        // 默认实现
        return 100;
    }
};

class Tank : public GameCharacter {
private:
    // 重写虚函数
    int doHealthValue() const override {
        return 200; // 坦克的特殊健康值计算
    }
};
```

**为什么基类的私有虚函数也可以被重写？**

在NVI惯用法中，即使基类将虚函数声明为私有（private），派生类仍然可以重写该虚函数。

虚函数的访问权限不影响重写能力：私有虚函数在基类中是隐藏的，但派生类仍可以通过函数签名匹配重写。派生类的重写函数可以是私有、保护或公有，但需确保派生类内部能够访问基类的虚函数（通常通过继承关系）。

















#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 36: 绝不要重定义一个 通过继承得到的非虚拟函数</font>
<font style="color:#DF2A3F;">绝不要重定义一个通过继承得到的非虚拟函数</font>

**原因**：

公有继承意味着“is-a”关系，即派生类对象也是基类对象，适用于基类对象的所有事情也应适用于派生类对象。声明非虚拟函数是为类设定一个超越特殊化的不变量，派生类从基类继承非虚拟函数时，需同时继承其接口和实现。<u>如果派生类重定义该非虚拟函数，就会产生矛盾</u>：<u>若派生类需要不同实现且基类实现是不变量，那么派生类就不应从基类公有继承</u>；<u>若派生类必须公有继承且需要不同实现，那么该函数就不应是非虚拟的；若派生类确实是基类的一种且函数是基类的不变量，那么派生类就不应重定义该函数。</u>

**解决方法及代码示例**：

**解决方法**：<font style="color:#DF2A3F;">绝不要重定义一个通过继承得到的非虚拟函数</font>。在设计类的继承体系时，若预计派生类可能需要不同的函数实现，应将函数声明为虚拟函数；若函数是基类的不变量，不希望派生类改变其行为，则保持函数为非虚拟函数且不允许派生类重定义。



#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 37: 绝不要重定义一个函数的 inherited default parameter value（通过继承得到的缺省参数值）</font>
**这个需要仔细看**

**问题**：在继承体系中，派生类重定义从基类继承的虚拟函数的缺省参数值时，会导致调用函数时出现混乱，即调用的是派生类中的虚拟函数，却使用了基类中的缺省参数值。

```cpp
class Shape {
public:
    enum ShapeColor { Red, Green, Blue };
    virtual void draw(ShapeColor color = Red) const = 0;
};
class Rectangle: public Shape {
public:
    // 重定义了缺省参数值，这是错误的
    virtual void draw(ShapeColor color = Green) const;
};
class Circle: public Shape {
public:
    virtual void draw(ShapeColor color) const;
};
int main() {
    Shape *pr = new Rectangle;
    // 调用Rectangle::draw(Shape::Red)，使用了Shape类的缺省参数值
    pr->draw();
    return 0;
}
```

虚拟函数是动态绑定的，即调用的特定函数取决于调用对象的动态类型。

<u>缺省参数值是静态绑定的，取决于调用对象的静态类型。如 </u>`<u>pr->draw()</u>`<u> 调用 </u>`<u>Rectangle::draw</u>`<u> 函数，但使用的缺省参数值却是 </u>`<u>Shape</u>`<u> 类中定义的 </u>`<u>Red</u>`<u>，而不是 </u>`<u>Rectangle</u>`<u> 类中定义的 </u>`<u>Green</u>`<u>，因为 </u>`<u>pr</u>`<u> 的静态类型是 </u>`<u>Shape*</u>`<u>。这是因为如果缺省参数值动态绑定，编译器需在运行时确定，会导致运行效率降低和实现复杂，所以C++选择在编译期确定缺省参数值。</u>

**解决方法及代码案例**：

不重定义继承的虚拟函数的缺省参数值。若想为基类和派生类用户提供相同的缺省参数值，可考虑使用非虚拟接口惯用法（NVI惯用法）。

    - 

```cpp
class Shape {
public:
    enum ShapeColor { Red, Green, Blue };
    virtual void draw(ShapeColor color = Red) const = 0;
};
class Rectangle: public Shape {
public:
    // 重定义了缺省参数值，这是错误的
    virtual void draw(ShapeColor color = Green) const;
};
class Circle: public Shape {
public:
    virtual void draw(ShapeColor color) const;
};
int main() {
    Shape *pr = new Rectangle;
    // 调用Rectangle::draw(Shape::Red)，使用了Shape类的缺省参数值
    pr->draw();
    return 0;
}
```

绝不要重定义一个通过继承得到的缺省参数值，因为缺省参数值是静态绑定，而虚拟函数是动态绑定，重定义会导致调用函数时出现混乱。若要为基类和派生类提供相同的缺省参数值，可使用非虚拟接口惯用法（NVI惯用法），即通过基类中的公有非虚拟函数调用派生类可能重定义的私有虚拟函数，用非虚拟函数指定缺省参数，虚拟函数做实际工作 。

这里的逻辑没有搞懂。







#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 38: 通过 组合模拟 "has-a"或 "is-implemented-in-terms-of"</font>
**组合 的定义及示例**：组合是一个类型的对象包含另一个类型的对象时类型之间的关系。

    - **在应用领域**：当 composition 发生在应用领域的对象之间时，表达 “has-a”（有一个）的关系。如`Person`对象有一个名字、一个地址以及语音和传真电话号码，即一个人有这些东西，而不是一个人是这些东西。
    - **在实现领域**：当 composition 发生在实现领域时，表达 “is-implemented-in-terms-of”（是根据…… 实现的）的关系。
+ **错误示例**：在设计表现小对象集合的`Set`模板时，最初想通过从`std::list<T>`公有继承来实现，认为`Set`对象就是`list`对象。但实际上，`list`对象可以包含重复元素，而`Set`对象不能包含重复元素，所以`Set`不是`list`，这种公有继承的方式错误，因为不符合 “is-a” 关系中对于基类和派生类的要求。
+ **正确示例**：正确的做法是认识到`Set`对象可以根据`list`对象实现，即 “is-implemented-in-terms-of” 关系。通过在`Set`类中包含一个`std::list<T>`对象作为数据表示，然后`Set`的成员函数利用`list`和标准库提供的功能来实现相应操作，如`member`、`insert`、`remove`、`size`等函数。











**问题：在C++中，private inheritance（私有继承）和public inheritance（公有继承）的行为和含义有何不同？**

C++中定义了不同的继承方式，public inheritance（公有继承）视为is-a关系，而private inheritance（私有继承。private inheritance（私有继承）意味着is-implemented-in-terms-of（是根据……实现的）。

<font style="color:rgb(44, 44, 54);">当希望基类的接口对派生类的用户不可见时，可以使用 </font>`<font style="color:rgb(44, 44, 54);">private</font>`<font style="color:rgb(44, 44, 54);"> 继承。这种方式强调派生类和基类之间的实现关系，而不是接口关系。</font>

```cpp
class Person { ... };
class Student: private Person { ... };     // inheritance is now private

void eat(const Person& p);                 // anyone can eat
void study(const Student& s);              // only students study
Person p;                                  // p is a Person
Student s;                                 // s is a Student
eat(p);                                    // fine, p is a Person
eat(s);                                    // error! a Student isn't a Person
```

**在什么情况下会倾向于选择private inheritance（私有继承）而不是composition（复合）？**

建议直接用组合，不要用私有继承，复杂而且不好理解。





#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 40: 谨慎使用 multiple inheritance（多继承）</font>
**问题1 ：在多继承中可能会出现同名成员导致的歧义问题**

+ **原因**：当使用多继承时，可能从多个基类中继承相同的名字（如函数、typedef 等），C++ 在解析重载函数调用时，先确定最佳匹配函数，再检查可访问性，若多个同名函数匹配程度相同，就会产生歧义。
+ **解决方法**：明确指定调用哪个基类的函数来消除歧义，如`mp.BorrowableItem::checkOut();` 。若调用的是私有成员函数，则会出现访问权限错误。

**问题2：多继承中存在 “菱形继承” 问题**

+ **原因**：见《对象模型》
+ **解决方法**：若不希望复制基类数据成员，可让相关基类使用虚拟继承。

这样会带来的额外的问题：虚拟继承会带来一些成本，包括对象大小增加、访问速度变慢以及初始化规则复杂。

**建****<u>议：永远不要使用多继承。</u>**







#### i<font style="background-color:#FBDE28;">tem41  理解隐式接口与编译期多态（模板编程）</font>
在面向对象编程（OOP）中，显式接口和运行时多态是核心概念。显式接口通过类的公共成员函数定义，运行时多态通过虚函数实现。

然而，在泛型编程（模板）中，隐式接口和编译期多态成为主导。

**显式接口：**显式接口由类的公共成员函数签名（函数名、参数、返回类型等）明确定义。

**模板中的隐式接口**  

<font style="color:rgb(44, 44, 54);">隐式接口是指通过对象的实际行为（即支持的操作）来定义的接口。</font>

<font style="color:rgb(44, 44, 54);">它不依赖于类的声明，而是依赖于模板或鸭子类型的概念。隐式接口在编译时通过模板实例化来验证，不要求对象属于某个特定类型，只要求对象支持所需的操作。</font>

<font style="color:rgb(44, 44, 54);">模板实例化是在编译时进行的。当编译器遇到模板代码时，并不会立即生成具体的代码，而是直到模板被实例化时，即使用特定类型参数调用模板代码时，编译器才会根据提供的类型参数生成相应的具体代码。这个过程发生在编译阶段。</font>

<font style="color:rgb(44, 44, 54);">在C++中，隐式接口通常与模板结合使用，因为它们允许函数或类以一种通用的方式定义，而不需要依赖于显式的接口声明（如基类或接口类）。只要使用的类型满足模板所要求的操作（例如，具有某些特定的成员函数或操作符），该类型就可以与模板一起工作。这种机制主要通过编译时的静态检查来实现，确保使用的类型符合模板的要求。</font>

```cpp
template <typename T>
void render(const T& drawable){
    drawable.draw();  //隐式接口调用，只要T有draw()方法即可
}

class Circle {
 public:
    void draw() const {
        std::cout << "Drawing a circle" << std::endl;
    }
};

class Square {
public:
    void draw() const {
        std::cout << "Drawing a square" << std::endl;
    }
};

int main() {
    Circle circle;
    Square square;
    render(circle); // 不需要继承自某个接口
    render(square);
    return 0;
}
```

**编译期多态**

编译期多态通过模板实例化和函数重载解析实现。不同模板参数实例化生成不同代码，函数调用在编译时确定。

**编译期多态的优势**  

+ 生成高效的特化代码，无运行时开销。
+ 错误在实例化时暴露，确保类型安全。



**补充：什么是鸭子类型**

鸭子类型（Duck Typing）是一种编程概念，它源自一句短语：“如果它走路像鸭子，游泳像鸭子，叫起来也像鸭子，那么它就可以被当作一只鸭子。”在编程中，这个概念意味着对象的类型或类别的确定是基于它所具有的方法和属性，而不是基于它的显式类型声明。换句话说，鸭子类型关注的是对象是否具有执行特定操作所需的方法或属性，而不是对象属于哪个具体的类。

在支持动态类型的编程语言中（如Python、Ruby等），鸭子类型是一种常见的做法。即使在静态类型语言（如C++通过模板实现）中，也可以使用类似的概念来实现更灵活的设计。





#### <font style="background-color:#FBDE28;">item 42 typename的两种用法 </font>
**1. 声明模板参数**

在声明模板参数时，`typename` 和 `class` 是等价的，都可以用来声明模板类型参数。例如：

```cpp
template<class T>  // 使用 class
class Widget {
    // ...
};

template<typename T>  // 使用 typename
class Widget {
    // ...
};
```

在这两种情况下，`class` 和 `typename` 的含义完全相同，编译器不会对它们进行区分。程序员可以根据自己的偏好选择使用哪一个。

**2. 标识嵌套依赖类型名**

当模板中的某个类型是依赖于模板参数的嵌套类型时，编译器无法直接推断出该类型是否是一个类型（type），因此需要显式地使用 `typename` 来告知编译器。

```cpp
template<typename C>
void print2nd(const C& container) {
    // 假设容器支持迭代器操作
    typename C::const_iterator iter(container.begin());
    // 使用 iter 进行一些操作...
}
```

编译器在解析模板时，并不知道 `C::const_iterator` 是否是一个类型（可能是一个静态成员变量）。需要通过 `typename` 明确告诉编译器：`C::const_iterator` 是一个类型。

**例外情况：基类列表和成员初始化列表**

在某些特定场景下，嵌套依赖类型名前不需要使用 `typename`。

**基类列表**：在模板类的基类列表中，嵌套依赖类型名前不需要 `typename`。

**成员初始化列表**：在构造函数的成员初始化列表中，作为基类标识符的嵌套依赖类型名前也不需要 `typename`。

```cpp
template<typename T>
class Base {
  public:
    class Nested {};  // 嵌套类
};

template<typename T>
class Derived : public Base<T>::Nested {  // 不需要 typename
  public:
    Derived() : Base<T>::Nested() {}  // 成员初始化列表也不需要 typename
};
```





#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 43: 了解如何访问 模板化基类中的名字</font>
**问题**：在派生类模板 `LoggingMsgSender` 中调用基类模板 `MsgSender` 中的函数 `sendClear` 时，代码不能通过编译，编译器会抱怨 `sendClear` 不存在，即使该函数在基类中。

```cpp
template<typename Company>
class MsgSender {
public:
    void sendClear(const MsgInfo& info) {
        // 假设这里是发送消息的实现
        std::cout << "Sending clear message to company: " << info << std::endl;
    }
};

template<typename Company>
class LoggingMsgSender : public MsgSender<Company> {
public:
    void sendClearMsg(const MsgInfo& info) {
        // 记录日志：发送前
        std::cout << "Before sending: " << info << std::endl;
        // 调用基类的 sendClear 方法
        sendClear(info); // 编译错误！编译器不知道 sendClear 是否存在
        // 记录日志：发送后
        std::cout << "After sending: " << info << std::endl;
    }
};
```

**原因**：当编译器遇到类模板 `LoggingMsgSender` 的定义时，<u>由于 </u>`<u>Company</u>`<u> 是模板参数，在编译时无法确定其具体类型，也就无法确定基类 </u>`<u>MsgSender<Company></u>`<u> 的具体样子，不知道它是否有 </u>`<u>sendClear</u>`<u> 函数。因为基类模板可能被特化，特化版本不一定提供和通用模板相同的接口，所以 C++ 通常会拒绝在模板化基类中寻找继承来的名字。</u>

**解决方法及代码示例**：

1. **使用 **`this->`** 前缀**：

```cpp
template<typename Company>
class LoggingMsgSender: public MsgSender<Company> {
public:
  ...
  void sendClearMsg(const MsgInfo& info)
  {
    write "before sending" info to the log;
    this->sendClear(info); // 告诉编译器假定 sendClear 会被继承
    write "after sending" info to the log;
  }
  ...
};
```

2. **使用 **`using`** 声明**：

```cpp
template<typename Company>
class LoggingMsgSender: public MsgSender<Company> {
public:
  using MsgSender<Company>::sendClear; // 告诉编译器假定 sendClear 在基类中
  ...
  void sendClearMsg(const MsgInfo& info)
  {
    ...
    sendClear(info); // 假定 sendClear 会被继承
    ...
  }
  ...
};
```

3. **显式指定函数在基类中**：

```cpp
template<typename Company>
class LoggingMsgSender: public MsgSender<Company> {
public:
  ...
  void sendClearMsg(const MsgInfo& info)
  {
    ...
    MsgSender<Company>::sendClear(info); // 假定 sendClear 会被继承
    ...
  }
  ...
};
```

显式限定方法如果被调用函数是虚拟的，会关闭虚拟绑定行为。上述方法都向编译器保证了基类模板的特化将支持通用模板提供的接口，若实际不成立，在后续编译实例化时会暴露错误。





#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 44: 从 templates（模板）中分离出 parameter-independent（参数无关）的代码</font>
这个没有看懂

**问题**：使用模板可能导致代码膨胀（code bloat），即产生重复的（或几乎重复的）的代码、数据或两者都有的二进制码，使得目标代码臃肿松散。在模板代码中，虽然仅有一份模板源代码拷贝，但当模板被多次实例化时，可能会出现代码重复。

<font style="color:#DF2A3F;">模板的本质是 </font>**<font style="color:#DF2A3F;">编译期多态</font>**<font style="color:#DF2A3F;">。每次使用不同的模板参数（如 </font>`<font style="color:#DF2A3F;">int</font>`<font style="color:#DF2A3F;">、</font>`<font style="color:#DF2A3F;">double</font>`<font style="color:#DF2A3F;"> 等），编译器都会生成一份新的代码。即使这些代码在逻辑上是相同的，编译器仍然会生成多个副本。</font>



**示例**：为固定大小的正方矩阵（square matrices）写模板 `SquareMatrix` 支持矩阵转置（matrix inversion），代码如下：

```cpp
template<typename T,           // template for n x n matrices of
         std::size_t n>        // objects of type T; see below for info
class SquareMatrix {           // on the size_t parameter
public:
  ...
  void invert();               // invert the matrix in place
};
```

使用时：

```cpp
SquareMatrix<double, 5> sm1;
...
sm1.invert();                  // call SquareMatrix<double, 5>::invert

SquareMatrix<double, 10> sm2;
...
sm2.invert();                  // call SquareMatrix<double, 10>::invert
```

这里会实例化两个 `invert` 函数的拷贝，除了矩阵大小（5 和 10）不同外，函数其他部分相同，这是模板导致代码膨胀的经典情况。

****

**针对非类型模板参数引起的膨胀**：

**方法**：将与参数无关的代码分离到一个基类中，用函数参数或类数据成员替换模板参数。

具体是创建一个参数仅为矩阵中对象类型的基类，将大小相关的参数变为函数参数，派生类通过调用基类函数来避免代码重复。

```cpp
template<typename T>                   // size-independent base class for
class SquareMatrixBase {               // square matrices
protected:
  ...
  void invert(std::size_t matrixSize); // invert matrix of the given size
  ...
};

template<typename T, std::size_t n>
class SquareMatrix: private SquareMatrixBase<T> {
private:
  using SquareMatrixBase<T>::invert;   // avoid hiding base version of
                                       // invert; see Item 33
public:
  ...
  void invert() { this->invert(n); }   // make inline call to base class
};                                     // version of invert; see below
                                       // for why "this->" is here
```

为了让基类知道操作的数据，还可以让基类存储指向矩阵值的内存区域的指针和矩阵大小，派生类决定如何分配内存，示例代码如下：

```cpp
template<typename T>
class SquareMatrixBase {
protected:
  SquareMatrixBase(std::size_t n, T *pMem)     // store matrix size and a
  : size(n), pData(pMem) {}                    // ptr to matrix values
  void setDataPtr(T *ptr) { pData = ptr; }     // reassign pData
  ...
private:
  std::size_t size;                            // size of matrix
  T *pData;                                    // pointer to matrix values
};

template<typename T, std::size_t n>
class SquareMatrix: private SquareMatrixBase<T> {
public:
  SquareMatrix()                               // send matrix size and
  : SquareMatrixBase<T>(n, data) {}            // data ptr to base class
  ...

private:
  T data[n*n];
};

template<typename T, std::size_t n>
class SquareMatrix: private SquareMatrixBase<T> {
public:
  SquareMatrix()                               // set base class data ptr to null,
  : SquareMatrixBase<T>(n, 0),                 // allocate memory for matrix
    pData(new T[n*n])                          // values, save a ptr to the
  { this->setDataPtr(pData.get()); }           // memory, and give a copy of it
  ...                                          // to the base class

private:
  boost::scoped_array<T> pData;                // see Item 13 for info on
};                                             // boost::scoped_array
```

基类 SquareMatrixBase 存储了与矩阵大小相关的通用信息(如矩阵大小 size 和指向矩阵数据的指针 pData)。这样基类可以提供一些与矩阵大小无关的操作(例如矩阵求逆 invert)，并通过 size 和 pData 来访问实际的矩阵数据。基类实现了通用的矩阵操作（如求逆），避免了重复代码。不同大小的矩阵可以通过相同的基类实现共享逻辑。

派生类负责具体实现：派生类 SquareMatrix 负责具体的矩阵大小和内存管理。通过构造函数，派生类将矩阵大小和数据指针传递给基类。派生类可以选择不同的内存分配策略（如静态数组或动态分配）。

**<font style="background-color:#FBDE28;">分离关注点：</font>**基类专注于通用逻辑（如算法实现）。派生类专注于特定实现（如内存分配、矩阵大小）。基类和派生类之间的职责分离清晰。



**针对类型参数引起的膨胀**：

+ **方法**：让具有相同二进制表示的实例化类型共享实现。例如，与强类型指针（`T*` 指针）一起工作的成员函数可以通过让它们调用与无类型指针（`void*` 指针）一起工作的函数来实现。
+ **示例**：一些标准 C++ 库的实现对于像 `vector`，`deque` 和 `list` 这样的模板就是采用这种方式，开发模板时若关心代码膨胀也可采用类似做法。

这个地方没有看懂。

**<font style="color:rgb(44, 44, 54);">用无类型指针（</font>**`**<font style="color:rgb(44, 44, 54);">void*</font>**`**<font style="color:rgb(44, 44, 54);">）统一底层实现</font>**

<font style="color:rgb(44, 44, 54);">对于某些模板类（如 </font>`<font style="color:rgb(44, 44, 54);">std::vector</font>`<font style="color:rgb(44, 44, 54);">、</font>`<font style="color:rgb(44, 44, 54);">std::list</font>`<font style="color:rgb(44, 44, 54);"> 等），它们的核心操作（如内存分配、元素移动等）并不依赖于具体的类型。因此，可以将这些操作提取到一个通用的底层实现中，使用无类型指针（</font>`<font style="color:rgb(44, 44, 54);">void*</font>`<font style="color:rgb(44, 44, 54);">）来操作数据。</font>

```cpp
template<typename T>
class MyVector {
public:
void push_back(const T& value) {
    // 调用通用的底层实现
    push_back_impl(reinterpret_cast<const void*>(&value), sizeof(T));
}

private:
void push_back_impl(const void* value, size_t size) {
    // 实现与类型无关的逻辑
    // 比如分配内存、复制数据等
}

void* data;  // 使用 void* 存储数据
};
```

<font style="color:rgb(44, 44, 54);">在这个例子中，无论 </font>`<font style="color:rgb(44, 44, 54);">T</font>`<font style="color:rgb(44, 44, 54);"> 是什么类型，</font>`<font style="color:rgb(44, 44, 54);">push_back_impl</font>`<font style="color:rgb(44, 44, 54);"> 的实现都是相同的。通过这种方式，编译器只需要生成一份 </font>`<font style="color:rgb(44, 44, 54);">push_back_impl</font>`<font style="color:rgb(44, 44, 54);"> 的代码，而不需要为每个类型生成独立的副本。</font>

**<font style="color:rgb(44, 44, 54);">使用类型擦除（Type Erasure）</font>**

<font style="color:rgb(44, 44, 54);">类型擦除是一种更高级的技术，常用于标准库（如 </font>`<font style="color:rgb(44, 44, 54);">std::function</font>`<font style="color:rgb(44, 44, 54);">）。它的核心思想是通过接口或虚函数隐藏具体类型，从而实现共享代码。</font>

```cpp
class AnyVector {
public:
    virtual ~AnyVector() {}
    virtual void push_back(const void* value) = 0;
};

template<typename T>
class ConcreteVector : public AnyVector {
    public:
        void push_back(const void* value) override {
        // 实现类型相关的逻辑
        new (data) T(*reinterpret_cast<const T*>(value));
    }
private:
    T data[100];  // 示例缓冲区
};
```

<font style="color:rgb(44, 44, 54);">在这种设计中：</font>

+ `<font style="color:rgb(44, 44, 54);">AnyVector</font>`<font style="color:rgb(44, 44, 54);"> </font><font style="color:rgb(44, 44, 54);">是一个抽象基类，提供了类型无关的接口。</font>
+ `<font style="color:rgb(44, 44, 54);">ConcreteVector<T></font>`<font style="color:rgb(44, 44, 54);"> </font><font style="color:rgb(44, 44, 54);">是具体的实现类，实现了类型相关的逻辑。</font>

<font style="color:rgb(44, 44, 54);">通过这种方式，用户可以通过 </font>`<font style="color:rgb(44, 44, 54);">AnyVector</font>`<font style="color:rgb(44, 44, 54);"> 接口操作不同类型的向量，而不需要为每个类型生成独立的代码。</font>

<font style="color:rgb(44, 44, 54);">这个地方没有看懂。</font>

<font style="color:rgb(44, 44, 54);"></font>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 45: 用 member function templates（成员函数模板） 接受 "all compatible types"（“所有兼容类型”）</font>
**<font style="color:rgb(44, 44, 54);">问题</font>**

<font style="color:rgb(44, 44, 54);">让智能指针类（如 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr</font>`<font style="color:rgb(44, 44, 54);">）支持所有兼容类型的隐式转换？例如，从 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<Middle></font>`<font style="color:rgb(44, 44, 54);"> 隐式转换为 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<Top></font>`<font style="color:rgb(44, 44, 54);">，或从 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<Top></font>`<font style="color:rgb(44, 44, 54);"> 隐式转换为 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<const Top></font>`<font style="color:rgb(44, 44, 54);">。</font>

1. **无限的类型组合**<font style="color:rgb(44, 44, 54);">：</font><font style="color:rgb(44, 44, 54);">  
</font><font style="color:rgb(44, 44, 54);">智能指针模板（如 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<T></font>`<font style="color:rgb(44, 44, 54);">）的不同实例（如 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<Middle></font>`<font style="color:rgb(44, 44, 54);"> 和 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<Top></font>`<font style="color:rgb(44, 44, 54);">）之间没有继承关系。若要支持所有兼容类型（如派生类到基类、非常量到常量）的转换，必须为每种可能的类型组合编写构造函数，这在类型扩展时无法手动维护。</font>
2. **隐式转换需求**<font style="color:rgb(44, 44, 54);">：  
</font><font style="color:rgb(44, 44, 54);">内置指针（如 </font>`<font style="color:rgb(44, 44, 54);">Top*</font>`<font style="color:rgb(44, 44, 54);">）支持隐式转换（例如 </font>`<font style="color:rgb(44, 44, 54);">Middle*</font>`<font style="color:rgb(44, 44, 54);"> → </font>`<font style="color:rgb(44, 44, 54);">Top*</font>`<font style="color:rgb(44, 44, 54);">），智能指针需要模仿这一行为。但手动为每种类型编写构造函数不现实，且无法应对未来新增的派生类型。</font>
3. **模板实例化限制**<font style="color:rgb(44, 44, 54);">：  
</font><font style="color:rgb(44, 44, 54);">模板构造函数（如 </font>`<font style="color:rgb(44, 44, 54);">template<typename U> SmartPtr(const SmartPtr<U>&)</font>`<font style="color:rgb(44, 44, 54);">）的实例化需满足指针类型间的隐式转换条件（如 </font>`<font style="color:rgb(44, 44, 54);">U*</font>`<font style="color:rgb(44, 44, 54);"> 可转换为 </font>`<font style="color:rgb(44, 44, 54);">T*</font>`<font style="color:rgb(44, 44, 54);">）。</font>

**<font style="color:rgb(44, 44, 54);">解决方法</font>**

<u><font style="color:rgb(44, 44, 54);">使用</font></u>**<u>成员函数模板</u>**<u><font style="color:rgb(44, 44, 54);">声明泛型化的拷贝构造函数和赋值操作符，结合模板参数的隐式转换规则，自动生成所有兼容类型的转换。</font></u>

**<font style="color:rgb(44, 44, 54);">智能指针类 </font>**`**<font style="color:rgb(44, 44, 54);">SmartPtr</font>**`**<font style="color:rgb(44, 44, 54);"> 的定义</font>**

```cpp
template<typename T>
class SmartPtr {
public:
    // 普通拷贝构造函数（非模板）
    SmartPtr(const SmartPtr& other) : heldPtr(other.heldPtr) { /* ... */ }
    // 泛型化拷贝构造函数（成员函数模板）
    template<typename U>
    SmartPtr(const SmartPtr<U>& other) : heldPtr(other.get()) {
        // 其他初始化逻辑
    }
    // 获取内置指针
    T* get() const { return heldPtr; }
    // 普通赋值操作符（非模板）
    SmartPtr& operator=(const SmartPtr& rhs) {
        if (this != &rhs) {
            // 赋值逻辑
        }
        return *this;
    }
    // 泛型化赋值操作符（成员函数模板）
    template<typename U>
    SmartPtr& operator=(const SmartPtr<U>& rhs) {
        if (this != dynamic_cast<SmartPtr<U>*>(this)) {
            // 赋值逻辑，确保 U* 可转换为 T*
        }
        return *this;
    }
private:
    T* heldPtr; // 持有的内置指针
};
```

**泛型化构造函数**<font style="color:rgb(44, 44, 54);">:</font>

`<font style="color:rgb(44, 44, 54);">template<typename U> SmartPtr(const SmartPtr<U>&)</font>`<font style="color:rgb(44, 44, 54);"> 通过模板参数 </font>`<font style="color:rgb(44, 44, 54);">U</font>`<font style="color:rgb(44, 44, 54);"> 接受任何兼容类型。通过 </font>`<font style="color:rgb(44, 44, 54);">other.get()</font>`<font style="color:rgb(44, 44, 54);"> 获取 </font>`<font style="color:rgb(44, 44, 54);">U*</font>`<font style="color:rgb(44, 44, 54);"> 指针，并尝试将其隐式转换为 </font>`<font style="color:rgb(44, 44, 54);">T*</font>`<font style="color:rgb(44, 44, 54);">（如 </font>`<font style="color:rgb(44, 44, 54);">Middle*</font>`<font style="color:rgb(44, 44, 54);"> → </font>`<font style="color:rgb(44, 44, 54);">Top*</font>`<font style="color:rgb(44, 44, 54);">）。</font>

**<font style="background-color:#FBDE28;">常量和非常量转换</font>**<font style="color:rgb(44, 44, 54);background-color:#FBDE28;">:</font><font style="color:rgb(44, 44, 54);">  
</font>`<font style="color:rgb(44, 44, 54);">SmartPtr<const Top></font>`<font style="color:rgb(44, 44, 54);">可从</font>`<font style="color:rgb(44, 44, 54);">SmartPtr<Top></font>`<font style="color:rgb(44, 44, 54);">隐式转换,因为</font>`<font style="color:rgb(44, 44, 54);">T*</font>`<font style="color:rgb(44, 44, 54);">(非 </font>`<font style="color:rgb(44, 44, 54);">const</font>`<font style="color:rgb(44, 44, 54);">)可转换为 </font>`<font style="color:rgb(44, 44, 54);">const T*</font>`<font style="color:rgb(44, 44, 54);">。</font>

**显式与隐式转换**<font style="color:rgb(44, 44, 54);">：  
</font><font style="color:rgb(44, 44, 54);">若需禁止隐式转换（如从内置指针到智能指针），可将构造函数声明为 </font>`<font style="color:rgb(44, 44, 54);">explicit</font>`<font style="color:rgb(44, 44, 54);">：</font>

explicit SmartPtr(T* realPtr); // 禁止隐式转换

**必须声明普通成员函数**<font style="color:rgb(44, 44, 54);">：  
</font><font style="color:rgb(44, 44, 54);">	若仅使用模板构造函数，则编译器仍会为 </font>`<font style="color:rgb(44, 44, 54);">SmartPtr<T></font>`<font style="color:rgb(44, 44, 54);"> 生成默认的拷贝构造函数（非模板）。因此，必须显式声明普通拷贝构造函数和赋值操作符，以覆盖默认行为。</font>

**<font style="color:rgb(44, 44, 54);">补充说明（TR1的 </font>**`**<font style="color:rgb(44, 44, 54);">shared_ptr</font>**`**<font style="color:rgb(44, 44, 54);"> 示例）</font>**

<font style="color:rgb(44, 44, 54);">TR1的</font>`<font style="color:rgb(44, 44, 54);">shared_ptr</font>`<font style="color:rgb(44, 44, 54);">使用类似方法，但更复杂(支持</font>`<font style="color:rgb(44, 44, 54);">auto_ptr</font>`<font style="color:rgb(44, 44, 54);">,</font>`<font style="color:rgb(44, 44, 54);">weak_ptr</font>`<font style="color:rgb(44, 44, 54);">等)。</font>

```cpp
template<class T> class shared_ptr {
public:
    // 普通拷贝构造函数
    shared_ptr(const shared_ptr& r);
    // 泛型化拷贝构造函数（支持其他 shared_ptr 类型）
    template<class Y>
    shared_ptr(const shared_ptr<Y>& r);
    // 构造函数从内置指针（显式禁止隐式转换）
    explicit shared_ptr(T* p);
    // 赋值操作符（支持其他 shared_ptr 类型）
    shared_ptr& operator=(const shared_ptr& r);
    template<class Y>
    shared_ptr& operator=(const shared_ptr<Y>& r);
};
```

+ **核心思想**<font style="color:rgb(44, 44, 54);">：使用成员函数模板（如 </font>`<font style="color:rgb(44, 44, 54);">template<typename U> SmartPtr(const SmartPtr<U>&)</font>`<font style="color:rgb(44, 44, 54);">）生成泛型化的拷贝构造函数和赋值操作符，通过模板参数的隐式转换规则自动支持所有兼容类型。</font>
+ **注意事项**<font style="color:rgb(44, 44, 54);">：必须显式声明普通拷贝构造函数和赋值操作符，避免编译器生成默认版本干扰泛型化模板。</font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

#### <font style="color:rgb(51, 51, 51);">It</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">em 46:需要 type conversions(类型转换)时在templates(模板)内定义non-member functions(非成员函数)</font>
**<font style="color:rgb(44, 44, 54);">问题</font>**

<font style="color:rgb(44, 44, 54);">在使用模板类 </font>`<font style="color:rgb(44, 44, 54);">Rational</font>`<font style="color:rgb(44, 44, 54);"> 和其对应的非成员函数 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 时，试图实现混合模式运算（例如 </font>`<font style="color:rgb(44, 44, 54);">Rational<int> * int</font>`<font style="color:rgb(44, 44, 54);">）会导致编译失败。这是因为模板参数推导（template argument deduction）过程中不会考虑隐式类型转换（implicit type conversion）。</font>

```cpp
#include <iostream>
// Rational 类模板
template <typename T>
class Rational {
public:
    // 构造函数
    Rational(T numerator = 0,T denominator = 1):numerator_(numerator), denominator_(denominator){}
    // 获取分子和分母
    T numerator() const { return numerator_; }
    T denominator() const { return denominator_; }
private:
    T numerator_;
    T denominator_;
};
// 非成员函数 operator* 支持两个 Rational 对象的乘法
template <typename T>
Rational<T> operator*(const Rational<T>& lhs, const Rational<T>& rhs) {
    return Rational<T>(
        lhs.numerator() * rhs.numerator(),
        lhs.denominator() * rhs.denominator()
    );
}
// 测试代码
int main() {
    Rational<int> r1(3, 4); // 创建一个有理数 3/4
    int x = 2;
    // 尝试混合模式运算
    auto result1 = r1 * x; // 编译失败！
    auto result2 = x * r1; // 编译失败！
    return 0;
}

```

+ <font style="color:rgb(44, 44, 54);">编译器无法推导出 </font>`<font style="color:rgb(44, 44, 54);">T</font>`<font style="color:rgb(44, 44, 54);"> 的值，因为模板参数推导不支持通过构造函数进行的隐式类型转换。</font>
+ <font style="color:rgb(44, 44, 54);">在 </font>`<font style="color:rgb(44, 44, 54);">Rational<int> result = oneHalf * 2;</font>`<font style="color:rgb(44, 44, 54);"> 中，</font>`<font style="color:rgb(44, 44, 54);">2</font>`<font style="color:rgb(44, 44, 54);"> 是一个 </font>`<font style="color:rgb(44, 44, 54);">int</font>`<font style="color:rgb(44, 44, 54);"> 类型，而 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 的第二个参数需要的是 </font>`<font style="color:rgb(44, 44, 54);">Rational<T></font>`<font style="color:rgb(44, 44, 54);"> 类型。编译器不会尝试通过 </font>`<font style="color:rgb(44, 44, 54);">Rational</font>`<font style="color:rgb(44, 44, 54);"> 的构造函数将 </font>`<font style="color:rgb(44, 44, 54);">2</font>`<font style="color:rgb(44, 44, 54);"> 转换为 </font>`<font style="color:rgb(44, 44, 54);">Rational<int></font>`<font style="color:rgb(44, 44, 54);">。</font>



**非模板类与模板类的区别**<font style="color:rgb(44, 44, 54);">：</font>

    - <font style="color:rgb(44, 44, 54);">在非模板类中，</font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 是普通函数，编译器可以利用隐式类型转换来处理混合模式运算。</font>
    - <font style="color:rgb(44, 44, 54);background-color:#FBDE28;">在模板类中，</font>`<font style="color:rgb(44, 44, 54);background-color:#FBDE28;">operator*</font>`<font style="color:rgb(44, 44, 54);background-color:#FBDE28;"> 是模板函数，隐式类型转换不再适用。</font>
    - <font style="color:rgb(44, 44, 54);background-color:#FBDE28;">这个对吗？</font>

<font style="color:rgb(44, 44, 54);">为了支持混合模式运算，可以通过以下步骤解决问题：</font>

**在类模板内部声明非成员函数作为友元**<font style="color:rgb(44, 44, 54);">：</font>

+ <font style="color:rgb(44, 44, 54);">在类模板内部声明 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 为友元函数。这样，当实例化 </font>`<font style="color:rgb(44, 44, 54);">Rational<T></font>`<font style="color:rgb(44, 44, 54);"> 时，相应的 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 函数会被自动声明。</font>

**定义友元函数**<font style="color:rgb(44, 44, 54);">：将 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 的定义放在类模板内部，以便它可以访问私有成员，并且支持隐式类型转换。</font>

**优化实现**<font style="color:rgb(44, 44, 54);">：如果 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 的逻辑复杂，可以将其委托给一个辅助函数（helper function），以减少内联的影响。</font>

**<font style="color:rgb(44, 44, 54);">解决方案 1：直接在类模板内部定义友元函数</font>**

```cpp
template<typename T>
class Rational {
public:
    Rational(const T& numerator = 0, const T& denominator = 1)
        : numerator_(numerator), denominator_(denominator) {}

    const T numerator() const { return numerator_; }
    const T denominator() const { return denominator_; }

    // 友元函数声明和定义
    friend const Rational operator*(const Rational& lhs, const Rational& rhs) {
        return Rational(lhs.numerator() * rhs.numerator(),
                        lhs.denominator() * rhs.denominator());
    }

private:
    T numerator_;
    T denominator_;
};

// 测试代码
int main() {
    Rational<int> oneHalf(1, 2);
    Rational<int> result = oneHalf * 2; // 支持混合模式运算
    return 0;
}
```

**<font style="color:rgb(44, 44, 54);">解决方案 2：使用辅助函数优化实现</font>**

```cpp
//前置声明模板类
template<typename T>
class Rational;

//声明辅助函数模板
template<typename T>
const Rational<T> doMultiply(const Rational<T>& lhs, const Rational<T>& rhs);

//定义模板类
template<typename T>
class Rational {
public:
    Rational(const T& numerator = 0, const T& denominator = 1)
        : numerator_(numerator), denominator_(denominator){}
    const T numerator() const {return numerator_;}
    const T denominator() const {return denominator_;}
    // 友元函数声明
    friend const Rational<T> operator*(const Rational<T>& lhs, const Rational<T>& rhs) {
        return doMultiply(lhs, rhs); // 调用辅助函数
    }
private:
    T numerator_;
    T denominator_;
};
// 定义辅助函数模板
template<typename T>
const Rational<T> doMultiply(const Rational<T>& lhs, const Rational<T>& rhs) {
    return Rational<T>(lhs.numerator() * rhs.numerator(),
                       lhs.denominator() * rhs.denominator());
}

int main() {
    Rational<int> oneHalf(1, 2);
    Rational<int> result = oneHalf * 2; // 支持混合模式运算
    return 0;
}
```

1. **友元函数的作用**<font style="color:rgb(44, 44, 54);">：</font>
    - <font style="color:rgb(44, 44, 54);">在类模板内部声明友元函数，使其能够支持隐式类型转换。</font>
    - <font style="color:rgb(44, 44, 54);">友元函数无需依赖模板参数推导。</font>
2. **模板参数推导的局限性**<font style="color:rgb(44, 44, 54);">：</font>
    - <font style="color:rgb(44, 44, 54);">模板参数推导不支持隐式类型转换。</font>
    - <font style="color:rgb(44, 44, 54);">必须通过其他方式（如友元函数）绕过这一限制。</font>
3. **优化实现**<font style="color:rgb(44, 44, 54);">：</font>
    - <font style="color:rgb(44, 44, 54);">对于复杂的函数逻辑，可以使用辅助函数（helper function）来分离实现细节，避免将所有逻辑都放入类模板内部。</font>

<font style="color:rgb(44, 44, 54);">在模板类 </font>`<font style="color:rgb(44, 44, 54);">Rational<T></font>`<font style="color:rgb(44, 44, 54);"> 内部声明 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 为友元函数可以支持混合模式运算的原因主要涉及到C++编译器如何处理函数调用和类型转换。让我们详细探讨一下原因：</font>

**<font style="color:rgb(44, 44, 54);">支持隐式类型转换</font>**

<font style="color:rgb(44, 44, 54);">当你在一个类模板内部声明一个非成员函数（例如 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);">）作为友元，这意味着这个函数可以在不依赖模板参数推导的情况下被定义。具体来说：</font>

1. **<font style="color:rgb(44, 44, 54);">友元函数的特殊性</font>**<font style="color:rgb(44, 44, 54);">：友元函数不是类的成员函数，但它可以访问类的私有和保护成员。更重要的是，对于友元函数，编译器知道其确切的定义（因为它已经被声明），因此它能够使用隐式类型转换来匹配参数。</font>
2. **<font style="color:rgb(44, 44, 54);">实例化时已知类型</font>**<font style="color:rgb(44, 44, 54);">：当类模板 </font>`<font style="color:rgb(44, 44, 54);">Rational<T></font>`<font style="color:rgb(44, 44, 54);"> 被实例化时，T 的类型是已知的。这样，对于 </font>`<font style="color:rgb(44, 44, 54);">Rational<int></font>`<font style="color:rgb(44, 44, 54);"> 类型的对象，如果声明了一个友元 </font>`<font style="color:rgb(44, 44, 54);">operator*</font>`<font style="color:rgb(44, 44, 54);"> 函数，那么这个函数的具体形式（即 </font>`<font style="color:rgb(44, 44, 54);">operator*(const Rational<int>& lhs, const Rational<int>& rhs)</font>`<font style="color:rgb(44, 44, 54);">）也是确定的。这允许编译器在遇到类似 </font>`<font style="color:rgb(44, 44, 54);">oneHalf * 2;</font>`<font style="color:rgb(44, 44, 54);"> 这样的表达式时，尝试将 </font>`<font style="color:rgb(44, 44, 54);">int</font>`<font style="color:rgb(44, 44, 54);"> 类型的 </font>`<font style="color:rgb(44, 44, 54);">2</font>`<font style="color:rgb(44, 44, 54);"> 隐式转换为 </font>`<font style="color:rgb(44, 44, 54);">Rational<int></font>`<font style="color:rgb(44, 44, 54);"> 类型（假设存在合适的构造函数），因为此时它已经知道了要调用的确切函数。</font>

<font style="color:rgb(44, 44, 54);">这个地方看不懂。</font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 47: 为类型信息使用 traits classes（特征类）</font>
参见《STL源码剖析》

**问题**<font style="color:rgb(44, 44, 54);">：</font>

+ <font style="color:rgb(44, 44, 54);">在实现</font>`<font style="color:rgb(44, 44, 54);">advance</font>`<font style="color:rgb(44, 44, 54);">函数时，对于不同类型的迭代器（如随机访问迭代器、双向迭代器、前向迭代器、输入迭代器等），需要根据其特性来移动迭代器。例如，随机访问迭代器支持常量时间的迭代器运算（如</font>`<font style="color:rgb(44, 44, 54);">iter += d</font>`<font style="color:rgb(44, 44, 54);">），而其他较弱类型的迭代器需要通过反复调用</font>`<font style="color:rgb(44, 44, 54);">++</font>`<font style="color:rgb(44, 44, 54);">或</font>`<font style="color:rgb(44, 44, 54);">--</font>`<font style="color:rgb(44, 44, 54);">来实现移动，且需要在编译期间确定迭代器类型以选择合适的移动方式。</font>
+ <font style="color:rgb(44, 44, 54);">对于内建类型（如指针）作为迭代器时，需要特殊处理以获取其迭代器种类信息，因为内建类型无法像用户定义类型那样嵌入嵌套的</font>`<font style="color:rgb(44, 44, 54);">typedef</font>`<font style="color:rgb(44, 44, 54);">来标识迭代器种类。</font>

**原因**<font style="color:rgb(44, 44, 54);">：</font>

+ <font style="color:rgb(44, 44, 54);">不同类型的迭代器具有不同的操作能力，如随机访问迭代器功能最强，输入和输出迭代器功能最弱。</font><u><font style="color:rgb(44, 44, 54);">为了高效地实现</font></u>`<u><font style="color:rgb(44, 44, 54);">advance</font></u>`<u><font style="color:rgb(44, 44, 54);">函数，需要根据迭代器类型选择合适的移动方式。</font></u>
+ <u><font style="color:rgb(44, 44, 54);">由于</font></u>`<u><font style="color:rgb(44, 44, 54);">traits</font></u>`<u><font style="color:rgb(44, 44, 54);">技术需要在内建类型和用户定义类型上都能有效工作，而内建类型（如指针）无法嵌入信息，所以需要特殊处理来获取其迭代器种类信息。</font></u>

**解决方法**<font style="color:rgb(44, 44, 54);">：</font>

**定义**`<font style="color:rgb(44, 44, 54);">traits</font>`**类**

<font style="color:rgb(44, 44, 54);">创建</font>`<font style="color:rgb(44, 44, 54);">iterator_traits</font>`<font style="color:rgb(44, 44, 54);">模板及特化来获取迭代器的类型信息（如迭代器种类）。对于用户定义类型，要求其迭代器类中包含</font>`<font style="color:rgb(44, 44, 54);">iterator_category</font>`<font style="color:rgb(44, 44, 54);">的嵌套</font>`<font style="color:rgb(44, 44, 54);">typedef</font>`<font style="color:rgb(44, 44, 54);">来标识迭代器种类；对于指针类型，提供</font>`<font style="color:rgb(44, 44, 54);">iterator_traits</font>`<font style="color:rgb(44, 44, 54);">的部分模板特化，指定指针类型的迭代器种类为随机访问迭代器。</font>

**使用重载函数**<font style="color:rgb(44, 44, 54);">：创建多个重载的</font>`<font style="color:rgb(44, 44, 54);">doAdvance</font>`<font style="color:rgb(44, 44, 54);">函数模板，每个函数针对不同的迭代器种类（通过</font>`<font style="color:rgb(44, 44, 54);">iterator_category</font>`<font style="color:rgb(44, 44, 54);">标识）实现相应的迭代器移动逻辑。然后在</font>`<font style="color:rgb(44, 44, 54);">advance</font>`<font style="color:rgb(44, 44, 54);">函数中调用</font>`<font style="color:rgb(44, 44, 54);">doAdvance</font>`<font style="color:rgb(44, 44, 54);">，并传递通过</font>`<font style="color:rgb(44, 44, 54);">iterator_traits</font>`<font style="color:rgb(44, 44, 54);">获取的迭代器种类信息，利用编译器的重载解析机制选择合适的</font>`<font style="color:rgb(44, 44, 54);">doAdvance</font>`<font style="color:rgb(44, 44, 54);">函数版本。</font>

**定义迭代器标签结构体**<font style="color:rgb(44, 44, 54);">：</font>

```cpp
struct input_iterator_tag {};
struct output_iterator_tag {};
struct forward_iterator_tag: public input_iterator_tag {};
struct bidirectional_iterator_tag: public forward_iterator_tag {};
struct random_access_iterator_tag: public bidirectional_iterator_tag {};
```

**定义**`<font style="color:rgb(44, 44, 54);">iterator_traits</font>`**模板及特化**<font style="color:rgb(44, 44, 54);">：</font>

```cpp
// 模板
template<typename IterT>
struct iterator_traits;

// 用户定义类型的处理方式（以deque迭代器为例）
template <...>
class deque {
public:
  class iterator {
  public:
    typedef random_access_iterator_tag iterator_category;
    ...
  };
  ...
};

// 针对用户定义类型的iterator_traits实现
template<typename IterT>
struct iterator_traits {
  typedef typename IterT::iterator_category iterator_category;
  ...
};
// 针对指针类型的部分模板特化
template<typename IterT>
struct iterator_traits<IterT*> {
  typedef random_access_iterator_tag iterator_category;
  ...
};
```

+ **定义重载的**`<font style="color:rgb(44, 44, 54);">doAdvance</font>`**函数模板**<font style="color:rgb(44, 44, 54);">：</font>

```cpp
//针对随机访问迭代器
template<typename IterT, typename DistT>
void doAdvance(IterT& iter, DistT d, std::random_access_iterator_tag){
  iter += d;
}

//针对双向迭代器
template<typename IterT, typename DistT>
void doAdvance(IterT& iter, DistT d, std::bidirectional_iterator_tag){
  if (d >= 0) { while (d--) ++iter; }
  else { while (d++) --iter; }
}

//针对输入迭代器（也处理前向迭代器，因为继承关系）
template<typename IterT, typename DistT>
void doAdvance(IterT& iter, DistT d, std::input_iterator_tag) {
  if (d < 0 ) {
     throw std::out_of_range("Negative distance");
  }
  while (d--) ++iter;
}
```

+ **定义**`<font style="color:rgb(44, 44, 54);">advance</font>`**函数**<font style="color:rgb(44, 44, 54);"></font>

```cpp
template<typename IterT, typename DistT>
void advance(IterT& iter, DistT d) {
  doAdvance(
    iter, d,
    typename std::iterator_traits<IterT>::iterator_category()
  );
}
```

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

#### <font style="background-color:#FBDE28;">item48 了解模板元编程</font>
1. **模板元编程的概念**：模板元编程（TMP）是编写基于模板且运行于编译期间的 C++ 程序的过程，其程序在 C++ 编译器中运行，输出的是从模板实例化出的 C++ 源代码片断，随后被正常编译。
2. **模板元编程的优势**
+ **使复杂事情变容易**：能实现一些用其他方法很难或不可能完成的事情。
+ **提前发现错误**：由于在编译期间执行，可将运行时才能察觉的错误提前到编译时发现。
+ **提高运行时性能**：使用 TMP 的 C++ 程序通常在可执行代码大小、运行时间、内存需求等方面更有效率，但会使编译过程变长。
3. **模板元编程的示例**
+ **advance 函数的实现对比**：以 STL 的 advance 函数为例，基于 typeid 的 “常规” C++ 方法在运行时进行类型检测，效率低且可能产生编译问题，如对 list<int>::iterator 使用 += 操作（list<int>::iterator 是双向迭代器，不支持 +=）；而基于 traits 的 TMP 方法在编译时进行类型计算，针对不同类型的代码分离到单独函数，使用适合该类型的操作。
+ **TMP 中循环的实现**：TMP 中没有真正的循环结构，通过递归模板实例化实现循环效果。以计算阶乘的 TMP 程序为例，通过递归模板实例化 Factorial<n>引用 Factorial<n-1>实现循环，模板特化 Factorial<0> 作为递归结束的特殊情况，每个实例化的 struct 使用 enum 声明名为 value 的 TMP 变量来保存阶乘计算的当前值。
4. **模板元编程的应用**
+ **确保计量单位正确性**：在科学和工程应用中，使用 TMP 可在编译期间确保程序中所有计量单位组合正确，还能支持分数指数并简化分数，以确认单位的一致性。
+ **优化矩阵操作**：利用与 TMP 相关的表达式模板技术，在不改变客户代码语法的情况下，可消除计算矩阵乘积时的临时对象并合并循环，减少内存使用并显著提高运行速度。
+ **生成自定义的设计模式实现**：基于 TMP 的基于 policy 设计技术，可创建代表独立设计选择的模板（“policies”），以任意方式组合生成带有自定义行为的模式实现，如生成数百个不同的智能指针类型，扩展了编程器件的范围。





#### <font style="color:rgb(51, 51, 51);">It</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">em 49: 了解 new-handler 的行为</font>
**问题**

**内存分配失败时的处理**：当 `operator new` 不能满足内存分配请求时，需要有合适的处理方式，如以前返回 `null pointer`，现在抛出 `exception`，但还希望能实现一些自定义的错误处理。

**不同类的特定内存分配失败处理**：有时希望根据被分配 `object` 的不同，用不同的方法处理内存分配的失败，即实现 `class-specific new-handlers`。

**旧代码兼容性及 **`nothrow new`** 的问题**：很多旧的 C++ 程序是在 `operator new` 抛出异常标准修订前编写的，需要考虑兼容性；同时 `nothrow new` 虽然能在内存分配失败时返回 `null`，但后续构造函数调用可能仍会抛出异常，存在一定局限性。

**解决方法**

**使用 **`set_new_handler`** 指定错误处理函数**：通过调用标准库函数 `set_new_handler` 来指定一个 `new-handler` 函数，当 `operator new` 无法分配内存时会调用该函数。

一个设计良好的 `new-handler` 函数可以做以下几件事之一：

+ 使更多内存可用（如在程序启动时预分配内存，在 `new-handler` 首次调用时释放）。
+ 安装一个不同的 `new-handler`。
+ 卸载 `new-handler`（将空指针传给 `set_new_handler`）。
+ 抛出一个 `bad_alloc` 或继承自 `bad_alloc` 的异常。
+ 不返回（典型情况是调用 `abort` 或 `exit`）。

**实现 **`class-specific new-handlers`：

+ 让每个类提供自己版本的 `set_new_handler` 和 `operator new`。
+ 声明一个 `new_handler` 类型的 `static member` 指向类的 `new-handler` 函数。
+ `set_new_handler` 函数保存传入的指针并返回之前保存的指针。
+ `operator new` 函数先将类的 `new-handler` 安装为全局 `new-handler`，调用全局 `operator new` 进行内存分配，若失败则调用类的 `new-handler`，若仍无法分配则恢复原来的全局 `new-handler` 并传播异常；若分配成功则返回指向分配内存的指针，并在对象析构时自动恢复全局 `new-handler`。
+ 可以创建一个 “混合风格” 基类模板 `NewHandlerSupport`，让派生类继承它来实现 `class-specific new-handler` 的功能，利用奇特的递归模板模式（CRTP）为每个继承类提供不同的 `currentHandler` 数据成员拷贝。

**代码示例**

1. **使用 **`set_new_handler`** 的基本示例**：

```cpp
// function to call if operator new can't allocate enough memory
void outOfMem() {
    std::cerr << "Unable to satisfy request for memory\n";
    std::abort();
}

int main() {
    std::set_new_handler(outOfMem);
    int *pBigDataArray = new int[100000000L];
    //...
    return 0;
}
```

2. **为类实现 **`class-specific new-handlers`** 的示例**：

```cpp
class Widget {
public:
    static std::new_handler set_new_handler(std::new_handler p) throw();
    static void * operator new(std::size_t size) throw(std::bad_alloc);
private:
    static std::new_handler currentHandler;
};

std::new_handler Widget::currentHandler = 0; // init to null in the class impl. file

std::new_handler Widget::set_new_handler(std::new_handler p) throw() {
    std::new_handler oldHandler = currentHandler;
    currentHandler = p;
    return oldHandler;
}

class NewHandlerHolder {
public:
    explicit NewHandlerHolder(std::new_handler nh) : handler(nh) {}
    ~NewHandlerHolder() {
        std::set_new_handler(handler);
    }
private:
    std::new_handler handler;
    NewHandlerHolder(const NewHandlerHolder&); 
    NewHandlerHolder& operator=(const NewHandlerHolder&); 
};

void * Widget::operator new(std::size_t size) throw(std::bad_alloc) {
    NewHandlerHolder h(std::set_new_handler(currentHandler));
    return ::operator new(size);
}

void outOfMem(); // decl. of func. to call if mem. alloc. for Widget objects fails

int main() {
    Widget::set_new_handler(outOfMem); 
    Widget *pw1 = new Widget; 
    std::string *ps = new std::string; 
    Widget::set_new_handler(0); 
    Widget *pw2 = new Widget; 
    return 0;
}
```



<font style="color:rgb(44, 44, 54);"></font>

<font style="color:rgb(44, 44, 54);"></font>

#### <font style="color:rgb(51, 51, 51);">I</font><font style="color:rgb(51, 51, 51);background-color:#FBDE28;">tem 50: 领会何时替换 new 和 delete 才有意义</font>
**问题**：

1. 内存使用错误监测问题：包括由new产生的内存未delete导致的内存泄漏、对new出的内存多次delete引发的未定义行为、data overruns（数据上溢）（在已分配块末端之后写入）和underruns（下溢）（在已分配块始端之前写入）。
2. 性能问题：编译器加载的operator new和operator delete版本为多种用途设计，采取中间路线策略，对特定程序并非最优，可能导致性能不佳，如速度慢、内存占用多等。
3. 内存使用统计数据收集问题：

<u>需要了解软件动态内存使用情况，如被分配区块大小分布、生存期分布、分配和释放顺序、使用模式是否随时间变化、任一时间内使用中动态分配内存的最大值等。</u>

4. 排列对齐问题：

很多计算机架构要求特定类型数据放置在具有特定性质的地址中，不遵守会导致运行时硬件异常或性能降低，而C++要求operator new返回适合任何数据类型排列的指针，自定义operator new可能因指针偏移导致排列对齐不恰当。

5. 缺省内存管理空间成本问题：

通用目的的内存管理器通常比自定义版本慢且使用更多内存，每个已分配区块会招致某些成本。

6. 缺省分配器排列对齐不适当问题：

有些随编译器提供的operator new不能保证某些数据类型（如doubles）的动态分配按最佳对齐方式（如八字节对齐）。

7. 相关对象聚集问题：

特定数据结构通常一起使用，希望降低页错误频率，需要将它们聚集在一起。

8. 获得不同寻常行为问题：如

在共享内存中分配和回收区块（只能通过C API管理）、用zeros复写被回收的内存以提高数据安全性等，而编译器装备版本的operators new和delete未提供这些功能。

**解决方法**：

1. 监测使用错误：自定义operator new保存已分配地址列表，operator delete从列表中移除地址监测内存泄漏和多次delete；在自定义operator new中，在分配块前后放置已知字节模式（"signatures"），operator delete检查signatures是否保持原样监测上溢和下溢。
2. <u>提升性能：对程序动态内存应用模式有充分理解时，编写operator new和operator delete的自定义版本，对于某些应用程序，可获得重大性能提升（运行更快，内存使用更少）。</u>
3. 收集使用方法的统计数据：编写operator new和operator delete的自定义版本收集相关信息，如被分配区块大小分布、生存期分布等。
4. 解决排列对齐问题：确保自定义operator new返回的指针符合C++对任何数据类型排列的要求，避免指针偏移导致的排列对齐问题。
5. 减少缺省内存管理的空间成本：使用针对small objects（小对象）调谐的分配器（如Boost的Pool library中的分配器），消除每个已分配区块的额外成本。
6. 调整缺省分配器不适当的排列对齐：用保证特定数据类型（如doubles）按最佳对齐方式（如八字节对齐）的operator new替换掉缺省版本。
7. 聚集相关的objects：使用new和delete的placement versions（参见Item 52）为特定data structures（数据结构）创建独立的heap（堆），使它们聚集在不多的几个页上。
8. 获得不同寻常的行为：编写new和delete的自定义版本（或许是placement versions），如在共享内存中分配和回收区块时用C++封装C API，或编写自定义的operator delete用zeros复写被回收的内存。

**代码示例**：

```cpp
static const int signature = 0xDEADBEEF;
typedef unsigned char Byte;

// 此代码有几个缺陷—见下文
void* operator new(std::size_t size) throw(std::bad_alloc)
{
  using namespace std;

  size_t realSize = size + 2 * sizeof(int);    // 增加请求大小以便signatures也能放进去
                                               // signatures will also fit inside

  void *pMem = malloc(realSize);               // 调用malloc获取实际内存
  if (!pMem) throw bad_alloc();                // 如果内存获取失败，抛出异常

  // 将signature写入内存的开头和结尾部分
  *(static_cast<int*>(pMem)) = signature;
  *(reinterpret_cast<int*>(static_cast<Byte*>(pMem)+realSize-sizeof(int))) =
  signature;

  // 返回一个指针，指向第一个signature之后的内存
  return static_cast<Byte*>(pMem) + sizeof(int);
}
```

此代码示例为一个便于检测underruns和overruns的global operator new的主要部分，但存在一些缺陷，如未遵循C++中operator new的惯例（如未包含调用new-handling function的循环），且存在排列对齐问题（返回的指针比从malloc得到的指针偏移了一个int大小，可能导致排列对齐不恰当)。

seastar中的内存分配就是自定义了operator new。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1742914344862-84923348-dd3b-4a11-be4b-5155f231618f.png)



#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 51: 编写 new 和 delete 时要遵守惯例</font>
operator new使用规则

1. **正确返回值**：若能分配所请求的内存，返回指向它的指针；若不能，遵循Item 49描述的规则，抛出`bad_alloc`类型的异常。
2. **多次尝试分配与new - handling function**：operator new实际上会多次尝试分配内存，每次失败后调用new - handling function，假设该函数能释放一些内存。只有当指向new - handling function的指针为空时，operator new才抛出异常。
3. **零字节请求处理**：C++要求即使请求零字节，operator new也要返回一个合理的指针。通常将零字节请求当作请求一个字节来处理，示例伪代码如下：

```cpp
void * operator new(std::size_t size) throw(std::bad_alloc)
{
    using namespace std;
    if (size == 0) {
        size = 1;
    }
    while (true) {
        // _attempt to allocate size bytes;_
        if (_the allocation was successful_)
            return (_a pointer to the memory_);

        new_handler globalHandler = set_new_handler(0);
        set_new_handler(globalHandler);

        if (globalHandler) (*globalHandler)();
        else throw std::bad_alloc();
    }
}
```

4. **获取new - handling function pointer**：由于没有办法直接得到new - handling function pointer，需调用`set_new_handler`获取其值。在单线程代码中这种方式可行，多线程环境中可能需要锁来安全操作new - handling function背后的全局数据结构。
5. **循环与new - handler行为**：operator new包含一个无限循环，跳出循环的唯一出路是内存被成功分配或new - handling function做出以下行为之一：使得更多的内存可用，安装一个不同的new - handler，卸载new - handler，抛出一个`bad_alloc`或从`bad_alloc`派生的异常，或不再返回。
6. **派生类继承问题**：operator new成员函数会被派生类继承。当基类的operator new被调用来为派生类对象分配内存时，如果基类的operator new不是为处理这种情况设计的，最佳办法是把请求转发给`::operator new`，示例如下：

```cpp
class Base {
public:
    static void * operator new(std::size_t size) throw(std::bad_alloc);
};
class Derived: public Base {
};
void * Base::operator new(std::size_t size) throw(std::bad_alloc)
{
    if (size != sizeof(Base))
        return ::operator new(size);

    // otherwise handle the request here
}
```

7. **数组内存分配**：若要在每个类的基础上控制数组的内存分配，需实现`operator new[]`。该函数仅分配一大块裸内存，不能针对数组中的对象做任何操作，也无法确定数组中对象的数量。

**operator delete使用规则**

**空指针处理**：C++保证删除空指针总是安全的，operator delete实现需遵循这一保证。非成员operator delete示例伪代码如下：

```cpp
void operator delete(void *rawMemory) throw()
{
    if (rawMemory == 0) return;
    // _deallocate the memory pointed to by rawMemory;_
}
```

**类专用版本处理**：类专用版本的operator delete需检查被删除对象的大小。若类专用的operator new将“错误”大小的请求转发给`::operator new`，则也应将“错误大小”的删除请求转发给`::operator delete`。

```cpp
class Base {
public:
    static void * operator new(std::size_t size) throw(std::bad_alloc);
    static void operator delete(void *rawMemory, std::size_t size) throw();
};
void Base::operator delete(void *rawMemory, std::size_t size) throw()
{
    if (rawMemory == 0) return;
    if (size != sizeof(Base)) {
        ::operator delete(rawMemory);
        return;
    }
    // _deallocate the memory pointed to by rawMemory;_
    return;
}
```

**虚拟析构函数的影响**：若被删除的对象是从一个缺少虚拟析构函数的基类派生出来的，C++传递给operator delete的`size_t`值可能不正确。因此，确保基类拥有虚拟析构函数很重要。下面是两个代码示例，分别展示了 `operator new` 和 `placement new` 的用法和区别。



#### <font style="color:rgb(51, 51, 51);background-color:#FBDE28;">Item 52: 如果编写了 placement new，就要编写 placement delete</font>
<font style="background-color:#FDE6D3;">前置：operator new和placement new区别</font>

1. **使用 **`**operator new**`** 示例**

```cpp
#include <iostream>
#include <new> // 包含 new 的定义
class MyClass {
public:
    MyClass() { std::cout << "MyClass Constructor\n"; }
    ~MyClass() { std::cout << "MyClass Destructor\n"; }
};
int main() {
    // 使用 operator new 分配内存并构造对象
    MyClass* obj = new MyClass(); // 调用了 operator new 和构造函数
    // 使用 delete 释放内存并调用析构函数
    delete obj;
    return 0;
}
```

```cpp
MyClass Constructor
MyClass Destructor
```

+ `new MyClass()` 做了两件事：
    1. 调用 `operator new` 分配内存。
    2. 调用 `MyClass` 的构造函数来初始化对象。
+ `delete obj` 做了两件事：
    1. 调用 `MyClass` 的析构函数。
    2. 调用 `operator delete` 释放内存。

2. 使用 `placement new` 示例

```cpp
#include <iostream>
#include <new> // 包含 placement new 的定义
#include <cstdlib> // 包含 malloc/free 的定义

class MyClass {
public:
    MyClass() { std::cout << "MyClass Constructor\n"; }
    ~MyClass() { std::cout << "MyClass Destructor\n"; }
};

int main() {
    // 手动分配一段原始内存（未初始化）
    void* memory = std::malloc(sizeof(MyClass));
    if (!memory) {
        std::cerr << "Memory allocation failed!\n";
        return 1;
    }
    // 使用 placement new 在已分配的内存上构造对象
    MyClass* obj = new(memory) MyClass(); // 只调用构造函数，不分配内存
    // 手动调用析构函数
    obj->~MyClass();

    // 释放原始内存
    std::free(memory);

    return 0;
}
```



```cpp
MyClass Constructor
MyClass Destructor
```

+ `std::malloc(sizeof(MyClass))` 分配了一段原始内存，但没有调用任何构造函数。
+ `new(memory) MyClass()` 是 `placement new` 的形式，它在已经分配好的内存地址 `memory` 上调用了 `MyClass` 的构造函数。
+ `obj->~MyClass()` 手动调用了析构函数，清理对象。
+ `std::free(memory)` 释放了原始内存。

| 特性 | `operator new` | `placement new` |
| --- | --- | --- |
| **分配内存** | 是，分配新的内存 | 否，使用已分配的内存 |
| **构造对象** | 是，调用构造函数 | 是，调用构造函数 |
| **释放内存** | 使用 `delete` 自动释放 | 需要手动释放（如 `free` 或其他方式） |
| **常见用途** | 普通动态对象创建 | 自定义内存池、性能优化等场景 |


****

****

**前置2：**`**placement new**`** 和 **`**placement delete**`** 的参数设置规则**

`<font style="background-color:#FDE6D3;">placement new</font>`<font style="background-color:#FDE6D3;"> 参数规则</font>

+ **必备参数**：`placement new` 函数必须有一个 `std::size_t` 类型的参数，它代表要分配的内存大小。这是所有 `operator new` 函数都必须具备的参数。
+ **额外参数**：除了 `std::size_t` 参数外，`placement new` 可以有额外的参数，这些额外参数能让你在分配内存时提供更多信息，比如指定内存地址、记录日志等。例如：

```cpp
#include <iostream>
#include <new>

// 自定义 placement new，带有额外的 std::ostream& 参数用于记录日志
void* operator new(std::size_t size, std::ostream& logStream) {
    logStream << "Allocating " << size << " bytes of memory." << std::endl;
    return ::operator new(size);
}

class MyClass {
public:
    MyClass() { std::cout << "MyClass constructed." << std::endl; }
};

int main() {
    MyClass* obj = new (std::cout) MyClass();
    delete obj;
    return 0;
}
```

+ **匹配原则**：`placement delete` 的参数必须和对应的 `placement new` 完全一致。这是因为当 `placement new` 分配内存后，如果对象构造函数抛出异常，C++ 运行时系统会依据参数匹配来调用相应的 `placement delete` 释放内存。若参数不匹配，运行时系统就无法找到合适的 `placement delete`，进而导致内存泄漏。例如：

```cpp
#include <iostream>
#include <new>
class Widget {
public:
    // placement new 带有额外的 std::ostream& 参数
    static void* operator new(std::size_t size, std::ostream& logStream) {
        logStream << "Allocating memory for Widget" << std::endl;
        return ::operator new(size);
    }
    // 对应的 placement delete，参数与 placement new 一致
    static void operator delete(void* pMemory, std::ostream& logStream) {
        logStream << "Deallocating memory for Widget" << std::endl;
        ::operator delete(pMemory);
    }
    Widget() {
        // 模拟构造函数抛出异常
        throw std::exception();
    }
};

int main() {
    try {
        Widget* pw = new (std::cout) Widget();
        delete pw;
    } catch (const std::exception& e) {
        std::cout << "Exception caught: " << e.what() << std::endl;
    }
    return 0;
}
```

C++ 标准库中定义了一个标准的 `placement new`，其形式为 `void* operator new(std::size_t, void* pMemory)`，用于在指定的内存位置构造对象。自定义 `placement new` 时应尽量遵循这种简洁和通用的设计原则。



**问题**

当使用带有额外参数（即 placement 版本）的 operator new 分配内存，若对象构造函数抛出异常，运行时系统需撤销内存分配，但由于运行时系统不了解该 operator new 具体工作方式，会寻找与该 operator new 具有相同数量和类型额外参数的 operator delete 来撤销分配。若未声明对应的 placement delete 版本，就会导致内存泄漏。

在类中声明 operator new 时，类的成员函数名字会覆盖外围具有相同名字的函数（包括全局和继承来的版本），可能导致客户无法使用他们期望的 new 形式。

**解决方法**

为消除内存泄漏，对于每一个带有额外参数的 operator new（placement new），都要声明一个带有同样额外参数的 operator delete（placement delete）与之匹配。这样当构造函数抛出异常时，运行时系统能调用相应的 placement delete 来撤销内存分配。

为避免覆盖常规的 new 和 delete 形式，可创建一个包含 new 和 delete 全部常规形式的基类，然后让需要增加自定义形式的类继承该基类，并使用 using declarations 使基类中的常规形式可见。

**注意事项**

1. 只有在调用与 placement new 相关联的构造函数时发生异常，placement delete 才会被调用。将 delete 施加于一个指针（如普通的 delete 操作）绝对不会引起 delete 的 placement 版本的调用。
2. 在类中声明任何 operator new 都会覆盖 C++ 在全局范围提供的标准形式（包括 normal new、placement new、nothrow new 等）。除非有意防止类的客户使用这些标准形式，否则需确保除自定义 new 形式外，这些标准形式都可用，并且为每个可用的 operator new 提供相应的 operator delete。若希望这些函数具有通常行为，可让类专用版本调用全局版本。



1. **导致内存泄漏的示例**

```cpp
class Widget {
public:
   ...
    static void* operator new(std::size_t size,              // non-normal
                              std::ostream& logStream)       // form of new
        throw(std::bad_alloc);

    static void operator delete(void *pMemory,               // normal class-
                               std::size_t size) throw();   // specific form
                                                           // of delete
   ...
};

// 客户代码，可能导致内存泄漏
Widget *pw = new (std::cerr) Widget; 
```

2. **修复内存泄漏的示例**

```cpp
class Widget {
public:
   ...
    static void* operator new(std::size_t size, std::ostream& logStream)
        throw(std::bad_alloc);
    static void operator delete(void *pMemory) throw();

    static void operator delete(void *pMemory, std::ostream& logStream)
        throw();
   ...
};

// 客户代码，不会发生内存泄漏
Widget *pw = new (std::cerr) Widget; 
```



