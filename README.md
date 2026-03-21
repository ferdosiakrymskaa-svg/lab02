

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


## Часть 1
#### I Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
Создали пустой репозиторий на сайте `github.com`.


#### II Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
Выполнили инструкцию по созданию первого коммита на странице репозитория, созданного на предыдущем шаге.


#### III Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге. Реализуйте программу `Hello world` на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
Создали файл `hello_world.cpp` в локальной копии репозитория с помощью команды: 
```sh
touch hello_world.cpp
```
Далее реализовали программу `Hello world` на языке C++ используя плохой стиль кода, в окне редактора `nano`:
```sh
nano hello_world.cpp
```
Код программы:
```sh
#include <iostream>
using namespace std;

int main() {
    cout << "Hello world" << endl;
    return 0;
}
```


#### IV Добавьте этот файл в локальную копию репозитория.
Добавили файл `hello_world.cpp` в локальную копию репозитория в помощью с помощью команды:
```sh
git add hello_world.cpp
```


#### V Закоммитьте изменения с осмысленным сообщением.
Закоммитили изменения с осмысленным сообщением с помощью команды:
```sh
git commit -am "Добалили файл hello_world.cpp с использованием `using namespace std`"
```
Параметр `-a` делает все добавленные и измененые файлы отслеживаемыми, поэтому в процессе коммитов далее не понадобится вводить команду `git add hello_world.cpp`.


#### VI Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение Hello world from @name, где @name имя пользователя.
Изменили исходный, чтобы через стандартный поток ввода запрашивалось имя пользователя, а в стандартный поток вывода печаталось сообщениесообщение Hello world from @name, где @name имя пользователя:
Код после изменений в редакторе `nano`, который можно открыть для редакции данного файла с помощью команды `nano hello_world.cpp`:
```sh
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string name;
    cout << "Input your name: ";
    cin >> name;
    cout << "Hello world from "<<name << endl;
    return 0;
}

```


#### VII Закоммитьте новую версию программы. Почему не надо добавлять файл повторно git add?
Закоммитили новую версию программы с помощью команды:
```sh
git commit -m "Добавили ввод в имени пользователя через стандартный поток ввода"
```
`git add` не нужно добавлять повторно, так как мы ранее прорисали в первом коммите параметр `-a` который сделал файл отслеживаемым.
Результат работы программы `git log`:
```sh
commit 19c36a595c3de431132ff42878aa791e2e4ea7f2 (HEAD -> main)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Mar 21 19:23:55 2026 +0000

    Добалили файл hello_world.cpp с использованием

commit 781a476048b439ec099454889094b1020309bac2 (origin/main)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Wed Mar 18 14:34:40 2026 +0000
```
---

#### VIII Запуште изменения в удалёный репозиторий.
Пушим изменения в удаленный репозиторий с помощью команды:
```sh
git push origin main
```
Результат работы программы:
```sh
Username for 'https://github.com': ferdosiakrymskaa-svg
Password for 'https://ferdosiakrymskaa-svg@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 478 bytes | 478.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   781a476..52e5061  main -> main
```


#### IX Проверьте, что история коммитов доступна в удалёный репозитории.
Из результата работы программы в прошлом пункте видно, что все коммиты были внесены в удаленный репозиторий из локального.


## Часть 2

#### I В локальной копии репозитория создайте локальную ветку `patch1`.
Создали в локальной копии репозитория локальную ветку `patch1` с помощью команды:
```sh
git branch patch1
```
#### II Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.
Переключаемся на ветку `patch1` с помощью команды `git checkout patch1`.
Редактируем с помощью редактора `nano` с помощью команды `nano hello_world.cpp`. Данный файл после редактирования:
```sh
#include <iostream>
#include <string>


int main()
{
    std::string name;
    std::cout << "Input your name: ";
    std::cin >> name;
    std::cout << "Hello world from "<<name << std::endl;
    return 0;
}
```

#### III `commit`, `push` локальную ветку в удалённый репозиторий.
Сделали коммит и пуш локальной ветки в удаленный рпозиторий с помощью команд:
```sh
git commit -am "Избавились от using namespace std"
git push -u origin patch1
```
Параметр `-u` позволяет единожды прописать `git push -u origin patch1` далее можно будет прописывать только `git push`.

#### IV Проверьте, что ветка `patch1` доступна в удалёный репозитории.
Проверили, что ветка `patch1` доступна в удаленный репозиторий тем, что вывело в терминал при вводе придыдущих команд:
```sh
numerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 422 bytes | 422.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/ferdosiakrymskaa-svg/Homework_lab02/pull/new/patch1
remote: 
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 * [new branch]      patch1 -> patch1
branch 'patch1' set up to track 'origin/patch1'.
```
#### V Создайте  `pull-request` `patch1 -> master`.
Создали `pull-request` ветки `patch1 -> master` в терминале с помощью следующих команд:
```sh
gh auth login
```
Данная команда позволяет пройти аунтефикацию в терминале(процесс аунтефикации):
```sh
 What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations on this host? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Paste an authentication token
Tip: you can generate a Personal Access Token here https://github.com/settings/tokens
The minimum required scopes are 'repo', 'read:org', 'workflow'.
? Paste your authentication token: ****************************************
- gh config set -h github.com git_protocol https
✓ Configured git protocol
✓ Logged in as ferdosiakrymskaa-svg
```
С помощью следующей команды выполняем непосредственно `pull-request`:
```sh
gh pr create --base main
```
`gh` означает GitHub, `pr` -  `pull-request`, `base` показывает, в какую ветку мы хотим влить данные из выбранной.
Результат работы программы:
```sh
Warning: 1 uncommitted change

Creating pull request for patch1 into main in ferdosiakrymskaa-svg/Homework_lab02

? Title Избавились от using namespace std
? Body <Received>
? What's next? Submit
https://github.com/ferdosiakrymskaa-svg/Homework_lab02/pull/1
```
#### VI В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
Добавили комметарии в исходный код с помощью редактора `nano`. Новый код с комментариями:
```sh
#include <iostream>//Библиотека для ввода\вывода
#include <string>//Библиотека для работы со строками


int main()
{
    std::string name;//Переменная, хранящая имя пользователя
    std::cout << "Input your name: ";//Печатает комментарий, который говорит, что пользователю необходимо ввести
    std::cin >> name;//Ввод имени пользователя
    std::cout << "Hello world from "<<name << std::endl;//Вывод программы
    return 0;
}
```
#### VII **commit**, **push**.
Выполнили коммит и пуш в удаленный репозиторий:
```sh
git commit -am "Добавили комментарии в код"
git push
```
Результат работы команд:
```sh
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 636 bytes | 636.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
   77bb14d..717e553  patch1 -> patch1
```
#### VIII Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
Да, действительно, новые изменения есть в `pull-request`.

#### IX В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
Выполнили указанные действия на странице GitHub.
#### X Локально выполните **pull**.
```sh
git pull
```
Результат работы команды(`pull` для ветки `main`):
```sh
Updating 52e5061..ca6d371
Fast-forward
 hello_world.cpp | 14 +++++++-------
 1 file changed, 7 insertions(+), 7 deletions(-)
```
#### XI С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.
Результат работы команды:
```sh
commit ca6d3715e849b54f1c3e06fda093568f3a91b5af (HEAD -> main, origin/main)
Merge: 52e5061 717e553
Author: Ilyasov_Ilya <ferdosiakrymskaa@gmail.com>
Date:   Sat Mar 21 20:59:17 2026 +0000

    Merge pull request #1 from ferdosiakrymskaa-svg/patch1
    
    Избавились от using namespace std

commit 717e5539c5022dcb64f884d07c79719878fdf535 (origin/patch1, patch1)
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Mar 21 20:41:53 2026 +0000

    Добавили комментарии в код

commit 77bb14d06541ea284f758427148ef59a82fedf15
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Mar 21 19:59:10 2026 +0000

    Избавились от using namespace std

commit 52e506115a26250f3f85057b5403d39c22680fbb
Author: ferdosiakrymskaa-svg <ferdosiakrymskaa@gmail.com>
Date:   Sat Mar 21 19:23:55 2026 +0000

    Добавили ввод в имени пользователя через стандартный поток ввода
```
#### XII Удалите локальную ветку `patch1`.
Удаляем ветку `patch1` с помощью данной команды(параметр `-d` - delete):
```sh
git branch -d patch1
```
Результат работы программы:
```sh
Deleted branch patch1 (was 717e553).
```

# Часть 3

#### I Создайте новую локальную ветку `patch2`.
Создали ветку и сразу выбрали ее с помощью команды:
```sh
git checkout -b patch2
```
#### II Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
Изменили формат файла `hello_world.cpp` на `Mozilla` с помощью команды(параметр `-i` позволяет изменить именно стиль файла, а не просто вывести в терминал в формате `Mozilla`):
```sh
clang-format -style=Mozilla -i hello_world.cpp
```
#### III **commit**, **push**, создайте pull-request `patch2 -> master`.
Выполнили коммит и запушили изменения с помощью следующих команд:
```sh
git commit -am "Изменили стиль на Mozzila"
git push -u origin patch2
```
Результат работы команд:
```sh
[patch2 48cdb47] Изменили стиль на Mozzila
 1 file changed, 10 insertions(+), 9 deletions(-)

Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 471 bytes | 235.00 KiB/s, done.
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
#### IV В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
В ветке main в удаленном репозитории изменил комментарии(стало):
```sh
#include <iostream>//Библиотека для ввода\вывода
#include <string>//Библиотека для работы со строками


int main()
{
    std::string name;//Переменная, хранящая имя пользователя(value, which save user name)
    std::cout << "Input your name: ";//Печатает комментарий, который говорит, что пользователю необходимо ввести
    std::cin >> name;//Ввод имени пользователя
    std::cout << "Hello world from "<<name << std::endl;//Вывод программы
    return 0;
}
```
#### V Убедитесь, что в pull-request появились *конфликтны*.
Действительно появились конфликты.
#### VI Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
Для начала синхронизуем все ветки  локального репозитория с удаленным с помощью команды:
```sh
git fetch origin
```
Далее вводим команды для `rebase`, но получаем сообщение о конфликте:
```sh
Auto-merging hello_world.cpp
CONFLICT (content): Merge conflict in hello_world.cpp
error: could not apply 48cdb47... Изменили стиль на Mozzila
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 48cdb47... Изменили стиль на Mozzila
```
Редактируем файл редактором `nano` и получаем в итоге:
```sh
#include <iostream> //Библиотека для ввода\вывода
#include <string> //Библиотека для работы со строками

int
main()
{

  
  std::string name; // Переменная, хранящая имя пользователя(value, which save user name)
  std::cout << "Input your name: "; // Печатает комментарий, который говорит,
                                    // что пользователю необходимо ввести
  std::cin >> name; // Ввод имени пользователя
  std::cout << "Hello world from " << name << std::endl; // Вывод программы
  return 0;

}
```
Теперь добавляем файл командой `git add hello_world.cpp`. Далее выполняем `git rebase --continue`
Результат работы программы:
```sh
[detached HEAD 4ad1a0b] Изменили стиль на Mozzila
 1 file changed, 13 insertions(+), 9 deletions(-)
Successfully rebased and updated refs/heads/patch2.
```
#### VII Сделайте *force push* в ветку `patch2`
Выполняем команду `git push --force-with-lease origin patch2`.
Результат работы команды:
```sh
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 3 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 479 bytes | 479.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/ferdosiakrymskaa-svg/Homework_lab02.git
 + 48cdb47...4ad1a0b patch2 -> patch2 (forced update)
```
#### VIII Убедитель, что в pull-request пропали конфликтны. 
Конфликты действительно пропали.
#### IX Вмержите pull-request `patch2 -> master`.
Мержим командой `gh pr merge --merge`.
Результат работы команды:
```sh
✓ Merged pull request #2 (Изменили стиль на Mozzila)
```

## Вывод о проделанной работе

В ходе выполнения данного практического задания были освоены основные принципы работы с системой контроля версий Git и платформой GitHub.

**На первом этапе** был создан репозиторий, выполнена базовая настройка и произведены первые коммиты. Было наглядно продемонстрировано различие между добавлением новых файлов через `git add` и коммитом изменений в уже отслеживаемых файлах с помощью флага `-am`.

**Второй этап** позволил изучить механизм ветвления в Git. Была создана отдельная ветка для внесения изменений, выполнена работа в ней, а затем создан pull-реквест для предложения этих изменений в основную ветку. Важным моментом стало понимание того, как дополнительные коммиты автоматически добавляются в уже существующий pull-реквест, а также процесс удаления веток как в локальном, так и в удаленном репозитории после успешного слияния.

**Третий этап** был наиболее сложным и полезным. Была смоделирована ситуация возникновения конфликта при слиянии, когда в разных ветках изменялись одни и те же строки кода. В процессе выполнения был освоен ключевой инструмент разрешения конфликтов — `git rebase`. Этот подход позволяет не просто устранить конфликт, но и переписать историю ветки таким образом, чтобы она начиналась от актуального состояния основной ветки, что делает историю проекта более линейной и чистой. Также был применен `git push --force-with-lease` для отправки изменений после перезаписи истории и финальное слияние pull-реквеста.

В результате выполнения всех трех частей задания были получены практические навыки работы с Git и GitHub, включая создание коммитов, управление ветками, работу с pull-реквестами и разрешение конфликтов слияния. 

