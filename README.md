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
  - [Estructura del Proyecto](#estructura-del-proyecto)
    - [Enviroments](#enviroments)
    - [Models](#models)
    - [Services](#services)
    - [Components](#components)
    - [Tablas relacionadas](#tablas-relacionadas)
  - [Instalación bibliotecas](#instalación-bibliotecas)
    - [Instalación Bootstrap5](#instalación-bootstrap5)
    - [Instalación Angular Material](#instalación-angular-material)



## ¿Qué es Angular?
Angular es un framework de desarrollo front-end utilizado que se usa junto a TypeScript para crear aplicaciones web de una sola página (SPA) y aplicaciones web progresivas (PWA). Se utiliza para construir interfaces de usuario interactivas y dinámicas. Angular utiliza el patrón Modelo-Vista-Controlador (MVC) para organizar y gestionar la estructura de las aplicaciones.

## Instalación Angular CLI
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

## Estructura del Proyecto

### Enviroments

Comando: ng generate environments

En el cual guardaremos la URL de nuestro servicio backend

```typescript
// enviroment.developments.ts
export const environment = {
    apiURL : "http://localhost:8101"
};
```



### Models

Comando: ng generate class models/nombreEntidad --type=model

En el cual guardaremos un modelo con la configuración que usa la bbdd sobre la entidad

```typescript
export interface Tipos {
    id: number;
    materia: string;
    borrado: boolean;
}
```
>[!NOTE]
Importante crear el modelo como una interfaz

### Services

Comando: ng generate service services/nombreEntidad

En el cual guardaremos los servicios necesarios para conectar los endpoint del backend con el frontend(Angular)

```typescript
// Atributo con la raiz de los endpoints
private baseURL = `${environment.apiURL}/tipos_expediente`;

// En el constructor inyecto el HttpClient para gestionar endpoints
constructor(private http: HttpClient) {}

// Insertar tipos
// @PostMapping("/insertar/{materia}")
insertarTipo(materia: string): Observable<Tipos> {
  const url = `${this.baseURL}/insertar/${materia}`;
  return this.http.post<Tipos>(url, {}); // {} body, siempre vacio
}

// Resto de servicios....
```

### Components

Comando: ng generate component nombreComponente

En el componente tendremos dos archivos:
- Archivo Component: Usado para crear todas las funciones que manejaremos en la página relacionado con ese componente.
- Archivo HTML: En donde definiremos la estructura de como vemos la página.

```typescript
export class FormulariosTiposComponent implements OnInit {
  tipos: Tipos[] = [];
  mensaje: string = "";
  materia: string = "";
constructor(private servicio: TiposService)

  ngOnInit(): void {
    this.cargarTipos();
  } 

cargarTipos(): void {
  this.servicio.consultarTipos().subscribe(datos => {
    this.tipos = datos;
    this.filtrarTipos();
  });
  }

insertarTipo(): void {
  this.servicio.insertarTipo(this.materia).subscribe(
    resultado => {
      if (resultado) {
        this.mensaje = "Tipo insertado";
        this.cargarTipos();
        }
      }
    );
  }

}
```

Este seria el resultado final que vería el usuario
```html
<!-- Formulario para insertar nuevos tipos -->
<form (ngSubmit)="insertarTipo()">
    <section class="mb-3">
        <label for="materia" class="form-label">Materia</label>
        <input placeholder="Introducir Materia" type="text" class="form-control" id="materia" [(ngModel)]="materia" name="materia">
    </section>
    <!-- Botón para insertar un nuevo documento -->
    <button type="submit" class="btn btn-primary" data-bs-dismiss="modal">Insertar</button>
</form>

<!-- Tabla para mostrar los datos -->
<table>
  <thead>
    <tr>
      <th>ID</th>
      <th>Materia</th>
      <th>Acciones</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let tipo of tiposFiltrados">
      <td>{{ tipo.id }}</td>
      <td>{{ tipo.materia }}</td>
      <td>
        <button (click)="prepararActualizacion(tipo)"></button>
        <button class="btn btn-danger btn-sm" (click)="borradoLogicoTipo(tipo.id)"></button>
      </td>
    </tr>
  </tbody>
</table>
```


### Tablas relacionadas

1. En la entidad "Tipos" crearemos las materias:
    ```typescript
    export interface Tipos {
        id: number;
        materia: string;
        borrado: boolean;
    }
    ```

2. En la entidad Expedientes, en el campo "tipo" nuestra intención es añadir un materia al expediente, por lo que importaremos la entidad al modelo y pondremos el nombre de la entidad en el campo
    ```typescript
    // Importamos la entidad Tipos
    import { Tipos } from "../../tipos-expediente/models/tipos.model";

    export interface Expedientes {
      id: number;
      codigo: string;
      fecha: string;
      descripcion: string;
      tipo: Tipos; // Ponemos el nombre de la entidad que queremos mostrar
      borrado: boolean;
    }
    ```

3. En el componente también tendremos que agregar las dos entidades 
    ```typescript
    import { Expedientes } from '../models/expedientes.model';
    import { Tipos } from '../../tipos-expediente/models/tipos.model';

    export class FormulariosExpedientesComponent implements OnInit {
      expedientes: Expedientes[] = [];
      tipos: Tipos[] = [];

    ngOnInit(): void {
      this.cargarExpedientes();
      this.cargarTipos();
    }

      cargarExpedientes(): void {
        this.servicio.consultarExpedientes().subscribe(datos => {
          this.expedientes = datos;
        });
      }

      cargarTipos(): void {
        this.servicio.consultarTipos().subscribe(tipos => {
          this.tipos = tipos;
        });
      }
    }
    ```

1. Ahora en la vista podremos añadir a las tablas los datos de las dos entidades de la siguiente manera

    ```html
    // Para mostrar los datos en una tabla:
    <td>{{ expediente.codigo }}</td>
    <td>{{ expediente.tipo.materia }}</td>
    ```


## Instalación bibliotecas

Las bibliotecas en Angular permiten reutilizar y compartir código entre diferentes proyectos, facilitando el desarrollo y mantenimiento de aplicaciones.

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