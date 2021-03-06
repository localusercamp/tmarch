# Архитетура бэкенда



### Схема основных сущностей приложения
![alt text](/assets/scheme.jpg)



***
<br>



### Основные сущности приложения
***
<details>
<summary><b>Request</b></summary>
<br>

*Request* является воплощением запроса от пользователя к серверу. 

#### Правила:
- **Должен** содержать валидацию данных, поступающих в контроллер.
- **Может** содержать проверку правовых политик.
- **Только** Request может валидировать входные данные.
- *Request* **не может** использовать другой *Request*.
- *Request* **не может** реализовывать *Contract*.
</details>


***
<details>
<summary><b>Controller</b></summary>
<br>

*Controller* отделяет бизнес логику от валидации данных и манипуляций с запросами.

#### Правила:
- *Controller* **обязан** использовать кастомный *Request* для **каждого** своего метода.
- *Controller* **должен** уметь:
    - получить **отвалидированные** данные из *Request*
    - передать эти данные в *Action*
    - получить ответ от *Action*
    - составить и вернуть ответ 
- *Controller* **не знает ничего** о сущностях, до которых не может дотянуться (```схема```), и использовать их, соответственно, не может.
- **Каждый** метод контроллера может вызывать **только один** *Action*.
- *Controller* **не может** использовать другой *Controller*.
- *Controller* **не может** реализовывать *Contract*.
</details>


***
<details>
<summary><b>Action</b></summary>
<br>

*Action* - это точка входа в бизнес логику приложения. *Action* представляет собой действие в приложении и по совокупности этих действий можно определить, что делает приложение.

#### Правила:
- *Action* состоит **только из одного публичного статического метода** ```run()```.
- *Action* **не может** поднимать вверх по схеме сущности, которыми может пользоваться.
- *Action* **может** использовать сколько угодно: 
    - *Task*
    - *Exception*
    - *Event*
    - *Collection*
    - *Entity*
    - *Help functions*
    - *Model* (**не желательно**)
- *Action* **не может** использовать другой *Action*.
- *Action* **не может** реализовывать *Contract*.
</details>


***
<details>
<summary><b>Task</b></summary>
<br>

*Task* - сущность, инкапсулирующая выполнение определенной задачи.

#### Правила:
- состоит **только из одного публичного статического метода** ```run()```
- *Task* **может** использовать сколько угодно: 
    - *Exception*
    - *Event*
    - *Collection*
    - *Help functions*
    - *Model*
- *Task* **не может** использовать другой *Task*.
- *Task* **не может** реализовывать *Contract*.
</details>


***
<details>
<summary><b>Model</b></summary>
<br>

*Model* - модель Laravel.

#### Правила:
- *Model* **может** использовать **свой** *Collection*.
- *Model* **может** реализовывать *Contract*.
- *Model* **может** использовать другую *Model*.
- *Model* **может** использовать трейты для шэринга скоупами.
</details>


***
<details>
<summary><b>Collection</b></summary>
<br>

*Collection* инкапсулирует чейнинг EloquentCollection для конкретной модели.

#### Правила:
- *Collection* **может** использовать *Model*.
- *Collection* **может** использовать *Entity*.
- *Collection* **может** использовать другую *Collection*.
- *Collection* **может** реализовывать *Contract*.
- *Collection* **не может** использовать другие сущности.
</details>


***
<details>
<summary><b>Entity</b></summary>
<br>

*Entity* - сущность, которая реализует конкретную задачу с использованием преимуществ ООП (сохранение состояния, наследование, инкапсуляция и т.д.), но при этом не нуждается в использовании `EloquentBuilder` как *Model*.

Большая задача может решаться целым _`namespace`'ом_, состоящим из *Entity* разного назначения: классы, классы-мэнеджеры, абстрактные классы, контракты.

#### Правила:
- *Entity* **может** использовать сколько угодно: 
    - *Task*
    - *Exception*
    - *Event*
    - *Collection*
    - *Entity*
    - *Help functions*
    - *Model*
- *Entity* **может** реализовывать *Contract*.
</details>


***
<details>
<summary><b>Exception</b></summary>
<br>

*Exception* - исключение, выбрасываемое в случае ошибки.

#### Правила:
- *Exception* **не может** использовать сущности.
</details>



***
<br>



### Дополнительно
***
<details>
<summary><b>Отличия Contract от Interface</b></summary>
<br>

*Contract* реализуются сущностям-наследниками родительских сущностей проекта.

*Interface* необходимы родительским сущностям проекта для создания ограничений и нужного функционала.

Примеры:
- ```LocationCollection extends Collection implements CanBeModified```. CanBeModified - это *Contract*
- ```Action implements IAction```. IAction - это *Interface*
</details>

***
