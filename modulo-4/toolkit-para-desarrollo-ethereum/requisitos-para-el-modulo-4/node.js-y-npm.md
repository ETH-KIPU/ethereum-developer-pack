# Node.js y npm

### Node.js

Node.js es un entorno de ejecución de JavaScript basado en el motor V8 de Google Chrome que permite a los desarrolladores ejecutar código JavaScript en el lado del servidor. Antes a su existencia el código JavaScript sólo se podía ejecutar en un navegador web.

Para el propósito de nuestro curso es importante porque ejecutaremos código JavaScript que interactúa con la blockchain y necesitamos un entorno que pueda procesar ese código.

#### **Instalación de Node.js y npm**

**Instalación en Windows**

1. **Descargar el Instalador:**
   * Ve a la [página de descarga de Node.js](https://nodejs.org/) y descarga el instalador para Windows.
2. **Ejecutar el Instalador:**
   * Ejecuta el instalador y sigue las instrucciones del asistente de instalación.
3. **Verificar la Instalación:**
   *   Abre una terminal (Command Prompt o PowerShell) y ejecuta los siguientes comandos:

       ```bash
       node -v
       npm -v
       ```

       Deberías ver las versiones instaladas de Node.js y npm.

**Instalación en macOS**

1. **Instalar Homebrew (si no está instalado):**
   *   Abre una terminal y ejecuta:

       ```bash
       /bin/bash -c "$(curl -fsSL <https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh>)"
       ```
2. **Instalar Node.js usando Homebrew:**
   *   Ejecuta el siguiente comando en la terminal:

       ```bash
       brew install node
       ```
3. **Verificar la Instalación:**
   *   Ejecuta los siguientes comandos en la terminal:

       ```bash
       node -v
       npm -v
       ```

       Deberías ver las versiones instaladas de Node.js y npm.

### npm

npm (Node Package Manager) es el gestor de paquetes de Node.js que permite instalar y actualizar paquetes de terceros para utilizarlos en tu propio código.

Veamos un ejemplo de cómo utilizar Node.js y npm en un proyecto sencillo que será un servidor web básico.

#### **Creación de un Proyecto Node.js**

**Crear un Directorio para el Proyecto**

*   Abre una terminal y navega hasta el directorio donde quieres crear tu proyecto, luego ejecuta:

    ```bash
    mkdir mi-proyecto-node
    cd mi-proyecto-node
    ```

**Inicializar el proyecto con npm**

*   Ejecuta el siguiente comando para crear un archivo **`package.json` ,** más adelante explicaremos el uso de este archivo con más detalle.

    ```bash
    npm init
    ```
* Sigue las instrucciones para completar la configuración del proyecto. Puedes aceptar las configuraciones por defecto presionando Enter.

**Instalar Express**

*   Asegúrate de estar en el directorio del proyecto y ejecuta:

    ```bash
    npm install express
    ```

    Express es un framework web minimalista y flexible que proporciona herramientas para desarrollar aplicaciones web y móviles.

**Crear el archivo del servidor**

*   En el directorio del proyecto, crea un archivo llamado **`server.js`** y agrega el siguiente código:

    ```jsx
    const express = require('express');
    const app = express();
    const port = 3000;

    app.get('/', (req, res) => {
      res.send('Hello World!');
    });

    app.listen(port, () => {
      console.log(`Server running at <http://localhost>:${port}/`);
    });
    ```

    Asegúrate de haber grabado los cambios.

**Ejecutar el servidor**

*   En la terminal, asegúrate de estar en el directorio del proyecto y ejecuta:

    ```bash
    node server.js
    ```

**Verificar el servidor:**

* Abre un navegador web e ingresa a **`http://localhost:3000/`**.
* Deberías ver el mensaje "Hello World!". Si es así, ¡felicitaciones!.

De esta manera vimos cómo se utiliza npm para instalar paquetes como Express y cómo Node.js nos ayuda a ejecutar el código JavaScript, que en este caso nos permitió activar un servidor web.

A continuación hablaremos del fichero **`package.json`** que se utiliza en la configuración de proyectos con Node.js.

#### **Archivo package.json**

El archivo **`package.json`** es un archivo de configuración en cualquier proyecto Node.js. Contiene información sobre el proyecto y sus dependencias, entre otros detalles.

A continuación un ejemplo

```json
{
  "name": "mi-proyecto",
  "version": "1.0.0",
  "description": "Descripción de mi proyecto",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  },
  "author": "Tu Nombre",
  "license": "MIT"
}
```

#### Dependencias

Las dependencias son módulos de código (también conocidos como paquetes o librerías) que tu proyecto necesita para funcionar correctamente. Estas dependencias son gestionadas automáticamente por npm, que facilita su instalación, actualización y gestión.

1. **Tipos de Dependencias en npm**

* **Dependencias de producción (`dependencies`):**
  *   Estas son las dependencias necesarias para que tu aplicación funcione en un entorno de producción. Se especifican en el archivo **`package.json`** bajo el campo **`dependencies`**.

      Ejemplo:

      ```json
      {
        "dependencies": {
          "express": "^4.17.1"
        }
      }
      ```

      En este caso, **`express`** es una dependencia de producción.
* **Dependencias de desarrollo (`devDependencies`):**
  *   Estas son las dependencias necesarias solo durante el desarrollo de tu aplicación, como herramientas de prueba, traductores y detectores de errores. Se especifican en el archivo **`package.json`** bajo el campo **`devDependencies`**.

      Ejemplo:

      ```json
      {
        "devDependencies": {
          "nodemon": "^2.0.7"
        }
      }
      ```

      En este caso, **`nodemon`** es una dependencia de desarrollo.

2. **Gestión de dependencias con npm**

* **Instalación de Dependencias**
  *   **Instalar todas las dependencias:**

      Ejecuta el siguiente comando en el directorio de tu proyecto. Esto instalará todas las dependencias especificadas en tu archivo **`package.json`**.

      ```bash
      npm install
      ```
  *   **Instalar una nueva dependencia de producción:**

      Usa el siguiente comando para instalar una nueva dependencia y agregarla automáticamente al campo **`dependencies`** en tu **`package.json`**.

      ```bash
      npm install nombre-del-paquete
      ```
  *   **Instalar una nueva dependencia de desarrollo:**

      Usa el siguiente comando para instalar una nueva dependencia y agregarla automáticamente al campo **`devDependencies`** en tu **`package.json`**.

      ```bash
      npm install nombre-del-paquete --save-dev
      ```
* **Actualizar dependencias**
  *   **Actualizar todas las dependencias:**

      Para actualizar todas las dependencias a sus versiones más recientes permitidas por el rango de versiones en **`package.json`**.

      ```bash
      npm update
      ```
  *   **Actualizar una dependencia específica:**

      Para actualizar una dependencia específica a su versión más reciente permitida por el rango de versiones en **`package.json`**.

      ```bash
      npm update nombre-del-paquete
      ```
* **Eliminar dependencias**
  *   **Eliminar una dependencia de producción:**

      Usa el siguiente comando para eliminar una dependencia y eliminarla automáticamente del campo **`dependencies`** en tu **`package.json`**.

      ```bash
      npm uninstall nombre-del-paquete
      ```
  *   **Eliminar una dependencia de desarrollo:**

      Usa el siguiente comando para eliminar una dependencia y eliminarla automáticamente del campo **`devDependencies`** en tu **`package.json`**.

      ```bash
      npm uninstall nombre-del-paquete --save-dev
      ```

3. **Beneficios de gestionar dependencias con npm**

* **Facilidad de instalación y actualización:** Simplifica la instalación y actualización de dependencias mediante comandos sencillos.
* **Control de versiones:** Permite especificar versiones exactas de dependencias, garantizando que el proyecto utilice las versiones correctas.
* **Reducción de tamaño del código:** Permite reutilizar código de terceros, lo que puede reducir el tamaño del código que necesitas escribir y mantener.
* **Automatización de tareas:** Permite automatizar tareas comunes (como la ejecución de pruebas o la construcción del proyecto) mediante comandos definidos en el archivo **`package.json`**.
