# Apuntes de Clase - Tópicos Avanzados de Programación
## Unidad I: Interfaz Gráfica de Usuario (GUI).

Una **Interfaz Gráfica de Usuario (GUI)** es el medio mediante el cual un usuario interactúa con un sistema informático utilizando elementos visuales como ventanas, botones, cuadros de texto, imágenes y menús.

A diferencia de una interfaz de línea de comandos (CLI), la GUI permite una interacción más intuitiva mediante el uso del mouse, teclado o pantallas táctiles.
En el desarrollo moderno de software, las interfaces gráficas son fundamentales porque:

. Mejoran la experiencia del usuario.

. Facilitan la interacción con el sistema.

. Permiten diseñar aplicaciones más accesibles y visuales.

. Hacen posible el desarrollo multiplataforma.

En el caso de Flet, la GUI se construye mediante controles (controls) que representan los elementos visuales.

---

## 1.1 Creación de Interfaz Gráfica para Usuarios

Una **interfaz gráfica de usuario (GUI)** es el conjunto de elementos visuales que permiten al usuario interactuar con una aplicación mediante ventanas, botones, formularios y otros componentes visuales, en lugar de usar comandos de texto.

### ¿Qué es Flet?

**Flet** es un framework de código abierto que permite desarrollar aplicaciones web, de escritorio y móviles usando únicamente **Python**, sin necesidad de conocimientos previos en desarrollo frontend. Internamente, Flet usa **Flutter** (el framework de Google), lo que le otorga una apariencia moderna y un rendimiento nativo.

Una aplicación en Flet puede ejecutarse como:
- Aplicación de escritorio nativa (Windows, macOS, Linux)
- Aplicación web en el navegador
- Aplicación móvil

### Estructura básica de una app en Flet

```python
import flet as ft

def main(page: ft.Page):
    page.title = "Mi primera app"
    page.add(ft.Text("¡Hola, mundo!"))

ft.app(target=main)
```

Toda aplicación Flet parte de una función `main` que recibe un objeto `page` (la ventana principal). Los controles se agregan a la página con `page.add()` y los cambios se envían con `page.update()`.

### Propiedades de la página (Page)

| Propiedad | Descripción |
|-----------|-------------|
| `page.title` | Título de la ventana |
| `page.bgcolor` | Color de fondo |
| `page.padding` | Espaciado interno |
| `page.theme_mode` | Tema claro u oscuro |

---

## 1.2 Tipos de Eventos

Un **evento** es una acción que ocurre cuando el usuario interactúa con un control, por ejemplo: hacer clic en un botón, escribir en un campo de texto o seleccionar un elemento de una lista.

En Flet, los eventos se definen mediante argumentos que comienzan con `on_*` en cada control.

### Tipos de eventos más comunes en Flet

| Evento | Control | Descripción |
|--------|---------|-------------|
| `on_click` | ElevatedButton, IconButton | Se ejecuta al hacer clic |
| `on_change` | TextField, Dropdown, Checkbox | Se ejecuta al cambiar el valor |
| `on_submit` | TextField | Se ejecuta al presionar Enter |
| `on_focus` | TextField | Se ejecuta al enfocar el campo |
| `on_blur` | TextField | Se ejecuta al perder el foco |
| `on_nav_change` | NavigationBar | Se ejecuta al cambiar de pestaña |

### Ejemplo de evento `on_click`

```python
def boton_click(e):
    print("¡Botón presionado!")

btn = ft.ElevatedButton(text="Presionar", on_click=boton_click)
page.add(btn)
```

Cada función manejadora de eventos recibe un argumento `e` (evento), que contiene información sobre el control que lo disparó y la acción realizada.

---

## 1.3 Manejo de Eventos

El **manejo de eventos** es el proceso mediante el cual la aplicación responde a las acciones del usuario. En Flet, esto se logra definiendo funciones que se asignan a los eventos de los controles.

### Flujo del manejo de eventos en Flet

```
Usuario interactúa → Control detecta el evento → Se llama al handler → Se actualiza la UI
```

### Ejemplo: Actualizar texto al hacer clic

```python
import flet as ft

def main(page: ft.Page):
    txt = ft.Text("Esperando...")

    def cambiar_texto(e):
        txt.value = "¡Evento disparado!"
        page.update()  # Siempre actualizar la página tras modificar controles

    btn = ft.ElevatedButton("Cambiar texto", on_click=cambiar_texto)
    page.add(btn, txt)

ft.app(target=main)
```

### Buenas prácticas en el manejo de eventos

- Siempre llamar a `page.update()` después de modificar controles para reflejar los cambios en pantalla.
- Validar los datos del usuario dentro del handler antes de procesarlos.
- Mantener los handlers cortos y delegar la lógica a funciones auxiliares.
- Usar `e.control` dentro del handler para acceder al control que disparó el evento.

### Ejemplo: Validación dentro de un handler

```python
def enviar(e):
    if not campo.value:
        error.value = "El campo no puede estar vacío"
    else:
        error.value = ""
        # procesar datos...
    page.update()
```

---

## 1.4 Manejo de Componentes Gráficos de Control

Los **componentes gráficos de control** son los elementos visuales que conforman la interfaz y permiten al usuario ingresar, seleccionar o visualizar datos.

Flet implementa un modelo de UI **imperativo**: el desarrollador construye y modifica los controles manualmente, actualizando sus propiedades cuando sea necesario.

### Controles de entrada de datos

#### TextField (Campo de texto)
Permite al usuario ingresar texto libre.
```python
campo = ft.TextField(label="Nombre", border_color="blue")
```

#### Dropdown (Lista desplegable)
Permite seleccionar una opción de una lista predefinida.
```python
dd = ft.Dropdown(
    label="Carrera",
    options=[
        ft.dropdown.Option("Ingeniería en Sistemas"),
        ft.dropdown.Option("Ingeniería Civil"),
    ]
)
```

#### RadioGroup y Radio (Botones de opción)
Permiten elegir una sola opción dentro de un grupo.
```python
grupo = ft.RadioGroup(
    content=ft.Row([
        ft.Radio(value="Masculino", label="Masculino"),
        ft.Radio(value="Femenino", label="Femenino"),
    ])
)
```

#### Checkbox (Casilla de verificación)
Permite al usuario activar o desactivar una opción booleana.
```python
check = ft.Checkbox(label="Acepto términos y condiciones")
```

### Controles de visualización

#### Text (Texto)
Muestra texto estático o dinámico en pantalla.
```python
texto = ft.Text("Hola mundo", color="blue", size=20, weight=ft.FontWeight.BOLD)
```

#### ElevatedButton (Botón)
Botón con elevación que ejecuta una acción al hacer clic.
```python
btn = ft.ElevatedButton("Enviar", on_click=mi_funcion)
```

#### AlertDialog (Ventana modal)
Muestra un cuadro de diálogo emergente con información o confirmación.
```python
dialog = ft.AlertDialog(
    title=ft.Text("Éxito"),
    content=ft.Text("Datos guardados correctamente"),
    actions=[ft.TextButton("Cerrar", on_click=lambda e: cerrar())]
)
page.overlay.append(dialog)
dialog.open = True
page.update()
```

### Controles de diseño (Layout)

| Control | Descripción |
|---------|-------------|
| `ft.Row` | Organiza controles en fila horizontal |
| `ft.Column` | Organiza controles en columna vertical |
| `ft.Container` | Contenedor con estilo, padding, bordes y color |
| `ft.Stack` | Apila controles uno sobre otro |

### Propiedades comunes de los controles

Todos los controles en Flet comparten propiedades base:

- `visible`: Muestra u oculta el control (`True`/`False`)
- `disabled`: Habilita o deshabilita la interacción (`True`/`False`)
- `expand`: Hace que el control ocupe el espacio disponible
- `width` / `height`: Dimensiones del control

---

## 📖 Bibliografía (Formato APA)

Flet. (s.f.). *Introduction*. Flet Documentation. https://flet.dev/docs/

Flet. (s.f.). *Flet controls*. Flet Documentation. https://flet.dev/docs/getting-started/flet-controls/

Flet. (s.f.). *Custom controls*. Flet Documentation. https://flet.dev/docs/getting-started/custom-controls/

Flet-dev. (s.f.). *Flet: Build multi-platform apps in Python powered by Flutter* [Repositorio de software]. GitHub. https://github.com/flet-dev/flet

Well, L. (2025, diciembre 13). *Getting started with Flet for GUI development*. Python GUIs. https://www.pythonguis.com/tutorials/getting-started-flet/

Rang, S. (2024, octubre 4). *A beginner's guide to Flet*. Medium - Top Python Libraries. https://medium.com/top-python-libraries/a-beginners-guide-to-flet-36c98f966011

LabDeck. (2025). *Flet tutorial*. https://labdeck.com/flet-tutorial/

DeepWiki. (2026, enero 24). *UI controls reference - flet-dev/flet*. https://deepwiki.com/flet-dev/flet/2.3-ui-controls-reference

---
