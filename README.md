# Lesson One

Полный текст руководства по настройке Visual Studio Code можно найти [по ссылке](https://code.visualstudio.com/docs/cpp/config-mingw) (на английском).

Ниже представлен процесс настройки среды для выполнения задания.

1. Установить [Visual Studio Code](https://code.visualstudio.com/download).
2. Установить расширение [C/C++ extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools). Вы можете установить расширение C/C++ через браузер расширений (Shift+Ctrl+X).

![CPP Extension](https://code.visualstudio.com/assets/docs/cpp/cpp/cpp-extension.png)

3. Установить Mingw-w64 через сайт SourceForge. Кликните [Mingw-w64](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/mingw-w64-install.exe/download) для скачивания.
4. Добавьте путь до директории bin из установленного Mingw-w64 в переменную окружения PATH.
	1. Откройте настройки переменных среды.
	3. Выберите переменную Path и нажмите Редактировать.
	4. Добавьте новый элемент с путем до директории с установленным Mingw-w64. Например: C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64\bin.
	5. Нажмите OK для сохранения обновленной переменной PATH. Нужно переоткрыть консольное окно, чтобы загрузились отредактированные переменные среды.

### Проверка MinGW

Для того, чтобы проверить правильность установки MinGW необходимо открыть терминал и выполнить следующие:

```bash
g++ --version
gdb --version
```

### Настройка проекта в Visual Studio Code

Для того, чтобы проинициализировать проект, необходимо вызвать `сode .` в директории склонированного проекта. Пример:

```bash
cd Projects
git clone <url> LessonOne
cd LessonOne
git branch develop
git checkout develop
code .
```

### Настройка сборки проекта

В главном меню нужно выбрать **Terminal > Configure Default Build Task**. В выпадающем списке нужно выбрать g++.exe build active file.

![Build Task List](https://code.visualstudio.com/assets/docs/cpp/mingw/build-active-file.png)

Это создаст `task.json` файл в директории `.vscode` и откроет его в редакторе.

Ваш файл `task.json` должен выглядить примерно так:
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

###Запуск сборки

1. Откройте ваш `.cpp` файл, который хотите собрать.
2. Нажмите `Shift+Ctrl+B` или выберите в главном меню **Terminal** и нажмите **Run Build Task**.
3. Будет запущена сборка проекта, результат будет выведен в терминале:

![Build Result](https://code.visualstudio.com/assets/docs/cpp/mingw/build-output-in-terminal.png)

4. Добавьте новое окно терминала с помощью кнопки **+**. Выполните `ls` или `dir`, чтобы увидеть файлы сборки.
![Build Result](https://code.visualstudio.com/assets/docs/cpp/mingw/helloworld-in-terminal.png)

5. Вы можете запустить программу выполнив в терминале `<имя вашей программы>.exe` (или `.\<имя вашей программы>.exe`).