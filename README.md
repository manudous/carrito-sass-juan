# carrito-sass-juan
Ejemplo de un carrito de la compra
> #### `- Instalación de dependencias` 
Instalamos gulp

``$ npm install --global gulp-cli``

Creamos el package.json con

``$ npm init``

Luego instalamos las dependencias de desarrillo de gulp,sass y autoprefixer

``$ npm install --save-dev gulp gulp-autoprefixer gulp sass``
> #### `- Creación del archivo gulpfile.js` 
Creamos un archivo llamado gulpfile.js que es un configurador de gulp y en primer lugar importamos los archivos de gulp, gulp sass- y gulp autoprefixer y luego creamos un función llamada css con los pipes para que el editor transpile de sass a css. 

```
const gulp = require('gulp');
const sass = require('gulp-sass');
const autoprefixer = require('gulp-autoprefixer');

function css() {
  return gulp
    .src('scss/app.scss')
    .pipe(autoprefixer({
      overrideBrowserslist: ['last 2 versions'],
      cascade: false
    }))
    .pipe(sass({
      outputStyle: 'expanded', // nested, compact, compressed
    }))
    .pipe(gulp.dest('css'));
}

gulp.task('css', css);
```

Ejecutamos en la consola gulp css y nos generará la carpeta css con el archivo app.css que será la que lea nuestro navegador después de transpilar sass.

``$ gulp css``

#### `- Agregando un watch`

Ahora vamos a agregar una nueva función a nuestro archivo gulpfile.js para tener un watch sobre los archivos que vayamos modificando en nuestros archivos sass y en nuestro html. Como queremos que esos watch se hagan de forma paralelas para mejorar nuestro rendimiento, es de dicir de forma asíncrona. En el gulp task utiliaremos gulp.pararell

```
+ function watchFiles() {
  gulp.watch('scss/*.scss', css);
  gulp.watch('index.html');
}

gulp.task('css', css);
+ gulp.task('watch',gulp.parallel(watchFiles) );
```
En la consola par ver el watch ejecutamos

``$ gulp watch``

#### `- Estilo para el carrito`

Cuando hacemos hover sobre el carrito queremos que nos muestre el contendio de ese carrito que lo tenemos oculto hasta que no hacemos hover sobre él. Dará un salto y para solucionar esto tenemos que hacer uso de los position

```
.carrito-compras{
  position: relative;
  &:hover .contenido{
    display: block;
    position: absolute;
    right: 0;
  }
}
```

#### `- Imports con Sass`

Creamos otro archivo de sass por ejemplo para meter las variables y tiene que ser de esta forma

``/_variables.scss``

luego en el archivo principal, app.scss, debemos importar ese archivo

``import 'variables';``

#### `- Mixin`

Un objeto que crees que vas a reutilizarlo más de una vez se recomienda utilizar los mixins se crearían de esta forma.

```
@mixin boton($color){
  display: block;
    background-color: $color;
    padding: .5rem 1rem;
    margin-top: 1rem;
    text-align: center;
    color: $blanco;
    text-decoration: none;
}
```

Ahí le hemos creado un parámetro para pasarle el color y según lo necesitemos de un color u otro podríamos cambiar el color del botón. Para introducirlo en nuestro css sería 

``@include boton($terciario);``

Una técnica que se emplea para cuando se crea el elemento de 500px, por ejemplo,y se le da un padding de 100px al elemento no crecería a 600px continúa con el 500px que ocupa.

```
@mixin box-sizing($box-model) {
  box-sizing: $box-model;
}
```

y lo agreagríamos como

```
html{
  @include box-sizing(border-box);
}
*,
*:after,
*:before {
  @include box-sizing(inherit);
}
```
