# What is UI-component?

It's a **reusable** block of functionality implementing interface between user and application.
It's a module of UI - **user interface**.

* [Responsibility - user interface](DEFINITION.md#responsibility---user-interface)
* [Native HTML5 examples](DEFINITION.md#native-html5-examples)
* [Custom examples](DEFINITION.md#custom-examples)
* [Responsibility specified](DEFINITION.md#responsibility-specified)
* [API](DEFINITION.md#api)

## Responsibility - user interface

The responsibility of UI-component is to:
- provide an interface for user input
- display application output to a user

In a **web document** an UI-component occupies certain space, usually a box, for the purpose of input and/or output.

According to best coding practices UI-components should:
- encapsulate its functionality
- hide implementation details
- have limited, singular responsibility
- be reusable
- have no side-effects
- not be affected implicitly
- have only explicit dependencies
- provide a way for customisation/parametrisation instead of hardcoding
- allow extension (e.g. inheritance, composition)

## Native HTML5 examples:

* `input type=number`
* `input type=radio`
* `input type=date`
* `textarea`
* `table` / `tr` / `td` / ..
* `img`
* `video`
* `audio`

## Custom examples:

* `Dialog`
* `Panel` / `Tab`
* `InfiniteScroll`
* `Overlay`
* `Notification`
* `Tooltip`
* `DateRangePicker`
* `Menu`
* `Sticky`
* `Carousel`
* `Collapse`
* `Paginator`
* `DragDrop`
* `RichTextEditor`
* `ImageEditor`
* `MultiSelect`

## Responsibility specified

Complex UI-components are composed from simpler UI-components. Web-based UI-components are implemented in `JavaScript`, `HTML` and `CSS`. Although the boundaries between those technologies are not absolutly strict, they mainly divide into:

* `HTML` is mainly used to implement **composition** and pass **parameters** and **data** (tags, attributes, content).
* `JavaScript` is used to implement **behavior** (lifecycle, events, methods) and define **API** used in HTML.
* `CSS` is used to implement **layout** and **look**.

It's a good practice that UI-components do NOT depend on (and for sure do NOT hardcode) the following:

* REST API  *(there might be many sources of data - it's not a job of UI to assume the source)*
* DOM  *(there might be more than 1 environment - nodejs)*
* data-processing  *(it's usually not a job of UI and may be decoupled)*
* data-flow  *(it's application-level logic)*

Just like responsibility of component's JavaScript implementation should be limited, in a similar way CSS-part of responsibility should also be limited: [CSS-responsibility](CSS-RESPONSIBILITY.md)

## API

The best way of limiting the component's responsibility is to define their API to connect them to functionality, parameters or data provided from outside.

* HTML part of API are **the tag name**, **attributes**, **the content**
* JavaScript part of API are **methods**, **properties**, **events**
* CSS part of API should yet be accomplished: [CSS-api](CSS-API.md)

