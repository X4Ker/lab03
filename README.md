## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>  // экспорт имени пользователя
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .  // сохраняем
$ source scripts/activate // активируем скрипты
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03 //клонируем репозиторий
$ cd projects/lab03
$ git remote remove origin  //удалить удаленно origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git  //удаленно добавляет origin
```

```ShellSession
$ g++ -std=c++11 -I./include -c sources/print.cpp //сборка
$ ls print.o  //выводит файл
$ nm print.o | grep print   // выводим на экран строки объектного файла, содержащие print
$ ar rvs print.a print.o .. // архивируем объектный файл
$ file print.a // определяем тип файла и понимаем, что архивирование прошло успешно
$ g++ -std=c++11 -I./include -c examples/example1.cpp //копируем файл исхожного кода
$ ls example1.o //выводим на экран содержание файла
$ g++ example1.o print.a -o example1 //компилируем объектный файл с функцией main и заархивированный ранее файл  
$ ./example1 && echo //# запускаем скомпилированный файл и выводим строку текста
```

```ShellSession
$ g++ -std=c++11 -I./include -c examples/example2.cpp  // компилируем второй файл исходного кода с функцией main
$ nm example2.o // выводим на экран бинарное содержимое объектного файла
$ g++ example2.o print.a -o example2 // компилиркуем файл, содержащий main, и архив
$ ./example2 // запускаем программу
$ cat log.txt && echo //выводим на экран содержимое файла в единый поток
```

```ShellSession
$ rm -rf example1.o example2.o print.o // удаляем в этой и след командах файлы с кодом, архив, скомпелированные файы и txt
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```ShellSession
$ cat > CMakeLists.txt <<EOF  //записываем в файл 
cmake_minimum_required(VERSION 3.4)  //проверка на версию
project(print)  //название проекта
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)  //устанавливаем стандарты
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)  //подключаем cpp файл
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)  //подключаем include
EOF
```

```ShellSession
$ cmake -H. -B_build  //подготовливаеи процесс построения проекта, проверяем работоспособность и записываем результат в файл
$ cmake --build _build  //создаем сгенерированное ранее двоичное дерево проекта
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF  //записываем в файл

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```ShellSession
$ cmake --build _build  // сборка в текущем каталоге
$ cmake --build _build --target print //запускаем сборку с целью print вместо цели по умолчанию
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```

```ShellSession
$ ls -la _build/libprint.a // выводим содержимое файла с расширением .a (статический файл библиотеки)
$ _build/example1 && echo //выполняем сборку и выводим на экран
hello
$ _build/example2 //выполняем сборку
$ cat log.txt && echo // выводим на экран содержимое файла в единый поток
hello
$ rm -rf log.txt //удаляем файл
```

```ShellSession
$ git clone https://github.com/tp-labs/lab03 tmp  //клонирование репозитория
$ mv -f tmp/CMakeLists.txt . //перемещаем файл в текущую директорию
$ rm -rf tmp  //удаляем
```

```ShellSession
$ cat CMakeLists.txt // выводим в поток содержимое файла
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install // подготовливаеи процесс построения проекта и создаем имя префиксной команды
-- Configuring done
$ cmake --build _build --target install // запускаем сборку с целью install
$ tree _install // создание дерева
```

```ShellSession
$ git add CMakeLists.txt  //отправление в удаленный репозиторий
$ git commit -m"added CMakeLists.txt"
$ git push origin master
```

## Report

```ShellSession
$ popd //возвращаемся в каталог, ранее запомненный командой popd
$ export LAB_NUMBER=03 //приваиваем переменнной значеиние
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}  //копируем в 2 папки репозиторий
$ mkdir reports/lab${LAB_NUMBER}  // сохдаем директорию 
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md  //копируем файл из 1 папки во 2
$ cd reports/lab${LAB_NUMBER} //переходим в директорию
$ edit REPORT.md //редактируем очёт
$ gistup -m "lab${LAB_NUMBER}" //отправляем на github gist
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

Создаем Сmakefile в директории formeter_lib со след содержанием:
```cmake
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter)

add_library(formatter STATIC formatter.cpp)
target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```

Собираем:
```
$ cmake -H. -B_build
$ cmake --build _build
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.
Создаем в директории formetter_ex_lib Cmake file со следующим содержанием:
Cодержимое файла CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex)

add_library(formatter_ex STATIC formatter_ex.cpp)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter)
target_include_directories(formatter_ex PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

target_link_libraries(formatter_ex formatter)
```
И собираем теми же командами.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

Сmake для папки solver_lib:
```cmake
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver_lib)

add_library(solver_lib STATIC solver.cpp)
target_include_directories(solver_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```

Cmake для папки solver_application:

```
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver_example)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib ${CMAKE_CURRENT_BINARY_DIR}/solver_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter_ex)

add_executable(example equation.cpp)

target_link_libraries(example solver_lib formatter_ex)
```
И Cmake для папки hello_world:
```cmake
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(hello_world_example)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib ${CMAKE_CURRENT_BINARY_DIR}/formatter_ex)
add_executable(example hello_world.cpp)

target_link_libraries(example formatter_ex)
```



**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2019 The ISC Authors
```
