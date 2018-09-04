# 1. Flask Configurations

**Instalación:**
	
	$ pip install flask

### Handling basic configurations
En **Flask** la configuración se realiza mediante el atributo _config_ del objeto _Flask_.
El atributo _config_ es una subclase del tipo diccionario y puede ser configurado como uno.

En caso de manejar la aplicación como un paquete:

- Estructura:
	my_app/
		app.py
		config.py
		__init__.py
	 	static/
	 
Los siguientes ejmplos de configuracion van en el archivo __init__.py:

	app = Flask(__name__)
	
> Desde un achivo de configuración de python (\*.cfg): 

	app.config.from_pyfile('myconfig.cfg')

> Desde un objeto:

	app.config.from_object('myapplication.default_settings')

Como alternativa se puede usar los siguiente para cargar las configuraciones desde el mismo
archivo:

	app.config.from_object(__name__)

> O desde una variable de entorno:

	app.config.from_envvar('PATH_TO_CONFIG_FILE')

Flask solo escoge las variables de configuración que están escritas en mayúsculas. Esto permite
definir variables locales en nuestros archivos/objetos y dejar el resto Flask.

#### Configuraciones basadas en clases (Class-based settings)

Es la mejor forma de manejar las configuraciones para mantener el control a medida que la aplicación
crece. Permite configurar todos los ambientes de desarrollo: development, staging, production, etc.

> Como:

	class BaseConfig(object):
		'Base config class'
		SECRET_KEY = 'A random secret key'
		DEBUG = True
		TESTING = False
		NEW_CONFIG_VARIABLE = 'my value'
	
	class ProductionConfig(BaseConfig):
		'Production specific config'
		DEBUG = False
		SECRET_KEY = open('/path/to/secret/file').read()
	
	class StagingConfig(BaseConfig):
		'Staging specific config'
		DEBUG = True
	
	class DevelopmentConfig(BaseConfig):
		'Development environment specific config'
		DEBUG = True
		TESTING = True
		SECRET_KEY = 'Another random secret key'

En el ambiente de producción la _secre key_ debe estar en un archivo separado por motivos de seguridad.
Este no debe ser parte del sistema de control de versiones.

Ejemplo:

	app.config.from_object('configuration.DevelopmentConfig')

