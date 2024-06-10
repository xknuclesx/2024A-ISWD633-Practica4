# Healthcheck
Un Healthcheck es una configuración que permite definir comandos específicos para comprobar si un contenedor está funcionando correctamente. Los Healthchecks se utilizan para monitorizar el estado de los servicios dentro de un contenedor y para que Docker pueda determinar si un contenedor está en buen estado o no.

Un Healthcheck consta de un comando que se ejecuta periódicamente dentro del contenedor. Si el comando devuelve un estado de éxito (código de salida 0), el contenedor se considera saludable. Si devuelve un estado de error (código de salida diferente de 0), el contenedor se considera no saludable.

## Configuración de un Healthcheck
### Healthcheck command
```
--health-cmd="<comando> || exit 1"
```
Define el comando que se ejecutará para verificar el estado del contenedor. Este comando depende del tipo de aplicación que se está ejecutando en el contenedor y de la mejor manera de verificar su estado de salud. 
El **|| exit 1** se utiliza para asegurarse de que el comando retorne un código de salida diferente de 0 si la verificación de salud falla. Esto es importante porque Docker interpreta un código de salida diferente de 0 como un fallo en el Healthcheck, lo que puede llevar a que Docker marque el contenedor como no saludable y tome las medidas configuradas por ejemplo reiniciar el contenedor.
Si no se incluye || exit 1, el comando debe ser cuidadosamente diseñado para devolver un código de salida diferente de 0 en caso de fallo, de lo contrario Docker puede interpretar incorrectamente el estado de salud del contenedor.

Para un servidor web (usando curl):

--health-cmd="curl -f http://localhost/ || exit 1"

Para un servidor de base de datos (usando mysqladmin):

--health-cmd="mysqladmin ping -h localhost -u root --password=rootpassword || exit 1"

Para un servicio que proporciona una API (usando wget):

--health-cmd="wget --spider http://localhost:8080/health || exit 1"

Para verificar un proceso específico:
Si el contenedor está ejecutando un proceso y quieres comprobar que está en ejecución:

--health-cmd="pgrep my_process_name || exit 1"

Para comprobar la existencia de un archivo específico:
Si tu aplicación crea un archivo al arrancar correctamente:

--health-cmd="test -f /path/to/your/file || exit 1"

Para verificar un puerto específico:
Si necesitas comprobar que un puerto está abierto:

--health-cmd="nc -z localhost 8080 || exit 1"

### Intervalo de Healthcheck 
Establece el intervalo entre las ejecuciones del comando de verificación de estado. El valor se especifica en una unidad ms para milisegundos, s para segundos, m para minutos, h para horas, y d para días. Para establecer un buen intervalo de Healthcheck es importante considerar varios factores para garantizar una monitorización efectiva sin afectar el rendimiento del sistema. 

- Frecuencia adecuada: El intervalo debe ser lo suficientemente frecuente como para detectar problemas de salud rápidamente, pero no tan frecuente como para crear una carga innecesaria en el sistema. Un intervalo de 30 segundos a 1 minuto suele ser razonable para la mayoría de las aplicaciones.

- Tiempo suficiente para recuperarse: El intervalo debe permitir que la aplicación se recupere si experimenta problemas temporales. Por ejemplo, si una aplicación tarda 20 segundos en reiniciarse después de un fallo, el intervalo debe ser mayor que 20 segundos para permitir que la aplicación se reinicie completamente antes del siguiente chequeo.

- Considerar el tiempo de timeout: Asegúrate de que el intervalo sea mayor que el tiempo de timeout (--health-timeout). Si el intervalo es menor que el tiempo de timeout, los chequeos de salud podrían acumularse y causar problemas si la aplicación no responde rápidamente.

- Ajuste según las necesidades: El intervalo puede variar según la aplicación y su criticidad. Aplicaciones críticas pueden requerir intervalos más cortos para detectar problemas rápidamente, mientras que aplicaciones menos críticas pueden funcionar bien con intervalos más largos para reducir la carga en el sistema.
```
 --health-interval = <valor><unidad>
```

### Timeout
Define el tiempo máximo que Docker esperará para que el comando de verificación de estado termine. Si el comando no termina dentro de este tiempo, se considera que ha fallado. El valor se especifica en segundos o en formato de duración.
```
--health-timeout=<valor><unidad>
```
### Retries
Especifica el número de intentos fallidos consecutivos que Docker permitirá antes de marcar el contenedor como no saludable (unhealthy).
```
--health-retries=<valor>
```
### Start period
Define un período de gracia inicial durante el cual los fallos del Healthcheck no cuentan como intentos fallidos. Esto puede ser útil para dar tiempo al servicio dentro del contenedor a que se inicie completamente antes de comenzar las verificaciones de estado. El valor se especifica en segundos s o en otra unidad similar.
```
--health-start-period=<valor><unidad>
```

### Para crear y ejecutar los siguientes contenedores usar la imagen de nginx:alpine
### Ejecutar un contenedor con un healthcheck que valide que el contenedor está funcionando

```
docker run -d --name server-nginx --health-cmd="curl http://localhost" --health-interval=3s --health-start-period=5s --health-retries=3 --health-timeout=10s nginx:alpine
```
![image](https://github.com/xknuclesx/2024A-ISWD633-Practica4/assets/120606471/d6749d5e-949a-4045-bc54-08e478fa837a)
