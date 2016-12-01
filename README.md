# sails_grunt_babel on the browser side
Este pequeño repo, tiene la intenció de ayudar a integrar ECMA6 (o ES6) en Sails JS del lado del cliente, para poder sacarle la mayoría de provecho a las nuevas características del nuevo estandar de JavaScript. Esto será posible gracias a los presets de babel.

<h2>Integra Babel, Browserify en SailsJS como una tarea de Grunt</h2>
##Primero crearemos la tarea que transcribirá (transpila) los archivos de ES6 a ES5
### Primero: 
Instala grunt-babel y los presets es2015 (se toma por hecho que ya está construido el servidor en SailsJS)
<br><code>npm install --save grunt-babel babel-preset-es2015 </code>
### Segundo:
Creamos en la ruta `task/config` un archivo llamado <i>babel.js</i>
### Tercero:

Antes que nada debemos de tener claro cuáles archivos o directorios serán los que se van a transpilar, tengamos encuenta que entre más tareas tenga que realizar nuestro servidor o más archivos tenga que procesar más se demorará en servir nuestros archivos al navegador. Por ejemplo (en mi caso) no quiero que la carpeta <i>dependencies</i> sea transcrita y solo la carpeta es6 (y sus archivos) sea transcrita, ya que generalmente en esta carpeta estarán las librerías o plugins que usaré por lo que no es código que tocaré.
<br>Escribimos el siguiente código:
```
  module.exports = function(grunt) {
  	grunt.config.set('babel', {
  		dev: {
  			options: {
  				presets:['es2015']
  			},
  			 files: [{
  	          expand: true,
  	          cwd: 'assets/js/',
  	          src: ['es6/**/*.js', '!dependencies/**/*'],
  	          dest: '.tmp/public/js/',
  	          ext: '.js'
  	         }]
  		}
  	});
  	grunt.loadNpmTasks('grunt-babel');
  };
  ```
### Cuarto:
Como se dan cuenta babel me enviará los archivos a la carpeta public, por lo que no no necesito que se copien estos archivos, entonces en el archivo <i>task/config/copy.js</i> quitamos las carpetas a transcribir.
<br>
### Quinto (registramos la tarea): 
Para este caso solo la pondré para <i>develop</i><br>
Vamos a <i>task/register/compileAssets.js</i> e insertamos antes de "copy:dev" "babel:dev", ya con esto cada vez que lancemos el servidor babel hará su trabajo, si necesitan que este trabaje con el servidor arriba y cada vez que guarden un archivo, copian "babel:dev" en <i>syncAssets</i>

## Nota:
* Si tienen curiosidad y no utilizan Jade, ni CoffeScript, ni otras tareas pueden quitarlas de los archivos que estan en la carpeta register (esto reducirá la carga de su servidor).
* Si quieren utilizar ES6 en SailsJS del lado del servidor deben instalar sails-hook-babel `npm i -S sails-hook-babel`
