# ANGULAR_Doc
--------------

[//]: # (version: 1.0)
[//]: # (author: Fran Dona)
[//]: # (date: 2024-02-29)



## Tabla de contenidos
- [ANGULAR\_Doc](#angular_doc)
  - [Tabla de contenidos](#tabla-de-contenidos)
  - [¿Qué es Angular?](#qué-es-angular)
  - [Instalacion Angular CLI](#instalacion-angular-cli)
  - [Creación de proyectos y sincronizarlos](#creación-de-proyectos-y-sincronizarlos)



## ¿Qué es Angular?
Angular es un framework de desarrollo front-end utilizado que se usa junto a TypeScript para crear aplicaciones web de una sola página (SPA) y aplicaciones web progresivas (PWA). Se utiliza para construir interfaces de usuario interactivas y dinámicas. Angular utiliza el patrón Modelo-Vista-Controlador (MVC) para organizar y gestionar la estructura de las aplicaciones.

## Instalacion Angular CLI
>[!WARNING]
> Para poder instalar Angular necesitaremos tener instalado NPM en nuestro sistema

>[!NOTE]
> En nuestro caso usaremos Angular 16.2.0

```bash
#Instalacion del cliente
sudo npm install -g @angular/cli
#Para comprobar la version
ng version
```


>[!NOTE]
> Angular no se instala globalmente en nuestro equipo, se instala individualmente en las carpetas que queramos

## Creación de proyectos y sincronizarlos

1. Creacion de las carpetas
```bash
#Creamos una carpeta para el proyecto donde queramos
mkdir /var/www/html/Angular
#Entramos dentro de la carpeta
cd /var/www/html/Angular
#Creamos el proyecto con el nombre que queramos
ng new nombreProyecto

#Nos preguntaran sobre el routing y el tipo de stylesheet
? Would you like to add Angular routing? No
? Which stylesheet format would you like to use? CSS
```

2. Iniciar proyecto
```bash
#Entramos dentro de la carpeta del proyecto
cd nombreProyecto
#Iniciamos el proyecto
ng serve --open
```
>[!NOTE]
Esto nos dejara la consola ocupada mientras estemos trabajando con angular, para cerrar CTR + C


