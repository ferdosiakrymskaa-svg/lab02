

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



# Homework
[Репозиторий Homework_lab02](https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git)

Я поняла! Вы хотите, чтобы я составила отчет **строго по заданиям** (Part I, Part II, Part III), используя **только ваши реальные данные из терминала** (которые вы скинули в длинном сообщении с `git init` и далее).

Я беру только то, что **реально происходило в вашем терминале**, и подставляю в структуру заданий. Ничего не выдумываю.

---

# Отчет по лабораторной работе №2

**Студент:** Ильясов Илья 
**Репозиторий:** https://github.com/ferdosiakrymskaa-svg/Homework_lab02

---

## Part I

### 1. Создан пустой репозиторий на GitHub
Репозиторий `Homework_lab02` создан на GitHub.

### 2. Выполнена инструкция по созданию первого коммита, создание файла `hello_world.cpp`

```bash
touch hello_world.cpp
git add hello_world.cpp
git commit -m "Добавили файл hello_world.cpp"
```
```bash
[main (root-commit) b054ecb] Добавили файл hello_world.cpp
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello_world.cpp
```

### 3. Реализация файла hello_world.cpp в локальной копии репозитория

```bash
#include <iostream>
using namespace std;

int main()
{
    cout << "Hello world from "<< name<< endl;
    return 0;
}
```

### 4. Файл добавлен в локальную копию репозитория

```bash
$ git add hello_world.cpp
```

### 5. Изменения закоммичены с осмысленным сообщением

```bash
$ git commit -m "Добавили файл hello_world.cpp"
```

**Вывод:**
```
[main (root-commit) b054ecb] Добавили файл hello_world.cpp
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello_world.cpp
```

> **Примечание:** Первый коммит был сделан с пустым файлом, затем код был добавлен позже.

### 6. Изменен исходный код: добавлен ввод имени пользователя

```bash
$ cat > hello_world.cpp << 'EOF'
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string name;
    cout << "Введите имя пользователя: "; cin >> name;
    cout << "Hello world from "<< name<< endl;
    return 0;
}
EOF
```

### 7. Новая версия закоммичена

```bash
git commit -am "В стандартный поток вывода печатается сообщение: "Hello world from @name", где @name имя пользователя "
```

**Вывод:**
```
[main 1680999] В стандартный поток вывода печатается сообщение: Hello world from @name, где @name имя пользователя
 1 file changed, 4 insertions(+), 1 deletion(-)
```

**Почему не надо добавлять файл повторно `git add`?**  
Файл `hello_world.cpp` уже отслеживается Git. Флаг `-am` автоматически добавляет изменения в отслеживаемых файлах перед коммитом.

### 8. Изменения отправлены в удаленный репозиторий

```bash
$ git push -u origin main
```

**Вывод:**
```
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 3 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 926 bytes | 926.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   b054ecb..1680999  main -> main
branch 'main' set up to track 'origin/main'.
```

### 9. Проверка истории коммитов в удаленном репозитории

```bash
$ git log
```

**Вывод:**
```
commit 16809996b67ef967877359f0d03a9b7eba216514 (HEAD -> main)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:03:50 2026 +0000

    В стандартный поток вывода печатается сообщение: Hello world from @name, где @name имя пользователя

commit 999f3e59d5037c682a831b5e5eb603d53e31e147
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 19:56:30 2026 +0000

    Реализовали в файле hello_world.cpp программу Hello world

commit b054ecb6f23e8dd1320734fa6ab18f303aca028f (origin/main)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 19:50:35 2026 +0000

    Добавили файл hello_world.cpp
```

---

## Part II

### 1. Создана локальная ветка patch1

```bash
$ git checkout -b patch1
```

**Вывод:**
```
Switched to a new branch 'patch1'
```

### 2. Внесены изменения: удален `using namespace std;`

```bash
nano hello_world.cpp
#include <iostream>
#include <string>

int main()
{
    std::string name;
    std::cout << "Введите имя пользователя: "; std::cin >> name;
    std::cout << "Hello world from " << name << std::endl;
    return 0;
}

```

### 3. Коммит и push ветки patch1

```bash
$ git add hello_world.cpp
$ git commit -m "Убрали using namespace std;"
```

**Вывод:**
```
[patch1 7232c19] Убрали using namespace std;
 1 file changed, 4 insertions(+), 4 deletions(-)
```

```bash
$ git push -u origin patch1
```

**Вывод:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 448 bytes | 448.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/ferdosiakrymskaa-svg/Homework_lab02/pull/new/patch1
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 * [new branch]      patch1 -> patch1
branch 'patch1' set up to track 'origin/patch1'.
```

### 4. Ветка patch1 доступна в удаленном репозитории

```bash
$ git log
```

**Вывод:**
```
commit 7232c19fab13639f64c224a9368057565bac913c (HEAD -> patch1, origin/patch1)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:12:26 2026 +0000

    Убрали using namespace std;

commit 16809996b67ef967877359f0d03a9b7eba216514 (origin/main, main)
...
```

### 5. Создан pull-request patch1 → main

На GitHub создан Pull Request #1.

### 6. В ветке patch1 добавлены комментарии в код

```bash
$ nano hello_world.cpp 
#include <iostream>
#include <string>

int main()
{
    std::string name; // Переменная, хранящая имя пользователя
    std::cout << "Введите имя пользователя: "; std::cin >> name;
    std::cout << "Hello world from " << name << std::endl;
    return 0;
}
```

### 7. Коммит и push

```bash
$ git add hello_world.cpp
$ git commit -m "Добавили комментарии в код"
```

**Вывод:**
```
[patch1 a1415fd] Добавили комментарии в код
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```bash
$ git push origin patch1
```

**Вывод:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 368 bytes | 368.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   7232c19..a1415fd  patch1 -> patch1
```

### 8. Новые изменения появились в pull-request

На GitHub в Pull Request #1 отображаются новые коммиты с комментариями.

### 9. Слияние PR patch1 → main и удаление ветки patch1 в удаленном репозитории

На GitHub выполнен merge Pull Request #1, ветка `patch1` удалена в удаленном репозитории.

### 10. Локально выполнен pull

```bash
$ git pull origin main
```

**Вывод:**
```
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), 930 bytes | 930.00 KiB/s, done.
From https://github.com/ferdosiakrymskaa-svg/Homework_lab02
 * branch            main       -> FETCH_HEAD
   1680999..62c3b5e  main       -> origin/main
Updating a1415fd..62c3b5e
Fast-forward
```

### 11. Просмотр истории в локальной версии ветки main

```bash
$ git log
```

**Вывод:**
```
commit 62c3b5ead2ec28d01636b1dd82be6f9cd023727b (HEAD -> main, origin/main)
Merge: 1680999 a1415fd
Author: Ilyasov_Ilya <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:41:58 2026 +0000

    Merge pull request #1 from ferdosiakrymskaa-svg/patch1
    
    Убрали using namespace std;

commit a1415fdbfb7a667dad72e077737d5df992a54eb7
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:34:29 2026 +0000

    Добавили комментарии в код

commit 7232c19fab13639f64c224a9368057565bac913c
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:12:26 2026 +0000

    Убрали using namespace std;

commit 16809996b67ef967877359f0d03a9b7eba216514
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:03:50 2026 +0000

    В стандартный поток вывода печатается сообщение: Hello world from @name, где @name имя пользователя

commit 999f3e59d5037c682a831b5e5eb603d53e31e147
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 19:56:30 2026 +0000

    Реализовали в файле hello_world.cpp программу Hello world

commit b054ecb6f23e8dd1320734fa6ab18f303aca028f
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 19:50:35 2026 +0000

    Добавили файл hello_world.cpp
```

### 12. Удалена локальная ветка patch1

```bash
$ git branch -d patch1
```

**Вывод:**
```
Deleted branch patch1 (was 62c3b5e).
```

---

## Part III

### 1. Создана новая локальная ветка patch2

```bash
$ git pull origin main
$ git checkout -b patch2
```

**Вывод:**
```
From https://github.com/ferdosiakrymskaa-svg/Homework_lab02
 * branch            main       -> FETCH_HEAD
Already up to date.
Switched to a new branch 'patch2'
```

### 2. Изменен code style с помощью clang-format

```bash
$ sudo apt install clang-format
$ clang-format -style=Mozilla -i hello_world.cpp
```

**Файл после форматирования:**
```cpp
#include <iostream>
#include <string>

int
main()
{
  std::string name; // Переменная, хранящая имя пользователя
  std::cout << "Введите имя пользователя: ";
  std::cin >> name;
  std::cout << "Hello world from " << name << std::endl;
  return 0;
}
```

### 3. Коммит, push, создан pull-request patch2 → main

```bash
$ git commit -am "Изменен стиль кода"
```

**Вывод:**
```
[patch2 90f8643] Изменен стиль кода
 1 file changed, 7 insertions(+), 6 deletions(-)
```

```bash
$ git push origin patch2
```

**Вывод:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 403 bytes | 403.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'patch2' on GitHub by visiting:
remote:      https://github.com/ferdosiakrymskaa-svg/Homework_lab02/pull/new/patch2
remote: 
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 * [new branch]      patch2 -> patch2
```

На GitHub создан Pull Request #2 (patch2 → main).

### 4. В ветке main изменены комментарии (переведены на английский)

```bash

#include <iostream>
#include <string>


int main()
{
    std::string name;//Переменная, хранящая имя пользователя(Value, which save user name)
    std::cout << "Введите имя пользователя: "; std::cin >> name;
    std::cout << "Hello world from "<< name<< std::endl;
    return 0;
}


$ git add hello_world.cpp
$ git commit -m "Перевели комментарии на английский язык"
$ git push origin main
```

**Вывод:**
```
[main 62d7ae3] Перевели комментарии на английский язык
 1 file changed, 1 insertion(+), 1 deletion(-)
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 374 bytes | 374.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   62c3b5e..62d7ae3  main -> main
```

### 5. В pull-request появились конфликты

На GitHub в Pull Request #2 отобразилось сообщение о конфликте.

### 6. Локально выполнены pull + rebase, конфликты исправлены

```bash
$ git checkout patch2
$ git rebase main
```

**Вывод:**
```
Switched to branch 'patch2'
Auto-merging hello_world.cpp
CONFLICT (content): Merge conflict in hello_world.cpp
error: could not apply 90f8643... Изменен стиль кода
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 90f8643... Изменен стиль кода
```

**Конфликт в файле:**
```cpp
#include <iostream>
#include <string>

int
main()
{
<<<<<<< HEAD
    std::string name;//Переменная, хранящая имя пользователя(Value, which save user name)
    std::cout << "Введите имя пользователя: "; std::cin >> name;
    std::cout << "Hello world from "<< name<< std::endl;
    return 0;
=======
  std::string name; // Переменная, хранящая имя пользователя
  std::cout << "Введите имя пользователя: ";
  std::cin >> name;
  std::cout << "Hello world from " << name << std::endl;
  return 0;
>>>>>>> 90f8643 (Изменен стиль кода)
}
```

**Исправление конфликта:**

```bash
$ nano hello_world.cpp
```

**Исправленный файл:**
```cpp
#include <iostream>
#include <string>

int
main()
{

    std::string name;//Переменная, хранящая имя пользователя(Value, which save user name)
    std::cout << "Введите имя пользователя: "; std::cin >> name;
    std::cout << "Hello world from "<< name<< std::endl;
    return 0;

}
```

```bash
$ git add hello_world.cpp
$ git rebase --continue
```

**Вывод:**
```
[detached HEAD e6bb579] Изменен стиль кода
 1 file changed, 4 insertions(+), 2 deletions(-)
Successfully rebased and updated refs/heads/patch2.
```

### 7. Сделан force push в ветку patch2

```bash
$ git push --force-with-lease origin patch2
```

**Вывод:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 340 bytes | 340.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 + 90f8643...e6bb579 patch2 -> patch2 (forced update)
```

### 8. Конфликты в pull-request пропали

На GitHub в Pull Request #2 конфликты больше не отображаются.

### 9. Выполнен merge pull-request patch2 → main

На GitHub выполнен merge Pull Request #2.

```bash
$ git log
```

**Вывод:**
```
commit 743649576e7fd5fba608ff30626df000b26fc7e6 (HEAD -> patch2, origin/main)
Merge: 62d7ae3 e6bb579
Author: Ilyasov_Ilya <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 21:30:04 2026 +0000

    Merge pull request #2 from ferdosiakrymskaa-svg/patch2
    
    Изменен стиль кода

commit e6bb57949ea4a911ff1b232397b4e8e0f5ffc680 (origin/patch2)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 21:12:26 2026 +0000

    Изменен стиль кода

commit 62d7ae3ca951ba1b127639ea9a7c57d125229905 (main)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 21:18:59 2026 +0000

    Перевели комментарии на английский язык

commit 62c3b5ead2ec28d01636b1dd82be6f9cd023727b
Merge: 1680999 a1415fd
Author: Ilyasov_Ilya <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:41:58 2026 +0000

    Merge pull request #1 from ferdosiakrymskaa-svg/patch1
    
    Убрали using namespace std;

commit a1415fdbfb7a667dad72e077737d5df992a54eb7 (origin/patch1)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:34:29 2026 +0000

    Добавили комментарии в код

commit 7232c19fab13639f64c224a9368057565bac913c
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:12:26 2026 +0000

    Убрали using namespace std;

commit 16809996b67ef967877359f0d03a9b7eba216514
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 20:03:50 2026 +0000

    В стандартный поток вывода печатается сообщение: Hello world from @name, где @name имя пользователя

commit 999f3e59d5037c682a831b5e5eb603d53e31e147
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 19:56:30 2026 +0000

    Реализовали в файле hello_world.cpp программу Hello world

commit b054ecb6f23e8dd1320734fa6ab18f303aca028f
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Apr 4 19:50:35 2026 +0000

    Добавили файл hello_world.cpp
```

---



## Выводы

В ходе выполнения лабораторной работы я:
1. Создал локальный и удаленный Git-репозитории.
2. Научился делать коммиты и отправлять изменения на GitHub.
3. Освоил создание веток (`patch1`, `patch2`).
4. Научился создавать pull
