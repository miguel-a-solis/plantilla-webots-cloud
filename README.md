# Plantilla para Webots Cloud
Este repositorio se puede usar como plantilla para desplegar una simulación en [webots.cloud](https://webots.cloud). Este repositorio es un fork de [https://github.com/cyberbotics/webots-cloud-simulation-template](https://github.com/cyberbotics/webots-cloud-simulation-template).

## Estructura
La plantilla contiene un proyecto genérico con el robot e-puck.
El mundo `e-puck.wbt` está ubicado en la carpeta `worlds`.
El controlador `e-puck.py` está ubicado en la carpeta `controllers/e-puck`.
La ventana del robot `simple_e-puck` está ubicada en la carpeta `plugins/robot_windows`.

Se necesita un archivo Docker. En caso de no definir algún Dockerfile, el archivo `Dockerfile.default` ubicado en la raíz del proyecto, contiene cuatro líneas como archivo Docker por defecto para usar por [webots.cloud](https://webots.cloud).

```dockerfile
FROM cyberbotics/webots.cloud:R2022b
ARG PROJECT_PATH
RUN mkdir -p $PROJECT_PATH
COPY . $PROJECT_PATH
```

La siguiente línea adicional permite compilar el plugin cuando [webots.cloud](https://webots.cloud) es quien crea la imagen Docker.

```dockerfile
RUN cd $PROJECT_PATH/plugins/robot_windows/simple_e-puck && make clean && make
```

**Nota**: Es posible también proveer directamente los binarios en las carpetas correspondientes y usar el Dockerfile por defecto sin compilación "on-the-fly".
Sin embargo, actualmente no es posible compilar controladores en C/C++ y la ventana del robot directamente desde la simulación ejecutándose en [webots.cloud](https://webots.cloud).

`webots.yaml` define el tipo de simulación como `demo`.
El parámetro `publish` permite publicar la simulación en [webots.cloud](https://webots.cloud) y la visibiliza en la lista pública de simulaciones.
Finalmente, `dockerCompose:theia` establece el espacio de trabajo para el IDE online.
Esto significa que con `webots-project/controllers/` cada usuario que inicie sesión en la simulación podría modificar los parámetros ubicados en la carpeta `controllers`.

Más información disponible en [Webots User Guide](https://cyberbotics.com/doc/guide/webots-cloud?version=master#publish-cloud-based-simulations).
Para proyectos más complejos y otras configuraciones puede ir al repositorio de ejemplos: [webots.cloud Simulation Examples](https://github.com/cyberbotics/webots-cloud-simulation-examples).
