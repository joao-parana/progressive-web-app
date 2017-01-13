# progressive-web-app/work

Instale o **bower** e o **polymer-cli** usando `npm install -g` e depois faça: 

```bash
polymer init
```

![polymer init](img/polymer-init.png)

```bash
? Which starter template would you like to use? application
info:    Running template application...
? Application name chrome-developer-channel
? Main element name show-app
? Brief description of the application Shows for web developers
   create bower.json
   create index.html
   . . . 
```

O esqueleto da aplicação é gerado. Veja o conteudo do diretório corrente.

```bash
ls -lat
```

> Entrypoint: When you generate a project using the Polymer CLI, the new project contains an entrypoint index.html, which imports and instantiates the app shell

O arquivo service-worker.js é gerado automaticamente.

```bash
touch service-worker.js
subl -a service-worker.js
subl -a index.html
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
    background-color: black;
    background-image: linear-gradient(to bottom, rgb(0, 0, 0) 0%, rgb(50, 50, 50) 50%, rgb(0, 0, 0) 100%);
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


Copying the seed element

```bash
cp ../resources/show-app.html src/show-app/show-app.html
```

The seed element contains the CSS, so we don't need to worry about writing all the CSS rules for the show-app element.


```javascript

```


Abra show-app.html no editor

```bash
subl -a src/show-app/show-app.html
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
