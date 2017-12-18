## What is UI-component?

It's a **reusable** block of functionality implementing interface between user and application.
It's a module of UI - **user interface**.

The responsibility of UI-component is to:
- provide an interface for user input
- display application output to a user

In a **web document** an UI-component occupies certain space, usually a box, for the purpose of input and/or output.

According to best coding practices UI-components should:
- encapsulate its functionality
- hide implementation details
- have no side-effects
- be reusable
- provide a way for customisation/parametrisation
- allow extension (e.g. through inheritance, composition)

## Native HTML5 examples:

* input type=number
* input type=radio
* input type=date
* textarea
* table / tr / td / ..
* img
* video
* audio

## Custom examples:

* Dialog
* Panel / Tab
* InfiniteScroll
* Overlay
* Notification
* Tooltip
* DateRangePicker
* Menu
* Sticky
* Carousel
* Collapse
* Paginator
* DragDrop
* RichTextEditor
* ImageEditor
* MultiSelect

## Responsibility specified

Complex UI-components are composed from simpler UI-components. Web-based UI-components are implemented in `JavaScript`, `HTML` and `CSS`. Although the boundaries between them are not absolutly strict, they mainly divide their responsibilities into:

* `HTML` is mainly used to implement **composition** and **parametrisation**/**data** (tags, attributes, content).
* `JavaScript` is used to implement **behavior** (lifecycle, events, passing data).
* `CSS` is used to implement **layout** and **look**.

## JavaScript responsibility (behavior)
It's a good practice that UI-components do NOT depend on (and for sure do NOT hardcode) the following:

* REST API  *(there might be many sources of data - it's not a job of UI to assume the source)*
* DOM  *(there might be more than 1 environment - nodejs)*
* data-processing  *(it's usually not a job of UI and may be decoupled)*
* data-flow  *(it's application-level logic)*

## CSS responsibility (layout, look)
Just like responsibility of component's JavaScript implementation should be limited, in a similar way CSS-part of responsibility should also be limited.







