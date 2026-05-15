<div align="center">

# AirGuard

**Sistema de monitoreo ambiental y control de dispositivos IoT**

Plataforma movil para la supervision en tiempo real de sensores ambientales, gestion de dispositivos IoT y visualizacion de datos de sensores con alertas inteligentes.

</div>

---

## Tabla de contenidos

- [Descripcion](#descripcion)
- [Caracteristicas](#caracteristicas)
- [Tecnologias](#tecnologias)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Instalacion y ejecucion](#instalacion-y-ejecucion)
- [Configuracion](#configuracion)
- [Pantallas principales](#pantallas-principales)
- [Mejoras futuras](#mejoras-futuras)
- [Creditos](#creditos)

---

## Descripcion

AirGuard es una aplicacion movil desarrollada con React Native y Expo que permite a usuarios y administradores monitorear sensores ambientales (gas, ultrasonico, temperatura, humedad) conectados a dispositivos IoT. La app se comunica con un backend REST para la autenticacion y gestion de datos, y utiliza el protocolo MQTT para la comunicacion en tiempo real con dispositivos fisicos.

El sistema implementa un flujo de autenticacion basado en JWT con roles diferenciados (admin/usuario), visualizacion de datos historicos mediante graficas, un sistema de alertas categorizadas por severidad, y un panel de administracion para la gestion de dispositivos y usuarios.

---

## Caracteristicas

- **Autenticacion con roles**: Registro e inicio de sesion con diferenciacion entre administradores y usuarios. Tokens JWT almacenados de forma persistente.
- **Monitoreo de sensores en tiempo real**: Suscripcion a topics MQTT para recibir datos de temperatura y otros sensores de forma continua.
- **Control de dispositivos IoT**: Publicacion de comandos MQTT (direccion, pasos, velocidad) para controlar actuadores conectados a microcontroladores.
- **Visualizacion de datos**: Graficas de barras interactivas por tipo de sensor (gas, ultrasonico, temperatura, humedad) con filtrado por categoria.
- **Sistema de alertas**: Tarjetas de alerta codificadas por color segun severidad (Alerta en rojo, Advertencia en amarillo, Error en gris/azul).
- **Panel de administracion**: CRUD de dispositivos IoT, asignacion/desasignacion de usuarios a dispositivos, gestion de solicitudes de monitoreo.
- **Perfil de usuario**: Visualizacion y edicion de datos personales con validacion de formularios (Formik + Yup).
- **Tema oscuro/claro**: Alternancia entre modos de color mediante un toggle en el menu lateral.
- **Menu lateral con gestos**: Apertura del menu de navegacion mediante deslizamiento horizontal (swipe right).
- **Notificaciones push**: Registro automatico para notificaciones Expo al iniciar la aplicacion.

---

## Tecnologias

| Categoria | Tecnologia |
|---|---|
| Framework movil | React Native 0.72, Expo SDK 49 |
| Navegacion | React Navigation 6 (Stack Navigator) |
| Estado y formularios | React Context, Formik, Yup |
| Comunicacion IoT | Paho MQTT, MQTT v5 |
| HTTP Client | Axios, Fetch API |
| Graficas | react-native-chart-kit, @shopify/react-native-skia |
| UI | React Native Paper, React Native Elements, React Native Vector Icons |
| Animaciones | React Native Reanimated, React Native Animated |
| Almacenamiento | AsyncStorage (tokens y datos de sesion) |
| Notificaciones | Expo Notifications |
| Validacion | Yup |
| Build | EAS (Expo Application Services) |
| Backend (externo) | API REST (Node.js/Express) con JWT |

---

## Estructura del proyecto

```
AirGuard/
├── App.js                          # Punto de entrada: fuentes, notificaciones, ThemeProvider
├── app.json                        # Configuracion Expo
├── eas.json                        # Perfiles de build EAS
├── router/
│   └── AppNavigator.js             # Navegador principal (Stack)
├── views/
│   ├── homeScreen/                 # Pantalla de inicio (admin/usuario)
│   ├── LoginScreenV/              # Inicio de sesion
│   ├── registerScreen/            # Registro de usuarios
│   ├── profile/                   # Perfil de usuario
│   ├── editProfile/               # Edicion de perfil
│   ├── cards/                     # Alertas y estados de sensores
│   ├── graphics/                  # Graficas de datos historicos
│   ├── dashboard/                 # Control de dispositivos (MQTT publish)
│   ├── comunication/             # Visor de mensajes MQTT (subscribe)
│   ├── adminDashboard/           # Panel de administracion
│   ├── request/                  # Solicitudes de monitoreo
│   ├── Nosotros/                 # Pagina del equipo
│   ├── SesionExpired/            # Pantalla de sesion expirada
│   └── graphAd/                  # Grafica avanzada con Skia (no activa)
├── components/
│   ├── theme/                     # ThemeContext y ThemeProvider
│   ├── menu/                      # Menu lateral (admin/usuario)
│   ├── switchTheme.js            # Toggle de tema oscuro/claro
│   ├── input.js, button.js        # Estilos de inputs y botones
│   ├── contentContainer.js        # Contenedores de contenido
│   └── backButtonc.js            # Boton de retroceso
├── services/
│   └── api.js, mqttService.js     # Servicios (esqueletos)
├── styles/
│   ├── globalStyles.js            # Estilos globales (fuente base)
│   └── fonts.js                   # Carga de fuentes (Gilroy, Poppins, Inter)
├── utils/
│   ├── auth.js                    # Decodificacion de JWT
│   └── storage.js                 # Helpers de AsyncStorage
├── assets/
│   ├── fonts/                     # Fuentes tipograficas
│   └── images/                    # Imagenes y fotos del equipo
└── android/                       # Configuracion de build Android
```

---

## Instalacion y ejecucion

### Requisitos previos

- Node.js 18+
- npm o yarn
- Expo CLI (`npm install -g expo-cli`)
- Backend API corriendo en la red local (puerto 3000)
- Dispositivo fisico o emulador con Expo Go

### Pasos

1. **Clonar el repositorio**

   ```bash
   git clone <url-del-repositorio>
   cd inte
   ```

2. **Instalar dependencias**

   ```bash
   npm install
   ```

3. **Configurar la IP del servidor**

   Editar la variable `global.ipDireccion` en `App.js` con la IP local donde corre el backend:

   ```js
   global.ipDireccion = 'TU_IP_LOCAL';
   ```

4. **Iniciar la aplicacion**

   ```bash
   npx expo start
   ```

5. **Ejecutar en dispositivo**

   Escanear el codigo QR con Expo Go (Android/iOS) o ejecutar en emulador.

---

## Configuracion

| Variable | Ubicacion | Descripcion |
|---|---|---|
| `global.ipDireccion` | `App.js` | IP del servidor backend en la red local (por defecto: `172.20.31.110`) |
| `global.categories` | `App.js` | Categorias de sensores disponibles |

### MQTT

La aplicacion utiliza dos brokers MQTT publicos:

| Pantalla | Broker | Puerto | Modo |
|---|---|---|---|
| Dashboard (control) | `test.mosquitto` | 1883 | Publicacion |
| Comunicacion (lectura) | `broker.hivemq.com` | 8000 (WebSocket) | Suscripcion |

---

## Pantallas principales

| Pantalla | Descripcion |
|---|---|
| **Home** | Pantalla de bienvenida con accesos directos y tarjetas de caracteristicas |
| **Login** | Formulario de inicio de sesion con validacion de email y contrasena |
| **Registro** | Formulario de registro con selector de fecha y opcion de administrador |
| **Perfil** | Datos del usuario, rol, solicitudes de monitoreo y menu de acciones |
| **Editar Perfil** | Formulario editable con validacion para nombre, apellido, email y fecha de nacimiento |
| **Graficas** | Graficas de barras por tipo de sensor con filtrado por categoria |
| **Estado y Avisos** | Tarjetas de alerta de sensores con codificacion por severidad |
| **Dashboard** | Control de motor/dispositivo con sliders y selector de direccion via MQTT |
| **Comunicacion** | Visor en tiempo real de mensajes MQTT recibidos |
| **Admin Dashboard** | Gestion de dispositivos y usuarios (CRUD, asignaciones) |
| **Solicitudes** | Envio y gestion de solicitudes de monitoreo a usuarios |
| **Nosotros** | Pagina del equipo de desarrollo |

---

## Mejoras futuras

- **Persistencia del tema**: Guardar la preferencia de tema oscuro/claro en AsyncStorage para que persista entre sesiones.
- **URL del backend configurable**: Reemplazar la IP hardcodeada con variables de entorno (`expo-constants` o `.env`).
- **Subida de foto de perfil**: Completar la implementacion del image picker para enviar la imagen al servidor.
- **Pantalla de usuario no-admin**: Corregir la navegacion post-login para usuarios (el destino `UserDashboard` no existe en el navigator).
- **Grafica avanzada con Skia**: Activar la pantalla `graphAd/sensorDataScreen.tsx` con datos en vivo desde la API en lugar del JSON estatico.
- **Reconexión automatica MQTT**: Implementar logica de reconexion con backoff exponencial para mayor robustez en la comunicacion IoT.
- **Internacionalizacion**: Agregar soporte para multiples idiomas (i18n).
- **Pruebas unitarias y de integracion**: Agregar Jest y React Testing Library para cobertura de pruebas.
- **Migracion a Expo Router**: Actualizar la navegacion a Expo Router (file-based routing) para mejor compatibilidad con Expo SDK 49+.
- **Seguridad**: Remover credenciales sensibles del repositorio (archivos `.json` con claves de servicio, keystores).

---

## Creditos

Proyecto colaborativo desarrollado por estudiantes universitarios:

| Integrante |
|---|
| Victor Hugo Perez Trujillo |
| Gerardo Isaac Ramirez Meza |
| Miguel Angel Vargas Reyes |
| Miguel Andy Contreras Esparza |
| Axel Herrera Sanchez |
| Andrea Garcia Galindo |
| Salvador Murillo Rosales |

---

<div align="center">

**AirGuard** -- Monitoreo ambiental inteligente

</div>
