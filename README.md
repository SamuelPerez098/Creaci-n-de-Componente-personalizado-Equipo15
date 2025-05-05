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


![Visor de Imágenes3]([https://private-user-images.githubusercontent.com/206933327/440260171-67c99dd5-b2af-4b00-aac2-b7cdd092f1b1.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjUzOTUsIm5iZiI6MTc0NjQyNTA5NSwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTcxLTY3Yzk5ZGQ1LWIyYWYtNGIwMC1hYWMyLWI3Y2RkMDkyZjFiMS5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYwNDU1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZDk5MTdkYTg2OGE1ZWYyMTMzYTExZWY4NzlkZGZiMjJjNzQwMTE0OGEwNjNlMzQzN2Q0MmIxOTcxMGMxYTVlNiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.SzVUcdGin4KQaklLJAwzhDfdHML3wA_4V9c2nsfbbfI](https://private-user-images.githubusercontent.com/206933327/440260171-67c99dd5-b2af-4b00-aac2-b7cdd092f1b1.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTcxLTY3Yzk5ZGQ1LWIyYWYtNGIwMC1hYWMyLWI3Y2RkMDkyZjFiMS5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MDg4MzNmZTI0MWIxMzIxYzkzYjBmMWExOGYwNDk0NTA0Yjg2NjRhMDExOTZkNTdmZWRjZTU3YjQ3M2Y0NDFlMiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.vyZFra2LdtpnKd0DBiS68LLUfnwstb7_4WGGUuWp8hA))


Al hacer click derecho sobre cualquier superficie dentro del componente nos aparecerán 4 opciones


![Visor de Imágenes4]([https://private-user-images.githubusercontent.com/206933327/440260168-9bfee3e0-7ee9-4792-a21e-113e487e413d.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjUzOTUsIm5iZiI6MTc0NjQyNTA5NSwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY4LTliZmVlM2UwLTdlZTktNDc5Mi1hMjFlLTExM2U0ODdlNDEzZC5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYwNDU1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MzExNWI0MTQ2ZTkyMmYxNTExM2JkYjc4YmMzMGQ4YzQ4YzM3NWE2NWEyYjNmZjRmNTdkODk2N2YzMTE4NGZhMSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.UiRvFdpB4zF_yINdYKcW4b_vJ2SVYKudQ8-Sm6Isjkg](https://private-user-images.githubusercontent.com/206933327/440260168-9bfee3e0-7ee9-4792-a21e-113e487e413d.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY4LTliZmVlM2UwLTdlZTktNDc5Mi1hMjFlLTExM2U0ODdlNDEzZC5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NmJkYjUwZjdjNjk0OGI5YjYxM2U5Y2JhYzE4ZTQwMmIxYmFkZjA0N2E4OTE5YWUyMmMzYmUxYjY4MzljMDM4MyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.1WwTc7um4Q7JyPUhhY3EJectgRSzwexMvEFy2wsp_Lc))

De esas 4 opciones
1- seleccionar imágenes abre una carpeta y selecciona imagen por imagen
2- seleccionar carpeta abre una carpeta y todas las imágenes dentro se añaden
3- sirve para rotar la imagen
4- elimina la imagen no deseada


![Visor de Imágenes5]([https://private-user-images.githubusercontent.com/206933327/440260172-02ff3c9c-2ca1-4ef4-902e-ad9b85f99ba3.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjUzOTUsIm5iZiI6MTc0NjQyNTA5NSwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTcyLTAyZmYzYzljLTJjYTEtNGVmNC05MDJlLWFkOWI4NWY5OWJhMy5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYwNDU1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NWMxZWFjOTYyODg4ZTFiZTNjOTZlZDc4NTQ1MDExOTViY2FiOTc3ODFmYWE0Y2YyNzk5MjA1NDNjNWY4NzYyMyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.5_QQj9ncBAatpWcJwtnPe9_7xJx4LJo78u50gjopYB4](https://private-user-images.githubusercontent.com/206933327/440260172-02ff3c9c-2ca1-4ef4-902e-ad9b85f99ba3.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTcyLTAyZmYzYzljLTJjYTEtNGVmNC05MDJlLWFkOWI4NWY5OWJhMy5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9N2UzYWIyZDQ4YmQzZWYyMjZkMDM4YWY3NDdmODQ3NzFkZmU1MmZmNDQ5ZWZiOTQ1NjM0N2JiYTQzMzg4MGQ0NCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.h573Jt0VjWToO9D5VDwGp8-Un2eWpYooRTVnn4oA6mc))



![Visor de Imágenes6]([https://private-user-images.githubusercontent.com/206933327/440260163-9a9a6276-9077-4ab8-baaa-59014f2d5a3d.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjUzOTUsIm5iZiI6MTc0NjQyNTA5NSwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTYzLTlhOWE2Mjc2LTkwNzctNGFiOC1iYWFhLTU5MDE0ZjJkNWEzZC5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYwNDU1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9OWNlMmE2MjI2MTZkYWVhODM5ZGJlMjUxMGMxYjIzYmM0NTUyNjQ0ODYwM2FlZTQ4NThhZjJhNzFiZDcxZmMzYyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.TqMibTXh3Lx2Ui8nv09Ib3caJ_PeVy3f3TGBv5_ej8A](https://private-user-images.githubusercontent.com/206933327/440260163-9a9a6276-9077-4ab8-baaa-59014f2d5a3d.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTYzLTlhOWE2Mjc2LTkwNzctNGFiOC1iYWFhLTU5MDE0ZjJkNWEzZC5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MDE0MmZmNzU5Yjk0ZGNjYTRjOTM4YTJkOTk1N2E4ZGJkODNlYjVhYTU2ZmQ4ZDI5OWM2NDI1MzM5MWFiZDQyYSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.YaGHYPG94yLv90wx41LNiTvXp5vogn57rwmPc0_LCko))


![Visor de Imágenes7]([https://private-user-images.githubusercontent.com/206933327/440260170-f3e973c8-6b75-4b57-8bc9-14e2612451c4.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjUzOTUsIm5iZiI6MTc0NjQyNTA5NSwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTcwLWYzZTk3M2M4LTZiNzUtNGI1Ny04YmM5LTE0ZTI2MTI0NTFjNC5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYwNDU1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9OGY4YjhkZjE2Yzc3ZmJiYjkyYWJkM2NmMGUyNWQzZmI1NDg2MzYxZjU1MmQzOWIxZjgyNzE5MDA0NDZmMTVhYiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.bc9hoqqLx4BnDeyyFJtba7O8dKsl2BBp36wh3V-6tSY](https://private-user-images.githubusercontent.com/206933327/440260170-f3e973c8-6b75-4b57-8bc9-14e2612451c4.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTcwLWYzZTk3M2M4LTZiNzUtNGI1Ny04YmM5LTE0ZTI2MTI0NTFjNC5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9YWZkZmI0MDM0ZTU3NWY0NTY5OTIxMGYzMWFhNjM5YjdmNTg3MGY3YzdiMGI1NjEyYzIwODYwNjVkZTNhZTQ2NSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.oL2m__JNeQcfRr9-rBKPhIq1JjXQcEhTfrH_TZIXWG4))

Al seleccionar la opción de cargar imagen seleccionamos una imagen 

![Visor de Imágenes8]([https://private-user-images.githubusercontent.com/206933327/440260165-1730fae9-06d0-4cf5-826a-77f737699ea2.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjUzOTUsIm5iZiI6MTc0NjQyNTA5NSwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY1LTE3MzBmYWU5LTA2ZDAtNGNmNS04MjZhLTc3ZjczNzY5OWVhMi5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYwNDU1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZTZhOWU2N2VkMDJhNzRjN2EwNWEwYmVlOTEzOGI5YjRlOGEyYjBhYTMxMWM0NzRlM2E1MjVkOTRjZmIxOWM5MCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.Kif1rwQ5QMJNw3dJRICuLMNq6djcBpF8s3VC6K9sbxY](https://private-user-images.githubusercontent.com/206933327/440260165-1730fae9-06d0-4cf5-826a-77f737699ea2.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY1LTE3MzBmYWU5LTA2ZDAtNGNmNS04MjZhLTc3ZjczNzY5OWVhMi5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9M2FiNmE5OWExODA0NDNiOTdkYjA1YmJkNDBhZmQ0MmNhN2EzMjgwMDhlOGI3MTgxNGZhMTBlZDBjYzFmNjJiZSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.HpVBQyKEiINsPABIBQadhY_uYhqLeUn9Iykguqi2ljk))

Nos va a mostrar la imagen seleccionada 


![Visor de Imágenes9]([https://private-user-images.githubusercontent.com/206933327/440260164-2dfd3aa2-81f7-4a75-898d-fba3b0d0e8d6.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjU2NzIsIm5iZiI6MTc0NjQyNTM3MiwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY0LTJkZmQzYWEyLTgxZjctNGE3NS04OThkLWZiYTNiMGQwZThkNi5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYwOTMyWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MDUwOWYyMGQ5MTc4ZDM1NGFkZjRlNTY0OTUyYzA3MDZhZDA1YmM2MzMxODRiZDFmMGIxNmM3YjQzMTkwNTNhMCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.Qyl0RNkyiutP1Me7rtgvrrgn4olLiC8SOa-ApjFnB3M](https://private-user-images.githubusercontent.com/206933327/440260164-2dfd3aa2-81f7-4a75-898d-fba3b0d0e8d6.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY0LTJkZmQzYWEyLTgxZjctNGE3NS04OThkLWZiYTNiMGQwZThkNi5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MDVkM2NlOGUxYjU1NWM5Njk1OGVjNTYxNjgyNGNjMDRlN2MwOTliZDk5YmNjODIzZTYwMzA2OTJkYWExZDZlMCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.39yh0wF11Nz7loNQxWyPsC5Nr-zjL6j6GSt5lqAHD5o))

Podremos rotar la imagen 

![Visor de Imágenes2]([https://private-user-images.githubusercontent.com/206933327/440260171-67c99dd5-b2af-4b00-aac2-b7cdd092f1b1.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjU5NTAsIm5iZiI6MTc0NjQyNTY1MCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTcxLTY3Yzk5ZGQ1LWIyYWYtNGIwMC1hYWMyLWI3Y2RkMDkyZjFiMS5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYxNDEwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZWQ1MjU5M2FiNWM0ZDlhMDM4MDQ5Nzc5ZGMyYzgyNzdmOGQ2NTFjM2QwZmUxNTFlNzkwZWJjYzIyMjU5Yjc2ZiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.sk8iJaAKGFgBNwuMHhVm0VX7MsmTDUkeuA1jU0rM0W8](https://private-user-images.githubusercontent.com/206933327/440260173-f460f9ad-1fbd-4173-b267-6cb92cf84b92.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTczLWY0NjBmOWFkLTFmYmQtNDE3My1iMjY3LTZjYjkyY2Y4NGI5Mi5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NDY3ZGI5ZDg4NWVkN2JlMDBlZTI3MWU0NDIyZmNhOTQ0MDJlOWU0N2Y4Y2ZjYjgzZDFkYzQ5OWIwZTM4NmExMyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.lQHnXDHElrYzEbjjxqNmPuUTkca-TcnHKjXy50lHMog))


Al seleccionar abrir carpeta de imágenes podemos seleccionar una carpeta que contenga varias imágenes  

![Visor de Imágenes2](https://private-user-images.githubusercontent.com/206933327/440260174-841dd0f8-ff87-49cc-92d1-95957d4a6b5a.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTc0LTg0MWRkMGY4LWZmODctNDljYy05MmQxLTk1OTU3ZDRhNmI1YS5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NGJjOTFlNjdjZmVjMzgwYTVlZDEyZDJjMDkxOTYxMjBjZDE3MWIxNjdiODA5ZWUyZjk0MDFmZTM1NzUwNGUwNCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.0C79stt79Ux8PxSrgEipmccXyCiVDr1kJHDKdYsj6u4)

Al darle abrir nos va a cargar automáticamente una serie de imágenes que podremos visualizar con la lista de imágenes en la parte baja del componente   

![Visor de Imágenes2](https://private-user-images.githubusercontent.com/206933327/440260167-11ffcf93-8e0b-4c70-b77b-ac2ec123992e.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY3LTExZmZjZjkzLThlMGItNGM3MC1iNzdiLWFjMmVjMTIzOTkyZS5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9Y2JlZjc4YjdjZTZlNGYyZWZiY2QyNzI5MDNmZjcyYzA4YWU4MzU4ZDI1ZTgzYjgzN2YyOTgwNjkxMWQ1ZDI0MiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.FfwotN4MgGX11IM1xHZtQv4OjdPEpaXbh8oZTq6yzik)

A las cuales podremos hacer zoom, recorrer con los botones laterales que se hacen visibles al pasar el cursor sobre ellos o seleccionando las miniaturas de las imágenes 

![Visor de Imágenes2](https://private-user-images.githubusercontent.com/206933327/440260166-495414a6-7c96-46c7-a4ba-95793567bf82.jpeg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDY0MjYzMzAsIm5iZiI6MTc0NjQyNjAzMCwicGF0aCI6Ii8yMDY5MzMzMjcvNDQwMjYwMTY2LTQ5NTQxNGE2LTdjOTYtNDZjNy1hNGJhLTk1NzkzNTY3YmY4Mi5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MDVUMDYyMDMwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MzM5YTJiNjg2M2IwOTU5MzZmZmI1N2U4NzJkYTYyZTgxZjQ1MWI4ZDdjOTFiN2QzODM3Y2IyODcwZWY2NzBkYyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.r9A9x_15UiVIh2QEDUzxu-17kUh6EwHxlL7F_cQnhts)

