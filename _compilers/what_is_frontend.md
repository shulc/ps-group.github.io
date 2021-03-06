---
title: 'А что такое Compiler Frontend?'
---

Compiler Frontend — первый из двух ключевых компонентов компилятора, о котором можно сказать следующее:

- Frontend получает на вход текст программы
- если в тексте есть синтаксические и семантические ошибки, Frontend находит их
- если текст корректен, Frontent строит абстрактное синтаксическое дерево (AST), хранящее логическую модель файла исходного кода
- превращение текста в AST происходит в три этапа: сначала текст делится на токены (лексический анализ), затем токены собираются по грамматике в AST (синтаксический анализ), а затем AST проходит постобработку (семантический анализ)

> Интересный факт: в интерпретаторах тоже есть Frontend, а вот Backend может не существовать — для интерпретации достаточно выполнить обход AST, последовательно вычисляя значения в нелистовых узлах. В интерпретаторах промышленного уровня качества Backend есть, и он генерирует байткод виртуальной машины.

## Синтаксический анализ

Формальный язык имеет грамматику, которая описывает файл с исходным кодом как набор рекурсивных конструкций (заданных правилами грамматики).

> Синтаксический анализ — ядро фронтенда, в самых простых языках (уровня калькулятора с переменными) это вообще единственный этап обработки текста во фронтенде.

## Лексический анализ

## Что надо знать до разработки фронтенда

Для реализации фронтенда простейшего, процедурного языка понадобится:

- Владеть паттернами проектирования: Посетитель (Visitor), Фасад (Facade)
- Владеть структурами данных: деревья, строки
- Понимать структуру формальных языков: выражения (expressions), инструкции (statements), области видимости переменных (scopes), подпрограммы (subroutines)

Для реализации бекенда объектно-ориентированного языка также нужно:

- Знать детали работы языка C++: что такое раскрутка стека при выбросе исключения (stack unwinding), кодирование имён (name mangling), как устроены vtable и как реализовать полиморфизм с помощью hash-таблицы методов

## Читать далее

- [Конечные автоматы](/compilers/fsm.html)
- [Грамматики](/compilers/grammars.html)
- [Калькулятор на основе рекурсивного спуска](/compilers/simple_recursive_parser.html)
- [Abstract Syntax Tree](/compilers/ast.html)
- [Восходящий разбор по принципу сдвига и свёртки (shift-reduce)](/compilers/shift_reduce.html)
- [Полезные утилиты из STL и Boost для фронтенда](/compilers/frontend_utils.html)
