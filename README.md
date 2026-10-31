# PhotoAI Studio Hybrid — PWA Android

## Qué funciona
- Elegir foto desde galería y previsualizarla.
- Ajustar brillo, contraste, saturación y desenfoque en el teléfono.
- Rotar, escala de grises, recorte cuadrado central, restablecer, deshacer (acciones discretas) y descargar PNG.
- Comparar original mientras se mantiene presionado el botón.
- Funcionar sin conexión tras la primera carga en HTTPS.
- Enviar una foto voluntariamente a un endpoint HTTPS de edición con IA configurado por el usuario.

## Limitaciones
- **No incluye modelo generativo local ni servidor de IA.** La función nube requiere que configures uno.
- El historial no registra cada movimiento de los deslizadores, sino operaciones discretas.
- La exportación conserva como máximo 2048 píxeles en el lado más largo de las imágenes importadas.
- El recorte cuadrado es central; no hay selección libre ni máscara.
- La aplicación no solicita ni almacena claves API; no coloques tokens privados en la URL.
- El servicio remoto debe habilitar CORS para el origen donde publiques la PWA.

## Publicar solo desde Samsung con GitHub Pages
1. En Chrome, inicia sesión o crea una cuenta en https://github.com/.
2. Crea un repositorio nuevo, por ejemplo `photoai-studio` y configúralo como **Public**.
3. Abre el repositorio y pulsa **Add file > Upload files**. Sube los archivos de esta carpeta, incluyendo la carpeta `icons` y sus dos imágenes. Si el navegador móvil no permite seleccionar carpetas, sube primero los archivos raíz y luego abre `icons` en GitHub para subir allí las imágenes.
4. En el repositorio, ve a **Settings > Pages**. En **Build and deployment**, selecciona **Deploy from a branch**, `main`, carpeta `/(root)` y guarda.
5. Espera la publicación y abre `https://TU-USUARIO.github.io/photoai-studio/` (sustituye TU-USUARIO y el nombre del repositorio).
6. Desde Chrome en Android, abre el menú ⋮ y selecciona **Agregar a pantalla principal** o **Instalar aplicación**.
7. Abre la aplicación desde el icono. La edición local funcionará offline después de la primera carga correcta.

GitHub Pages publica código estático. No ejecuta modelos de IA ni servidores Python. Los archivos publicados serán visibles al público, por lo que no agregues secretos ni fotos personales al repositorio.

## Contrato del servidor de IA opcional
La PWA envía `POST` HTTPS con `multipart/form-data`:
- `image`: fotografía actual PNG
- `prompt`: instrucción de edición (texto UTF-8)

El servidor debe devolver `200 OK` con cuerpo binario de imagen (`Content-Type: image/png` o `image/jpeg`) y cabeceras CORS que permitan el origen de tu PWA. Debe gestionar autenticación de forma segura en el servidor; no incrustes una clave privada en la PWA. La aplicación no incluye servidor de ejemplo porque integrar un modelo real requiere elegir un proveedor y configurar credenciales/infraestructura.

## Prueba local para desarrolladores
Sirve la carpeta con un servidor HTTP (por ejemplo `python -m http.server 8000`) y abre `http://localhost:8000`. El service worker requiere HTTPS o localhost. No funcionará correctamente al abrir `index.html` directamente desde archivos.
