# Selenium Grid

Remote system for running distributed tests in multiple environments

## Entorno Selenium local

Para disponer del servicio en un entorno local, es posible desplegar la infraestructura mediante los siguientes comandos:

```sh
# Se requiere tener instalado Docker en el entorno local

# Crea red de intercomunicación entre los servicios
docker network create selenium-net

# Lanza el hub central de Selenium
docker run --rm -d \
  --name selenium-hub \
  --net selenium-net \
  -p 4444:4444 \
  selenium/hub:4.1.3

# Lanza un nodo de navegador Chrome
docker run --rm -d \
  --name selenium-chrome \
  --net selenium-net \
  --shm-size=2G \
  -e SE_EVENT_BUS_HOST=selenium-hub \
  -e SE_EVENT_BUS_PUBLISH_PORT=4442 \
  -e SE_EVENT_BUS_SUBSCRIBE_PORT=4443 \
  selenium/node-chrome:99.0

# Lanza un nodo de navegador Firefox
docker run --rm -d \
  --name selenium-firefox \
  --net selenium-net \
  --shm-size=2G \
  -e SE_EVENT_BUS_HOST=selenium-hub \
  -e SE_EVENT_BUS_PUBLISH_PORT=4442 \
  -e SE_EVENT_BUS_SUBSCRIBE_PORT=4443 \
  selenium/node-firefox:98.0
```

Si todo ha ido correctamente, el servicio *Selenium Hub* estará accesible en <http://localhost:4444> con 2 nodos añadidos, formando un *Selenium Grid* funcional.

Hay que prestar atención a los tags desplegados para cada imagen. En el ejemplo, se usan:

* `selenium/hub:4.1.3` (versión **4.1.3** de **Selenium Hub**, ver más en <https://hub.docker.com/r/selenium/hub>).
* `selenium/node-chrome:99.0` (versión **99.0** de **Google Chrome**, ver más en <https://hub.docker.com/r/selenium/node-chrome>).
* `selenium/node-firefox:98.0` (versión **98.0** de **Mozilla Firefox**, ver más en <https://hub.docker.com/r/selenium/node-firefox>).

Existen otras etiquetas más específicas (consultar en los enlaces anteriores) si se quiere fijar con más certeza las versiones usadas, al igual que imágenes para otros navegadores (disponibles en <https://hub.docker.com/u/selenium>). También hay disponibles multitud de opciones para configurar el entorno de testeo, consultar documentación en <https://github.com/SeleniumHQ/docker-selenium>.
