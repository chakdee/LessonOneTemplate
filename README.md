# Lesson One

Все консольные команды необходимо выполнять в `git bash` (должен появиться после установки [git](https://git-scm.com/downloads
)). Команды для навигации по файловой системе:
* `pwd` – вывести путь до текущей директории;
* `ls` – вывести содержимое текущей директории;
* `ls -a` – вывести содержимое текущей директории, включая скрытые файлы и директории (начинаются с `.`);
* `cd <название директории>` – смена текущей директории;
* `cd ..` – смена текущей директории на родительскую.

Полный текст руководства по настройке Visual Studio Code можно найти [по ссылке](https://code.visualstudio.com/docs/cpp/config-mingw) (на английском).

## Настройка проекта для выполнения задания

1. Установить [Visual Studio Code](https://code.visualstudio.com/download).
2. Установить расширение [C/C++ extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools). Вы можете установить расширение C/C++ через браузер расширений (Ctrl+Shift+X).

![CPP Extension](https://code.visualstudio.com/assets/docs/cpp/cpp/cpp-extension.png)

3. Установить Mingw-w64 через сайт SourceForge. Кликните [Mingw-w64](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/mingw-w64-install.exe/download) для скачивания.
4. Добавьте путь до директории `bin` из установленного Mingw-w64 в переменную окружения PATH.
	1. Откройте настройки переменных среды.
	3. Выберите переменную Path и нажмите Редактировать.
	4. Добавьте новый элемент с путем до директории с установленным Mingw-w64. Например: C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64\bin.
	5. Нажмите OK для сохранения обновленной переменной PATH. Нужно переоткрыть консольное окно, чтобы загрузились отредактированные переменные среды.

>Для Windows 7 и ниже требуется отредактировать переменную PATH, добавив в конец строки `;`, а уже после вставить путь до директории `bin`.

### Проверка MinGW

Для того, чтобы проверить правильность установки MinGW необходимо открыть терминал и выполнить следующие:

```bash
g++ --version
gdb --version
```

### Настройка проекта в Visual Studio Code

Для того, чтобы проинициализировать проект, необходимо вызвать `сode .` в директории склонированного проекта в терминале:

```bash
mkdir Projects
cd Projects
git clone <url до репозитория с заданием> LessonOne
cd LessonOne
git branch develop
git checkout develop
code .
```

Команда `code .` открывает VS Code в текущей директории `LessonOne`, делая ее директорией проекта. По итогу выполнения руководства должны создаться 3 файла в `LessonOne/.vscode`:
* `tasks.json` (инструкуции сборки проекта);
* `launch.json` (настройки отладки);
* `c_cpp_properties.json` (настройка компиляции и IntelliSense - подсветка синтаксиса, всплывающие окна с подсказками и автодополнение кода)`.


### Настройка сборки проекта

В главном меню нужно выбрать **Terminal > Configure Default Build Task**. В выпадающем списке нужно выбрать **g++.exe build active file**.

![Build Task List](https://code.visualstudio.com/assets/docs/cpp/mingw/build-active-file.png)

Это создаст `task.json` файл в директории `.vscode` и откроет его в редакторе.

Ваш файл `task.json` должен выглядеть примерно так:
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "type": "shell",
      "label": "C/C++: g++.exe build active file",
      "command": "C:\\Program Files\\mingw-w64\\x86_64-8.1.0-posix-seh-rt_v6-rev0\\mingw64\\bin\\g++.exe",
      "args": ["-g", "${file}", "-o", "${fileDirname}\\${fileBasenameNoExtension}.exe"],
      "options": {
        "cwd": "${workspaceFolder}"
      },
      "problemMatcher": ["$gcc"],
      "group": {
        "kind": "build",
        "isDefault": true
      }
    }
  ]
}
```

### Настройка отладки проекта
Далее, создадим `launch.json` файл, чтобы VS Code смог запустить отладчик GDB по нажатию клавиши `F5` для выполнения вашей программы.

Для этого нужно выбрать **Run > Add Configuration...** в главном меню и нажать **C++ (GDB/LLDB)**. После, необходимо выбрать **g++.exe build and debug active file** из выпадающего списка.

![Debug Dropdown](https://code.visualstudio.com/assets/docs/cpp/mingw/build-and-debug-active-file.png)

VS Code создаст `launch.json` файл и откроет его в редакторе. Пример файла `launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "g++.exe - Build and debug active file",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "C:\\Program Files\\mingw-w64\\x86_64-8.1.0-posix-seh-rt_v6-rev0\\mingw64\\bin\\gdb.exe",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "C/C++: g++.exe build active file"
    }
  ]
}
```
По умолчанию, расширение C++ не поддерживает остановку выполнения программы по точкам остановки (breakpoints). Для того, чтобы включить поддержку точек остановки необходимо изменить значение `stopAtEntry` на `true`.

### Добвление исходного кода

1. Создайте `.cpp` файл в корне проекта, например `helloworld.cpp`.

![Create a new file](https://code.visualstudio.com/assets/docs/cpp/mingw/new-file-button.png)

2. Добавьте в него следующий код, для тестирования сборки и правильной настройки проекта:

```c++
#include <iostream>

int main() 
{
	std::cout << "Hello, World!" << std::endl;
}
```

### Запуск сборки

1. Откройте ваш `helloworld.cpp` файл выбрав его из списка файлов проекта.
2. Нажмите `Ctrl+Shift+B` или выберите в главном меню **Terminal** и нажмите **Run Build Task**.
3. Будет запущена сборка проекта, результат будет выведен в терминале:

![Build Result](https://code.visualstudio.com/assets/docs/cpp/mingw/build-output-in-terminal.png)

4. Добавьте новое окно терминала с помощью кнопки **+**. Выполните `ls`, чтобы убедиться, что был создан исполняемый `helloworld.exe` файл.

![Build Result](https://code.visualstudio.com/assets/docs/cpp/mingw/helloworld-in-terminal.png)

5. Вы можете запустить программу выполнив в терминале `helloworld.exe` (или `.\helloworld.exe`).

### Отправка выполненного задания на проверку
1. Выполните `git status`, чтобы проверить текущее состояние локального репозитория.
2. Чтобы добавить исходные файлы в репозиторий или подготовить их к коммиту выполните `git add <имя файла.cpp>` или `git add .`, чтобы добавить все файлы в текущей директории.
3. Для сохранения изменений в локальном репозитории выполните `git commit -m "комментарий к коммиту"`. В комментарии указываем, что было сделано в рамках коммита, например: 
	* был добавлен вывод списка учеников в консоль; 
	* исправлен цикл обработки ввода; 
	* выполнено первое задание;
	* внесены исправления по ревью.
4. Передача всех сохраненных изменений из локального репозитория на удаленный выполняется командой `git push -u origin develop`.

### Настройка для будущих проектов

> Информация предоставлена для ознакомления и будет продублирована в следующей работе.

Для следующих заданий не требуется повторная настройка проекта, достаточно скопировать `.vscode` в директорию нового проекта. Ниже представлен пример с использованием терминала.
1. Необходимо перейти в директорию со всеми проектами `Projects`:
```bash
$ pwd
/Users/vchernov/Projects/
```
2. В директории должны находится настроенный проект `LessonOne` и склонированный из репозитория новый проект `LessonTwo`:
```bash
$ ls
LessonOne         LessonTwo
```
3. Проверяем, что в `LessonOne` есть директория `.vscode`:
```bash
$ ls -la LessonOne
total 136
drwxr-xr-x  6 vchernov  staff    192 Oct  2 11:48 .
drwxr-xr-x  9 vchernov  staff    288 Oct  2 13:04 ..
drwxr-xr-x  5 vchernov  staff    160 Oct  2 11:50 .vscode
-rwxr-xr-x  1 vchernov  staff  62392 Oct  2 11:48 main
-rw-r--r--  1 vchernov  staff     82 Oct  2 11:46 main.cpp
drwxr-xr-x  3 vchernov  staff     96 Oct  2 11:45 main.dSYM
```
4. Копируем `.vscode` в новый проект командной `cp -R`:
```bash
$ cp -R LessonOne/.vscode LessonTwo
```
5. Проверяем, что все перенеслось успешно, помощью команды `find`:
```bash
$ find LessonTwo
CppProjectTwo
CppProjectTwo/.vscode
CppProjectTwo/.vscode/c_cpp_properties.json
CppProjectTwo/.vscode/launch.json
CppProjectTwo/.vscode/tasks.json
```
