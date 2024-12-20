# Лабораторная работа 5
## Выполнила: Журбина Марина Андреевна К3139

### Ход работы
### Задание 1
1. Находясь в корне репозитория с помощью данной команды переходим в скрытую папку Hooks:
```
cd .git/hooks
```
2. Создаём файл pre-commit, где будет написан скрипт для проверки коммитов:
```
#!/bin/bash

for file in $(git diff --cached --name-only); do
    if [[ $file == *.txt ]]; then
        if grep -q "Автор:" "$file"; then
            echo "Файл $file соответствует формату и имеет подпись автора."
        else
            echo "Ошибка: Файл $file соответствует формату, но не имеет подписи>
            exit 1
        fi
    else
        echo "Ошибка: Файл $file не соответствует формату!"
        exit 1
    fi
done

exit 0
```
Если файл формата .txt, то начнётся проверка на наличие подписи автора в файле: если подпись в виде "Автор: ***" имеется, то произойдёт коммит и появится сообщние в консоли, если подписи нет, то коммит прервётся и появится соответствующее собщение в консоли. Если формат файла не .txt, то коммит тоже прервётся и появится сообщение.

3. Возвращаемся в корень репозитория и выполняем команды:
```
git add commit_test.txt
git commit -m "Проверка файла"
git push origin main
```
![image](https://github.com/user-attachments/assets/995acea8-40bf-4b94-878c-487ae1b0c0ed)
![image](https://github.com/user-attachments/assets/423369f8-bae8-4fb3-9ea0-70986369602b)

### Задание 2
1. Устанавливаем Git Flow и инициализируем его с помощью данных команд:
```
sudo apt-get install git-flow
git flow init
```
2. Создадим ветку для новой функциональности:
```
git flow feature start task-management
```
3. Создаём файл task_manager.py и напишем какой-нибудь простой скрипт:
```
def create_task(title, description):
    # Логика создания задачи
    print(f"Создана новая задача: {title}")
```
4. Закоммитим изменения:
```
git add task_manager.py
git commit -m "Добавлен функционал управления задачами"
```
5. После завершения разработки функции завершим фичу и объединим ее с основной веткой:
```
git flow feature finish task-management
```
6. Переключимся на ветку "develop" и начнем создание релиза:
```
git checkout develop
git flow release start v1.0.0
```
7. Внесем изменения, связанные с релизом, например создадим файл version.txt и запишем туда что-нибудь, далее закоммитим наши изменения:
```
echo "v1.0.0" > version.txt
git add version.txt
git commit -m "Обновлена версия для релиза v1.0.0"
```
8. Завершим релиз и объединим его с ветками "develop" и "main":
```
git flow release finish v1.0.0
```
9. Если в процессе использования выявлена критическая ошибка, нужно создать hotfix (у нас нет никакой ошибки, но hotfix все равно создаем, чтобы сымитировать ошибку):
```
git flow hotfix start hotfix-1.0.1
```
10. Создадим новый файл file_with_error.py и запишем туда "исправленный" код:
```
def create_task(title, description):
    # Логика создания задачи
    print("Исправлена критическая ошибка")
```
11. Закоммитим изменения:
```
git add file_with_error.py
git commit -m "Исправлена критическая ошибка"
```
12. Завершим hotfix и объединим его с ветками "develop" и "main":
```
git flow hotfix finish hotfix-1.0.1
```
13. Отправим изменения на удаленный репозиторий:
```
git push origin develop
git push origin main
```
По итогу работы ваши коммиты должны выглядеть подобным образом:

![image](https://github.com/user-attachments/assets/c388be0e-5b88-4b57-9eec-649e6f08c93e)

**Вывод:** Был создан репозиторий и клонирован на виртуальную машину, была реализована автоматизация проверки формата файлов при коммите, а также сымитировано использование Git Flow в проекте.
