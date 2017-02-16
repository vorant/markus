Markus
========================

**Markus** - this is set of gulp tasks and watchers for converting psd to html using SwigJS template engine.

Goal:
------------

The main goal of `Markus`is creating the fastest html integration with Symphany. Decrease time for replacing and editing templates by means of using Symphany methodology, SwigJS template engine and gorgeouse Gulp bundler.

Project structure:
------------

В проекте три основных директории: `web`, `markup` и `gulp`. Подробнее о каждой из папок ниже.

There are three main directories: `web`, `markup` and `gulp`. Description of each one is below

Web:
------------
Web folder contains all project static files: 
- `components` - bower components;
- `less` - less resources;
- `css`  - css compiled files;
- `js` - js compiled files;
- `images|videos` - all video and imeges assets;
- `fonts` - all fonts;
- `plugins` - all custom vendors;
- `*.html` - html compiled files.

Markup:
------------

murkup folder contains 3 subfolders:
- models – keeps instances files,
- views - keeps templates files, 
- controllers - keep configurations for views and models,

Is uses popular programming pattern - [Model-View-Controller]. 

***


Models - contains all instances project's files. We strongly reccomend to create separate file per each instance.

For example: Person instance looks like:

```shell
 function Person(id, name, isActive, image) {
    this.id = id;
    this.name = name;
    this.isActive = isActive;
    this.image = image;
}

module.exports.persons = [
    new Person(1, 'John',  true,  'images/persons/person1.jpg'),
    new Person(2, 'Max',   false, 'images/persons/person2.jpg'),
    new Person(3, 'Igor',  true,  'images/persons/person3.jpg'),
    new Person(4, 'Semen', false, 'images/persons/person4.jpg')
];
``` 

***

Views - contains html templates (modules/blocks/widgets).

Base of html templates is [SwigJS] template engine, which is similar with Twig template engine uses at Symphany by default. Full documentation is here [SwigJS/doc]

We create base template per each page where are header and footer as usual. We call it base.html.twig (.html postfix is'n necessary but it needs for projects, which will be integrated with Symphany). Base template is at root directory of each templates and is bone for folloving pages 

Per common blocks which are used on several pages we create at `markup/views/common` folder. Inserting of such blocks by means of `include` tag. For example:

```shell
{% include 'common/navbar.html.twig' %}
``` 
Important: folders path is determined regarding root template folder `markup/views`. That is to include block `common/test.html.twig` at custom component `news/my/custom/folder/page_news.html.twig` include path is the same 

```shell
{% include 'common/test.html.twig' %}
``` 

All external files includes by asset function, which generates right paths to resources

```shell
<link rel="stylesheet" href="{{ asset('components/bootstrap/dist/css/bootstrap.min.css') }}">
<script src="{{ asset('components/jquery/dist/jquery.min.js') }}"></script>
<script src="{{ asset('components/bootstrap/dist/js/bootstrap.min.js') }}"></script>
``` 

***

Controllers - contains compiled files configuration.

For example Person controller:

```shell
exports.person = {
    list: {
        alias: 'persons',
        template: 'person/list.html.twig',
        models: ['person']
    },
    show: {
        alias: 'person-show',
        template: 'person/show.html.twig',
        models: ['person']
    }
};
``` 
 
This controlles containes two actions `list` and `show`.

Action `list` has following configuration:
- `alias` - file name
- `template` - template, will be used by compilation
- `models` - list of instances 

Result of  `person` template compilation is two files `persons.html` and `person-show.html` at `web` folder.

Each controller action has unic route, it looks like  `controller_action`.

For paths generation might be used `path` function with route name:
For example: `Person` controller and `show` action:

```shell
<a href="{{ path('person_show') }}" class="link">Link</a>
```


Gulp:
------------

Gulp contains all bundlers tasks and watchers. All tasks are easy to understand:

- `css` - all task for compiling CSS;
- `html` - all task for compiling HTML templates;
- `generator` - all task for generating code;
- `images` - all task for compiling images;
- `js` - all task for compiling javascript ;
- `server` - all task for compiling for developing mode.


Generators:
------------

There is task `gulp entity`, who generates controller, model and view for instance.

```shell
gulp entity --e test,res,fd --a list,show,create,edit --m id,title,body
```

Attributes:

--е - instances list, through comma

--a - actions list (not necessary, default: list,show )

--m - instance properties list (not necessary, default: id,name )


[markup-swig]: https://github.com/Fafnur/markup-swig
[Model-View-Controller]: https://ru.wikipedia.org/wiki/Model-View-Controller
[SwigJS]: http://paularmstrong.github.io/swig/
[SwigJS/doc]: http://paularmstrong.github.io/swig/docs/
