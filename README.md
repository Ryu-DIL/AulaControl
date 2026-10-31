# AulaControl

Plugin para servidores **Paper** de Minecraft que añade un **panel de control web** pensado para dar clases: el profesor gestiona a los alumnos, lanza actividades y construye espacios desde el navegador, sin escribir comandos en el juego.

Está pensado para grupos de alumnos conectados a un servidor local (por ejemplo, gestionado con Crafty) mientras el profesor controla la sesión desde un ordenador.

## Qué permite hacer

El panel tiene una barra lateral con estas secciones:

| Sección | Qué hace |
|---|---|
| **Jugadores** | Lista de conectados, alias cosméticos para reconocer nombres poco claros, expulsar, banear y desbanear. |
| **Equipos** | Crear equipos con nombre y color, asignar miembros y usarlos como atajo para elegir alumnos en el resto de secciones. |
| **Bossbar** | Barras de jefe con título y temporizador (o infinitas), varias a la vez para distintos grupos, con fuegos artificiales opcionales al terminar. |
| **Ítems** | Dar ítems a uno, varios o todos los alumnos, con buscador, encantamientos sin límite de nivel, opción de irrompible y sets favoritos guardados. |
| **Efectos** | Activar o desactivar vuelo, PvP e invisibilidad por alumno. Se reaplican al reaparecer. |
| **Teleport** | Teletransportar alumnos hacia otro jugador o a unas coordenadas, con ubicaciones favoritas con nombre. |
| **Construcciones** | Levantar una sala o plataforma centrada en la posición de un jugador, con tamaño y materiales de suelo, paredes y techo configurables (y doble capa de suelo opcional). |
| **Mapa** | Mapa del terreno con la posición de los alumnos, con zoom. |
| **Mobs** | Invocar y eliminar mobs, con sus variantes. |
| **Día/Clima** | Cambiar entre día y noche, y entre despejado, lluvia y tormenta; consultar y cambiar gamerules; cambiar el modo de juego. |
| **Minijuego** | Competiciones por equipos: cada equipo tiene una misión (romper o colocar ciertos bloques) y hay un marcador. Dos modos: puntos acumulados o carrera hasta un objetivo. |

## Requisitos

- Un servidor **Paper** compatible con la API `26.2`.
- **Java 25** (es el toolchain con el que se compila el proyecto).
- Un navegador en el mismo equipo que el servidor.

## Instalación

1. Descarga `AulaControl-1.0-SNAPSHOT.jar` desde la sección **Releases** (o [compílalo](#compilar-desde-el-código)).
2. Copia el `.jar` en la carpeta `plugins/` del servidor y arranca (o reinicia) el servidor.
3. Abre **http://localhost:8080** en el navegador del equipo donde corre el servidor.

### Iconos y nombres en español (opcional)

El panel funciona sin ellos, pero se verá sin iconos y con los nombres de ítems en inglés. Estos archivos son propiedad de Mojang y **no se incluyen en este repositorio**; cópialos desde tu propia instalación de Minecraft:

- **Texturas**: del `.jar` del cliente (`versions/<versión>/<versión>.jar` dentro de tu carpeta de Minecraft), copia el contenido de `assets/minecraft/textures/` en `plugins/AulaControl/web-assets/`.
- **Traducciones**: copia `es_es.json` en `plugins/AulaControl/lang/es_es.json`. El launcher guarda los idiomas en `assets/objects/`, enlazados desde `assets/indexes/<versión>.json`.

Los datos del panel (ubicaciones favoritas, sets de ítems, estados de los jugadores) se guardan en `plugins/AulaControl/`.

## Compilar desde el código

```bash
git clone https://github.com/Ryu-DIL/AulaControl.git
cd AulaControl
./gradlew build
```

El plugin queda en `build/libs/AulaControl-1.0-SNAPSHOT.jar`.

Para probarlo en un servidor Paper local de desarrollo:

```bash
./gradlew runServer
```

Esto descarga Paper `26.2` y lo arranca en la carpeta `run/` (que está en el `.gitignore`). Acepta la EULA en `run/eula.txt` la primera vez.

## Seguridad

- El panel escucha **solo en `localhost:8080`**: no es accesible desde otros equipos de la red.
- **No tiene autenticación.** Cualquiera que pueda abrir esa dirección controla el servidor. No lo expongas a Internet ni a la red del aula (por ejemplo, con un proxy inverso o con port forwarding).
- Si el servidor está en otra máquina, accede con un túnel (por ejemplo, SSH: `ssh -L 8080:localhost:8080 usuario@servidor`) en lugar de abrir el puerto.

## Limitaciones conocidas

- El puerto **8080 está fijado en el código**: si ejecutas varios servidores a la vez en la misma máquina, solo el primero podrá abrir el panel.
- Los estados de los jugadores (PvP, vuelo, invisibilidad) se identifican por **nombre de jugador**.

## Estructura

```
src/main/java/org/jere/aulaControl/
├── AulaControl.java        # clase principal del plugin
├── ControlServer.java      # servidor HTTP y panel web (API + interfaz)
├── EstadosListener.java    # PvP, reaparición y salida de jugadores
└── MinijuegoListener.java  # recuento de bloques para los minijuegos
src/main/resources/plugin.yml
```

## Aviso legal

Proyecto no oficial. Minecraft es una marca de Mojang Studios / Microsoft. Este proyecto no está afiliado ni respaldado por Mojang ni por Microsoft.
