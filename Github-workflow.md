# Práctica de Workflow en GitHub

Este repositorio documenta los pasos necesarios para realizar un flujo de trabajo profesional en GitHub utilizando el terminal. Está pensado para mostrar habilidades técnicas a empleadores potenciales.

---

## Pasos principales

### 1. Creación del repositorio
- Creamos un repositorio en GitHub llamado `Practica_de_workflow_en_GitHub`.
- Configuración inicial:
  - Añadimos un archivo `README.md` automáticamente.
  - Configuramos el repositorio como público para su accesibilidad.

### 2. Configuración local
- Clonamos el repositorio con el comando:
  ```bash
  git clone git@github.com:gerarklopz/Practica_de_workflow_en_GitHub.git
  ```
- Ubicamos el repositorio en el directorio `~/workspace/Practica_de_workflow_en_GitHub`.

### 3. Alias personalizados para GitHub

Para agilizar el uso de Git, configuramos alias en el archivo `~/.bashrc`. Estos alias nos permiten ejecutar comandos de Git de forma más eficiente.

#### Alias configurados:

- **`gs`**: Muestra el estado del repositorio.
  ```bash
  alias gs='git status'
  ```
- **`ga`**: Añade archivos al área de staging.
  ```bash
  alias ga='git add'
  ```
- **`gc`**: Realiza un commit con mensaje.
  ```bash
  alias gc='git commit -m'
  ```
- **`gp`**: Sube los cambios al repositorio remoto.
  ```bash
  alias gp='git push origin main'
  ```
- **`gd`**: Muestra diferencias entre cambios.
  ```bash
  alias gd='git diff'
  ```
- **`gl`**: Lista el historial de commits en formato compacto.
  ```bash
  alias gl='git log --oneline'
  ```

#### Pasos para configurarlos:
1. Abrimos el archivo `~/.bashrc`:
   ```bash
   vi ~/.bashrc
   ```
2. Añadimos los alias al final del archivo:
   ```bash
   alias gs='git status'
   alias ga='git add'
   alias gc='git commit -m'
   alias gp='git push origin main'
   alias gd='git diff'
   alias gl='git log --oneline'
   ```
3. Aplicamos los cambios:
   ```bash
   source ~/.bashrc
   ```
4. Verificamos que los alias funcionan correctamente:
   ```bash
   alias
   ```

#### Ejemplo de uso:
Para subir cambios al repositorio:
```bash
# Verificamos el estado del repositorio
gs

# Añadimos un archivo modificado
ga workflow.md

# Confirmamos los cambios
gc "Actualizar documentación de alias"

# Subimos los cambios al remoto
gp
```

### 4. Documentación del workflow
- Creamos el archivo `workflow.md` para describir los pasos realizados.
- Este archivo detalla desde la configuración inicial hasta el uso de alias personalizados.

### 5. Subida de cambios al repositorio
Para subir los cambios realizados en este archivo:
1. Añadimos los cambios al área de staging:
   ```bash
   ga workflow.md
   ```
2. Confirmamos los cambios:
   ```bash
   gc "Añadir documentación sobre alias y workflow"
   ```
3. Subimos los cambios al repositorio remoto:
   ```bash
   gp
   ```

---

¡Listos para mostrar un flujo de trabajo ordenado y profesional! 😊


