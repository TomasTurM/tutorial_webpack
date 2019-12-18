# webpack
---

## Indice

- [Descripcion](#Descripcion)
  * [Conceptos](#Conceptos)
  * [Funcionamiento](#Funcionamiento)
- [Requerimientos](#Requerimientos)
- [Tutorial](#Tutorial)
  * [Instalacion y Configuracion](#Instalacion_y_Configuracion)
  * [Directorios](#Directorios)
  * [Librerias](#Librerias)
  * [Ejecucion](#Ejecucion)
  * [Manejo_de_assets](#Manejo_de_assets)

---

## Descripcion
webpack es un static module bundler para aplicaciones JavaScript

### Conceptos
    
    • Entry
        ◦ Cuando se crea un dependency graph, el punto inicial de este gráfico es conocido como un entry point. Este punto le dice a webpack donde empezar y luego sigue el gráfico para saber que bundlear. Este entry point se define en el arhcivo de configuracion del objeto webpack
    • Output
        ◦ Se define donde y como webpack hara el bundle.
    • Loaders
        ◦ Webpack trata a todos los archivos como módulos, sin embargo solo entiende JavaScript.
        ◦ Los loaders en webpack transforman los archivos (.css, .html, etc) en módulos al mismo tiempo de que son añadidos en el dependency graph.
    • Plugins
        ◦ Los plugins son usados comúnmente performando acciones y funcionalidades customizadas en las “compilaciones” o “chunks” de tus módulos bundleados. Hacen cosas que el loader no puede hacer.
        ◦ Para ser usados, tiene que ser requeridos con require() y ser aniadidos a la array de plugins. Tienen que ser instanciados cada uno, ya que pueden usarse múltiples veces en el config.

    • module bundle: Herramienta toma piezas de JavaScript y sus dependencias y las junta en un archivo, usualmente para el su uso en el browser.

    • assets: Comprende a cualquier archivo necesario para el funcionamiento de una página web, cargados vía script o tags de script del archivo HTML.  Ejemplos: scripts, contenido de texto, gráficos, fotografías, videos, audios y bases de datos

    • CLI: Command Line Interface. Con webpack-cli se puede especificar reglas de loader, plugins, resolve options, etc... 

    • lodash: Libreria JavaScript

    • scripts dentro de package.json: Podemos referenciar a paquetes npm instalados localmente por nombre

    • npx: Herramienta de npm que facilita el uso de CLI tools y otros ejecutables hosteados en el registro de npm

### Funcionamiento
Cuando webpack procesa una aplicación, construye internamente un dependency graph el cual mapea todos los módulos que el proyecto necesita y genera uno o más bundles.

#### Proceso de module bundler
• Empieza con un entry file, “app.js” por ejemplo, y de ahí empieza a “atar” todo el código necesario para ese entry file.

• Hay dos escenarios principales de un bundler:
1. Resolución de Dependencias (Dependency resolution)

    Empezando por el entry point, el objetivo de este escenario es de buscar todas las dependencias del código y construye un gráfico (dependency graph)
    
2. Packing

    Luego de la Resolucion de Dependencias, se convierte el dependency graph generado en un simple archivo que la aplicación puede usar

## Requerimientos

• npm

• Node.js

• webpack (Modulo node)

## Tutorial

### Instalacion_y_Configuracion

Primero creamos una carpeta en donde estara la app

```
mkdir webpack-demo
cd webpack-demo
```

Realizamos un *npm init* e instalamos webpack
```
npm init -y
```

Esto creara un *package.json* en la carpeta
```
# package.json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Luego, instalamos el modulo webpack

```
npm install webpack-cli
```

Con un editor de texto, modificamos el *package.json* generado (En mi caso, usare *nano* para modificar los archivos del tutorial)
```
nano package.json
```

Hacemos dos modificaciones:

1. Borramos la linea que tiene el parametro "main" y agregamos "private" para prevenir una publicacion inesperada del proyecto

2. En la seccion *"scripts"* creamos un metodo *"build"* que advierte a webpack la nueva configuracion escrita en el archivo *webpack.config.js*, el cual usaremos mas adelante

```
# package.json
	...
+	"private": true,
-	"main": "index.html",
	"scripts": {
		"build": "npx webpack --config webpack.config.js"
	}
	...
```


### Directorios

Ahora, nececitamos crear un directorio llamado *"dist"*. En este, estara el *output* de webpack o mejor dicho el codigo optimizado de nuestro proyecto
```
mkdir dist
touch dist/index.html
```
Luego, modificaremos *index.html* 

```
nano dist/index.html
```

```
# dist/index.html

<!doctype html>
<html>
  <head>
    <title>Getting Started</title>
  </head>
  <body>
    <script src="main.js"></script>
  </body>
</html>
```


Otro direcctorio sera *"src"*, el cual alberga el codigo que podremos escribir y editar
```
mkdir src
touch src/index.js
```

### Librerias

Para mostrar el funcionamiento de librerias junto a webpack, utilizaremos *lodash*
```
npm install lodash
```

Luego de instalarlo, lo utilizaremos en el index.js
```
nano src/index.js
```
```
# src/index.js

import _ from 'lodash';

function component() {
  const element = document.createElement('div');

  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

Este archvio JavaScript crea un componente dentro de la pagina HTML que tiene el texto 'Hello webpack'

### Ejecucion

Ahora, generaremos el *main.js* corriendo el modulo node de webpack
```
npx webpack
```

Pero webpack no sabe cual es el *entry point* y el *output* del proyecto, por lo que no creeara el main.js

Por eso, debemos crear y editar un archivo de configuracion para webpack
```
touch webpack.config.js
nano webpack.config.js
```
```
# webpack.config.js

const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: path.resolve(__dirname, 'dist')
    }
};
```

Con esto hecho, tendremos que buildear un nuevo archivo de configuracion para webpack con el comando anteriormente creado en el *package.json*

```
npm run build
```

webpack creara el *main.js* en la carpeat *dist*. Asi, ya tendremos el bundle creado y funcionando

Para testear el bundle, abrimos el archivo *dist/index.html*

### Manejo_de_assets

webpack puede manejar *assets* dentro de una app, pero se necesita una configuracion extra para poder hacer un *bundle* con *assets*

Instalamos los siguientes modulos
```
npm install style-loader css-loader
```

Editamos el siguiente archivo con un editor de texto
```
nano webpack.config.js
```

Agregamos la seccion *module*

```
# webpack.config.js

const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: path.resolve(__dirname, 'dist')
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
};
```

Creamos un archivo CSS dentro de la carpeta *src/*
```
touch src/style.css
nano src/style.css
```
```
# src/style.css

.hello {
    color: red;
}
```

Luego, importamos y usamos el CSS dentro del index.js
```
nano src/index.js
```
```
# src/index.js

import _ from 'lodash';
import './style.css';

function component() {
  const element = document.createElement('div');

  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  element.classList.add('hello');

  return element;
}

document.body.appendChild(component());
```

Y buildeamos
```
npm run build
```

Si funciona, veremos un *Hello webpack* en color rojo, mostrando de que hemos usado webpack adecuadamente

