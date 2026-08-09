# Acerca de UV
> este apunte esta enfocado en ser práctico desde el comienzo puesto que para eso es la herramienta

## definiciones

1. `uv`: gestor de proyectos, dependencias y versiones de python (no requiere tener python instalado)
2. `pyproject.toml`: archivo donde se declara el proyecto, sus dependencias y su configuracion
3. `uv.lock`: archivo que fija las versiones exactas de todo lo instalado, para que el proyecto sea reproducible en cualquier maquina
4. `.venv`: carpeta donde vive el entorno virtual del proyecto, uv la crea y gestiona automaticamente

reemplaza el uso de pip y mejora el rendimiento, tampoco hace falta activar constantemente el entorno virtual puesto que se gestiona automaticamente, suele ser más práctico en el dia a dia

## Actualizar el sistema
recordar siempre mantener actualizado el sistema 

```bash
sudo pacman -Syu # en mi caso q uso linux
```

## Instalar uv

De manera general se puede instalar ejecutando este comando:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

en caso de no resultar, sugiero revisar la página oficial de uv [aqui](https://docs.astral.sh/uv/)

## Confirmar la instalacion

```bash
uv --version
```

## Instalar una version de python

como dije, no es necesario tener descargado python puesto que uv puede descargar e instalar sus propias versiones

```bash
uv python install 3.12 # instala una version especifica
uv python list # ver versiones disponibles/instaladas
```

## Empezar un nuevo proyecto

```bash
# iniciar un proyecto nuevo
uv init --no-package nombre-proyecto # crea la carpeta, el pyproject.toml, un README y un .python-version
cd nombre-proyecto
```

si ya tenias la carpeta creada, basta con iniciar el proyecto dentro

```bash
mkdir nombre-proyecto
cd nombre-proyecto
uv init --no-package
# nota: para fijar la version de python del proyecto (opcional, si no se declara toma la que esté disponible)
uv python pin 3.XX # la version de python tambien determina las librerias que puedes usar, verifica las versiones siempre que puedas
```

## Agregar y quitar dependencias

```bash
uv add libreria # agrega una dependencia y actualiza pyproject.toml + uv.lock
uv add libreria1 libreria2 # se pueden agregar varias a la vez
uv add --dev libreria_para_test # esta libreria no se instalará hasta que lo corras en modo desarrollador independiente al uv sync
uv remove libreria # quita una dependencia
```

## Ejecutar codigo

basta con ejecutar

```bash
uv run script.py # correr directamente el archivo
uv run carpeta/script.py # correr a traves un archivo en una carpeta tambien es posible poniendo la direccion
```

si de verdad se necesita activar el entorno manualmente (por ejemplo para usarlo con otra herramienta), tambien se puede

```bash
source .venv/bin/activate # linux/mac
deactivate # para salir
```

## Algunas consideraciones

especificaciones de cada comando:

1. `uv init --no-package` este comando crea la estructura basica de un proyecto (`pyproject.toml`, `.python-version`, `README.md`) en la carpeta donde se ejecute
2. `uv add` agrega una dependencia y actualiza tanto `pyproject.toml` como `uv.lock` automaticamente, no hace falta tocar ningun archivo a mano
3. `uv remove` hace lo mismo pero al reves, saca la dependencia de todos lados
4. `uv run` ejecuta un script dentro del entorno del proyecto sin necesidad de activarlo antes
5. `uv sync` sincroniza el entorno virtual y dependencias tal cual lo declara `uv.lock`
6. `uv lock` actualiza el `uv.lock` sin instalar nada, por si se quiere solo recalcular versiones

## Desde proyectos ajenos

si el proyecto ya viene con `pyproject.toml` y `uv.lock`, basta con

```bash
git clone "URL del repositorio"
cd nombre-proyecto
uv sync
```
no hace falta gestionar python, versiones ni dependencias. he ahi la razón de ser tan útil

# Resumen

Una vez instalado uv podemos crear un proyecto:

1. desde cero

```bash
uv init --no-package nombre-proyecto
cd nombre-proyecto
uv python pin 3.12
uv add requests
```

2. desde un proyecto ya clonado

```bash
git clone "URL del repositorio"
cd nombre-proyecto
uv sync
```

## Algunos comandos utiles

```bash
uv add libreria           # agregar dependencia
uv remove libreria        # quitar dependencia
uv run script.py          # ejecutar dentro del entorno del proyecto
uv sync                   # instalar todo segun el uv.lock
uv lock                   # recalcular el uv.lock sin instalar
uv python list            # ver versiones de python disponibles e instaladas
uv tree                   # ver arbol de dependencias
```

## Sobre `--no-package`

> nota: la razon de usar --no-package viene de un cambio reciente en la forma en que se crean los proyectos con uv, para mantenerlo simple usemos el comando `--no-package`

Para visualizarlo, ejemplo sin el comando 

```bash
user@machine ~ $ uv init package_ver # inicializamos sin el comando
Initialized project `package-ver` at `/home/user/package_ver`
user@machine ~ $ cd package_ver/
user@machine ~/package_ver [master] $ tree # este comando es para visualizar el arbol de trabajo
.
├── pyproject.toml
├── README.md
└── src
    └── package_ver # se crea en formato paquete editable de python, sirve para resolver problemas de informaticos... hacer caso omiso
        └── __init__.py

3 directories, 3 files
```
Con el comando:
```bash
user@machine ~ $ uv init --no-package nopackage_ver # usando el comando
Initialized project `nopackage-ver` at `/home/user/nopackage_ver`
user@machine ~ $ cd nopackage_ver/
user@machine ~/nopackage_ver [master] $ tree 
.
├── main.py # crea solo el pyproject, main y readme manteniendolo simple, por eso sugiero aprender con este
├── pyproject.toml
└── README.md

1 directory, 3 files
user@machine ~/nopackage_ver [master] $ 
```
