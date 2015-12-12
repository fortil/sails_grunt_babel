# sails_grunt_babel in browser
Integrar las características de ecmascript 6 (babel) en Sails JS con Grunt del lado del cliente (en el navegador)


<h2>Integra Babel, Browserify en SailsJS como una tarea de Grunt</h2>
##Primero crearemos la tarea que transcribirá los archivos de ES6 a ES5
### Primero: 
Instala grunt-babel y los presets es2015 (se toma como cierto que ya está construido el servidor en SailsJS)
<br><code>npm install --save grunt-babel babel-preset-es2015 </code>
### Segundo:
Creamos en <code>task/config</code> un archivo llamado <i>babel.js</i>
### Tercero:

Antes que nada debemos de tener claro cuáles archivos o directorios serán los que se van a transcribir, tengamos encuenta que entre más tareas tenga que realizar nuestro servidor o más archivos tenga que pasar más se demorará en servir nuestros archivos. Por ejemplo (en mi caso) no quiero que la carpeta <i>dependencies</i> sea transcrita y solo la carpeta es6 (y sus archivos) sea transcrita, ya que generalmente en esta carpeta estarán las librerías o plugins que usaré por lo que no es código que tocaré.
<br>Escribimos el siguiente código:
<pre>
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
  </pre>
### Cuarto:
Como se dan cuenta babel me enviará los archivos a la carpeta public, por lo que no no necesito que se copien estos archivos, entonces en el archivo <i>task/config/copy.js</i> quitamos las carpetas a transcribir.
<br>
### Quinto (registramos la tarea): 
Para este caso solo la pondré para <i>develop</i><br>
Vamos a <i>task/register/compileAssets.js</i> e insertamos antes de "copy:dev" "babel:dev", ya con esto cada vez que lancemos el servidor babel hará su trabajo, si necesitan que este trabaje con el servidor arriba y cada vez que guarden un archivo, copian "babel:dev" en <i>syncAssets</i>

## Nota:
* Si tienen curiosidad, no utilizan Jade ni CoffeScript, ni otras tareas pueden quitarlas de los archivos que estan en la carpeta register (esto reducirá la carga de su servidor).
* Si quieren utilizar ES6 en SailsJS (servidor) instalen sails-hook-babel.
