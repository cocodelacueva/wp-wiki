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

### Activar y desactivar pluggin  

* registrar la ruta: define();
* register_activation_hook('ruta del archivo','funcion que se va a ejecutar para activacion');
* desinstalacion: register_uninstall_hook();
* se puede poner un archivo uninstall.php en la carpeta raiz, esta es una mejor opcion

Activation  
``` [php]
 <?php
if ( ! function_exists( 'portfolio_activation' ) ) :
register_activation_hook( PORTFOLIO_PLUGIN_MAIN_FILE, 'portfolio_activation' );
/**
 * Process plugin activation.
 *
 * @since 1.0
 */
function portfolio_activation() {
	add_option( 'portfolio_settings', array(
		'portfolio_page'         => null,
		'show_logged_only'       => true,
		'remove_on_deactivation' => false,
	) );
}
endif;
 ```
deactivation  
``` [php]
<?php
if ( ! function_exists( 'portfolio_deactivation' ) ) :
register_deactivation_hook( PORTFOLIO_PLUGIN_MAIN_FILE, 'portfolio_deactivation' );
/**
 * Process plugin deactivation.
 *
 * @since 1.0
 */
function portfolio_deactivation() {
	$settings = get_option( 'portfolio_settings' );
	if ( $settings['remove_on_deactivation'] ) {
		delete_option( 'portfolio_settings' );
	}
	flush_rewrite_rules();
}
endif;
 ```
 
 uninstall  
 ``` [php]
 <?php
if ( ! function_exists( 'portfolio_uninstall' ) ) :
register_uninstall_hook( PORTFOLIO_PLUGIN_MAIN_FILE, 'portfolio_uninstall' );
/**
 * Process plugin uninstall.
 *
 * @since 1.0
 */
function portfolio_uninstall() {
	delete_option( 'portfolio_settings' );
}
endif;
```
 
[Ver info en developer wordpress](https://developer.wordpress.org/plugins/the-basics/activation-deactivation-hooks/)

### Transient API

Tipo de datos que se guarda en la base de datos tipo cache  
[Transient API Codex](https://codex.wordpress.org/Transients_API)

### Templates

Para poder seleccionar una pagina donde mostrar el template creado
En settings se puede usar una funcion llamada wp_dropdown_pages(array());  

add_filter ('template_include', funcion);  
function portfolio_locate_template();  

