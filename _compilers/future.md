---
title: "Средства работы со строками в C++"
---

## Наследие языка C89

Язык программирования C изначально спроектирован как приближённый к Assembler и к принципам организации ЭВМ. Для ЭВМ не существует текстовых строк – есть только байты, последовательности байт и адреса в памяти.

Отдельные байты и фиксированные группы байт в языке C представлены:

- Целочисленными типами: «char», «int», «unsigned» и типами из заголовочного файла <stdint.h>. Арифметические операции над этими типами после компиляции превратятся ассемблерные команды для ALU.
- Числами с плавающей точкой: «float», «double», «long double». Арифметические операции над этими типами после компиляции превратятся ассемблерные команды для FPU.

Последовательности байт представляются двумя элементами языка:

- массивами (Array)
- массивами переменной длины (Variadic Length Array) – это нововведение C99, которое поддерживается только в некоторых компиляторах

Адреса в памяти и арифметика над ними представлены тремя элементами языка:

- указателями «Type*»,
- целочисленным типом «size_t», способным хранить значение указателя

Хотя строки как отдельный элемент языка отсутствуют, считается, что в C89 всё же есть строки. Строкой считается указатель на массив байт, завершённый нулевым байтом. Поскольку в C89 размер типа «char» практически везде равен одному байту, то строкой считается значение типа «char*» или «const char*», при условии, что указатель указывает на последовательность, завершённую нулевым «char».
Для определения размера строки в C89 служит функция «`strlen(const char*)`»: она проходится по последовательности, пока не найдёт нулевой «char». Если последовательность явзяется корректно представленной строкой, то функция вернёт её длину.

Строка должна быть размещена в памяти. Если память взята из стека программы или статической области, то строку не надо удалять. Если память выделена на куче функциями-аллокаторами, такими как «malloc(size_t size)», «calloc(size_t size)» или оператор «new», то строку надо вовремя вручную удалить с помощью вызова парной функции «free(void *pointer)» или парного оператора «delete».


## Владеющие строки в языке C++
Строки, унаследованные из языка C89, неудобны в использовании:

- Нужно постоянно вызывать «strlen(const char*)» для определения размера строки, или хранить размер в отдельной переменной типа «size_t».
- Нужно следить за временем жизни строки, и вовремя удалять выделенную для неё память – если строка размещена на куче.
Язык C++ позволил делать то же самое гораздо проще с помощью шаблонного класса «std::basic_string<T>» и его специализаций:
- «std::string» хранит последовательность однобайтовых целых типа «char». Информация о кодировке не сохраняется, и программа сама должна решить, как интерпретировать строку – счтитать её строкой в кодировке UTF8, или строкой в одной из множества локальных 1-байтных кодировок, таких как CP1251, CP1252, IBM866, KOI-8R и других.
- «std::wstring» хранит последовательность многобайтовых целых типа «wchar_t». Размер типа «wchar_t» на Windows равен 2-м байтам, а на Linux и MacOSX – 4-м байтам. Эта особенность делает «std::wstring» неудобным для использования в кроссплатформенных приложениях. Информация о кодировке символов в строке не сохраняется, но принято считать, что в Windows используется UTF16
- В C++ 2011 появился «std::u16string», который хранит последовательность двухбайтовых целых типа «char16_t». Это позволяет одинаковым образом представить 2-байтные кодировки на разных платформах.

## Методы сканирования строки в STL
Классы строк языка C++ обеспечивают простые и безопасные ввод, вывод, копирование и конкатенацию строк. Но для многих задач, связанных с поиском в строке или разбором строки, нужен доступ к произвольным символам и возможность переместить позицию сканнера. Есть два хороших и один сомнительный способ реализации random access:
1.	Хранить саму строку и одну или несколько переменных типа «size_t», сохраняющих индекс сканируемого сейчас символа
2.	Хранить итераторы начала, конца строки и вспомогательные итераторы, обеспечивающие доступ к сканируемым в данный момент символам. Это предпочтительный способ, начиная с C++ 2011.
3.	Сомнительный способ: получить указатель на начало строки, и использовать арифметику указателей для random access и перемещения сканера.
Многие готовые решения в сети используют третий способ, завязанный на арифметику указателей. Следует остерегаться подобного подхода – итераторы строк работают так же быстро, как указатели.
Стандартная библиотека регулярных выражений, которая появилась в C++ 2011 в виде заголовка <regex>, поддерживает поиск по регулярному выражению на диапазоне итераторов также просто, как и на объекте типа «std::basic_string<T>».
Стандартная библиотека алгоритмов, представленная в заголовке <algorithm>,  работает только с итераторами строк, хотя в стандарте 2017-го года это может измениться, если в STL добавят концепцию «Ranges» и класс «std::string_view».

## Методы сканирования строки в Boost
Библиотека boost предоставляет более современные и удобные средства обработки строк, часть которых может войти в новый стандарт C++. В их  число входят:
•	Класс «boost::string_ref», который войдёт в C++ 2017 под именем «std::string_view». Этот класс расположен в заголовочном файле <boost/utility/string_ref.hpp> и представляет из себя невладеющую ссылку на строку. Ссылка на строку не выделяет память, не освобождает память и никак не влияет на время жизни последовательности символов, размещённой в куче. Задача класса – всего лишь предоставить программный интерфейс, аналогичный «std::string», более безопасный и простой, чем итераторы. Кроме интерфейса строки, у ссылки на строку есть методы «remove_prefix(size_t count)» и «remove_suffix(size_t count)», позволяющие сузить область сканирования строки, или отрезать уже просканированную левую/праву часть. Также есть метод «str()», копирующий и возвращающий владеющую памятью строку.
•	Алгоритмы для строк, заданные в заголовочном файле <boost/algorithm/string.hpp>, предоставляют готовые шаблонные функции «replace_all» для поиска и замены подстроки, «trim» для обрезания пробелов слева и справа, «split» для разделения строки на массив подстрок, разделённых символом-разделителем и другие.
o	Все алгоритмы имеют два варианта: один модифицирует переданную 1-м параметром строку, другой лишь возвращает преобразованную копию. Копирующие функции принимают тот же набор аргументов, но имеют суффикс «_copy» – например, «replace_all_copy».
o	В качестве входной строки может служить «std::string», «boost::string_ref» или любой другой класс, предоставляющий интерфейс строки и методы «begin()», «end()» для получения итераторов. Такое возможно благодаря реализации всех алгоритмов в виде шаблонных функций.