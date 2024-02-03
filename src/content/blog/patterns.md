---
author: Vladimir Avakumov
pubDatetime: 2023-09-23T15:22:00Z
modDatetime: 2023-12-21T09:12:47.400Z
title: Паттерны программирования 
slug: patterns 
featured: true
draft: false
tags:
  - паттерны 
description: Рассматриваем основные паттерны программирования.
---

Рассмотрим основные паттерны программирования
## Content 


## Singleton (Одиночка)

Синглетон - это шаблон программирования, который позволяет создать объект только один раз и обеспечивает доступ к этому объекту из любой части программы. Этот объект будет представлять собой единственный экземпляр класса, и при обращении к нему будет возвращаться именно этот экземпляр. Синглетон позволяет гарантировать наличие только одного экземпляра класса, что может быть полезно, например, при работе с различными ресурсами или приложениями, где нужен только один экземпляр определенного объекта.

```typescript
class Singleton {
  private static instance: Singleton;

  private constructor() { }

  public static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }

  public doSomething(): void {
    console.log("Doing something...");
  }
}

// Использование:

const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();

singleton1.doSomething(); // Вывод: Doing something...
singleton2.doSomething(); // Вывод: Doing something...

console.log(singleton1 === singleton2); // Вывод: true
```

## Factory Method (Фабричный метод)
## Abstract Factory (Абстрактная фабрика)
## Builder (Строитель)
## Prototype (Прототип)
## Adapter (Адаптер)
## Bridge (Мост)
## Composite (Композиция)
## Decorator (Декоратор)
## Facade (Фасад)
## Flyweight (Легковес)
## Proxy (Заместитель)
## Chain of Responsibility (Цепочка обязанностей)
## Command (Команда)
## Iterator (Итератор)
## Mediator (Посредник)
## Memento (Хранитель)
## Observer (Наблюдатель)
## State (Состояние)
## Strategy (Стратегия)
## Template Method (Шаблонный метод)
## Visitor (Посетитель)
## Null Object (Пустой объект)
## Composite Entity (Составной объект)
## Front Controller (Контроллер представления)
## Intercepting Filter (Перехватчик фильтра)
## Data Access Object (Объект доступа к данным)
## MVC (Модель-Вид-Контроллер)
## MVP (Модель-Представление-Представитель)
## MVVM (Модель-Представление-Представление-Модель)
## Dependency Injection (Внедрение зависимостей)
## Inversion of Control (Инверсия управления)
## Первый паттерн
