# Creacion-de-Componente-personalizado-Equipo15
Un componente básico de un visor de imágenes en JAVA 

##  Estructura del Código

###  Clase `Registro`

Contiene los datos personales de un usuario, necesarios para la generación del correo institucional, además de representar a una persona con los siguientes atributos:

#### Atributos:
- `String nombre` – Primer nombre del usuario.
- `String segundoNombre` – Segundo nombre, si existe.
- `String apellidoPaterno` – Apellido paterno del usuario.
- `String apellidoMaterno` – Apellido materno del usuario.
- `String fechaNacimiento` – Fecha en formato `YYYYMMDD`.
- `String institucion` – Nombre de la institución.
- `String correo` – Correo generado (posteriormente).

#### Constructor:
```java```
public Registro(String nombreCompleto, String apellidoPaterno, String apellidoMaterno, String fechaNacimiento, String institucion)

##  Validaciones

- Lanza `IllegalArgumentException` si cualquier parámetro obligatorio es `null`.
- Se separa `nombreCompleto` en `nombre` y `segundoNombre`.
- El campo `correo` se inicializa como cadena vacía.

---

##  Clase final `GeneradorCorreo`

###  Variables

- `private static final HashSet<String> correosExistentes`  
  Para evitar correos duplicados.

- `public static final ArrayList<Registro> registros`  
  Lista de registros creados.

###  Métodos

#### `public static String generarCorreo(Registro registro)`

- Genera un correo electrónico único.
- Valida campos no nulos y solo letras.
- Añade `segundoNombre` o `fechaNacimiento` para asegurar unicidad.

#### `public static void registrarUsuario(...)`

- Valida campos, crea un registro y genera correo.
- **Parámetros:**  
  `nombre`, `apellidoP`, `apellidoM`, `fechaNacimiento`, `institucion`

#### `public static void mostrarCorreos()`

- Muestra todos los correos generados usando `JOptionPane`.

---

##  Clase `Prueba` (Interfaz gráfica)

Interfaz Swing para registrar usuarios y generar correos.

###  Imports
```java```
import Metodos.GeneradorCorreo;
import Metodos.Registro;

## 🧱 Componentes

| Componente Swing            | Variable       | Descripción                    |
|----------------------------|----------------|--------------------------------|
| `JTextField`               | `Nombretxt`    | Campo nombre completo          |
| `JTextField`               | `ApellidoPtxt` | Apellido paterno               |
| `JTextField`               | `ApellidoMtxt` | Apellido materno               |
| `JTextField`               | `Dominiotxt`   | Dominio o institución          |
| `JDateChooser` (lib externa) | `Fecha`      | Selector de fecha de nacimiento |
| `JButton`                  | `generar`      | Botón para generar correo      |
| `JButton`                  | `ver`          | Ver correos generados          |

---

##  Métodos clave

### `generarActionPerformed(...)`
- Toma datos, valida, genera correo y muestra resultado.

### `verActionPerformed(...)`
- Muestra lista de correos registrados usando `JOptionPane`.

---

##  Clase `mijpanelmodificado` (Galería de Imágenes)

Componente Swing que permite visualizar, seleccionar, rotar y eliminar imágenes, con una tira de miniaturas y zoom.

---

##  Funcionalidades principales

-  Cargar carpeta o múltiples imágenes.
-  Zoom con scroll del mouse.
- Rotar imagen actual.
-  Eliminar imagen.
- ⬅ Navegación entre imágenes.
-  Miniaturas en tira inferior.

---

##  Interfaz gráfica

- `JLabel imageLabel`: Muestra imagen principal.
- `JScrollPane imageScrollPane`: Panel con scroll de imagen.
- `JPanel galleryPanel`: Contenedor de miniaturas.
- `JScrollPane galleryScrollPane`: Scroll horizontal para miniaturas.
- Botones invisibles `⯇ ⯈` para navegación.

---

##  Eventos

- `MouseWheelListener`: Zoom.
- `MouseAdapter`: Menú contextual (clic derecho).
- `MouseMotionAdapter`: Mostrar botones al acercarse a los márgenes.
- `ComponentListener`: Redimensionamiento.

---

##  Métodos clave

### `seleccionarImagenes()`
- Abre un `JFileChooser` para seleccionar múltiples imágenes.

### `seleccionarCarpeta()`
- Abre un directorio completo con imágenes.

### `precargarImagenes(String ruta)`
- Carga automáticamente imágenes desde una ruta fija.

### `rotateImage()`
- Rota la imagen actual (pendiente de implementación completa).

### `displayImage()`
- Escala la imagen según el `zoomFactor` y la muestra.

### `updateGallery()`
- Actualiza la tira de miniaturas debajo de la imagen.

### `deleteImage()`
- Elimina la imagen actual del arreglo y actualiza la vista.

---

##  Requisitos

- Java 8 o superior  
- `com.toedter.calendar` para `JDateChooser`  
- Swing (incluido en el JDK)





## Aquí el código completo

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.File;
import javax.swing.filechooser.FileNameExtensionFilter;

public class VisorImagenes extends JFrame {

    private JLabel etiquetaImagen; // Componente para mostrar la imagen cargada
    private JButton botonAbrir;    // Botón para abrir el selector de archivos
    private JFileChooser selectorArchivos; // Componente para seleccionar imágenes

    /**
     * Constructor principal: configura la interfaz gráfica y sus componentes.
     */
    public VisorImagenes() {
        super("Visor de Imágenes");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Inicialización de componentes
        etiquetaImagen = new JLabel("No hay imagen cargada", SwingConstants.CENTER);
        botonAbrir = new JButton("Abrir Imagen");
        selectorArchivos = new JFileChooser();

        // Filtro para mostrar solo archivos de imagen (JPG, PNG, GIF)
        FileNameExtensionFilter filtro = new FileNameExtensionFilter(
                "Imágenes (JPG, PNG, GIF)", "jpg", "jpeg", "png", "gif");
        selectorArchivos.setFileFilter(filtro);

        // Evento al presionar el botón
        botonAbrir.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                int resultado = selectorArchivos.showOpenDialog(VisorImagenes.this);
                if (resultado == JFileChooser.APPROVE_OPTION) {
                    File archivo = selectorArchivos.getSelectedFile();
                    ImageIcon imagen = new ImageIcon(archivo.getAbsolutePath());

                    // Escalado proporcional al tamaño del label
                    Image imagenEscalada = imagen.getImage().getScaledInstance(
                            etiquetaImagen.getWidth(), etiquetaImagen.getHeight(), Image.SCALE_SMOOTH);
                    etiquetaImagen.setIcon(new ImageIcon(imagenEscalada));
                    etiquetaImagen.setText(null); // Elimina el texto predeterminado
                }
            }
        });

        // Agrega los componentes al JFrame
        add(etiquetaImagen, BorderLayout.CENTER);
        add(botonAbrir, BorderLayout.SOUTH);

        setSize(600, 400);
        setLocationRelativeTo(null); // Centra la ventana
        setVisible(true);
    }

    /**
     * Método principal para ejecutar el visor de imágenes.
     */
    public static void main(String[] args) {
        new VisorImagenes();
    }

    /**
     * Métodos y componentes relevantes:
     *
     * - JLabel: Muestra la imagen o texto inicial.
     *   Métodos: setIcon(Icon), setText(String), setHorizontalAlignment(int)
     *
     * - JButton: Dispara la acción para seleccionar una imagen.
     *   Métodos: addActionListener(ActionListener)
     *
     * - JFileChooser: Permite seleccionar archivos del sistema.
     *   Métodos: setFileFilter(FileFilter), showOpenDialog(Component), getSelectedFile()
     *
     * - ImageIcon: Representa una imagen cargada.
     *   Métodos: getImage()
     *
     * - Image: Escala imágenes usando getScaledInstance(ancho, alto, tipo)
     */

    /**
     * Descripción del componente:
     *
     * Este visor de imágenes usa una interfaz sencilla con Swing. Permite al usuario seleccionar una imagen
     * desde el sistema de archivos, que luego se muestra en el área central de la ventana.
     * La imagen se escala automáticamente para ajustarse al área de visualización.
     */

    /**
     * Instrucciones de uso:
     *
     * 1. Crea un nuevo proyecto en tu IDE.
     * 2. Agrega esta clase con extensión .java.
     * 3. Ejecuta el programa.
     * 4. Presiona el botón "Abrir Imagen" y selecciona una imagen válida (JPG, PNG o GIF).
     * 5. La imagen se mostrará dentro de la ventana.
     */
}
```


##  Link del video de Youtube
https://www.youtube.com/watch?v=lrKfkxJZg1k

### Vista previa del visor de imágenes

![Visor de Imágenes](https://private-user-images.githubusercontent.com/206933327/440260166-495414a6-7c96-46c7-a4ba-95793567bf82.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjQ2MDgsIm5iZiI6MTc0NjQyNDMwOCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY2LTQ5NTQxNGE2LTdjOTYtNDZjNy1hNGJhLTk1NzkzNTY3YmY4Mi5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDU1MTQ4WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZDM0NTBiYmZhY2I3MGM5NWZjOTM3OWNhMmIxNjQ2MjVmMGVhNTM3MWU2NmM0OTUwOWM1ZjBkZWU2NWY1YWRiYiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.46sqKLrGS4IQ_jRL88pKNsmEgfXwSQMD61JecdE4uHA)
