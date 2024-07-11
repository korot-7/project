# Первый README.md

### Шрифт может быть курсивным и жирным

Первая *попытка* сделать **что-то** или ~~не сделать~~

### Фрагмент кода на СИ

```C
printf("Hello world!");
```

### Ссылка на мой GitHub

[Ссылка на мой GitHub](https://github.com/korot-7 "GitHub")

# Шпаргалка по работе с Git

### Создание и привязывание SSH-ключей

##### Генерируем SSH-ключи
```
$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
```

##### Привязывание SSH-ключей

Сначала скопируем содержимое файла с публичным ключом (.pub)
```
# скопировать содержимое ключа в буфер обмена:
$ clip < ~/.ssh/id_rsa.pub
# для ed25519:
$ clip < ~/.ssh/id_ed25519.pub
```
Перейдите на GitHub и выберите пункт Settings в меню аккаунта. В меню слева нажмите на пункт SSH and GPG keys. В открывшейся вкладке выберите New SSH key. В поле Title напишите название ключа. Например, Personal key. В поле Key type должно быть Authentication Key. В поле Key скопируйте ваш ключ из буфера обмена.

### Связываем локальный и удалённый репозитории

##### Привязать удалённый репозиторий к локальному — git remote add

Перейдите на страницу удалённого репозитория, выберите тип SSH и скопируйте URL. Откройте консоль, перейдите в каталог локального репозитория и введите команду git remote add.
```
$ git remote add origin git@github.com:%ИМЯ_АККАУНТА%/first-project.git
```

### Синхронизируем локальный и удалённый репозитории

Вы уже прошли весь «цикл коммита»: подготовили файлы с помощью git add, закоммитили их с комментарием командой git commit -m. Осталось загрузить содержимое локального репозитория на GitHub. За это отвечает команда git push

В первый раз эту команду нужно вызвать с флагом -u и параметрами origin (имя удалённого репозитория) и main или master (название текущей ветки). Флаг -u свяжет локальную ветку с одноимённой удалённой.

```
$ git push -u origin main # Если команда приведёт к ошибке, попробуйте 
                          # заменить main на master. 
```

### Хеш — идентификатор коммита

Git хранит таблицу соответствий хеш → информация о коммите

### Исследуем лог

После вызова git log появляется список коммитов:

строка из цифр и латинских букв после слова commit — это хеш коммита;

Author — имя автора и его электронная почта;

Date — дата и время создания коммита;

в конце находится сообщение коммита.

Получить сокращённый лог — git log --oneline

### HEAD

Файл HEAD — один из служебных файлов папки .git. Он указывает на коммит, который сделан последним (то есть на самый новый).

### Статусы файлов в Git

Одна из ключевых задач Git — отслеживать изменения файлов в репозитории. Для этого каждый файл помечается каким-либо статусом. Рассмотрим основные.

**untracked** (англ. "неотслеживаемый")

Мы говорили, что новые файлы в Git-репозитории помечаются как untracked, то есть неотслеживаемые. 

**staged** (англ. «подготовленный»)

После выполнения команды git add файл попадает в staging area , то есть в список файлов, которые войдут в коммит. В этот момент файл находится в состоянии staged.

**tracked** (англ. «отслеживаемый»)

Состояние tracked — это противоположность untracked. Оно довольно широкое по смыслу: в него попадают файлы, которые уже были зафиксированы с помощью git commit, а также файлы, которые были добавлены в staging area командой git add. То есть все файлы, в которых Git так или иначе отслеживает изменения.


**modified** (англ. «изменённый»)

Состояние modified означает, что Git сравнил содержимое файла с последней сохранённой версией и нашёл отличия. Например, файл был закоммичен и после этого изменён.

### Типичный жизненный цикл файла в Git

```mermaid
graph TD;
	untracked(неотслеживаемый) -- "git add" --> staged(ВСпискеКоммитов)/tracked;
	staged(ВСпискеКоммитов)/tracked -- "git commit" --> tracked(отслеживаемый);
	tracked(отслеживаемый) -- "Изменения" --> modified(изменённый);
	modified(изменённый) -- "git add" --> staged(ВСпискеКоммитов)/tracked;
	staged(ВСпискеКоммитов)/tracked -- "Изменения" --> modified(изменённый);
```

