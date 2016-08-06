#Armar pluggin

* crear un archivo .php con el nombre del pluggin y el siguiente encabezado:  

/*
Plugin Name: Portfolio by CoderHouse  
Description: Sample Portfolio plugin.  
Version:     1.0  
Author:      CoderHouse  
Author URI:  http://coderhouse.com  
License:     GPL2  
License URI: https://www.gnu.org/licenses/gpl-2.0.html  
Domain Path: /languages  
Text Domain: portfolio  
*/  

* se trata de que este archivo tenga poca información, por lo tanto refiere a otro archivo que inicia el pluggin y carga todo  

* jerarquía:  
Tiene 4 carpetas: LANGUAGES para traducciones, ADMIN, para la seccion de administracion, PUBLIC, que es lo que se ve en  el front end e INCLUDES que tiene archivos para admin y public, por ejemplo, post type.  

* Este archivo tendria que iniciar y cargar todo lo necesario:  
1. Load plugin textdomain  
2. Load files inside `includes` folder.  
3. Load files inside `admin` folder.  
4. Load files inside `public` folder.  
5. Initialize plugin functionality, si todas las funciones anteriores no se asignaron a una acción de WordPress, entonces hay que ejecutar tu propia accion:  

* definir nuevo post type, si el pluggin lo requiere (esto es algo que va tanto en admin como public, por lo tanto va en includes)

* definir meta boxes para este post type, si se requiere. esto va en admin  

* crear un menu de administracion. esto va en admin  

* y en public va la parte de templates.php que imprime el html en el front end
