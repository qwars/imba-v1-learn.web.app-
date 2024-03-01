---
title: Custom Tags
order: 4
---

# Пользовательские теги

## Объявление тегов

Вы можете легко определить свои собственные теги / компоненты ( tag/component ) так же просто, как создавать классы. Они похожи на компоненты в React. Теги определяются с использованием ключевого слова tag:

```imba
tag App
    # Пользовательские методы экземпляра, свойства и т.д.

# Создание экземпляра приложения - так же, как и для любого другого тега.
let app = <App.main> <h1> "Hello"
```

Ваши пользовательские теги по умолчанию наследуются от `div`, но они могут наследоваться от любого другого тега. Вы также можете определять методы экземпляра для них.

```imba
# Определение пользовательского тега, наследующего от form (форма), может выглядеть следующим образом
tag RegisterForm < form
    def onsubmit event
        # Объявление обработчиков событий в виде методов
        console.log "submitted"

    def someMethod
        console.log "hello"
        self

# Создать экземпляр приложения - точно так же, как и любого другого тега.
let form = <RegisterForm>
form.someMethod # => "hello"
```

> Когда вы объявляете tag SomeComponent, вы объявляете новый *тип* тега, а не экземпляр. Это аналогично объявлению class SomeClass. <SomeComponent> создает новый *экземпляр* этого тега, точно так же, как SomeClass.new создает новый экземпляр этого класса.


## Пользовательский рендеринг

Как и в компонентах React, вы можете определить, как должны отрисовываться пользовательские теги, объявив метод render:

```imba
tag App
    def render
        <self> <h1> "Hello world"

let app = <App.main>
# DOM-дерево приложения теперь выглядит следующим образом:
# <div class='App main'><h1>Hello world</h1></div>
```

> `<self>` внутри метода render заслуживает некоторого объяснения. В Imba экземпляры тегов прямо связаны с их реальным DOM-элементом. `<self>` ссылается на сам компонент и является способом сказать "теперь я хочу, чтобы содержимое внутри `self` выглядело так, как описано ниже. Это важно для понимания".

```imba
tag Wrong
    def render
        <h1> "Hello {Math.random}"

let wrong = <Wrong>
# wrong.render просто создаст новый элемент h1
# каждый раз, когда он вызывается. DOM-элемент "wrong" будет
# по-прежнему не будет иметь дочерних элементов.

tag Right
    def render
        <self> <h1> "Hello {Math.random}"
let right = <Right>
# right.render теперь будет обновлять своё DOM-дерево каждый раз.
# его вызов, гарантируя, что DOM-структура фактически отражает изменения.
# представление, объявленное внутри `<self>`.
```


## Наследование

Пользовательские теги могут наследоваться от других пользовательских тегов или от встроенных тегов. Например, если вы хотите создать пользовательский компонент формы, вы можете просто наследоваться от тега `form`:

```imba
# Определите пользовательский тег, наследующийся от form
tag RegisterForm < form

let view = <RegisterForm>
# DOM-элемент представления теперь имеет тип "form".
# html: <form class='RegisterForm'></form>
```


## Свойства

```imba
tag App

    # Объявление пользовательских свойств может быть выполнено следующим образом:
    prop slug
    
    # Объявление свойств с значениями по умолчанию может выглядеть следующим образом:
    prop greeting default: 'Hello human!'

    def render
        <self>
            <h1> "Slug is: {slug}"
            if slug == '/home'
                <div> "{greeting} You are home"

Imba.mount <App slug='/home'>
```
