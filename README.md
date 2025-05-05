# Creaci-n-de-Componente-personalizado-Equipo15
Un componente básico de un visor de imágenes en JAVA 

/**
 * VisorImagenes.java
 *
 * Este programa implementa componente modificado simulando un    visor de imágenes usando Swing en Java.
 * Permite abrir imágenes desde el sistema de archivos y mostrarlas en un JFrame.
 *
 * Autor: Braulio Mendoza Vargas y Samuel Pérez Ramirez
 * Fecha: Mayo 2025
 */

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

                    // Escalado proporcional a la ventana
                    Image imagenEscalada = imagen.getImage().getScaledInstance(
                            etiquetaImagen.getWidth(), etiquetaImagen.getHeight(), Image.SCALE_SMOOTH);
                    etiquetaImagen.setIcon(new ImageIcon(imagenEscalada));
                    etiquetaImagen.setText(null);
                }
            }
        });

        // Agrega los componentes al JFrame
        add(etiquetaImagen, BorderLayout.CENTER);
        add(botonAbrir, BorderLayout.SOUTH);

        setSize(600, 400);
        setLocationRelativeTo(null); // Centrar ventana
        setVisible(true);
    }

    /**
     * Método principal para ejecutar el visor de imágenes.
     */
    public static void main(String[] args) {
        new VisorImagenes();
    }

    /**
     * Métodos y propiedades relevantes:
     *
     * - JLabel: utilizado para mostrar la imagen o texto inicial.
     *   Métodos: setIcon(Icon), setText(String), setHorizontalAlignment(int)
     *
     * - JButton: botón que dispara la acción de seleccionar imagen.
     *   Métodos: addActionListener(ActionListener)
     *
     * - JFileChooser: componente para seleccionar archivos del sistema.
     *   Métodos: setFileFilter(FileFilter), showOpenDialog(Component), getSelectedFile()
     *
     * - ImageIcon: clase que representa una imagen cargada.
     *   Métodos: getImage()
     *
     * - Image: permite escalar imágenes con getScaledInstance(ancho, alto, tipo)
     */

    /**
     * Breve explicación del componente:
     *
     * Este visor de imágenes usa una interfaz sencilla de Swing. Al hacer clic derecho en cualquier parte,
     * se abre un diálogo con múltiples opciones que te permiten seleccionar una imagen, una carpeta de imágenes, rotar o eliminar la imagen,
     * que luego se muestra en el área central.
     * La imagen se escala automáticamente para ajustarse al tamaño del área de visualización.
     */

    /**
     * Instrucciones de uso:
     *
     * 1. Crea un nuevo proyecto
     * 2. agrega un JFrame
     * 3. añade el componente
     * 4. Ejecuta el programa
     * 5. Presiona el click derecho en la opción "Abrir Imagen" y selecciona una imagen válida (JPG, PNG o GIF).
     * 6. La imagen se mostrará en la ventana.
     */
}
