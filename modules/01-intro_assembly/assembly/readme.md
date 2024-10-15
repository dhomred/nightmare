# Introduction to Assembly

So the first big wall you will need to tackle is starting to learn assembly. It may be a little bit tough, but it is perfectly doable and a critical step for what comes after. To start this off, I would recommend watching this video. It was made by the guy who actually got me interested in this line of work. I started off learning assembly by watching this video several times. It's really well put together:

# مقدمة إلى لغة التجميع

أول حاجز كبير ستحتاج إلى تجاوزه هو البدء في تعلم لغة التجميع. قد يكون الأمر صعبًا بعض الشيء، لكنه ممكن تمامًا وهو خطوة حاسمة لما يأتي بعد ذلك. للبدء، أوصي بمشاهدة هذا الفيديو. تم إنشاؤه بواسطة الشخص الذي جعلني مهتمًا بهذا المجال. بدأت بتعلم لغة التجميع بمشاهدة هذا الفيديو عدة مرات. إنه منظم بشكل جيد للغاية:


```
[x86 Assembly Crash Course](https://www.youtube.com/watch?v=75gBFiFtAb8)
```

Now that you have watched the video, we will go through some documentation explaining some of the concepts around assembly code. A lot of this will be a repeat of that video, some of it won't be. Also all of this documentation will be for the Intel syntax. And one more thing; you don't need to have everything here memorized before moving on, and parts of it will make more sense when you actually see it in action.

الآن بعد أن شاهدت الفيديو، سنمر على بعض الوثائق التي تشرح بعض المفاهيم حول كود التجميع. الكثير من هذا سيكون تكرارًا لذلك الفيديو، وبعضه لن يكون كذلك. أيضًا، كل هذه الوثائق ستكون للتركيب Intel. وشيء آخر؛ لا تحتاج إلى حفظ كل شيء هنا قبل الانتقال، وستكون بعض الأجزاء أكثر منطقية عندما تراها فعليًا في العمل.

## Compiling

So first off, what is assembly code? Assembly code is the code that is actually run on your computer by the processor. For instance take some C code:

## التجميع

أولاً، ما هو كود التجميع؟ كود التجميع هو الكود الذي يتم تشغيله فعليًا على جهاز الكمبيوتر الخاص بك بواسطة المعالج. على سبيل المثال، خذ بعض كود C:



```
#include <stdio.h>

void main(void)
{
    puts("Hello World!");
}
```

This code is not what runs. This code is compiled into assembly code, which looks like this:


هذا الكود ليس ما يتم تشغيله. يتم تجميع هذا الكود إلى كود تجميع، والذي يبدو هكذا:



```
0000000000001135 <main>:
    1135:       55                      push   rbp
    1136:       48 89 e5                mov    rbp,rsp
    1139:       48 8d 3d c4 0e 00 00    lea    rdi,[rip+0xec4]        # 2004 <_IO_stdin_used+0x4>
    1140:       e8 eb fe ff ff          call   1030 <puts@plt>
    1145:       90                      nop
    1146:       5d                      pop    rbp
    1147:       c3                      ret
    1148:       0f 1f 84 00 00 00 00    nop    DWORD PTR [rax+rax*1+0x0]
    114f:       00
```

The purpose of languages like C, is that we can program without having to really deal with assembly code. We write code that is handed to a compiler, and the compiler takes that code and generates assembly code that will accomplish whatever the C code tells it to. Then the assembly code is what is actually ran on the processor. Since this is the code that is actually ran, it helps to understand it. Also since most of the time we are handed compiled binaries, we often only have the assembly code to work from. However, we have tools such as Ghidra that will take compiled assembly code and give us a view of what it thinks the C code that the code was compiled from looks like, so we don't need to read endless lines of assembly code. This is called a decompiler.

With assembly code, there are a lot of different architectures. Different types of processors can run different types of assembly code. The two we are dealing with the most here will be 64 bit, and 32 bit ELF (Executable and Linkable Format). I will often call these two things `x64` and `x86`.


الغرض من لغات مثل C هو أننا يمكننا البرمجة دون الحاجة إلى التعامل مع كود التجميع. نكتب كودًا يتم تسليمه إلى المجمع، ويأخذ المجمع هذا الكود وينشئ كود تجميع يحقق ما يخبره به كود C. ثم يتم تشغيل كود التجميع فعليًا على المعالج. نظرًا لأن هذا هو الكود الذي يتم تشغيله فعليًا، فإنه يساعد على فهمه. أيضًا، نظرًا لأننا في معظم الأحيان نحصل على ملفات ثنائية مجمعة، فإننا غالبًا ما يكون لدينا كود التجميع فقط للعمل منه. ومع ذلك، لدينا أدوات مثل Ghidra التي ستأخذ كود التجميع المجمّع وتمنحنا عرضًا لما تعتقد أن كود C الذي تم تجميع الكود منه يبدو عليه، لذلك لا نحتاج إلى قراءة سطور لا نهاية لها من كود التجميع. هذا يسمى مجمع عكسي.

مع كود التجميع، هناك الكثير من المعماريات المختلفة. يمكن لأنواع مختلفة من المعالجات تشغيل أنواع مختلفة من كود التجميع. النوعان اللذان نتعامل معهما هنا هما 64 بت و 32 بت ELF (تنسيق تنفيذي وقابل للربط). سأطلق على هذين النوعين غالبًا `x64` و `x86`.


## Registers

Registers are essentially places that the processor can store memory. You can think of them as buckets which the processor can store information in. Here is a list of the `x64` registers, and what their common use cases are.

## السجلات

السجلات هي في الأساس أماكن يمكن للمعالج تخزين الذاكرة فيها. يمكنك التفكير فيها كدلاء يمكن للمعالج تخزين المعلومات فيها. إليك قائمة بسجلات `x64` وما هي استخداماتها الشائعة.



```
rbp: Base Pointer, points to the bottom of the current stack frame | مؤشر القاعدة، يشير إلى أسفل إطار المكدس الحالي
rsp: Stack Pointer, points to the top of the current stack frame   | مؤشر المكدس، يشير إلى أعلى إطار المكدس الحالي
rip: Instruction Pointer, points to the instruction to be executed | مؤشر التعليمات، يشير إلى التعليمات التي سيتم تنفيذها

General Purpose Registers | السجلات العامة
These can be used for a variety of different things. | .يمكن استخدامها لمجموعة متنوعة من الأشياء
rax:
rbx:
rcx:
rdx:
rsi:
rdi:
r8:
r9:
r10:
r11:
r12:
r13:
r14:
r15:
```

In `x64` linux arguments to a function are passed in via registers. The first few args are passed in by these registers:


في `x64` يتم تمرير الوسائط إلى دالة عبر السجلات. يتم تمرير الوسائط القليلة الأولى بواسطة هذه السجلات:

```
rdi:    First Argument  |  الوسيط الأول
rsi:    Second Argument | الوسيط الثاني
rdx:    Third Argument  | الوسيط الثالث
rcx:    Fourth Argument | الوسيط الرابع
r8:     Fifth Argument  | الوسيط الخامس
r9:     Sixth Argument  | الوسيط السادس
```

With the `x86` elf architecture, arguments are passed onto the stack. Also, as you may know, in C, functions can return a value. In `x64`, this value is passed in the `rax` register. In `x86` this value is passed in the `eax` register.

في بنية `x86` elf، يتم تمرير الوسائط إلى المكدس. أيضًا، كما قد تعرف، في C، يمكن للدوال إرجاع قيمة. في `x64`، يتم تمرير هذه القيمة في سجل `rax`. في `x86` يتم تمرير هذه القيمة في سجل `eax`.

هناك أيضًا اختلافات


There are also different sizes for registers. The typical sizes we will be dealing with are `8` bytes, `4` bytes, `2` bytes, and `1` byte. The reason for these different sizes is due to the advancement of technology, so that we can store more data in a register.

هناك أيضًا أحجام مختلفة للسجلات. الأحجام النموذجية التي سنتعامل معها هي `8` بايت، `4` بايت، `2` بايت، و `1` بايت. السبب في هذه الأحجام المختلفة هو التقدم التكنولوجي، بحيث يمكننا تخزين المزيد من البيانات في السجل.

```
+-----------------+---------------+---------------+------------+
| 8 Byte Register | Lower 4 Bytes | Lower 2 Bytes | Lower Byte |
+-----------------+---------------+---------------+------------+
|   rbp           |     ebp       |     bp        |     bpl    |
|   rsp           |     esp       |     sp        |     spl    |
|   rip           |     eip       |               |            |
|   rax           |     eax       |     ax        |     al     |
|   rbx           |     ebx       |     bx        |     bl     |
|   rcx           |     ecx       |     cx        |     cl     |
|   rdx           |     edx       |     dx        |     dl     |
|   rsi           |     esi       |     si        |     sil    |
|   rdi           |     edi       |     di        |     dil    |
|   r8            |     r8d       |     r8w       |     r8b    |
|   r9            |     r9d       |     r9w       |     r9b    |
|   r10           |     r10d      |     r10w      |     r10b   |
|   r11           |     r11d      |     r11w      |     r11b   |
|   r12           |     r12d      |     r12w      |     r12b   |
|   r13           |     r13d      |     r13w      |     r13b   |
|   r14           |     r14d      |     r14w      |     r14b   |
|   r15           |     r15d      |     r15w      |     r15b   |
+-----------------+---------------+---------------+------------+
```

In `x64` we will see the `8` byte registers. However in `x86` the largest registers we can use are the `4` byte registers like `ebp`, `esp`, `eip` etc. Now we can also use smaller registers than the maximum sized registers for the architecture.

في `x64` سنرى السجلات ذات الـ `8` بايت. ومع ذلك، في `x86` أكبر السجلات التي يمكننا استخدامها هي السجلات ذات الـ `4` بايت مثل `ebp`، `esp`، `eip`، إلخ. الآن يمكننا أيضًا استخدام سجلات أصغر من السجلات ذات الحجم الأقصى للمعمارية.

In `x64` there is the `rax`, `eax`, `ax`, and `al` register. The `rax` register points to the full `8`. The `eax` register is just the lower four bytes of the `rax` register. The `ax` register is the last `2` bytes of the `rax` register. Lastly the `al` register is the last byte of the `rax` register.


في `x64` هناك سجلات `rax`، `eax`، `ax`، و `al`. سجل `rax` يشير إلى الـ `8` بايت الكاملة. سجل `eax` هو فقط الأربعة بايت السفلى من سجل `rax`. سجل `ax` هو آخر `2` بايت من سجل `rax`. وأخيرًا، سجل `al` هو آخر بايت من سجل `rax`.


## Words

You might hear the term word throughout this. A word is just two bytes of data. A dword is four bytes of data. A qword is eight bytes of data.

## الكلمات

قد تسمع مصطلح "كلمة" خلال هذا. الكلمة هي مجرد اثنين من البايتات من البيانات. الكلمة المزدوجة (dword) هي أربعة بايتات من البيانات. الكلمة الرباعية (qword) هي ثمانية بايتات من البيانات.

## Stacks

Now one of the most common memory regions you will be dealing with is the stack. It is where local function variables in the code are stored.

For instance, in this code the variable `x` is stored in the stack:

## المكدسات

الآن، واحدة من أكثر مناطق الذاكرة شيوعًا التي ستتعامل معها هي المكدس. إنه المكان الذي يتم فيه تخزين المتغيرات المحلية للدوال في الكود.

على سبيل المثال، في هذا الكود يتم تخزين المتغير `x` في المكدس:
```
#include <stdio.h>

void main(void)
{
    int x = 5;
    puts("hi");
}
```

Now we can see it is stored on the stack at `rbp-0x4`.


الآن يمكننا أن نرى أنه يتم تخزينه في المكدس عند `rbp-0x4`.


```
0000000000001135 <main>:
    1135:       55                      push   rbp
    1136:       48 89 e5                mov    rbp,rsp
    1139:       48 83 ec 10             sub    rsp,0x10
    113d:       c7 45 fc 05 00 00 00    mov    DWORD PTR [rbp-0x4],0x5
    1144:       48 8d 3d b9 0e 00 00    lea    rdi,[rip+0xeb9]        # 2004 <_IO_stdin_used+0x4>
    114b:       e8 e0 fe ff ff          call   1030 <puts@plt>
    1150:       90                      nop
    1151:       c9                      leave
    1152:       c3                      ret
    1153:       66 2e 0f 1f 84 00 00    nop    WORD PTR cs:[rax+rax*1+0x0]
    115a:       00 00 00
    115d:       0f 1f 00                nop    DWORD PTR [rax]
```

Now values on the stack are moved on by either pushing them onto the stack, or popping them off. That is the only way to add or remove values from the stack, as it is a FILO(First In, Last Out) data structure. However, we can read and reference values on the stack at any time.

الآن يتم نقل القيم على المكدس إما بدفعها إلى المكدس (push) أو بإزالتها منه (pop). هذه هي الطريقة الوحيدة لإضافة أو إزالة القيم من المكدس، حيث إنه هيكل بيانات من نوع FILO (First In, Last Out) أي "الأول في الدخول، الأخير في الخروج". ومع ذلك، يمكننا قراءة والإشارة إلى القيم على المكدس في أي وقت.

The exact bounds of the stack is recorded by two registers, `rbp` and `rsp`. The base pointer `rbp` points to the bottom of the stack. The stack pointer `rsp` points to the top of the stack.

يتم تسجيل الحدود الدقيقة للمكدس بواسطة سجلين، `rbp` و `rsp`. يشير مؤشر القاعدة `rbp` إلى أسفل المكدس. يشير مؤشر المكدس `rsp` إلى أعلى المكدس.

## Flags

There is one register that contains flags. A flag is a particular bit of this register. If it is set or not typically means something. Here is the list of flags.

## الأعلام

هناك سجل واحد يحتوي على الأعلام. العلم هو بت معين في هذا السجل. إذا تم تعيينه أو لم يتم تعيينه عادةً يعني شيئًا ما. إليك قائمة بالأعلام.
```
00:     Carry Flag       |  علم الحمل
01:     always 1         |  دائمًا 1
02:     Parity Flag      |  علم التماثل
03:     always 0         |  دائمًا 0 
04:     Adjust Flag      |  علم التعديل
05:     always 0         |  دائمًا 0
06:     Zero Flag        |  علم الصفر
07:     Sign Flag        |  علم الإشارة
08:     Trap Flag        |  علم الفخ
09:     Interruption Flag    |  علم المقاطعة
10:     Direction Flag    |  علم الاتجاه
11:     Overflow Flag     |  علم الفائض
12:     I/O Privilege Field lower bit    |   حقل امتياز الإدخال/الإخراج البت الأدنى
13:     I/O Privilege Field higher bit    |  حقل امتياز الإدخال/الإخراج البت الأعلى
14:     Nested Task Flag     |  علم المهمة المتداخلة
15:     Resume Flag       |  علم الاستئناف
```

There are other flags then the one listed, however we really don't deal with them too much (and out of these, there are only a few we actively deal with).

هناك أعلام أخرى غير المذكورة، ومع ذلك، نحن لا نتعامل معها كثيرًا (ومن بين هذه الأعلام، هناك عدد قليل فقط نتعامل معه بشكل نشط).

If you want to hear more about this, check out: [Book on x86 Assembly and Architecture](https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture)

إذا كنت ترغب في معرفة المزيد عن هذا، تحقق من: [كتاب عن x86 Assembly and Architecture](https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture)

## Instructions

Now we will be covering some of the more common instructions you will see. This isn't every instruction you will see, just the often used ones.


## التعليمات

الآن سنغطي بعض التعليمات الأكثر شيوعًا التي ستراها. هذه ليست كل التعليمات التي ستراها، فقط الأكثر استخدامًا.

#### mov

The move instruction just moves data from one register to another. For instance:

#### mov

تعليمات النقل (mov) تنقل البيانات من سجل إلى آخر. على سبيل المثال:
```
mov rax, rdx
```

This will just move the data from the `rdx` register to the `rax` register. Note that the data is moved into the *first* argument, not the second.

هذا سينقل البيانات من سجل `rdx` إلى سجل `rax`. لاحظ أن البيانات تُنقل إلى الوسيط *الأول*، وليس الثاني.
#### dereference

If you ever see brackets like `[]`, they are meant to dereference, which deals with pointers. A pointer is a value that points to a particular memory address (it is a memory address). Dereferencing a pointer means to treat a pointer like the value it points to. Put another way, a pointer is a variable that holds a memory address, and to dereference that pointer means you are accessing the value stored at that memory address. For instance:


#### إلغاء المرجع

إذا رأيت أقواسًا مثل `[]`، فهي تعني إلغاء المرجع، والذي يتعامل مع المؤشرات. المؤشر هو قيمة تشير إلى عنوان ذاكرة معين (هو عنوان ذاكرة). إلغاء مرجع المؤشر يعني التعامل مع المؤشر كالقيمة التي يشير إليها. بعبارة أخرى، المؤشر هو متغير يحمل عنوان ذاكرة، وإلغاء مرجع هذا المؤشر يعني أنك تصل إلى القيمة المخزنة في ذلك العنوان. على سبيل المثال:
```
mov rax, [rdx]
```

Will move the value pointed to by `rdx` into the `rax` register. On the flipside:

سينقل القيمة التي يشير إليها `rdx` إلى سجل `rax`. على الجانب الآخر:

```
mov [rax], rdx
```

Will move the value of the `rdx` register into whatever memory is pointed to by the `rax` register. The actual value of the `rax` register does not change.

سينقل قيمة سجل `rdx` إلى أي ذاكرة يشير إليها سجل `rax`. القيمة الفعلية لسجل `rax` لا تتغير.
#### lea

The lea instruction calculates the address of the second operand, and moves that address in the first. For instance:


#### lea

تعليمات `lea` تحسب عنوان المعامل الثاني، وتنقل ذلك العنوان إلى الأول. على سبيل المثال:
```
lea rdi, [rbx+0x10]
```

This will move the address `rbx+0x10` into the `rdi` register.

سيقوم هذا بنقل العنوان `rbx+0x10` إلى السجل `rdi`.

#### add
This just adds the two values together, and stores the sum in the first argument. For instance:

### add               |                 اضف 
تقوم هذه العملية بجمع القيمتين معًا وتخزين المجموع في الوسيطة الأولى. على سبيل المثال:
```
add rax, rdx
```

That will add the value of `rdx` to `rax`, setting `rax` equal to `rax + rdx`.

سيضيف ذلك قيمة `rdx` إلى `rax`، مما يجعل `rax` يساوي `rax + rdx`.

#### sub              |                 طرح

This value will subtract the second operand from the first one, and store the difference in the first argument. For instance:


#### sub
تقوم هذه العملية بطرح المعامل الثاني من الأول، وتخزين الفرق في الوسيطة الأولى. على سبيل المثال:

```
sub rsp, 0x10
```

This will subract 10 from `rsp`, setting the `rsp` register equal to `rsp - 0x10`

سيطرح ذلك 10 من `rsp`، مما يجعل السجل `rsp` يساوي `rsp - 0x10`.

#### xor

This will perform the binary operation xor on the two arguments it is given, and stores the result in the first argument:

#### XOR
تقوم هذه العملية بتنفيذ عملية XOR الثنائية على الوسيطتين المعطاتين، وتخزين النتيجة في الوسيطة الأولى. على سبيل المثال:
```
xor rdx, rax
```

This will set the `rdx` register equal to `rdx ^ rax`. If  the `xor` instruction is used with the same register for both argument, for example `xor rax, rax`, it will set all bits to zero, clearing the register.

سيجعل ذلك السجل `rdx` يساوي `rdx ^ rax`. إذا تم استخدام تعليمة `xor` مع نفس السجل لكل من الوسيطتين، على سبيل المثال `xor rax, rax`، فسيتم تعيين جميع البتات إلى الصفر، مما يؤدي إلى مسح السجل.

To understand how `xor` works, you must understand that it is a *bitwise* operation, meaning it operates on the bits of a register. It compares the bits in each place, and sets the resulting bit to `1` if the bits are different, and `0` if they are the same. So for example, `xor 1011 1100` would return `0111`.

لفهم كيفية عمل `xor`، يجب أن تفهم أنها عملية *ثنائية*، مما يعني أنها تعمل على بتات السجل. تقارن البتات في كل مكان، وتحدد البت الناتج إلى `1` إذا كانت البتات مختلفة، و `0` إذا كانت متشابهة. على سبيل المثال، `xor 1011 1100` ستعيد `0111`.

The `and` and `or` instructions essentially do the same thing, except with the `AND` or `OR` binary operators. For `and` it sets the resulting bit to `1` if both bits are also `1`, otherwise it sets it to `0`. For `or` it sets the resulting bit to `1` if either bit is `1`, otherwise it is set to `0`.

تعليمات `and` و `or` تقومان بنفس الشيء بشكل أساسي، باستثناء استخدام مشغلي `AND` أو `OR` الثنائيين. بالنسبة لـ `and`، يتم تعيين البت الناتج إلى `1` إذا كانت كلا البتين أيضًا `1`، وإلا يتم تعيينها إلى `0`. بالنسبة لـ `or`، يتم تعيين البت الناتج إلى `1` إذا كانت أي من البتين `1`، وإلا يتم تعيينها إلى `0`.

#### push

The `push` instruction will grow the stack by either `8` bytes (for `x64`, `4` for `x86`), then push the contents of a register onto the new stack space. For instance:

#### push                 |                     دفع
تعليمة `push` ستزيد المكدس بمقدار `8` بايت (لـ `x64`، `4` لـ `x86`)، ثم تدفع محتويات السجل إلى مساحة المكدس الجديدة. على سبيل المثال:
```
push rax
```

This will grow the stack by `8` bytes, and the contents of the `rax` register will be on top of the stack.

سيزيد هذا من حجم المكدس بمقدار `8` بايت، وستكون محتويات مسجل `rax` في أعلى المكدس.

#### pop

The `pop` instruction will pop the top `8` bytes (for `x64`, `4` for `x86`) off of the stack and into the argument. Then it will shrink the stack. For instance:


#### pop

تعليمة `pop` ستزيل أعلى `8` بايت (لـ `x64`, `4` لـ `x86`) من المكدس وتضعها في الوسيطة. ثم ستقلص حجم المكدس. على سبيل المثال:
```
pop rax
```

The top `8` bytes of the stack will end up in the `rax` register.

أعلى `8` بايت من المكدس ستنتهي في مسجل `rax`.

#### jmp

The `jmp` instruction will jump to an instruction address. It is used to redirect code execution. For instance:

#### jmp                    |                     قفز 

تعليمة `jmp` ستقفز إلى عنوان تعليمة. تُستخدم لإعادة توجيه تنفيذ الكود. على سبيل المثال:
```
jmp 0x602010
```

That instruction will cause the code execution to jump to `0x602010`, and execute whatever instruction is there.

ستتسبب هذه التعليمة في قفز تنفيذ الكود إلى `0x602010`، وتنفيذ أي تعليمة موجودة هناك.

#### call & ret

This is similar to the `jmp` instruction. The difference is it will push the values of `rbp` and `rip` onto the stack, then jump to whatever address it is given. This is used for calling functions. After the function is finished, a `ret` instruction is called which uses the pushed values of `rbp` and `rip` (saved base and instruction pointers) to return, it can continue execution right where it left off.

#### call & ret

هذا مشابه لتعليمة `jmp`. الفرق هو أنها ستدفع قيم `rbp` و `rip` إلى المكدس، ثم تقفز إلى أي عنوان يتم إعطاؤه. تُستخدم هذه التعليمة لاستدعاء الدوال. بعد انتهاء الدالة، يتم استدعاء تعليمة `ret` التي تستخدم القيم المدفوعة لـ `rbp` و `rip` (مؤشرات الأساس والتعليمات المحفوظة) للعودة، ويمكنها متابعة التنفيذ من حيث توقفت.

#### cmp

The cmp instruction is similar to that of the sub instruction. Except it doesn't store the result in the first argument. It checks if the result is less than zero, greater than zero, or equal to zero. Depending on the value it will set the flags accordingly.

#### cmp

تعليمة `cmp` مشابهة لتعليمة `sub`. الفرق هو أنها لا تخزن النتيجة في الوسيطة الأولى. تتحقق مما إذا كانت النتيجة أقل من الصفر، أكبر من الصفر، أو تساوي الصفر. بناءً على القيمة، ستضبط الأعلام وفقًا لذلك.
#### jnz / jz

The `jump if not zero` and `jump if zero` (`jnz/jz`) instructions are pretty similar to the jump instruction. The difference is they will only execute the jump depending on the status of the `zero` flag. For `jz` it will only jump if the `zero` flag is set. The opposite is true for `jnz`. These instructions are how control flow is implemented in assembly.

#### jnz / jz

تعليمات `القفز إذا لم يكن صفرًا` و `القفز إذا كان صفرًا` (`jnz/jz`) مشابهة جدًا لتعليمة القفز. الفرق هو أنها ستنفذ القفز فقط بناءً على حالة علم الصفر. بالنسبة لـ `jz`، ستقفز فقط إذا تم ضبط علم الصفر. والعكس صحيح بالنسبة لـ `jnz`. تُستخدم هذه التعليمات لتنفيذ تدفق التحكم في لغة التجميع.
