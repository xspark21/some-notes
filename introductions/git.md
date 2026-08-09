# Primeros pasos - git/github/github-cli
> estos apuntes estan hechos desde un entorno wayland/hyrprland + arch linux y pueden ser usados desde cualquier OS, lo unico que varia es la forma de actualizar el sistema (no es necesario) e instalar github-cli

cuando ya se ha hecho una vez, no será necesario repetir todos los pasos, se vuelve natural o por inercia el flujo de trabajo
## definiciones
1. `git`: control de versiones local (historial, commits, ramas)
2. `github`: servidor remoto que aloja repositorios git
3. `github-cli` (gh): cliente que automatiza la interacción entre git y github
## Actualizar el sistema

recordar siempre mantener actualizado el sistema para evitar trabajar con dependencias rotas o conflictivas, no es necesario para usar git/github/github-cli pero es una buena práctica

```zsh
sudo pacman -Syu
```
## Instalar github-cli

github-cli es la herramienta que nos permite interactuar con github, contiene herramientas mas amigable y es mas sencillo de configurar todo

```zsh
sudo pacman -S github-cli
```
## Autenticarse

para loguearnos, hacemos

```zsh 
gh auth login
```

lo que nos hara elegir la forma de iniciar sesion, como recomendacion: github.com -> https -> login with browser
abriremos el enlace y terminamos de iniciar sesion en el navegador, como recien estamos empezando recomiendo hacerlo asi

## Establecer nombre y correo 

En este apartado no es necesario usar nuestro nombre/correo de github ya que se establece con el fin de saber quien y cuando hizo el commit (avance) 

```zsh
git config --global user.name "nombre"
git config --global user.email "email@correo.com"
```
## Confirmar nuestra identidad

con

```zsh
git config --global --list
```
## vincular git con github-cli

podemos confirmar que lo hemos hecho bien y quienes somos, por ultimo enlazar github-cli a git para que lo use como gestor de credenciales de lo contrario nos pedira contraseña mas adelante

```zsh
gh auth setup-git
```
una vez logueado y vinculado no es necesario volver a hacerlo a menos que quites el token de acceso desde tu cuenta en github o borres el archivo de configuracion con el token de acceso
## Previo a empezar

este comando crea el repositorio en github, el nombre del repositorio es el que aparecera oficialmente en tu cuenta de github, de lo contrario git no sabra a donde subir los archivos 

```zsh
gh repo create nombre-proyecto --public # usar --private para un repositorio privado, aunque por si solo se explica
```
## Empezar un nuevo proyecto 

Como nuestra area es la ciencia y programacion, una buena practica es que en cada proyecto que empecemos usemos git, claro que depende de cada persona y es solo una recomendacion

```zsh
# --- iniciar un directorio git ---

mkdir nombre-proyecto # no es necesario que se llame igual que el repositorio, pero es una buena practica hacerlo
cd nombre-proyecto
git init 

# --- hasta aqui esta bien para empezar, luego de hacer avances se pueden usar los siguientes comandos, si se hace antes no se guarda ni sube nada ---

touch README.md # con esto basta para poder empezar, si es que se tienen claras las ideas o para dar un preliminar

# --- cuando se termina de trabajar se usa ---

git add .
git commit -m "primer commit"

# --- siempre la primera vez que se conecta un proyecto a la nube se usa ---

git branch -M main 
git remote add origin https://github.com/usuario/repo.git
git push -u origin main
```
## Algunas consideraciones

especificaciones de cada comando:
1. `git init` este comando inicializa git en la carpeta donde se ejecute este comando
2. `git add .`  este comando prepara todo lo que esta dentro de la carpeta para ser cargado debido al ".", se puede tambien especificar un solo archivo o directorio a cargar 
3. `git commit -m` guarda exactamente todo lo que previamente se preparó, se puede considerar un backup, el subcomando -m sirve para indicar un mensaje en el commit para llevar registros de que se hizo, ayuda a auditar
4. `git branch -M main` usar este comando renombra la linea/rama de trabajo para que coincida con la principal de github (algunas versiones de git usan "master" en lugar de "main")
5. `git remote add origin` indica a donde se va a enviar todo el trabajo o avance realizado
6. `git push -u origin main` indica que se hará directamente en la linea principal de trabajo, puede usarse origin "nombre-rama-desarrollo" para decir que sera un trabajo en paralelo 

desglosando el comando numero 6,

```zsh
git push -u origin main # computador sin enlace a la linea main, -u hace referencia a la configuracion upstream 
git push # una vez hecho -u origin main se puede usar tanto push/pull sin parametros
```
ambos son validos dependiendo de la rama a la que pertenece el repositorio donde se usa el comando, para evitar esto, naturalmente se hace con el subcomando **-u** que sirve para indicar las configuraciones de git 

## Como bajar proyectos de la nube

usualmente al entrar a un repositorio, este trae su README.md que entrega la informacion necesaria tanto para la clonacion del proyecto como la instalacion, a modo de tener todo lo esencial junto en esta guia, basta con:

```zsh
git clone "URL del repositorio"
```
esta accion tambien deja establecida las configuraciones apuntando a la linea principal por lo que usar los parametros -u origin main es innecesario, tampoco es necesario usar git remote add origin
## Como seguir subiendo avances

luego de haber configurado todo, naturalmente basta con hacer

```zsh
git add .
git commit -m "mensaje del avance"
git push
```
y con eso puedes mantener tu repositorio de github actualizado cada vez que haces un avance y si estas en otro PC y tienes el repositorio, puedes actualizar los cambios que hiciste con 

```zsh
git pull origin main # si es que en ese pc no esta configurado como linea principal (branch main)
git pull # si se usó git clone, se configura solo a la linea main por lo que se puede evitar usar los parametros origin main 
```
## importante

si se esta trabajando en un proyecto importante donde se usan claves privadas o durante el desarrollo del proyecto se instalaron dependencias o existen carpetas de compilacion se recomienda usar .gitignore para evitar subir informacion sensible, cargar archivos innecesarios como las dependencias o archivos temporales. por lo tanto, dentro de la carpeta del repositorio hacer

```zsh
touch .gitignore
```
editar con el editor de texto preferido, en mi caso nvim

```zsh
nvim .gitignore
```

ejemplo practico de un .gitignore

```zsh
# --- Archivos del sistema ---

.DS_Store # <- archivo oculto
Thumbs.db # <- database
*~
*.swp

# --- Binarios y carpetas pesadas ---

node_modules/ # <- la carpeta node_modules con todo su contenido
bin/  
obj/ 
*.exe # <- aqui decimos todos los archivos que terminen .exe
*.o # <- todos los archivos .o

# --- Archivos de configuración personal / Secretos ---

.env # <- archivo con configuracion sobre las variables de entorno
.venv/ # <- carpeta que contiene un entorno virtual, no siempre son ligeras
secretos.txt # <- puede ser la api-key o informacion confidencial
*.pem 
```
# Resumen

Una vez logueado y enlazado git con github podemos crear un repositorio:

1. desde un directorio previo

```zsh
git init
touch README.md
git add .
git commit -m "primer commit"
git branch -M main 
gh repo create nombre-proyecto --source=. --remote=origin --push # podemos usar la carpeta como fuente a la hora de crear el repositorio, tambien reemplaza usar git remote add origin
```

2. como proyecto nuevo

```zsh
gh repo create nombre-proyecto --public --add-readme --clone
cd nombre-proyecto
```
## Algunos comandos utiles

```zsh
git status        # ver estado del repo
git branch        # ver ramas
git remote -v     # ver repositorios remotos
gh auth status    # ver estado de login en GitHub
```
