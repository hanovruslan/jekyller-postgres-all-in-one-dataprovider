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

## Особенности современного приложения

**Разные виды данных**

**Eventual consistency**

**Несколько видов баз данных**

## Типы данных

- {:.next}Структурированные данные со схемой
- {:.next}Структурированные данные без схемы
- {:.next}Произвольные данные

## Поток данных

- {:.next}Перекладываем байтики
- {:.next}Порождаем события

## Типы хранилищ

- {:.next}RDBMS
- {:.next}NoSQL
- {:.next}Key-Value
- {:.next}MQ

## Доступ к данным

1. Хранилище
1. Структура

## Инструменты

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

## Реализация соединения

**Каждому типу данных - свое соединение**

**Каждому типу данных - свой маппинг**

**Каждому типу данных - свой миграции**

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
            odm:
                host:     "%odm_host%"
                port:     "%odm_port%"
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
            odm:
                connection: odm
                mappings:
                    Odm:
                        dir: src/AppBundle/Entity/Odm
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

/**
 * @ORM\Entity @ORM\Table(name="uzer")
 */
class Uzer implements UserInterface, Serializable
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
