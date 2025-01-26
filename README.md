# Interfaz Web para Visualización y Gestión de Streams de Video

Esta interfaz web está diseñada para recibir múltiples streams de video simultáneamente, permitiendo al usuario seleccionar cualquiera de ellos para visualizarlo. Además, ofrece la opción de grabar los streams y, en el futuro, retransmitirlos a otros dispositivos.

## Requisitos

- GStreamer y sus plugins.
- Un servidor de signalling para WebRTC.
- Node.js y npm para la interfaz web.

## Instrucciones de Uso

### 1. Lanzar el servidor de signalling

Ejecute los siguientes comandos para iniciar el servidor de signalling:

```bash
cd /opt/gst-plugins-rs/net/webrtc/signalling
WEBRTCSINK_SIGNALLING_SERVER_LOG=debug cargo run --bin gst-webrtc-signalling-server
```

### 2. Lanzar un stream de video desde GStreamer

#### Para visualizar una webcam:
```bash
gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw,width=1280,height=720 ! webrtcsink
```

#### Para visualizar un video con sonido:
```bash
gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! queue ! webrtcsink name=ws \
    alsasrc device=hw:1,0 ! audioconvert ! audioresample ! queue ! ws.
```

### 3. Lanzar la interfaz web

Para iniciar la interfaz web, ejecute los siguientes comandos:

```bash
cd gstwebrtc-api
npm build
npm start
```

## Funcionalidades Futuras

- Retransmisión de los streams a otros dispositivos.
- Mejora en la grabación y gestión de los archivos de video.
- Soporte para más protocolos de streaming.
