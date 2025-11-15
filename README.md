# 7.7_HomeWork_GitLab

Упростить себе жизнь можно с помощью **SSH-key**, чтобы каждый раз не вводить **Login** и **token**. Для этого нужно:

1. Добавить публичный ключ в аккаунт **GitHub**.
    * Click на изображение профиля в правом верхнем углу
    * **Settings**
    * Слева в меню пункт **SSH and GPG keys**
    * **New SSH key**
    * Ввести название ключа (*ssh-key-1759501063847.pub*)
    * Вставить содержимое ключа
    * **Add SSH key**
2. Запустить агента: 
   ````
   eval "$(ssh-agent -s)"
   ````
4. Проверить соединение:
   ````
   ssh -T git@github.com
   ````
5. Изменить адрес с https формата на SSH:
   ````
   git remote set-url origin git@github.com:smyity/7.7_HomeWork_GitLab.git
   ````
    * **smyity** - имя пользователя
    * **7.7_HomeWork_GitLab** - рабочая директория
6. Теперь push выполняется без пароля:
   ````
   git push origin main
   ````

# Домашнее задание к занятию "GitLab"

### Задание 1
  Что нужно сделать:
1. Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в [этом репозитории](https://github.com/netology-code/sdvps-materials/tree/main/gitlab).
2. Создайте новый проект и пустой репозиторий в нём.
3. Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.

  В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.

### Решение 1
![](https://github.com/smyity/7.7_HomeWork_GitLab/blob/main/pic/SMK058.PNG)

![](https://github.com/smyity/7.7_HomeWork_GitLab/blob/main/pic/SMK060.PNG)

### Задание 2

**Что нужно сделать:**

1. Запушьте [репозиторий](https://github.com/netology-code/sdvps-materials/tree/main/gitlab) на GitLab, изменив origin. Это изучалось на занятии по Git.
2. Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.

В качестве ответа в шаблон с решением добавьте: 

 * файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне; 
 * скриншоты с успешно собранными сборками.
 
### Решение 2

*.gitlab-ci.yml*
````yaml
stages:
 - test
 - build

test:
 stage: test
 image: golang:1.17
 script:
  - go test .
 tags:
  - task7.7

build:
 stage: build
 image: docker:latest
 script:
  - docker build .
 tags:
  - task7.7
````

![](https://github.com/smyity/7.7_HomeWork_GitLab/blob/main/pic/SMK078.PNG)

### Задание 3

Измените CI так, чтобы:

 - этап сборки запускался сразу, не дожидаясь результатов тестов;
 - тесты запускались только при изменении файлов с расширением *.go.

В качестве ответа добавьте в шаблон с решением файл gitlab-ci.yml своего проекта или вставьте код в соответсвующее поле в шаблоне.

### Решение 3

* сборка запускается сразу, не дожидаясь результатов тестов.

*.gitlab-ci.yml*
````yaml
stages:
 - test
 - build

test:
 stage: test
 image: golang:1.17
 script:
  - go test .
 tags:
  - task7.7

build:
 stage: build
 image: docker:latest
 script:
  - docker build .
 tags:
  - task7.7
 needs: []
````

![](https://github.com/smyity/7.7_HomeWork_GitLab/blob/main/pic/SMK080.PNG)

* тесты запускаются только при изменении файлов с расширением *.go.

*.gitlab-ci.yml*
````yaml
stages:
  - test
  - build

test:
  stage: test
  image: golang:1.17
  script:
   - go test .
  tags:
   - task7.7
  rules:
    - changes: # условия при которых будет запускаться test
      - "*.go" # какие должны быть изменения
    - when: never # если условие changes не выполняется, то задача не запускается

build:
  stage: build
  image: docker:latest
  script:
   - docker build .
  tags:
   - task7.7
  needs: []
````

![](https://github.com/smyity/7.7_HomeWork_GitLab/blob/main/pic/SMK083.png)
