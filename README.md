

# Laboratory work II


### 1. Настройка переменных окружения

```bash
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>
```

**Что делают эти команды:**
- `export` - устанавливает переменные окружения для текущей сессии терминала
- `GITHUB_USERNAME` - хранит имя пользователя GitHub
- `GITHUB_EMAIL` - хранит email, связанный с аккаунтом GitHub
- `GITHUB_TOKEN` - хранит токен доступа к GitHub API
- `alias edit=nano` - создает псевдоним `edit` для текстового редактора nano

---

### 2. Переход в рабочую директорию

```bash
cd ${GITHUB_USERNAME}/workspace
source scripts/activate
```

**Что делают эти команды:**
- `cd` - переходит в директорию workspace
- `source scripts/activate` - активирует окружение (выполняет скрипт настройки)

---

### 3. Настройка конфигурации для hub

```bash
mkdir ~/.config
cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
git config --global hub.protocol https
```

**Что делают эти команды:**
- `mkdir ~/.config` - создает директорию для конфигурационных файлов (если не существует)
- `cat > ~/.config/hub <<EOF` - создает файл с конфигурацией hub, подставляя значения переменных
- `github.com: - user:` - указывает username для GitHub
- `oauth_token:` - сохраняет токен для аутентификации
- `protocol: https` - устанавливает протокол HTTPS
- `git config --global hub.protocol https` - добавляет настройку протокола в глобальный конфиг Git

---

### 4. Создание и настройка локального репозитория

```bash
mkdir projects/lab02 && cd projects/lab02
git init
git config --global user.name ${GITHUB_USERNAME}
git config --global user.email ${GITHUB_EMAIL}
git config -e --global
```

**Что делают эти команды:**
- `mkdir projects/lab02` - создает директорию для проекта
- `cd projects/lab02` - переходит в созданную директорию
- `git init` - инициализирует пустой Git репозиторий (создает папку .git)
- `git config --global user.name` - устанавливает имя пользователя для всех коммитов
- `git config --global user.email` - устанавливает email для всех коммитов
- `git config -e --global` - открывает глобальный конфиг для проверки

---

### 5. Подключение к удаленному репозиторию

```bash
git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
git pull origin main
```

**Что делают эти команды:**
- `git remote add origin` - связывает локальный репозиторий с удаленным на GitHub
- `origin` - традиционное имя для основного удаленного репозитория
- `git pull origin main` - скачивает изменения с GitHub (включая .gitignore)

**Результат выполнения:**
```
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 7 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (7/7), 2.47 KiB | 844.00 KiB/s, done.
From https://github.com/ferdosiakrymskaa-svg/lab02
 * branch            main       -> FETCH_HEAD
 * [new branch]      main       -> origin/main
```

---

### 6. Работа с README.md

```bash
git branch -a
touch README.md
git status
git add README.md
git commit -m"added README.md"
git push origin main
```

**Что делают эти команды:**
- `git branch -a` - показывает все ветки (локальные и удаленные)
- `touch README.md` - создает файл README.md
- `git status` - показывает состояние рабочей директории
- `git add README.md` - добавляет файл в staging area
- `git commit -m"added README.md"` - создает коммит с сообщением
- `git push origin main` - отправляет изменения на GitHub



---

### 7. Создание структуры проекта

```bash
mkdir sources include examples
```

**Создание файла sources/print.cpp:**
```bash
cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

**Создание файла include/print.hpp:**
```bash
cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

**Создание файла examples/example1.cpp:**
```bash
cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

**Создание файла examples/example2.cpp:**
```bash
cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

**Что делают эти команды:**
- `mkdir` - создает директории для исходного кода, заголовочных файлов и примеров
- `cat > файл <<EOF` - создает файл с многострочным содержимым
- `#include` - подключает заголовочные файлы
- `void print(...)` - объявление и определение функций

---

### 8. Проверка состояния и отправка изменений

```bash
git status
git add .
git commit -m"added sources"
git push origin main
```

**Результат `git status`:**
```
On branch main
Changes not staged for commit:
  modified:   README.md
Untracked files:
  examples/
  include/
  sources/
```

**Результат `git push`:**
```
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 3 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (10/10), 1.04 KiB | 1.04 MiB/s, done.
Total 10 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/lab02.git
   4a006c5..96893da  main -> main
```

**Что делают эти команды:**
- `git add .` - добавляет все изменения (новые и измененные файлы) в staging area
- `git commit -m"added sources"` - создает коммит с исходным кодом
- `git push` - отправляет все коммиты на GitHub

---

### 9. Создание отчета

```bash
cd ~/workspace/
export LAB_NUMBER=02
git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
mkdir reports/lab${LAB_NUMBER}
cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
cd reports/lab${LAB_NUMBER}
edit REPORT.md
```

**Что делают эти команды:**
- `cd ~/workspace/` - переходит в рабочую директорию
- `export LAB_NUMBER=02` - устанавливает номер лабораторной работы
- `git clone` - клонирует шаблон отчета
- `mkdir` - создает директорию для отчета
- `cp` - копирует шаблон в директорию отчета
- `edit REPORT.md` - открывает отчет для редактирования

---



## Part I: Начало работы с репозиторием
[Репозиторий Homework_lab02](https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git)
### 1. Создание репозитория на GitHub
Был создан пустой репозиторий `Homework_lab02` на GitHub.

### 2. Создание первого коммита
```bash
echo "# Homework_lab02" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
git push -u origin main
```

**Результат выполнения:**
```
Reinitialized existing Git repository in /home/ilya_ilyasov/ferdosiakrymskaa-svg/workspace/projects/lab02_hw/.git/
[master (root-commit) 704ff46] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 233 bytes | 233.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

### 3. Создание файла hello_world.cpp с "плохим стилем"
```bash
cat > hello_world.cpp <<EOF
#include <iostream>
using namespace std;

int main() {
    cout << "Hello world" << endl;
    return 0;
}
EOF
```

### 4. Добавление файла в репозиторий
```bash
git add hello_world.cpp
```

### 5. Первый коммит с программой
```bash
git commit -m "Add Hello World program with bad style"
```

**Результат:**
```
[main 11ac9d4] Add Hello World program with bad style
 1 file changed, 6 insertions(+)
 create mode 100644 hello_world.cpp
```

### 6. Модификация программы с запросом имени пользователя
```cpp
#include <iostream>
#include <string>

int main() {
    std::cout << "Enter your name: ";
    std::string name;
    std::cin >> name;
    std::cout << "Hello world from " << name << std::endl;
    return 0;
}
```

### 7. Коммит изменений
```bash
git commit -am "Добавлен ввод имени пользователя в программу Hello World"
```

**Результат:**
```
[main 44278e2] Добавлен ввод имени пользователя в программу Hello World
 1 file changed, 5 insertions(+), 2 deletions(-)
```

> **Почему не нужен git add?**  
> Флаг `-a` в `git commit -am` автоматически добавляет все изменения в отслеживаемых файлах. `git add` требуется только для новых (неотслеживаемых) файлов.

### 8. Отправка изменений на GitHub
```bash
git push origin main
```

**Результат:**
```
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 3 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 857 bytes | 857.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   704ff46..44278e2  main -> main
```

### 9. Проверка истории коммитов в удаленном репозитории
```bash
git log origin/main --oneline
```

**Результат:**
```
44278e2 (HEAD -> main, origin/main) Добавлен ввод имени пользователя в программу Hello World
48cab2c Add Hello World program with bad style
704ff46 first commit
```

---

## Part II: Работа с ветками и pull-реквестами

### 1. Создание ветки patch1
```bash
git checkout -b patch1
```
```
Switched to a new branch 'patch1'
```

### 2. Исправление кода (удаление using namespace std;)
```cpp
#include <iostream>
#include <string>

int main() {
    std::cout << "Enter your name: ";
    std::string name;
    std::cin >> name;
    std::cout << "Hello world from " << name << std::endl;
    return 0;
}
```

### 3. Коммит и отправка ветки
```bash
git commit -am "Удалить 'using namespace std'"
git push -u origin patch1
```

**Результат:**
```
[patch1 11a9728] Удалить 'using namespace std'
 1 file changed, 5 insertions(+), 5 deletions(-)
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 455 bytes | 455.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/ferdosiakrymskaa-svg/Homework_lab02/pull/new/patch1
remote: 
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 * [new branch]      patch1 -> patch1
branch 'patch1' set up to track 'origin/patch1'.
```

### 4. Проверка наличия ветки в удаленном репозитории
```bash
git log origin/patch1 --oneline
```

**Результат:**
```
11a9728 (HEAD -> patch1, origin/patch1) Удалить 'using namespace std'
d206f72 (origin/main, main) Добавлен ввод имени пользователя в программу Hello World
11ac9d4 Add Hello World program with bad style
b6b2a6e first commit
```

### 5. Создание pull-реквеста
Pull-реквест создан через веб-интерфейс GitHub из ветки `patch1` в `main`.

### 6. Добавление комментариев в код
```cpp
#include <iostream>
#include <string>

int main() {
    // Запрос имени у пользователя  
    std::cout << "Enter your name: ";
    std::string name;
    std::cin >> name;
    // Вывод персонализированного приветствия
    std::cout << "Hello world from " << name << std::endl;
    
    return 0;
}
```

### 7. Коммит и отправка изменений
```bash
git commit -am "Добавить поясняющие комментарии в код"
git push
```

**Результат:**
```
[patch1 fb3e14b] Добавить поясняющие комментарии в код
 1 file changed, 4 insertions(+), 2 deletions(-)
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 591 bytes | 591.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   11a9728..fb3e14b  patch1 -> patch1
```

### 8. Проверка обновлений в pull-реквесте
```bash
gh pr status
```

**Результат:**
```
Relevant pull requests in ferdosiakrymskaa-svg/Homework_lab02

Current branch
  #1  Удалить 'using namespace std' [patch1]

Created by you
  #1  Удалить 'using namespace std' [patch1]

Requesting a code review from you
  You have no pull requests to review
```

### 9. Слияние pull-реквеста и удаление ветки
```bash
gh pr merge 1 --merge --delete-branch
```

**Результат:**
```
✓ Merged pull request #1 (Удалить 'using namespace std')
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 936 bytes | 936.00 KiB/s, done.
From https://github.com/ferdosiakrymskaa-svg/Homework_lab02
 * branch            main       -> FETCH_HEAD
   d206f72..cc1de60  main       -> origin/main
Updating d206f72..cc1de60
Fast-forward
 hello_world.cpp | 14 ++++++++------
 1 file changed, 8 insertions(+), 6 deletions(-)
✓ Deleted local branch patch1 and switched to branch main
✓ Deleted remote branch patch1
```

### 10. Синхронизация локальной ветки main
```bash
git checkout main
git pull origin main
```

**Результат:**
```
Already on 'main'
Your branch is up to date with 'origin/main'.
From https://github.com/ferdosiakrymskaa-svg/Homework_lab02
 * branch            main       -> FETCH_HEAD
Already up to date.
```

### 11. Просмотр истории коммитов
```bash
git log --oneline --graph
```

**Результат:**
```
*   cc1de60 (HEAD -> main, origin/main) Merge pull request #1 from ferdosiakrymskaa-svg/patch1
|\  
| * fb3e14b Добавить поясняющие комментарии в код
| * 11a9728 Удалить 'using namespace std'
* | d206f72 Добавлен ввод имени пользователя в программу Hello World
* | 11ac9d4 Add Hello World program with bad style
|/  
* b6b2a6e first commit
```

### 12. Удаление локальной ветки patch1
```bash
git branch -d patch1
git branch
```

**Результат:**
```
* main
```

---

## Part III: Работа с конфликтами и rebase

### 1. Создание ветки patch2
```bash
git checkout main
git pull origin main
git checkout -b patch2
```

**Результат:**
```
Already on 'main'
Your branch is up to date with 'origin/main'.
From https://github.com/ferdosiakrymskaa-svg/Homework_lab02
 * branch            main       -> FETCH_HEAD
Already up to date.
Switched to a new branch 'patch2'
```

### 2. Применение clang-format в стиле Mozilla
```bash
sudo apt install clang-format
clang-format -style=Mozilla -i hello_world.cpp
git diff
```

**Результат форматирования:**
```diff
diff --git a/hello_world.cpp b/hello_world.cpp
index d26c56a..71fde7f 100644
--- a/hello_world.cpp
+++ b/hello_world.cpp
@@ -1,14 +1,15 @@
 #include <iostream>
 #include <string>
 
-int main(){
-    // Запрос имени у пользователя  
-    std::cout << "Enter your name: ";
-    std::string name;
-    std::cin >> name;
-    // Вывод персонализированного приветствия
-    std::cout << "Hello world from "<<name << std::endl;
-    
-    return 0;
-}
+int
+main()
+{
+  // Запрос имени у пользователя
+  std::cout << "Enter your name: ";
+  std::string name;
+  std::cin >> name;
+  // Вывод персонализированного приветствия
+  std::cout << "Hello world from " << name << std::endl;
+
+  return 0;
+}
```

### 3. Коммит и создание pull-реквеста
```bash
git commit -am "Apply Mozilla code style"
git push -u origin patch2
```

**Результат:**
```
[patch2 df3cfec] Apply Mozilla code style
 1 file changed, 11 insertions(+), 10 deletions(-)
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 404 bytes | 404.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'patch2' on GitHub by visiting:
remote:      https://github.com/ferdosiakrymskaa-svg/Homework_lab02/pull/new/patch2
remote: 
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 * [new branch]      patch2 -> patch2
branch 'patch2' set up to track 'origin/patch2'.
```

### 4. Создание конфликта (изменение комментариев в main)
```bash
git checkout main
```

**Изменение файла hello_world.cpp:**
```cpp
#include <iostream>
#include <string>

int main() {
    // Запрос имени у пользователя(Enter username):  
    std::cout << "Enter your name: ";
    std::string name;
    std::cin >> name;
    // Вывод персонализированного приветствия::
    std::cout << "Hello world from " << name << std::endl;
    
    return 0;
}
```

```bash
git commit -am "Add punctuation to comments"
git push origin main
```

**Результат:**
```
[main fc3dd18] Add punctuation to comments
 1 file changed, 2 insertions(+), 2 deletions(-)
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 354 bytes | 354.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   cc1de60..fc3dd18  main -> main
```

### 5. Появление конфликта в pull-реквесте
В pull-реквесте появилось уведомление о конфликте.

### 6. Разрешение конфликта через rebase
```bash
git checkout patch2
git fetch origin
git rebase main
```

**Возникновение конфликта:**
```
Auto-merging hello_world.cpp
CONFLICT (content): Merge conflict in hello_world.cpp
error: could not apply df3cfec... Apply Mozilla code style
```

**Разрешение конфликта в файле:**
```cpp
#include <iostream>
#include <string>

int
main()
{
  // Запрос имени у пользователя(Enter username):  
  std::cout << "Enter your name: ";
  std::string name;
  std::cin >> name;
  // Вывод персонализированного приветствия::
  std::cout << "Hello world from " << name << std::endl;

  return 0;
}
```

```bash
git add hello_world.cpp
git rebase --continue
```

**Результат:**
```
Successfully rebased and updated refs/heads/patch2.
```

### 7. Force push в ветку patch2
```bash
git push --force-with-lease origin patch2
```

**Результат:**
```
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 + df3cfec...fc3dd18 patch2 -> patch2 (forced update)
```

### 8. Проверка отсутствия конфликтов
Конфликты в pull-реквесте исчезли.

### 9. Финальная очистка
```bash
git branch -d patch2
git branch
```

**Результат:**
```
Deleted branch patch2 (was fc3dd18).
* main
```

---

## Вывод о проделанной работе

В ходе выполнения данного практического задания были освоены основные принципы работы с системой контроля версий Git и платформой GitHub.

**На первом этапе** был создан репозиторий, выполнена базовая настройка и произведены первые коммиты. Было наглядно продемонстрировано различие между добавлением новых файлов через `git add` и коммитом изменений в уже отслеживаемых файлах с помощью флага `-am`.

**Второй этап** позволил изучить механизм ветвления в Git. Была создана отдельная ветка для внесения изменений, выполнена работа в ней, а затем создан pull-реквест для предложения этих изменений в основную ветку. Важным моментом стало понимание того, как дополнительные коммиты автоматически добавляются в уже существующий pull-реквест, а также процесс удаления веток как в локальном, так и в удаленном репозитории после успешного слияния.

**Третий этап** был наиболее сложным и полезным. Была смоделирована ситуация возникновения конфликта при слиянии, когда в разных ветках изменялись одни и те же строки кода. В процессе выполнения был освоен ключевой инструмент разрешения конфликтов — `git rebase`. Этот подход позволяет не просто устранить конфликт, но и переписать историю ветки таким образом, чтобы она начиналась от актуального состояния основной ветки, что делает историю проекта более линейной и чистой. Также был применен `git push --force-with-lease` для отправки изменений после перезаписи истории и финальное слияние pull-реквеста.

В результате выполнения всех трех частей задания были получены практические навыки работы с Git и GitHub, включая создание коммитов, управление ветками, работу с pull-реквестами и разрешение конфликтов слияния. Эти навыки являются фундаментальными для современной командной разработки программного обеспечения.

