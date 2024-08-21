# Terminal

El terminal es una herramienta que permite a los usuarios interactuar con su sistema operativo mediante comandos de texto. En Windows, se conoce comúnmente como "Command Prompt" o "PowerShell", mientras que en macOS se utiliza la "Terminal". A menudo se tendrá que utilizar el terminal para ejecutar diversos programas, así como para trabajar con directorios y archivos.

### **Introducción al Terminal**

#### **Acceso al Terminal**

**Windows:**

* **Command Prompt:**
  * Abre el menú de inicio y escribe "cmd".
  * Selecciona "Command Prompt".
* **PowerShell:**
  * Abre el menú de inicio y escribe "powershell".
  * Selecciona "Windows PowerShell".

**macOS:**

* Abre Spotlight (Cmd + Espacio), escribe "Terminal" y selecciona la aplicación Terminal.

### **Comandos Básicos**

#### **Navegación de Directorios**

**Windows:**

* **`dir`**: Lista los archivos y carpetas en el directorio actual.
* **`cd [ruta]`**: Cambia al directorio especificado.
* **`cd ..`**: Sube un nivel en la estructura de directorios.
* **`cd`**: Vuelve al directorio raíz del usuario.

**macOS:**

* **`ls`**: Lista los archivos y carpetas en el directorio actual.
* **`cd [ruta]`**: Cambia al directorio especificado.
* **`cd ..`**: Sube un nivel en la estructura de directorios.
* **`cd`**: Vuelve al directorio raíz del usuario.
* **`pwd`**: Indica el directorio en el que estás ubicado.

#### Limpieza del terminal

**Windows:**

* **`cls`**: Borra los comandos anteriores que quedaron en el terminal.

**macOs:**

* **`clear`**: Borra los comandos anteriores que quedaron en el terminal.

#### **Manipulación de Archivos y Directorios**

**Windows:**

* **`mkdir [nombre_del_directorio]`**: Crea un nuevo directorio.
* **`rmdir [nombre_del_directorio]`**: Elimina un directorio vacío.
* **`del [nombre_del_archivo]`**: Elimina un archivo.
* **`copy [origen] [destino]`**: Copia un archivo de origen a destino.
* **`move [origen] [destino]`**: Mueve un archivo de origen a destino.

**macOS:**

* **`mkdir [nombre_del_directorio]`**: Crea un nuevo directorio.
* **`rmdir [nombre_del_directorio]`**: Elimina un directorio vacío.
* **`rm -r [nombre_del_directorio]`**: Elimina un directorio y su contenido.
* **`rm [nombre_del_archivo]`**: Elimina un archivo.
* **`cp [origen] [destino]`**: Copia un archivo o directorio de origen a destino.
* **`mv [origen] [destino]`**: Mueve un archivo o directorio de origen a destino.
* **`touch [nombre_del_archivo]`**: Crea un archivo de texto.

#### **Visualización y Edición de Archivos**

**Windows:**

* **`type [nombre_del_archivo]`**: Muestra el contenido de un archivo.
* **`notepad [nombre_del_archivo]`**: Abre un archivo en el editor de texto Notepad.

**macOS:**

* **`cat [nombre_del_archivo]`**: Muestra el contenido de un archivo.
* **`nano [nombre_del_archivo]`**: Abre un archivo en el editor de texto Nano.
* **`open -e [nombre_del_archivo]`**: Abre un archivo en el editor de texto por defecto (TextEdit).

### **Comandos Avanzados**

#### **Gestión de Procesos**

**Windows:**

* **`tasklist`**: Lista todos los procesos en ejecución.
* **`taskkill /PID [pid]`**: Termina un proceso por su ID.

**macOS:**

* **`ps -A`**: Lista todos los procesos en ejecución.
* **`kill [pid]`**: Termina un proceso por su ID.
* **`top`**: Muestra los procesos en ejecución en tiempo real.

#### **Redes**

**Windows:**

* **`ipconfig`**: Muestra la configuración de red.
* **`ping [dirección]`**: Envía paquetes ICMP a una dirección de red.

**macOS:**

* **`ifconfig`**: Muestra la configuración de red.
* **`ping [dirección]`**: Envía paquetes ICMP a una dirección de red.

### **Recursos Adicionales**

#### **Ayuda y Documentación**

**Windows:**

* **`help [comando]`**: Muestra la ayuda para un comando específico.
* **`get-help [comando]`** (PowerShell): Muestra la ayuda para un comando específico.

**macOS:**

* **`man [comando]`**: Muestra el manual de usuario para un comando específico.

#### **Enlaces Útiles**

* [Documentación de PowerShell](https://docs.microsoft.com/en-us/powershell/)
* [Documentación de Terminal de macOS](https://support.apple.com/guide/terminal/welcome/mac)
* [Comandos de terminal que un usuario de Mac debe conocer](https://www.techrepublic.com/article/16-terminal-commands-every-user-should-know/)
* [Tutorial para para Windows (video)](https://www.youtube.com/watch?v=erKosEQaaFc)
*   [Tutorial para macOs (video)](https://www.youtube.com/watch?v=TmP3y7Z2kk4)&#x20;

