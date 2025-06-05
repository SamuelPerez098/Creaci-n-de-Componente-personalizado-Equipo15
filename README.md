# Creacion-de-Componente-personalizado-Equipo15
Un componente básico de un visor de imágenes en JAVA 

##  Clase `JpanelMOD` (Galería de Imágenes)

componente swing personalizado en Java Swing para visualizar imágenes desde una carpeta cargada de imagenes o simplemente cargar imagen por imagen. Permite hacer zoom con la rueda del mouse, arrastrar con el cursor y navegar por múltiples imágenes mediante dos botones de izquierda a derecha y te permite ver el total de imagenes con una tira abajo de la imagen que se muestra.

## Funciones Principales
- Ejecuta la clase principal.
- Usa el componente JPanelMOD
- Insetar imagenes ya sea una por una o cragando una carpeta.
- Haz zoom con la rueda del mouse.
- Arrastra la imagen haciendo clic y moviéndola.
- puedes borrar imagenes.
- puedes rotar imagenes. 
- navega entre imagenes para visualizarlas.

## Interfaz grafica
- JLabel imageLabel: Muestra imagen principal.
- JScrollPane imageScrollPane: Panel con scroll de imagen.
- JPanel galleryPanel: Contenedor de miniaturas.
- JScrollPane galleryScrollPane: Scroll horizontal para miniaturas.
- Botones invisibles ⯇ ⯈ para navegación.

## Explicacion
### Componente JpanelMOD
JpanelMOD es un componente personalizado en Java Swing que permite visualizar y administrar imágenes con diversas funcionalidades como zoom, rotación, navegación, galería de miniaturas, carga desde carpeta, selección múltiple y eliminación.

### Funcionalidades principales:
- Carga de imágenes desde archivos individuales o carpetas completas.
- Visualización escalable con zoom (incluye scroll automático al punto del cursor).
- Navegación entre imágenes (siguiente/anterior).
- Rotación de imágenes (90°).
- Eliminación de imágenes seleccionadas.
- Galería de miniaturas con selección directa.
- Soporte para drag & drop visual en NetBeans con propiedades personalizables.
###  Métodos destacados:
- setRutaImagenes(String ruta): Establece la carpeta desde donde se cargan las imágenes.
- precargarImagenes(String ruta): Carga las imágenes de la carpeta indicada.
- displayImage(): Muestra la imagen actual adaptada al tamaño del panel y zoom.
- displayImageConZoom(Point punto): Muestra la imagen actual con zoom centrado en el cursor.
- rotateImage(): Rota la imagen actual 90 grados.
- deleteImage(): Elimina la imagen actual.
- zoomImage(int wheelRotation): Aplica zoom in/out con la rueda del ratón.
- updateGallery(): Actualiza la galería de miniaturas.

### Propiedades personalizables (desde el editor visual de NetBeans):
- tamaventanaaaaaa1: Tamaño del panel de visualización de imagen.
- tamaventanaaaaaa2: Altura del panel de galería.
- colorventanaaaaa: Color de fondo de la imagen.
- colorbordeee: Color del borde de las miniaturas.
- zoomInicial: Zoom inicial al cargar la imagen.
- indiceImagenActual: Índice de la imagen actual mostrada.


## Métodos clave de JpanelMOD
### Navegación y visualización
- void displayImage(): Muestra la imagen actual, ajustándola al tamaño del panel con el zoom definido.
- void displayImageConZoom(Point punto): Muestra la imagen con zoom centrado en la posición del cursor (usado con scroll del mouse).
- void siguienteImagen(): Avanza a la siguiente imagen (si existe).
- void anteriorImagen(): Retrocede a la imagen anterior (si existe).
### Zoom y rotación
- void zoomImage(int wheelRotation): Aplica zoom in/out según la dirección del scroll del mouse.
- void rotateImage(): Rota la imagen actual 90 grados en sentido horario.
### Carga de imágenes
- void setRutaImagenes(String ruta): Establece la carpeta base para buscar imágenes.
- void precargarImagenes(String ruta): Carga todas las imágenes desde una carpeta y reinicia el índice actual.
- void cargarImagenesDesdeCarpetaPorId(String rutaBase, int idProducto): Carga imágenes desde una subcarpeta cuyo nombre es el idProducto (por ejemplo, imagenes/15/).
### Eliminación y manejo
- void deleteImage(): Elimina la imagen actualmente visible del disco y la lista.
- void updateGallery(): Actualiza la galería de miniaturas, útil después de agregar o eliminar imágenes.
### Personalización visual
- void setColorVentanaaaaaa(Color color): Cambia el color de fondo del panel principal.
- void setColorBordeee(Color color): Cambia el color del borde en las miniaturas seleccionadas.
- void setZoomInicial(double zoom): Define el valor de zoom inicial al mostrar imágenes
---

##  Eventos

- `MouseWheelListener`: Zoom.
- `MouseAdapter`: Menú contextual (clic derecho).
- `MouseMotionAdapter`: Mostrar botones al acercarse a los márgenes.
- `ComponentListener`: Redimensionamiento.

---

## Eventos de mouse
### MouseListener

#### mouseClicked:
Se usa para:
- Detectar clic en la imagen principal (marcar selección si hay múltiples).
- Detectar doble clic en miniaturas para visualizarla en grande.
---
### MouseWheelListener
#### mouseWheelMoved:
- Controla el zoom al hacer scroll con el mouse sobre la imagen.
---
### MouseMotionListener

#### mouseMoved:
- Se usa para detectar si el cursor pasa sobre una miniatura, aplicando efectos visuales.

---

## Formato de imagenes compatibles
- .jpg
- .jpeg
- .png
- .bmp 
- .gif
---  




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


## Tutorial de como poner el .jar en la paleta!!!!! (se usó netBeans para este proyecto)




primero, vamos a agregar el componente a la paleta de componentes
el primer paso es descargar el .jar que tiene de nombre componentemodificadooo.jar 

![Image](https://github.com/user-attachments/assets/72a46c66-daff-4e27-805b-efeac82aaf79)

luego nos vamos a NetBeans y marcamos las opciones que se muestran en la imagen siguiente 

![Image](https://github.com/user-attachments/assets/e53b1a3c-f7a5-44e1-855d-d239d8ae6ccd)

luego nos va a aparecer la ventana de Palette Manager, ahí le vamos a dar en la opción New Category..., no se ve mucho pero es la que esta marcada de azul

![Image](https://github.com/user-attachments/assets/049c4da0-37d7-477f-a6a3-ea428b4e6a6e)

al darle esa opción de nueva categoria, nos aparecerá otra ventana en la cual nos dejara ingresar el nombre de la categori, ustedes pueen ponerle como gusten y le dan ok

![Image](https://github.com/user-attachments/assets/fd56b75d-e991-43bd-897b-917b4f02c280)

una vez que le dan ok, se va a mostrar la nueva carpeta con las demás categorias, la seleccionan y le van a dar "Add from JAR.."

![Image](https://github.com/user-attachments/assets/0fd8ccbd-395f-4829-813a-831fa1cde295)

después van a buscar el jar que descargaron anteriormente que normalmente esta en descargas, lo seleccionan y le dan next

![Image](https://github.com/user-attachments/assets/1ad1396f-77eb-4cd8-b4ad-7516041b219d)

de ahí se les van a mostrar 3 nombres, le van a dar el de enmedio o el que se llama mijpanelmodificado y pulsan next

![Image](https://github.com/user-attachments/assets/f53bf8f1-8460-4632-b3a6-84e3e18ec1ac)

y listo, ya está en la paleta, ahora para usarlo crean un jframe y arrastran el componente de la paleta al jframe.

## Uso del componente!!!



Ahora cuando se ejecuta el jframe, se nos va a mostrar el componente así

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

