---
title: 'Конечные автоматы'
---

Конечный автомат &mdash; абстрактная модель, типичный паттерн в разработке компиляторов и не только. Реализовать автомат можно в любом стиле программирования &mdash; процедурном, объектно-ориентированном или функциональном.

## Детерминированный и недерминированный автоматы



# Примеры для std::regex

Здесь размещены три примера, демонстрирующих применение регулярных выражений в составе STL.
Регулярные выражения в STL представлены классом `std::regex` и несколькими функциями в заголовке `<regex>`.
Все примеры покрыты тестами на boost.test.

### Распознаватель идентификаторов (`IdMatcher.h`)

Класс CIdMatcher получает строку и проверяет, является ли строка целиком допустимым идентификатором.
Для реализации класса использована функция [`std::regex_match`](http://www.cplusplus.com/reference/regex/regex_match/).

### Простой сканер идентификаторов (`SimpleIdScanner.h`)

Класс CSimpleIdScanner получает текст построчно и извлекает из него идентификаторы.
Он может вернуть отсортированный список использованных идентификаторов.
Для реализации класса использована функция [`std::regex_seach`](http://www.cplusplus.com/reference/regex/regex_search/).

```cpp
std::regex_search(std::string::iterator begin,
                  std::string::iterator end,
                  std::smatch & match,
                  std::regex const& pattern)
```

### Улучшенный сканер идентификаторов (`AdvancedIdScanner.cpp`)

В отличие от простого сканера, улучшенный умеет игнорировать комментарии в стиле C: `/* it's comment */`.
Для реализации класса также использована функция [`std::regex_seach`](http://www.cplusplus.com/reference/regex/regex_search/).