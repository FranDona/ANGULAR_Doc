# ANGULAR_Doc
--------------

[//]: # (version: 1.0)
[//]: # (author: Fran Dona)
[//]: # (date: 2024-02-29)



## Tabla de contenidos
- [ANGULAR\_Doc](#angular_doc)
  - [Tabla de contenidos](#tabla-de-contenidos)
  - [¿Qué es Angular?](#qué-es-angular)
    - [Instalación Angular CLI](#instalación-angular-cli)
  - [Creación de un proyecto](#creación-de-un-proyecto)
  - [Instalación bibliotecas](#instalación-bibliotecas)
    - [Instalación Bootstrap5](#instalación-bootstrap5)
    - [Instalación Angular Material](#instalación-angular-material)



## ¿Qué es Angular?
Angular es un framework de desarrollo front-end utilizado que se usa junto a TypeScript para crear aplicaciones web de una sola página (SPA) y aplicaciones web progresivas (PWA). Se utiliza para construir interfaces de usuario interactivas y dinámicas. Angular utiliza el patrón Modelo-Vista-Controlador (MVC) para organizar y gestionar la estructura de las aplicaciones.

### Instalación Angular CLI
>[!WARNING]
> Para poder instalar Angular necesitaremos tener instalado NPM en nuestro sistema

>[!NOTE]
> En nuestro caso usaremos Angular 16.2.0

```bash
#Instalación del cliente
sudo npm install -g @angular/cli
#Para comprobar la versión
ng version
```


>[!NOTE]
> Angular no se instala globalmente en nuestro equipo, se instala individualmente en las carpetas que queramos

## Creación de un proyecto

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


## Instalación bibliotecas

### Instalación Bootstrap5

Para comenzar la instalación de la biblioteca nos dirigiremos a la raiz de proyecto e instalamos:
```bash
npm install bootstrap@5
```
Nos dirigiremos a angular.json y añadimos las sigueintes lineas para que bootstrap funcione

```json
"styles": [
              "node_modules/bootstrap/dist/css/bootstrap.min.css",
              "node_modules/@fortawesome/fontawesome-free/css/all.css",
              "src/styles.css"
            ],
            "scripts": [
              "node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"
            ]
```

### Instalación Angular Material
Comenzamos instalando la dependencia estando en la carpeta raiz de nuestro proyecto Angular
```bash
ng add @angular/material
```
Ahora podremos añadir los modulos que necesitemos a nuestro app.modules para podes usar lo que necesitemos

Haremos un ejemplo con `MatSnackBar` usado para mostrar notificaciones 
emergentes personalizados en la pantalla

Nos dirigimos al archivo angular.json en la raíz de nuestro proyecto para agregar angular material:

```json
"styles": [
              "@angular/material/prebuilt-themes/indigo-pink.css",
              "src/styles.css"
            ]
```


Dentro de nuestro componente:

```typescript
// Añadimos la importación arriba del documentos
import { MatSnackBar } from '@angular/material/snack-bar';

// Lo inyectamos en el constructor
constructor(private snackBar: MatSnackBar) {}

// Agregamos el mensaje al la funciona que deseemos
this.snackBar.open('Cuerpo del mensaje', 'Cerrar', {
          duration: 3000,
          horizontalPosition: 'center',
          verticalPosition: 'top'
        });
```

>[!TIP]
Para más información puedes ver la [Documentación Oficial ](https://material.angular.io)