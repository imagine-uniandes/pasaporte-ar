# Pasaporte AR - Family Fest Uniandes

Este proyecto fue desarrollado para el evento "Family Fest" de la Universidad de los Andes. El objetivo era ofrecer una experiencia interactiva a los aspirantes y sus familias, permitiéndoles conocer diferentes aspectos de la Ingeniería de Sistemas a través de la Realidad Aumentada (AR).

## Descripción del Proyecto

En el evento, se establecieron varias estaciones temáticas relacionadas con la Ingeniería de Sistemas. En cada estación, los participantes recibían un sticker. Cada sticker funcionaba como un marcador de Realidad Aumentada.

Al escanear estos stickers con un dispositivo móvil (a través de esta aplicación web), los usuarios podían ver un modelo 3D de Séneca realizando una actividad relacionada con el tema de la estación donde obtuvieron el sticker.

La aplicación también incluye una interfaz de usuario que muestra el progreso, indicando cuántos de los 5 stickers/marcadores diferentes han sido detectados.

## Cómo Funciona

La aplicación está construida como una página web única (`index.html`) que utiliza las siguientes tecnologías y conceptos:

1.  **A-Frame:** Un framework web para construir experiencias de Realidad Virtual (VR) y Realidad Aumentada (AR) directamente en el navegador.
2.  **MindAR:** Una librería de JavaScript para el reconocimiento de imágenes en AR, integrada con A-Frame. Utiliza un archivo compilado (`seneca.mind`) que contiene los datos de los 5 marcadores de imagen.
3.  **Modelos 3D (glTF):** Los modelos de Séneca se cargan en formato `.gltf`. Cada uno de los 5 marcadores está asociado a un modelo 3D específico que se superpone en el mundo real cuando el marcador es detectado por la cámara.
4.  **Interfaz de Progreso:**
    *   Una barra de progreso y un texto indican cuántos de los 5 marcadores han sido encontrados.
    *   Una imagen de Séneca corriendo (`seneca_corriendo.png`) se mueve a lo largo de la barra de progreso y comienza una animación de rebote una vez que se detecta al menos un marcador.

## Flujo de Creación de Modelos 3D

Los modelos 3D de Séneca utilizados en este proyecto siguieron un proceso de creación específico:

1.  **Generación Inicial:** Los modelos base de Séneca se generaron a partir de una imagen 2D utilizando la herramienta [Hyper3D.ai](https://hyper3d.ai/).
2.  **Corrección y Modelado:** Los modelos generados requirieron correcciones, como la optimización del número de polígonos y la reparación de pequeños errores. Este trabajo se realizó utilizando [Blender](https://www.blender.org/).
3.  **Animación:** Las animaciones de Séneca se crearon utilizando [Mixamo](https://www.mixamo.com/). Esta plataforma permite generar un "rig" (esqueleto) para el modelo y aplicar animaciones predeterminadas.
    *   **NOTA DE EXPORTACIÓN DESDE MIXAMO:** Para asegurar la compatibilidad con MindAR y evitar que el modelo se "rompa" (mostrando su interior) durante la animación, es crucial seguir estos pasos:
        1.  Exportar el modelo animado desde Mixamo en formato `.dae` (Collada).
        2.  Importar y unir los archivos del `.dae` en Blender.
        3.  Proceder con los siguientes pasos desde Blender. *Exportar directamente como `.fbx` desde Mixamo puede causar problemas de visualización del modelo en la aplicación de AR.*
4.  **Añadir Objetos (si es necesario):** En algunos casos, se añadieron objetos que Séneca sostiene. Este proceso se realizó en Blender, siguiendo guías como este tutorial: [Blender Tutorial - Parent Objects](https://www.youtube.com/watch?v=3jCE2Va0ChM).
5.  **Exportación Final:** Finalmente, los modelos completos (con animaciones y objetos) se exportaron desde Blender en formato `.gltf`. Es importante especificar durante la exportación que se incluyan las texturas dentro del archivo `.gltf` para una correcta visualización.

## Requisitos para Ejecutar el Proyecto

1.  **Servidor Web:** La carpeta completa del proyecto debe ser alojada en un servidor web. Puede ser cualquier servidor web simple (por ejemplo, usando Python `http.server`, Node.js `http-server`, o Apache/Nginx).
2.  **HTTPS (Certificado SSL):** **Este es un requisito crítico.** La aplicación **no funcionará** si se sirve a través de HTTP. Debe ser servida a través de **HTTPS**. Esto se debe a que los navegadores modernos requieren una conexión segura (HTTPS) para permitir el acceso a la cámara del dispositivo, lo cual es esencial para la funcionalidad de AR.

## Estructura del Código Principal (`index.html`)

*   **Cabecera (`<head>`):**
    *   Importa las librerías necesarias: A-Frame, A-Frame Extras (para `animation-mixer`) y MindAR.
    *   Define los estilos CSS para la interfaz de progreso (barra, texto, imagen de Séneca).
*   **Cuerpo (`<body>`):**
    *   **Interfaz de Progreso:** Elementos HTML (`<img>`, `<div>`) para mostrar la barra de progreso, el texto y la imagen animada de Séneca.
    *   **Escena de A-Frame (`<a-scene>`):**
        *   Configura `mindar-image` con la ruta al archivo de marcadores (`imageTargetSrc: seneca.mind`) y el número máximo de marcadores a rastrear simultáneamente (`maxTrack: 5`).
        *   Deshabilita la interfaz de usuario del modo VR y la solicitud de permisos de orientación del dispositivo.
    *   **Activos (`<a-assets>`):** Precarga los 5 modelos `.gltf` de Séneca.
    *   **Cámara (`<a-camera>`):** Configurada para la vista de AR, con los controles de mirada deshabilitados.
    *   **Entidades de Marcador (`<a-entity mindar-image-target>`):**
        *   Se definen 5 entidades, una para cada marcador (`targetIndex: 0` a `targetIndex: 4`).
        *   Cada entidad contiene un `<a-gltf-model>` que referencia uno de los modelos de Séneca precargados.
        *   Los modelos se posicionan, escalan y rotan según sea necesario. El atributo `animation-mixer` activa la animación embebida en el modelo glTF.
    *   **Script de Progreso (`<script>`):**
        *   Escucha los eventos `targetFound` y `targetLost` para cada uno de los 5 marcadores.
        *   Mantiene un registro de los marcadores detectados.
        *   Actualiza dinámicamente la barra de progreso, el texto y la posición/animación de la imagen de Séneca corriendo en función del número de marcadores detectados.

Este proyecto demuestra una aplicación práctica y atractiva de la Realidad Aumentada basada en la web para eventos educativos y de divulgación.