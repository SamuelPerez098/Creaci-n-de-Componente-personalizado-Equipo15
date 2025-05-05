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

Cuando se ejecuta el jframe, se nos va a mostrar el componente así

![Image](https://github.com/user-attachments/assets/0f4800e6-b1f5-45dd-b170-a1ef84c6a38a)

Al hacer click derecho sobre cualquier superficie dentro del componente nos aparecerán 4 opciones

![Image](https://github.com/user-attachments/assets/a150c1df-6196-49a4-9677-e7c3cb440fe4)

De esas 4 opciones
1- seleccionar imágenes abre una carpeta y selecciona imagen por imagen
2- seleccionar carpeta abre una carpeta y todas las imágenes dentro se añaden
3- sirve para rotar la imagen
4- elimina la imagen no deseada


![Image](https://github.com/user-attachments/assets/5709b231-9d7a-4c61-b85a-925354c4425e)

![Image](https://github.com/user-attachments/assets/fdad0a39-d904-487f-8f9e-d47f36b39d6d)

![Image](https://github.com/user-attachments/assets/9b004741-7930-4a95-90aa-744eb806242d)


Al seleccionar la opción de cargar imagen seleccionamos una imagen 

![Image](https://github.com/user-attachments/assets/864a148f-44b1-4d41-93f1-976378654245)
Nos va a mostrar la imagen seleccionada 

![Image](https://github.com/user-attachments/assets/002bbbdc-598f-46d3-94a9-eef3f2174467)

Podremos rotar la imagen 

![Image](https://github.com/user-attachments/assets/f40d0176-ba23-4a3f-bc30-1504aca43905)
Al seleccionar abrir carpeta de imágenes podemos seleccionar una carpeta que contenga varias imágenes  


![Image](https://github.com/user-attachments/assets/45d278a5-a19c-4db9-abd7-2eb1b729beee)
Al darle abrir nos va a cargar automáticamente una serie de imágenes que podremos visualizar con la lista de imágenes en la parte baja del componente   



![Image](https://github.com/user-attachments/assets/8fb87d23-ebb9-4c25-ba7f-526bd7ebc488)

A las cuales podremos hacer zoom, recorrer con los botones laterales que se hacen visibles al pasar el cursor sobre ellos o seleccionando las miniaturas de las imágenes 

![Image](https://github.com/user-attachments/assets/764ca82d-a0e3-4518-8c0f-18a5fd370394)

