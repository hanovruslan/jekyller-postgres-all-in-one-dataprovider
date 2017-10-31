---

layout: yandex2

style: |
    /* собственные стили можно писать здесь!! */
    .pre-small pre code { font-size: 24px!important; line-height: 48px!important; }
    .pre-big pre code { font-size: 54px !important; line-height: 108px !important; } #  9 lines x 52 symbols
    .big-list { font-size: 80px!important; line-height: 160px!important; }
    .images-w { background-color: #fff !important; }
    .slide-red { border-left: 9px solid #f00 !important; }
    figure.short { width: 480px !important; }
    .text-center { text-align: center !important; }
    img.center { margin: auto !important; }

---

# ![](themes/yandex2/images/logo-{{ site.presentation.lang }}.svg){:.logo}

## {{ site.presentation.title }}
{:.title}

### ![](themes/yandex2/images/title-logo-{{ site.presentation.lang }}.svg){{ site.presentation.service }}

<div class="authors">
{% if site.author %}
<p>{{ site.author.name }}{% if site.author.position %}, {{ site.author.position }}{% endif %}</p>
{% endif %}
</div>

## Цели

**Использовать только одну базу данных в приложении**

## План

- {:.next}О реальности
- {:.next}Описание эксперимента
- {:.next}Реализация
- {:.next}Заключение

## Особенности современного приложения

**Разные виды данных**

**Eventual consistency**

**Несколько видов баз данных**

## Типы данных

- {:.next}Структурированные данные со схемой
- {:.next}Структурированные данные без схемы
- {:.next}Неструктурированные данные

## Потоки данных

- {:.next}Перекладываем байтики
- {:.next}Порождаем события

## Типы хранилищ

- {:.next}RDBMS
- {:.next}NoSQL
- {:.next}Key-Value
- {:.next}MQ
- {:.next}Разное

## Доступ к данным

1. Хранилище
1. Структура

## Эксперимент
{:.section}

## Основа

1. Postgres
1. Symfony 3
1. Doctrine

## Doctrine

1. В symfony c версии 1.3 (1.4)
1. Парадигма - data mapper
1. DQL и обычные SQL запросы без гидрации

## Postgres

**Расширяемая**

1. Структура данных
2. Обработка данных

## Symfony 3

**Dependency injection**

## Песочница

- Github
- Vagrant
- Docker + Compose
- PHP 7.1
- PhpStorm

## Реализация
{:.section}

## Message queue

**MQ-сервис только на базе postgres+php - пока рано**{:.slide-red.next}

### Увы, но пока нет

## Из работающего

1. {:.next}SQL
1. {:.next}NoSQL
1. {:.next}Key-Value

## Пример

1. Пользователи в SQL
2. Кешируемый список
3. Сообщения в JSON

## Конфигурация

1. Доступ
1. Схема данных

## Doctrine инструменты

- Схема данных
- Миграции
- Фикстуры

## Работа с разными типами данных

**Каждому типу данных - свое соединение**

**Каждому типу данных - свой маппинг**

**Каждому типу данных - свои миграции**

## Реализация соединения
{:.fullscreen}
```yaml
doctrine:
    dbal:
        default_connection: orm
        connections:
            keyvalue:
                host:     "%keyvalue_host%"
                port:     "%keyvalue_port%"
            orm:
                host:     "%orm_host%"
                port:     "%orm_port%"
```

## Реализация маппинга
{:.fullscreen}
```yaml
doctrine:
    orm:
        entity_managers:
            keyvalue:
                connection: keyvalue
                mappings:
                    Keyvalue:
                        dir: src/AppBundle/Entity/Keyvalue
            orm:
                connection: orm
                mappings:
                    Orm:
                        dir: src/AppBundle/Entity/Orm
```

## Тип данных Orm
{:.fullscreen}
```php
<?php

/** @ORM\Entity @ORM\Table */
class User implements UserInterface, Serializable
{
    /**
     * @ORM\Id @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

    /** @ORM\Column(unique=true) */
    protected $username;

    /** @ORM\Column(unique=true) */
    protected $token;
}
```

## Тип данных Odm
{:.fullscreen}
```php
<?php

/** @ORM\Entity @ORM\Table */
class Message
{
    /** @ORM\Id @ORM\GeneratedValue(strategy="AUTO") */
    protected $id;

    /** @ORM\Column(type="json_document", options={"jsonb": true}) */
    protected $body;
}
```

## Провайдер данных Orm + Key-Value
{:.fullscreen}
```php
<?php

class UserProvider {
    /** @var EntityManager */
    private $entityManager;

    /** @var KeyvalueProvider */
    private $cacheProvider;

    public function __construct(
        EntityManager $entityManager,
        KeyvalueProvider $cacheProvider
    ) {
        $this->entityManager = $entityManager;
        $this->cacheProvider = $cacheProvider;
    }
}

```

## Провайдер данных Orm + Key-Value
{:.fullscreen.pre-small}
```php
<?php
class UserProvider {
    public function findAll(): array {
        $find = function () {
            return $this->entityManager
                ->getRepository(User::class)
                ->findAll();
        };
        $serialize = function (array $array) {
            $result = [];
            foreach ($array as $item) {
                $result[] = serialize($item);
            }
            return $result;
        };
        $unserialize = function (array $array) {
            $result = [];
            foreach ($array as $item) {
                $result[] = unserialize($item);
            }
            return $result;
        };
        return $this->cacheProvider
            ->getOrRefresh('users', $find, $serialize, $unserialize);
    }
}
```

## Заключение
{:.section}

## Замечания

1. {:.next}Предсказуемо работает только ORM
1. {:.next}Проверенное временем только ORM
1. {:.next}Путаница в параметрах при работе с несколькими Entity Manager

## В основе все равно остаётся схема
{:.blockquote}

## Что имеем

**традиционные sql данные**

**nosql - в формате jsonb**

**cache - в формате blob**

## Вопросы?
{:.contacts}

{% if site.author %}

<figure markdown="1">

### {{ site.author.name }}

{% if site.author.position %}
{{ site.author.position }}
{% endif %}

</figure>

{% endif %}

{% if site.author2 %}

<figure markdown="1">

### {{ site.author2.name }}

{% if site.author2.position %}
{{ site.author2.position }}
{% endif %}

</figure>

{% endif %}

<!-- разделитель контактов -->
-------

<!-- left -->
- {:.github}hanovruslan
- {:.telegram}hanovruslan


<!-- right -->
- {:.mail}hanov.ruslan@gmail.com
- {:.twitter}@hanovruslan
- {:.facebook}hanov.ruslan
