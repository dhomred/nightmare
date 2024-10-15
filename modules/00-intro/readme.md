# Intro

So I just want to say a few things for the people who are super new to binary exploitation / reverse engineering. If you are already familiar with assembly code / binary exploitation and reverse engineering, and tools like ghidra / pwntools / gdb, feel free to skip this whole section (and any other content you already know). The purpose of this section is to give an introduction to the super new people.

# المقدمة

أود أن أقول بعض الأشياء للأشخاص الجدد تمامًا في استغلال الثغرات في البرمجيات العكسية. إذا كنت بالفعل على دراية بلغة التجميع واستغلال الثغرات العكسية وأدوات مثل Ghidra و Pwntools و GDB، يمكنك تخطي هذا القسم بالكامل (وأي محتوى آخر تعرفه بالفعل). الغرض من هذا القسم هو تقديم مقدمة للأشخاص الجدد تمامًا.

## Binary Exploitation

First off what's a binary?

A binary is compiled code. When a programmer writes code in a language like C, the C code isn't what gets actually ran. It is compiled into a binary file and the binary is run. Binary exploitation is the process of actually exploiting a binary, but what does that mean?

In a lot of code, you will find bugs. Think of a bug as a mistake in code that will allow for unintended functionality. As an attacker we can leverage this bug to attack the binary, and actually force it to do what we want by getting code execution. That means we actually have the binary execute whatever code that we want, and can essentially hack the code.

## استغلال الثغرات في البرمجيات

أولاً، ما هو الملف الثنائي؟

الملف الثنائي هو كود مجمع. عندما يكتب المبرمج كودًا بلغة مثل C، فإن الكود المكتوب بلغة C ليس هو ما يتم تشغيله فعليًا. يتم تجميعه إلى ملف ثنائي ويتم تشغيل الملف الثنائي. استغلال الثغرات في البرمجيات هو عملية استغلال الملف الثنائي، ولكن ماذا يعني ذلك؟

في الكثير من الأكواد، ستجد أخطاء. فكر في الخطأ كخطأ في الكود يسمح بوظائف غير مقصودة. كمهاجم، يمكننا استغلال هذا الخطأ لمهاجمة الملف الثنائي، وإجباره فعليًا على تنفيذ الكود الذي نريده، ويمكننا بشكل أساسي اختراق الكود.

## Reverse Engineering

What is reverse engineering?

Reverse engineering is the process of figuring out how something works. It is a critical part of binary exploitation, since most of the time you are just handed a binary without any clue as to what it does. You have to figure out how it works, so you can attack it.

## الهندسة العكسية

ما هي الهندسة العكسية؟

الهندسة العكسية هي عملية معرفة كيفية عمل شيء ما. إنها جزء حاسم من استغلال الثغرات في البرمجيات، حيث أنك في معظم الأحيان تحصل فقط على ملف ثنائي دون أي فكرة عن ما يفعله. عليك معرفة كيفية عمله، حتى تتمكن من مهاجمته.

## Objective

Most of the time, your objective is to obtain code execution on a box and pop a root shell. If you have a different objective, it will usually be stated on the top line of the writeup. In almost every instance where your objective isn't to pop a shell, it's to get a CTF flag associated with this challenge, from the binary.

## الهدف

في معظم الأحيان، يكون هدفك هو الحصول على تنفيذ الكود على جهاز والحصول على صلاحيات الجذر. إذا كان لديك هدف مختلف، فسيتم ذكره عادةً في السطر العلوي من الشرح. في كل حالة تقريبًا حيث لا يكون هدفك هو الحصول على صلاحيات الجذر، يكون الهدف هو الحصول على علم CTF المرتبط بهذا التحدي، من الملف الثنائي.

## What should I know going into this course?

There are a few areas that will help. If you know how to code, that will help. If you know how to code somewhat low level languages like C, that will help more. Also an understanding of the basics of how to use Linux helps a lot. But realistically, the only thing you really need is the ability to google things, and find answers by yourself.

## ماذا يجب أن أعرف قبل الدخول في هذه الدورة؟

هناك بعض المجالات التي ستساعد. إذا كنت تعرف كيفية البرمجة، فسيكون ذلك مفيدًا. إذا كنت تعرف كيفية البرمجة بلغات منخفضة المستوى مثل C، فسيكون ذلك أكثر فائدة. أيضًا، فهم أساسيات كيفية استخدام Linux يساعد كثيرًا. ولكن بشكل واقعي، الشيء الوحيد الذي تحتاجه حقًا هو القدرة على البحث عن الأشياء على Google والعثور على الإجابات بنفسك.

## Why CTF Challenges?

The reason why I went with CTF challenges for teaching binary exploitation / reverse engineering, is because most challenges only contain a small subset of exploitation knowledge. With this format I can split it up into different subjects like `buffer overflow into calling shellcode` and `fast bin exploitation`, so it can be covered like a somewhat normal course.

## لماذا تحديات CTF؟

السبب في اختياري لتحديات CTF لتعليم استغلال الثغرات في البرمجيات والهندسة العكسية، هو أن معظم التحديات تحتوي فقط على مجموعة صغيرة من المعرفة بالاستغلال. مع هذا التنسيق، يمكنني تقسيمه إلى مواضيع مختلفة مثل "تجاوز المخزن المؤقت إلى استدعاء الشيل كود" و "استغلال الفاست بن"، بحيث يمكن تغطيته كدورة عادية إلى حد ما.

## Environment

For your environment to actually do this work, I would recommend having an Ubuntu VM. However don't let me tell you how to live your life.

## البيئة

لبيئتك للقيام بهذا العمل، أوصي بالحصول على جهاز افتراضي Ubuntu. ومع ذلك، لا تدعني أخبرك كيف تعيش حياتك.

## Why Should I do this?

First off, I find it fun (if you don't find this fun, I wouldn't recommend doing this). Plus there are a lot of jobs out there to do this work, with not a lot of people to do the work. Plus who doesn't want to drop that chrome 0-day and watch the world burn?

## لماذا يجب أن أفعل هذا؟

أولاً، أجدها ممتعة (إذا لم تجد هذا ممتعًا، لا أوصي بفعل ذلك). بالإضافة إلى ذلك، هناك الكثير من الوظائف المتاحة للقيام بهذا العمل، مع عدم وجود الكثير من الأشخاص للقيام بالعمل. بالإضافة إلى من لا يريد إسقاط ثغرة يوم الصفر في Chrome ومشاهدة العالم يحترق؟

## Difficulty curve

One more thing I want to say is the difficulty curve, in my opinion, is like that of a roller coaster. There are certain parts that are easier, and certain parts that are harder. Granted, difficulty is relative to the person.

## منحنى الصعوبة

شيء آخر أود قوله هو أن منحنى الصعوبة، في رأيي، يشبه الأفعوانية. هناك أجزاء معينة أسهل، وأجزاء معينة أصعب. بالطبع، الصعوبة نسبية للشخص.


## Update

So, I just tried to update nightmare. It is a bit old, and it's all in python2 which has been deprecated. While trying to update it, it become clear that there are some problems with Nightmare, that it seems that it would be better to just remake the entire thing from scratch. One of the primary problems being, a lot of these challenges require different environments to run in. You might have a heap challenge that can only run correctly on Ubuntu 18, while another might only be able to be correctly ran on Ubuntu 16. Having the need to be able to run challenges on so many different (and outdated) environments causes a lot of problems. So I will probably be "remaking" this to hopefully solve that, and similar problems.

However, if you are at all interested in the work I did do for the update, you can find that here: https://github.com/guyinatuxedo/new_nightmare/

## تحديث

لقد حاولت تحديث Nightmare. إنه قديم بعض الشيء، وكل شيء فيه مكتوب بلغة Python2 التي تم إيقاف دعمها. أثناء محاولة التحديث، اتضح أن هناك بعض المشاكل في Nightmare، ويبدو أنه سيكون من الأفضل إعادة صنعه بالكامل من الصفر. واحدة من المشاكل الرئيسية هي أن العديد من هذه التحديات تتطلب بيئات مختلفة للتشغيل. قد يكون لديك تحدي heap يمكن تشغيله بشكل صحيح فقط على Ubuntu 18، بينما قد يتطلب تحدي آخر Ubuntu 16. الحاجة إلى تشغيل التحديات على العديد من البيئات المختلفة (والقديمة) تسبب الكثير من المشاكل. لذلك من المحتمل أن أقوم "بإعادة صنع" هذا لحل هذه المشكلة والمشاكل المشابهة.

ومع ذلك، إذا كنت مهتمًا بالعمل الذي قمت به للتحديث، يمكنك العثور عليه هنا: https://github.com/guyinatuxedo/new_nightmare/

