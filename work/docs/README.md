# progressive-web-app

Instale o **bower** e o **polymer-cli** usando `npm install -g` e depois faça: 

```bash
cd work
polymer init
```

![polymer init](img/polymer-init.png)

```bash
? Which starter template would you like to use? application
info:    Running template application...
? Application name first-pwa
? Main element name pwa-app
? Brief description of the application Shows for web developers My First Progressive Web App
   create bower.json
   create index.html
   . . . 
```

O esqueleto da aplicação é gerado. Veja o conteudo do diretório corrente.

```bash
ls -lat
```

> Entrypoint: When you generate a project using the Polymer CLI, the new project contains an entrypoint index.html, which imports and instantiates the app shell

O arquivo service-worker.js é gerado automaticamente quando implantamos a aplicação em produção no ultimo passo.

Por hora fazemos:

```bash
touch service-worker.js
subl .
```

Edite o index.html colocando o código abaixo no `<head>`

```javascript
<script>
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', function() {
      navigator.serviceWorker.register('/service-worker.js');
    });
  }
</script>
```

> Chrome automatically installs Roboto in the user's computer.

Adicione um estilo

```css
<style>
  body {
    margin: 0;
    background-color: rgb(255, 195, 0);
    background-image: linear-gradient(to bottom, rgb(255, 195, 0) 0%, rgb(236, 132, 14) 50%, rgb(255, 195, 0) 100%);
    background-attachment: fixed;
    font-family: 'Roboto', 'Noto', sans-serif;
  }
</style>
```

Numa **outra bash shell** faça:

```bash
cd work
polymer serve -p 3000 --open
```

To start working on the app shell you will need to install a few dependencies using bower:

```bash
bower install  --save PolymerElements/app-layout#^0.10.0 PolymerElements/app-route#^0.9.0 PolymerElements/iron-flex-layout#^1.0.0 PolymerElements/iron-pages#^1.0.0 PolymerElements/paper-icon-button#^1.0.0
```


Em vez de copiar o **seed element** vamos fazer um Merge.

```bash
# cp ../resources/show-app.html src/show-app/show-app.html
touch  src/pwa-app/pwa-app.css
# copie o código abaixo para src/pwa-app/pwa-app.css
```


```css
:host {
  --paper-icon-button-ink-color: white;
  display: block;
  padding-top: 64px;
}
app-header {
  @apply(--layout-fixed-top);
  z-index: 1;
  height: 64px;
  line-height: 64px;
  background-color: rgba(0, 0, 0, 0.85);
  border-bottom: 1px solid #222;
  -webkit-backdrop-filter: saturate(180%) blur(20px);
}
a {
  @apply(--layout-flex-auto);
  @apply(--layout-horizontal);
  @apply(--layout-center);
  @apply(--layout-center-justified);
  text-decoration: none;
  color: white;
  margin-right: 40px;
}
a:hover {
  color: #ddd;
}
.title {
  font-size: 14px;
  margin: 0;
  text-align: center;
  text-transform: uppercase;
  font-weight: 100;
  letter-spacing: 5px;
  word-spacing: 7px;
  display: inline;
  margin-left: 20px;
  margin-right: 20px;
  transition: opacity 0.2s;
  white-space: nowrap;
}
.chrome-logo {
  margin-left: 16px;
  width: 30px;
  height: 30px;
  transition: transform 0.2s ease-out;
}
#leftItem {
  min-width: 40px;
  color: white;
}
#leftItem:not([icon]) {
  visibility: hidden;
}
[threshold-triggered] .title {
  opacity: 0;
}
[threshold-triggered] .chrome-logo {
  transform: translateX(129px) rotateZ(360deg);
}
@media (max-width: 640px) {
  .title {
    font-size: 10px;
  }
  [threshold-triggered] .chrome-logo {
    transform: translateX(120px) rotateZ(360deg);
  }
}
```
No arquivo `src/pwa-app/pwa-app.html`, adicione `<link rel="import" type="css" href="pwa-app.css">` logo depois de `<dom-module id="pwa-app">`

> The seed element contains the CSS, so we don't need to worry about writing all the CSS rules for the pwa-app element.


```javascript

```


Abra pwa-app.html no editor

```bash
subl -a src/pwa-app/pwa-app.html
```

Adicione os imports

```html
<link rel="import" href="../../bower_components/app-layout/app-header/app-header.html">
<link rel="import" href="../../bower_components/app-layout/app-toolbar/app-toolbar.html">
<link rel="import" href="../../bower_components/app-route/app-location.html">
<link rel="import" href="../../bower_components/app-route/app-route.html">
<link rel="import" href="../../bower_components/iron-flex-layout/iron-flex-layout.html">
<link rel="import" href="../../bower_components/iron-pages/iron-pages.html">
<link rel="import" href="../../bower_components/paper-icon-button/paper-icon-button.html">
```

## Adding the routes

We will use app-route, an element that enables declarative, self-describing routing for a web app. To start adding the routes to our app, we insert the app-location and app-route elements to the template of the show-app element: 

Adicione o codigo abaixo no inicio do fonte `src/pwa-app/pwa-app.html` dentro do `<template>` após o código `<h2>Hello [[prop1]]</h2>`.

```html
<app-location route="{{route}}"></app-location>
<app-route
    route="{{route}}"
    pattern="/:page"
    data="{{routeData}}"
    tail="{{subroute}}"></app-route>
```

The app-location produces a route value. Then, the route.path property is matched by comparing it to the pattern property /:page. If the pattern property matches route.path, the app-route will set or update its data property with an object whose properties correspond to the parameters in pattern. For example, if the route is /video, the routeData.page property will equal video. For more about app-route, check the elements catalog.

## Adding the header

Next, we are going to start working on the header. App-layout gives us app-header and app-toolbar, which we can use to display the logo and the app name. Below the app-route line, add the following snippet:

```html
<app-header condenses reveals threshold="1">
  <app-toolbar>
    <paper-icon-button id="leftItem"></paper-icon-button>
    <a href="/" title="Developer Channel">
      <!-- svg logo -->
      <h1 class="title">Developer Channel</h1>
    </a>
  </app-toolbar>
</app-header>
```

As we can see, the app-header element has the attributes condenses and reveals which will allow the header to slide in and out as the user scrolls the main content. It also has the threshold attribute, which will allow us to animate the Chrome logo when the user has scrolled at least one pixel
