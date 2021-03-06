---
title: 'F.A.Q. по первой главе'
preview: img/help-contents.png
---

## Что такое OpenGL и DirectX?

OpenGL - это открытый индустриальный стандарт доступа к видеокарте и вывода двумерной или трёхмерной графики. DirectX (в частности Direct3D) - это конкурирующий стандарт от Microsoft, работающий только на платформе Windows.

Реализацию OpenGL и DirectX в виде динамических библиотек предоставляет производитель видеокарты в своём видеодрайвере (исключением является Linux, где в разработку видеодрайверов вносит свой вклад сообщество).

## Чем обусловлен выбор библиотеки SDL2?

SDL2 был выбран как единая абстракция над мультимедийным API различных операционных систем: Windows, MacOSX, Linux, Android, iOS. Он предоставляет средства для создания окна и контекста OpenGL, для обработки событий ввода, для вывода звука и текста, для загрузки изображений, для работы с файлами и другие вспомогательные инструменты (часть которых дублируется в библиотеке STL версии C++11). Альтернативы:

- На Windows можно было бы использовать DirectX (DirectInput, DirectAudio, DirectWrite, Direct2D и т.д.)
- На Java можно использовать JOGL вместе со Swing или AWT
- В примерах для OpenGL в Сети часто используются GLUT (FreeGLUT), GLFW или GLAux, но SDL2 технически совершеннее всех трёх перечисленных библиотек

## Чем обусловлен выбор библиотеки GLM?

В компьютерной графике используется линейная алгебра, в частности, двух-, трёх- и чётырёхкомпонентные вектора, матрицы от 2x2 до 4x4, квантерионы (хранящие углы Эйлера). Также часто используются языки шейдеров, в которых уже есть готовые типы данных и функции для работы с линейной алгеброй. Библиотека GLM имитирует типы данных и функции языка шейдеров GLSL, что позволяет студенту один раз изучить возможности GLM и потом легче начать писать на языке GLSL.

Альтернативы для других языков:

- Java: [JOML](https://github.com/JOML-CI/JOML)
- Javascript: [gl-matrix](https://github.com/toji/gl-matrix)

## Как рисовать на OpenGL
Смотри примеры, начиная с [третьего](/opengl/lesson_3.html).

В OpenGL есть несколько режимов рисования, позволяющих, по сути, сделать одно и то же. Причиной дублирования является эволюционное развитие.
- OpenGL появился в конце 80-х, и в нём сразу был Immediate Mode, то есть рисование между вызовами “glBegin” и “glEnd”
- Позже в OpenGL 1.x появились дисплейные списки (см. [4-й пример](/opengl/lesson_4.html))
- Одновременно с ними появилась передача массивов атрибутов вершин (см. [10-й пример](/opengl/lesson_10.html))
- В начале двухтысячных в OpenGL версий 2.x и 3.x появились совершенно новые методы рисования, описанные в 3-й и 4-й главах примеров

## Где взять документацию библиотек

См. [список полезных ссылок](/opengl/useful-links.html).

## С помощью чего выводить текст на OpenGL?

Существуют разные способы для разных целей, например, есть перспективные алгоритмы Scale-Independent вывода текста с помощью шейдеров. Но если вы не разрабатываете свой высокопроизводительный движок графики, вам подойдёт несколько простых решений.

Во-первых, можно во время выполнения программы растеризовать текст в изображение, превратить изображение в текстуру и затем вывести квадрат с натянутой на него текстурой.

Полезные ссылки:

- [Растеризация текста в изображение средствам модуля SDL_ttf библиотеки SDL2 (англ.)](http://www.sdltutorials.com/sdl-ttf)
- [2D Texture Font (англ.)](http://nehe.gamedev.net/tutorial/2d_texture_font/18002/)
- [Пример вывода текстуры средствами OpenGL 1.x (рус.)](plambir.blogspot.ru/2010/09/3opengl.html)

Во-вторых, можно использовать специфичную для Windows функцию [wglUseFontOutlines](https://msdn.microsoft.com/en-us/library/windows/desktop/dd374393(v=vs.85).aspx), чтобы сформировать дисплейный список, рисующий буквы нужного шрифта.

Полезные ссылки:

- [Outline Fonts](http://nehe.gamedev.net/tutorial/outline_fonts/15004/)

## Как увеличить длину вектора?

- вычислить скаляр — желаемую длину вектора
- нормализовать (то есть привести к единичной длине, сохранив направление вектора).
- затем умножить на новую длину.
