---
author: Vladimir Avakumov
pubDatetime: 2023-09-23T15:22:00Z
modDatetime: 2023-12-21T09:12:47.400Z
title: Паттерны программирования 
slug: patterns 
featured: true
draft: true 
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

Фабричный метод — это паттерн проектирования, который предоставляет интерфейс для создания объектов, но позволяет субклассам выбрать класс создаваемого объекта. Суть паттерна заключается в том, чтобы абстрагировать создание объектов от клиентского кода.

Вместо того, чтобы явно создавать объекты в клиентском коде, мы создаем фабричный метод в абстрактном классе или интерфейсе. Конкретные классы-создатели, которые наследуются от абстрактного класса, реализуют фабричный метод и определяют, какой конкретный класс объекта будет создан.

Таким образом, фабричный метод позволяет сделать создание объектов более гибким и адаптивным, так как клиентский код не зависит от конкретного класса создаваемого объекта, а только от интерфейса или абстрактного класса создателя.

Паттерн фабричный метод часто используется в тех случаях, когда требуется создание объектов с разными параметрами или конфигурациями, а также при необходимости добавления новых классов-продуктов без изменения клиентского кода.

```typescript
// Абстрактный класс Водитель
abstract class Driver {
  abstract getInfo(): string;
}

// Конкретный класс Водитель Грузовика
class TruckDriver extends Driver {
  getInfo(): string {
    return "Я водитель грузовика";
  }
}

// Конкретный класс Водитель Автобуса
class BusDriver extends Driver {
  getInfo(): string {
    return "Я водитель автобуса";
  }
}

// Абстрактный класс Компания
abstract class Company {
  abstract createDriver(): Driver;

  // Главный метод, который вызывает фабричный метод для создания объекта Водитель
  announceDriver(): void {
    const driver = this.createDriver();
    console.log(driver.getInfo());
  }
}

// Конкретный класс Грузовая Компания
class TruckingCompany extends Company {
  createDriver(): Driver {
    return new TruckDriver();
  }
}

// Конкретный класс Автобусная Компания
class BusCompany extends Company {
  createDriver(): Driver {
    return new BusDriver();
  }
}

// Создание объектов компаний
const truckingCompany = new TruckingCompany();
const busCompany = new BusCompany();

// Создание объектов Водителя
const truckDriver = truckingCompany.createDriver();
const busDriver = busCompany.createDriver();

// Вызов метода announceDriver() для объявления информации о Водителе
truckingCompany.announceDriver(); // Output: Я водитель грузовика
busCompany.announceDriver(); // Output: Я водитель автобуса
```

В данном примере классы `Driver`, `TruckDriver`, `BusDriver` представляют различных водителей, которые могут быть созданы. Абстрактный класс `Company` содержит абстрактный метод `createDriver()`, который должен быть реализован в каждом конкретном классе компании, чтобы создавать своих собственных водителей. Метод `announceDriver()` объявляет информацию о созданном водителе, вызывая метод `createDriver()` и выводя информацию о водителе с помощью метода `getInfo()`.


## Abstract Factory (Абстрактная фабрика)

```typescript
// Абстрактные классы для создания продуктов
abstract class AbstractProductA {
  public abstract doSomething(): void;
}

abstract class AbstractProductB {
  public abstract doSomethingElse(): void;
}

// Конкретные классы продуктов
class ConcreteProductA1 extends AbstractProductA {
  public doSomething(): void {
    console.log("Конкретный продукт A1 работает.");
  }
}

class ConcreteProductA2 extends AbstractProductA {
  public doSomething(): void {
    console.log("Конкретный продукт A2 работает.");
  }
}

class ConcreteProductB1 extends AbstractProductB {
  public doSomethingElse(): void {
    console.log("Конкретный продукт B1 работает.");
  }
}

class ConcreteProductB2 extends AbstractProductB {
  public doSomethingElse(): void {
    console.log("Конкретный продукт B2 работает.");
  }
}

// Абстрактная фабрика
abstract class AbstractFactory {
  public abstract createProductA(): AbstractProductA;
  public abstract createProductB(): AbstractProductB;
}

// Конкретная фабрика
class ConcreteFactory1 extends AbstractFactory {
  public createProductA(): AbstractProductA {
    return new ConcreteProductA1();
  }

  public createProductB(): AbstractProductB {
    return new ConcreteProductB1();
  }
}

class ConcreteFactory2 extends AbstractFactory {
  public createProductA(): AbstractProductA {
    return new ConcreteProductA2();
  }

  public createProductB(): AbstractProductB {
    return new ConcreteProductB2();
  }
}

// Использование
const factory1: AbstractFactory = new ConcreteFactory1();
const productA1: AbstractProductA = factory1.createProductA();
const productB1: AbstractProductB = factory1.createProductB();

productA1.doSomething(); // Вывод: "Конкретный продукт A1 работает."
productB1.doSomethingElse(); // Вывод: "Конкретный продукт B1 работает."

const factory2: AbstractFactory = new ConcreteFactory2();
const productA2: AbstractProductA = factory2.createProductA();
const productB2: AbstractProductB = factory2.createProductB();

productA2.doSomething(); // Вывод: "Конкретный продукт A2 работает."
productB2.doSomethingElse(); // Вывод: "Конкретный продукт B2 работает."
```

Это простой пример абстрактной фабрики, в котором `AbstractFactory` представляет абстрактную фабрику, `AbstractProductA` и `AbstractProductB` представляют абстрактные классы для создания продуктов, а `ConcreteFactory1` и `ConcreteFactory2` - конкретные фабрики, реализующие методы создания конкретных продуктов `ConcreteProductA1`, `ConcreteProductA2`, `ConcreteProductB1` и `ConcreteProductB2`.


## Builder (Строитель)

```typescript
class House {
  private floors: number;
  private bedrooms: number;
  private bathrooms: number;

  constructor(floors: number, bedrooms: number, bathrooms: number) {
    this.floors = floors;
    this.bedrooms = bedrooms;
    this.bathrooms = bathrooms;
  }

  public getInfo(): void {
    console.log(`This house has ${this.floors} floor(s), ${this.bedrooms} bedroom(s), and ${this.bathrooms} bathroom(s).`);
  }
}

class HouseBuilder {
  private floors: number;
  private bedrooms: number;
  private bathrooms: number;

  public setFloors(floors: number): HouseBuilder {
    this.floors = floors;
    return this;
  }

  public setBedrooms(bedrooms: number): HouseBuilder {
    this.bedrooms = bedrooms;
    return this;
  }

  public setBathrooms(bathrooms: number): HouseBuilder {
    this.bathrooms = bathrooms;
    return this;
  }

  public build(): House {
    return new House(this.floors, this.bedrooms, this.bathrooms);
  }
}

// Использование паттерна "Строитель"
const house1 = new HouseBuilder()
  .setFloors(2)
  .setBedrooms(3)
  .setBathrooms(2)
  .build();

house1.getInfo(); // Output: This house has 2 floor(s), 3 bedroom(s), and 2 bathroom(s).

const house2 = new HouseBuilder()
  .setFloors(1)
  .setBedrooms(2)
  .setBathrooms(1)
  .build();

house2.getInfo(); // Output: This house has 1 floor(s), 2 bedroom(s), and 1 bathroom(s).
```

В этом примере `House` - это класс, представляющий строение дома, а `HouseBuilder` - класс, который отвечает за пошаговое создание экземпляра `House`. Каждый метод `setX` устанавливает соответствующее значение (количество этажей, спален и ванных комнат) и возвращает самого себя, чтобы можно было вызывать методы цепочкой. Метод `build` создает и возвращает готовый объект `House` на основе значений, установленных в `HouseBuilder`. Затем можно использовать созданный объект `House` для выполнения различных операций, в данном случае вывода информации о доме с помощью метода `getInfo`.


## Prototype (Прототип)

```typescript
class Prototype {
  public property1: string;
  public property2: number;

  public clone(): this {
    const clone = Object.create(this);
    clone.property1 = this.property1;
    clone.property2 = this.property2;
    return clone;
  }
}

// Использование

const prototype = new Prototype();
prototype.property1 = "Property 1";
prototype.property2 = 123;

const clone = prototype.clone();
console.log(clone.property1); // Output: Property 1
console.log(clone.property2); // Output: 123
```

В приведенном примере класс `Prototype` имеет два свойства: `property1` (тип `string`) и `property2` (тип `number`). Метод `clone()` создает и возвращает копию объекта `Prototype` с теми же значениями свойств.

Для использования прототипа, мы создаем объект `prototype` с определенными значениями свойств, а затем создаем его клон `clone`. Мы можем проверить, что свойства клонированного объекта имеют те же значения, что и у исходного объекта `prototype`.

## Adapter (Адаптер)

```typescript
// Целевой интерфейс
interface MediaPlayer {
  play(audioType: string, fileName: string): void;
}

// Адаптируемый класс
class AdvancedMediaPlayer {
  playVlc(fileName: string): void {
    console.log("Playing vlc file: " + fileName);
  }

  playMp4(fileName: string): void {
    console.log("Playing mp4 file: " + fileName);
  }
}

// Адаптер
class MediaAdapter implements MediaPlayer {
  advancedMusicPlayer: AdvancedMediaPlayer;

  constructor(audioType: string) {
    if (audioType === "vlc") {
      this.advancedMusicPlayer = new AdvancedMediaPlayer();
    } else if (audioType === "mp4") {
      this.advancedMusicPlayer = new AdvancedMediaPlayer();
    }
  }

  play(audioType: string, fileName: string): void {
    if (audioType === "vlc") {
      this.advancedMusicPlayer.playVlc(fileName);
    } else if (audioType === "mp4") {
      this.advancedMusicPlayer.playMp4(fileName);
    }
  }
}

// Клиентский код
class AudioPlayer implements MediaPlayer {
  mediaAdapter: MediaAdapter;

  play(audioType: string, fileName: string): void {
    // Поддерживаемые типы файлов
    if (audioType === "mp3") {
      console.log("Playing mp3 file: " + fileName);
    } else if (audioType === "vlc" || audioType === "mp4") {
      this.mediaAdapter = new MediaAdapter(audioType);
      this.mediaAdapter.play(audioType, fileName);
    } else {
      console.log("Invalid media. " + audioType + " format not supported");
    }
  }
}

// Использование
const audioPlayer = new AudioPlayer();

audioPlayer.play("mp3", "song.mp3");
audioPlayer.play("vlc", "song.vlc");
audioPlayer.play("mp4", "song.mp4");
audioPlayer.play("avi", "song.avi");
```

В этом примере у нас есть интерфейс `MediaPlayer`, который определяет метод `play` для воспроизведения аудиофайлов. Затем у нас есть класс `AdvancedMediaPlayer`, который является адаптируемым классом и предоставляет функционал для воспроизведения файлов в разных форматах (`vlc` и `mp4`).

Затем у нас есть класс `MediaAdapter`, который реализует интерфейс `MediaPlayer` и адаптирует функционал класса `AdvancedMediaPlayer`. Он принимает тип аудиофайла в конструкторе и соответствующим образом создает экземпляр `AdvancedMediaPlayer`. Метод `play` адаптирует вызовы методов `playVlc` и `playMp4` класса `AdvancedMediaPlayer`.

Наконец, у нас есть класс `AudioPlayer`, который реализует интерфейс `MediaPlayer`. Он принимает тип аудиофайла и имя файла в методе `play`. Если тип файла - `mp3`, он воспроизводит его напрямую. Если тип файла - `vlc` или `mp4`, он создает экземпляр `MediaAdapter` и вызывает его метод `play`.

В конечном итоге, мы можем использовать `AudioPlayer` для воспроизведения файлов разных форматов. Если формат файла не поддерживается, будет выведено сообщение об ошибке.

## Bridge (Мост)

Паттерн проектирования Bridge - это шаблон проектирования, который позволяет отделить абстракцию от реализации. Он используется для разделения интерфейса и его реализации, что позволяет легко изменять их независимо друг от друга.

```typescript

interface IAbstraction {
    doSomething(): void;
}

class ConcreteImplementation1 implements IAbstraction {
    doSomething(): void {
        console.log('ConcreteImplementation1');
    }
}

class ConcreteImplementation2 implements IAbstraction {
    doSomething(): void {
        console.log('ConcreteImplementation2');
    }
}

class Abstraction {
    private _implementation: IAbstraction;

    constructor(implementation: IAbstraction) {
        this._implementation = implementation;
    }

    doSomething(): void {
        this._implementation.doSomething();
    }
}

let abstraction1 = new Abstraction(new ConcreteImplementation1());
abstraction1.doSomething(); // выводит 'ConcreteImplementation1'

let abstraction2 = new Abstraction(new ConcreteImplementation2());
abstraction2.doSomething(); // выводит 'ConcreteImplementation2'

В этом примере мы отделяем интерфейс (IAbstraction) от его реализации (ConcreteImplementation1 и ConcreteImplementation2). Это позволяет нам легко менять реализацию, не затрагивая интерфейс.

```
## Composite (Композиция)

Паттерн проектирования Композиция - это шаблон проектирования, который позволяет создавать объекты из других объектов. Он используется для создания составных объектов, которые могут содержать другие объекты.

```typescript
class Component {
    private components: Component[];

    constructor() {
        this.components = [];
    }

    addComponent(component: Component): void {
        this.components.push(component);
    }

    removeComponent(component: Component): void {
        this.components = this.\_components.filter(c => c !== component);
    }

    getComponents(): Component[] {
        return this.components;
    }
}

class Composite extends Component {
    private components: Component[];

    constructor() {
        super();
        this.components = [];
    }

    addComponent(component: Component): void {
        super.addComponent(component);
        this.components.push(component);
    }

    removeComponent(component: Component): void {
        super.removeComponent(component);
        this.components = this.\_components.filter(c => c !== component);
    }

    getComponents(): Component[] {
        return this.components;
    }
}

let composite = new Composite();
composite.addComponent(new Component());
composite.addComponent(new Component());
composite.addComponent(new Component());

console.log(composite.getComponents()); // выводит массив из трех компонентов

```
## Decorator (Декоратор)

Паттерн проектирования Декоратор - это шаблон проектирования, который позволяет добавлять поведение к существующим классам без изменения их исходного кода. Он используется для расширения функциональности классов без изменения их исходного кода.

```typescript

interface ITarget {
    doSomething(): void;
}

class Target implements ITarget {
    doSomething(): void {
        console.log('doSomething');
    }
}

class Decorator implements ITarget {
    private target: ITarget;

    constructor(target: ITarget) {
        this.target = target;
    }

    doSomething(): void {
        console.log('before doSomething');
        this.target.doSomething();
        console.log('after doSomething');
    }
}

let target = new Target();
let decorator = new Decorator(target);
decorator.doSomething(); // выводит 'before doSomething', 'doSomething', 'after doSomething'

```
В этом примере мы создаем класс Decorator, который расширяет интерфейс ITarget и содержит ссылку на объект ITarget. Метод doSomething класса Decorator вызывает метод doSomething объекта ITarget перед и после выполнения своего собственного кода. Таким образом, мы можем добавить поведение к существующему классу без изменения его исходного кода.

## Facade (Фасад)
Паттерн проектирования Фасад - это шаблон проектирования, который позволяет скрыть сложность взаимодействия с несколькими системами или подсистемами за единым интерфейсом. Он используется для упрощения доступа к нескольким системам или подсистемам, объединяя их в один интерфейс.
```typescript

interface ISystem1 {
    doSomething1(): void;
}

class System1 implements ISystem1 {
    doSomething1(): void {
        console.log('doSomething1');
    }
}

interface ISystem2 {
    doSomething2(): void;
}

class System2 implements ISystem2 {
    doSomething2(): void {
        console.log('doSomething2');
    }
}

class Facade {
    private system1: ISystem1;
    private system2: ISystem2;

    constructor(system1: ISystem1, system2: ISystem2) {
        this.system1 = system1;
        this.system2 = system2;
    }

    doSomething(): void {
        this.system1.doSomething1();
        this.system2.doSomething2();
    }
}

let system1 = new System1();
let system2 = new System2();
let facade = new Facade(system1, system2);
facade.doSomething(); // выводит 'doSomething1', 'doSomething2'

```

В этом примере мы создаем класс Facade, который содержит ссылки на объекты ISystem1 и ISystem2. Метод doSomething класса Facade вызывает методы doSomething1 и doSomething2 объектов ISystem1 и ISystem2 соответственно. Таким образом, мы можем упростить доступ к нескольким системам или подсистемам, объединив их в один интерфейс.

## Flyweight (Легковес)

Паттерн проектирования Flyweight используется для оптимизации памяти, когда у нас есть большое количество объектов с одинаковыми свойствами. Вот пример кода на TypeScript, который демонстрирует использование паттерна Flyweight для конкретной программной задачи:
```typescript

class User {
    constructor(public name: string, public age: number) { }
}

class UserFlyweight {
    private static users: Map<string, User> = new Map();

    static getUser(name: string, age: number): User {
        let user = this.users.get(name);
        if (!user) {
            user = new User(name, age);
            this.users.set(name, user);
        }
        return user;
    }
}

let user1 = UserFlyweight.getUser('John', 25);
let user2 = UserFlyweight.getUser('John', 25);

console.log(user1 === user2); // true

```



В этом примере мы создаем класс User, который представляет пользователя. Затем мы создаем класс UserFlyweight, который использует паттерн Flyweight для оптимизации памяти. Когда мы вызываем метод getUser с именем пользователя и его возрастом, он проверяет, есть ли уже пользователь с таким именем в Map. Если такого пользователя нет, он создает нового пользователя и добавляет его в Map. Если пользователь уже существует, он просто возвращает его. Таким образом, мы используем один экземпляр User для всех пользователей с одинаковыми именами и возрастом, что позволяет нам оптимизировать использование памяти.

## Proxy (Заместитель)

Паттерн Proxy - это структурный паттерн проектирования, который позволяет создать объект, который контролирует доступ к другому объекту, называемому целевым объектом. Паттерн Proxy используется для добавления дополнительной логики или функциональности к целевому объекту, не изменяя его исходный код. Он может быть использован для защиты целевого объекта, мониторинга его использования, добавления кэширования или других оптимизаций.

```typescript

class User {
    constructor(public name: string, public age: number) { }
}

let user = new User('John', 25);

let proxy = new Proxy(user, {
    get: function(target, property, receiver) {
        console.log('Getting property: ' + property);
        return Reflect.get(target, property, receiver);
    },
    set: function(target, property, value, receiver) {
        console.log('Setting property: ' + property + ' to ' + value);
        return Reflect.set(target, property, value, receiver);
    }
});

proxy.name; // Выводит: "Getting property: name"
proxy.age; // Выводит: "Getting property: age"
proxy.name = 'Jane'; // Выводит: "Setting property: name to Jane"
```

## Chain of Responsibility (Цепочка обязанностей)

Паттерн Chain of Responsibility (Цепочка обязанностей) - это структурный паттерн проектирования, который позволяет передать ответственность за обработку события от одного объекта к другому. Этот паттерн используется, когда у нас есть несколько объектов, которые могут обрабатывать одно и то же событие, и мы хотим определить порядок, в котором эти объекты будут обрабатывать событие.

Паттерн Chain of Responsibility состоит из следующих компонентов:

1. Коллбэк (Handler) - это объект, который может обрабатывать событие.
2. Цепочка (Chain) - это последовательность коллбеков, которые будут обрабатывать событие.
3. Источник событий (Event Source) - это объект, который генерирует события.

При использовании паттерна Chain of Responsibility, источник событий передает событие первому коллбеку в цепочке. Если этот коллбек может обработать событие, он обрабатывает его и останавливает цепочку. Если коллбек не может обработать событие, он передает его следующему коллбеку в цепочке. Этот процесс продолжается до тех пор, пока событие не будет обработано или пока не будет достигнут конец цепочки.

Паттерн Chain of Responsibility позволяет нам гибко управлять обработкой событий в нашем приложении, позволяя нам определять порядок, в котором коллбеки будут обрабатывать события, и добавлять или удалять коллбеки из цепочки в любое время.

```typescript

class Handler1 implements Handler {
    handleEvent(event: any) {
        console.log('Handler1 обработал событие: ' + event);
    }
}

class Handler2 implements Handler {
    handleEvent(event: any) {
        console.log('Handler2 обработал событие: ' + event);
    }
}

class Handler3 implements Handler {
    handleEvent(event: any) {
        console.log('Handler3 обработал событие: ' + event);
    }
}

class EventSource {
    private handlers: Handler[] = [];

    addHandler(handler: Handler) {
        this.handlers.push(handler);
    }

    removeHandler(handler: Handler) {
        this.handlers = this.handlers.filter(h => h !== handler);
    }

    fireEvent(event: any) {
        this.handlers.forEach(handler => {
            if (handler.handleEvent) {
                handler.handleEvent(event);
            }
        });
    }
}

let eventSource = new EventSource();
eventSource.addHandler(new Handler1());
eventSource.addHandler(new Handler2());
eventSource.addHandler(new Handler3());

eventSource.fireEvent('Some event');


```
## Command (Команда)

Паттерн Command (Команда) - это структурный паттерн проектирования, который позволяет отделить запрос на выполнение некоторого действия от его фактического выполнения. Этот паттерн используется, когда нам нужно сохранить или передать запрос на выполнение действия, но мы хотим отложить его выполнение до тех пор, пока оно не будет необходимо.

Паттерн Command состоит из следующих компонентов:

1. Команда (Command) - это объект, который содержит информацию о запросе на выполнение действия.
2. Исполнитель (Invoker) - это объект, который вызывает выполнение команды.
3. Исполнитель команд (Receiver) - это объект, который фактически выполняет команду.

При использовании паттерна Command, мы создаем объект Команда, который содержит информацию о запросе на выполнение действия. Затем мы передаем этот объект Команда объекту Исполнитель, который вызывает выполнение команды. Объект Исполнитель команд затем вызывает метод execute объекта Команда, который фактически выполняет действие.

Паттерн Command позволяет нам отделить логику выполнения действия от логики его запроса, что делает наше приложение более гибким и расширяемым. Мы можем сохранять команды и выполнять их позже, передавать команды между различными частями приложения и легко изменять или заменять исполнителей команд без изменения кода, который запрашивает выполнение действия.

```typescript

class Command {
    constructor(public name: string, public executor: Executor) { }

    execute() {
        this.executor.execute(this.name);
    }
}

class Executor {
    execute(commandName: string) {
        console.log('Выполняю команду: ' + commandName);
    }
}

let command = new Command('Команда 1', new Executor());
command.execute(); // Выводит: "Выполняю команду: Команда 1"


```
## Iterator (Итератор)

Паттерн Iterator (Итератор) - это структурный паттерн проектирования, который позволяет перебрать элементы коллекции без непосредственного доступа к ее внутреннему представлению. Этот паттерн используется, когда нам нужно предоставить способ перебора элементов коллекции, но мы не хотим раскрывать детали ее внутреннего представления.

Паттерн Iterator состоит из следующих компонентов:

1. Коллекция (Collection) - это объект, который содержит элементы, которые мы хотим перебрать.
2. Итератор (Iterator) - это объект, который предоставляет способ перебора элементов коллекции.
3. Метод hasNext() - это метод итератора, который проверяет, есть ли еще элементы для перебора.
4. Метод next() - это метод итератора, который возвращает следующий элемент из коллекции.

При использовании паттерна Iterator, мы создаем объект Итератор, который предоставляет методы hasNext() и next() для перебора элементов коллекции. Когда мы вызываем метод hasNext(), он проверяет, есть ли еще элементы для перебора. Когда мы вызываем метод next(), он возвращает следующий элемент из коллекции.

Паттерн Iterator позволяет нам предоставить гибкий и удобный способ перебора элементов коллекции, не раскрывая детали ее внутреннего представления. Мы можем легко изменить способ хранения элементов в коллекции, не затрагивая код, который перебирает эти элементы.

```typescript

class Collection {
    private items: any[] = [];

    add(item: any) {
        this.items.push(item);
    }

     \* [Symbol.iterator]() {
        for (let item of this.items) {
            yield item;
        }
    }
}

let collection = new Collection();
collection.add('Элемент 1');
collection.add('Элемент 2');
collection.add('Элемент 3');

for (let item of collection) {
    console.log(item); // Выводит: "Элемент 1", "Элемент 2", "Элемент 3"
}


```
## Mediator (Посредник)
Паттерн проектирования Посредник - это шаблон проектирования, который позволяет управлять взаимодействием между классами, скрывая детали реализации от этих классов. Он используется для упрощения взаимодействия между классами, обеспечивая единый интерфейс для обмена данными.

```typescript

interface ISubject {
    notifyObservers(): void;
}

class Subject implements ISubject {
    private observers: Observer[];

    constructor() {
        this.observers = [];
    }

    addObserver(observer: Observer): void {
        this.observers.push(observer);
    }

    notifyObservers(): void {
        this.observers.forEach(observer => observer.update());
    }
}

interface Observer {
    update(): void;
}

class ConcreteObserver implements Observer {
    update(): void {
        console.log('update');
    }
}

class Mediator {
    private subject: ISubject;

    constructor(subject: ISubject) {
        this.subject = subject;
    }

    notifyObservers(): void {
        this.subject.notifyObservers();
    }
}

let subject = new Subject();
let observer = new ConcreteObserver();
let mediator = new Mediator(subject);

subject.addObserver(observer);
mediator.notifyObservers(); // выводит 'update'

```
В этом примере мы создаем класс Subject, который реализует интерфейс ISubject и содержит список наблюдателей. Класс ConcreteObserver реализует интерфейс Observer и содержит метод update. Класс Mediator содержит ссылку на объект ISubject и вызывает метод notifyObservers этого объекта. Таким образом, мы можем управлять взаимодействием между классами, скрывая детали реализации от этих классов.


## Memento (Хранитель)

Паттерн проектирования Memento - это шаблон проектирования, который позволяет сохранять и восстанавливать состояние объекта. Он используется для сохранения состояния объекта в определенный момент времени, чтобы его можно было восстановить позже.
```typescript

class Memento {
    private state: any;

    constructor(state: any) {
        this.state = state;
    }

    getState(): any {
        return this.state;
    }
}

class Caretaker {
    private memento: Memento;

    setMemento(memento: Memento) {
        this.memento = memento;
    }

    getMemento(): Memento {
        return this.memento;
    }
}

class Originator {
    private state: any;
    private caretaker: Caretaker;

    constructor(caretaker: Caretaker) {
        this.caretaker = caretaker;
    }

    setState(state: any) {
        this.state = state;
    }

    getState(): any {
        return this.state;
    }

    saveState(): void {
        this.caretaker.setMemento(new Memento(this.\_state));
    }

    restoreState(): void {
        let memento = this.\_caretaker.getMemento();
        this.setState(memento.getState());
    }
}

let caretaker = new Caretaker();
let originator = new Originator(caretaker);

originator.setState({ some: 'state' });
originator.saveState();

originator.setState({ another: 'state' });
originator.restoreState(); // восстанавливает исходное состояние

```
В этом примере мы создаем классы Memento, Caretaker и Originator. Класс Memento используется для сохранения состояния объекта, класс Caretaker хранит ссылку на объект Memento, а класс Originator содержит состояние и взаимодействует с классом Caretaker для сохранения и восстановления состояния.

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
