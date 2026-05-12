# FinanTrack — Plan de Implementación Profesional
**Documento de Planificación de Software**
**Versión 1.0 | Flutter + Firebase | Multiplataforma**

---

> *Este documento describe de manera completa y profesional la arquitectura, tecnologías, diseño y fases de desarrollo de FinanTrack, una aplicación de administración financiera multiplataforma construida con Flutter y Firebase.*

---

## Tabla de Contenidos

1. [Descripción General del Sistema](#1-descripción-general-del-sistema)
2. [Arquitectura del Proyecto](#2-arquitectura-del-proyecto)
3. [Tecnologías Necesarias](#3-tecnologías-necesarias)
4. [Diseño UI/UX](#4-diseño-uiux)
5. [Planeación del Desarrollo](#5-planeación-del-desarrollo)
6. [Dependencias Recomendadas](#6-dependencias-recomendadas)
7. [Seguridad y Autenticación](#7-seguridad-y-autenticación)
8. [Flujo de Navegación](#8-flujo-de-navegación)
9. [Plan de Implantación](#9-plan-de-implantación)
10. [Recomendaciones Profesionales](#10-recomendaciones-profesionales)

---

## 1. Descripción General del Sistema

### ¿Qué es FinanTrack?

FinanTrack es una aplicación multiplataforma de administración financiera diseñada para equipos y organizaciones que necesitan gestionar proyectos con presupuestos definidos, controlar el flujo de dinero entre cuentas, categorizar sus gastos e ingresos, registrar transacciones, administrar proveedores y llevar un control claro de facturas y pagos pendientes.

A diferencia de las aplicaciones financieras personales convencionales, FinanTrack está orientada a **proyectos**: cada conjunto de cuentas, categorías, presupuestos y transacciones vive dentro de un proyecto específico, lo que permite a distintos equipos o departamentos trabajar con su propia información de forma organizada, sin mezclarla con la de otros.

### Objetivo Principal

El objetivo central de FinanTrack es **dar visibilidad financiera en tiempo real** a cualquier persona o equipo que administre recursos económicos dentro de proyectos. Esto se logra a través de:

- **Control de cuentas**: El usuario puede registrar múltiples cuentas bancarias, cajas o fondos, cada una con su saldo inicial y su saldo actualizado automáticamente según las transacciones realizadas.
- **Categorización inteligente**: Las transacciones se agrupan por categorías con tipo (ingreso/gasto) y color, lo que facilita visualizar en qué se está gastando y cuánto se está ingresando.
- **Presupuestos por período**: Para cada categoría de un proyecto se pueden definir montos planificados, y el sistema compara lo planificado contra lo ejecutado de manera automática.
- **Transacciones trazables**: Cada movimiento de dinero queda registrado con su cuenta origen, cuenta destino, categoría, usuario responsable, fecha, estado y descripción, garantizando trazabilidad total.
- **Gestión de proveedores y facturas**: FinanTrack permite registrar proveedores, vincular facturas a transacciones y dar seguimiento al estado de cada documento fiscal (pagada, pendiente, vencida).
- **Multiusuario con roles**: Distintos usuarios pueden acceder al sistema con permisos diferenciados, desde administradores que configuran proyectos hasta colaboradores que solo registran transacciones.

### ¿Para quién está hecha?

FinanTrack está pensada para:

- Pequeñas y medianas empresas que manejan varios proyectos simultáneamente.
- Equipos de trabajo que necesitan controlar gastos por proyecto.
- Administradores financieros que requieren reportes de presupuesto vs. ejecución.
- Freelancers o consultores que gestionan múltiples clientes con presupuestos independientes.
- Estudiantes y desarrolladores que desean aprender a construir una aplicación financiera real con Flutter y Firebase.

### Plataformas Objetivo

La aplicación correrá de manera nativa y óptima en:

| Plataforma | Descripción |
|---|---|
| Android | App móvil para smartphones y tablets Android |
| iOS | App móvil para iPhone y iPad |
| Web | Aplicación web accesible desde cualquier navegador moderno |
| Windows | Aplicación de escritorio para computadoras con Windows |

---

## 2. Arquitectura del Proyecto

### Arquitectura Recomendada: Clean Architecture + MVVM

Para FinanTrack se recomienda implementar **Clean Architecture** combinada con el patrón **MVVM (Model-View-ViewModel)**. Este enfoque es el estándar profesional en proyectos Flutter de mediana y gran escala, ya que separa claramente las responsabilidades del sistema en capas bien definidas, facilita el testing, permite cambiar implementaciones sin afectar la lógica de negocio, y hace el código mantenible a largo plazo.

La arquitectura se divide en tres capas principales:

#### Capa de Presentación (UI Layer)

Es todo lo que el usuario ve e interactúa. Contiene las pantallas (Screens/Pages), los widgets reutilizables y los ViewModels (o Controllers con Provider). Esta capa **nunca habla directamente con Firebase**; solo consume datos que vienen de los casos de uso.

#### Capa de Dominio (Domain Layer)

Es el corazón de la aplicación. Contiene las entidades del negocio (Proyecto, Cuenta, Transaccion, etc.), los casos de uso (CreateTransaction, GetBudgetsByProject, etc.) y las interfaces de los repositorios. Esta capa es completamente independiente de Flutter y de Firebase; es Dart puro, lo que la hace fácilmente testeable.

#### Capa de Datos (Data Layer)

Se encarga de obtener y guardar datos. Contiene los repositorios concretos (que implementan las interfaces del dominio), los modelos de datos (que extienden las entidades pero saben cómo serializar/deserializar desde Firestore), y las fuentes de datos remotas (Firebase, Firestore, Authentication).

---

### Organización de Carpetas

```
finantrack/
├── lib/
│   ├── core/
│   │   ├── constants/          # Colores, tamaños, strings constantes
│   │   ├── errors/             # Clases de errores personalizados
│   │   ├── extensions/         # Extensiones de Dart (DateTime, String, etc.)
│   │   ├── router/             # Configuración de rutas (go_router)
│   │   ├── theme/              # Tema visual de la app (colores, tipografía)
│   │   └── utils/              # Funciones utilitarias generales
│   │
│   ├── data/
│   │   ├── datasources/
│   │   │   └── remote/         # Comunicación con Firestore y Firebase Auth
│   │   ├── models/             # Modelos con fromJson/toJson para Firestore
│   │   └── repositories/       # Implementaciones concretas de los repositorios
│   │
│   ├── domain/
│   │   ├── entities/           # Entidades puras del negocio (Proyecto, Cuenta, etc.)
│   │   ├── repositories/       # Interfaces/contratos de los repositorios
│   │   └── usecases/           # Casos de uso (una clase por acción)
│   │
│   ├── presentation/
│   │   ├── common/
│   │   │   └── widgets/        # Widgets reutilizables (botones, cards, inputs)
│   │   ├── features/
│   │   │   ├── auth/           # Pantallas de Login y Registro
│   │   │   ├── dashboard/      # Pantalla principal / resumen
│   │   │   ├── projects/       # CRUD de proyectos
│   │   │   ├── accounts/       # CRUD de cuentas
│   │   │   ├── categories/     # CRUD de categorías
│   │   │   ├── transactions/   # CRUD de transacciones
│   │   │   ├── budgets/        # CRUD de presupuestos
│   │   │   ├── providers/      # CRUD de proveedores
│   │   │   ├── invoices/       # CRUD de facturas
│   │   │   └── profile/        # Perfil del usuario
│   │   └── providers/          # Providers de estado (con Provider/Riverpod)
│   │
│   ├── firebase_options.dart    # Generado por FlutterFire CLI
│   └── main.dart               # Punto de entrada de la aplicación
│
├── test/
│   ├── unit/                   # Tests de casos de uso y repositorios
│   ├── widget/                 # Tests de widgets de UI
│   └── integration/            # Tests de flujos completos
│
├── assets/
│   ├── images/                 # Imágenes e íconos de la aplicación
│   └── fonts/                  # Fuentes personalizadas
│
├── android/                    # Configuración nativa Android
├── ios/                        # Configuración nativa iOS
├── web/                        # Configuración para Flutter Web
├── windows/                    # Configuración para Flutter Windows
├── pubspec.yaml                # Dependencias del proyecto
└── README.md                   # Documentación del proyecto
```

---

### Separación Frontend / Backend

En FinanTrack no existe un backend tradicional con servidor propio. Firebase actúa como el backend completo (BaaS — Backend as a Service):

- **Frontend**: La aplicación Flutter gestiona toda la interfaz, la lógica de presentación y el estado local.
- **Backend**: Firestore almacena todos los datos estructurados. Firebase Authentication gestiona identidades. Las reglas de seguridad de Firestore son el "backend" de permisos.
- **Comunicación**: La capa de datos en Flutter se comunica directamente con los SDKs de Firebase mediante los repositorios, que son los únicos "traductores" entre el mundo Flutter y el mundo Firebase.

---

### Manejo de Estados

Se recomienda usar **Provider** como solución principal de manejo de estado, por las siguientes razones:

- Es el paquete recomendado oficialmente por el equipo de Flutter para proyectos de escala media.
- Es simple de aprender para equipos en formación.
- Se integra perfectamente con la arquitectura Clean Architecture.
- Es suficientemente poderoso para las necesidades de FinanTrack.

Cada feature tendrá su propio `ChangeNotifier` o `ChangeNotifierProvider`, el cual es accedido desde los widgets con `context.watch<T>()` o `context.read<T>()`. Los estados posibles de cada pantalla son: **Loading**, **Loaded** y **Error**, garantizando que la UI siempre tenga una respuesta visual adecuada.

---

### Estructura Escalable y Buenas Prácticas

- **Un archivo por clase**: cada entidad, modelo, repositorio y caso de uso vive en su propio archivo.
- **Nomenclatura consistente**: archivos en `snake_case`, clases en `PascalCase`, variables y métodos en `camelCase`.
- **Inyección de dependencias**: Los repositorios se inyectan en los casos de uso mediante constructores, facilitando los tests y el cambio de implementaciones.
- **No lógica en los widgets**: Los widgets solo renderizan datos y disparan eventos; toda la lógica vive en ViewModels o casos de uso.
- **Manejo de errores global**: Se define una clase `AppError` que centraliza los mensajes de error traducibles y amigables para el usuario.

---

## 3. Tecnologías Necesarias

### Flutter

Flutter es el framework de Google para construir aplicaciones nativas multiplataforma desde una única base de código en Dart. Para FinanTrack, Flutter permite generar builds para Android, iOS, Web y Windows sin reescribir lógica específica por plataforma.

La versión recomendada es **Flutter 3.x** (estable), que incluye soporte maduro para las cuatro plataformas objetivo y mejoras significativas en rendimiento con el motor Impeller (Android/iOS) y CanvasKit (Web).

---

### Dart

Dart es el lenguaje de programación detrás de Flutter. Es fuertemente tipado, orientado a objetos y asíncrono por naturaleza (async/await). Para este proyecto, Dart ofrece:

- Null safety nativa, que elimina una gran clase de errores en tiempo de ejecución.
- Generics y tipos parametrizados para modelos robustos.
- Streams integrados, ideales para escuchar cambios en tiempo real desde Firestore.

---

### Firebase

Firebase es la plataforma de desarrollo de Google que provee infraestructura backend lista para usar. En FinanTrack se utilizan los siguientes servicios:

#### Firestore (Firebase Cloud Firestore)

Base de datos NoSQL en tiempo real, orientada a documentos y colecciones. Es la fuente de verdad de toda la información de FinanTrack: proyectos, cuentas, transacciones, presupuestos y facturas se almacenan aquí. Las ventajas clave son:

- Escucha en tiempo real con `snapshots()`, ideal para dashboards que se actualizan solos.
- Consultas flexibles con filtros, ordenamiento y paginación.
- Offli persistence: el SDK guarda datos en caché local para funcionar sin conexión y sincroniza cuando vuelve el internet.

#### Firebase Authentication

Maneja el registro, inicio de sesión y gestión de sesiones de usuario. En FinanTrack se usará autenticación con **Email y Password** como método principal. Firebase Authentication genera tokens JWT seguros que se validan automáticamente en las reglas de Firestore.

#### Firebase Storage

Servicio de almacenamiento de archivos. En FinanTrack se usará para guardar archivos adjuntos de facturas (PDFs, imágenes de comprobantes). Aunque es opcional en una primera versión, es importante tenerlo contemplado desde el inicio.

---

### Provider

Paquete de manejo de estado para Flutter. Actúa como un contenedor de dependencias y notificador de cambios. En FinanTrack, cada módulo (cuentas, transacciones, presupuestos) tendrá su propio Provider que mantiene el estado local de esa pantalla y reacciona a los cambios de Firestore en tiempo real.

---

### Herramientas de Desarrollo

| Herramienta | Propósito |
|---|---|
| VS Code | Editor de código principal |
| Flutter SDK | Framework multiplataforma |
| FlutterFire CLI | Configuración automática de Firebase en el proyecto |
| Firebase CLI | Administración de proyectos Firebase desde terminal |
| Android Studio (emulador) | Emulador Android para pruebas |
| Chrome | Navegador para pruebas de Flutter Web |
| Git + GitHub | Control de versiones y repositorio remoto |
| Flutter DevTools | Herramienta de debugging y análisis de rendimiento |
| dart_code_metrics | Análisis estático de calidad de código |

---

## 4. Diseño UI/UX

### Filosofía de Diseño

FinanTrack debe comunicar **confianza, claridad y modernidad**. Una aplicación financiera mal diseñada genera desconfianza. Por ello, el diseño debe ser limpio, consistente y funcional. Se inspira en aplicaciones como Linear, Notion y apps bancarias modernas: espaciado generoso, tipografía clara y datos presentados de forma visual e inmediata.

---

### Paleta de Colores

| Rol | Color | Hex |
|---|---|---|
| Primary (Acento principal) | Azul profundo | `#1E3A5F` |
| Secondary (Acento secundario) | Teal moderno | `#00BFA5` |
| Background | Blanco roto (Light) / Gris oscuro (Dark) | `#F5F7FA` / `#1A1D23` |
| Surface (Cards, inputs) | Blanco puro / Gris carbón | `#FFFFFF` / `#242830` |
| Error | Rojo suave | `#E53935` |
| Success | Verde esmeralda | `#43A047` |
| Warning | Ámbar | `#FB8C00` |
| Text Primary | Casi negro | `#1C1C1E` |
| Text Secondary | Gris medio | `#6B7280` |

La paleta es compatible con modo claro y modo oscuro. Flutter permite manejar ambos con `ThemeData.light()` y `ThemeData.dark()` sin ningún cambio en los widgets.

---

### Tipografía

- **Fuente principal**: `Inter` (disponible en Google Fonts) — moderna, altamente legible en pantallas de cualquier densidad.
- **Jerarquía tipográfica**:
  - Títulos de sección: `Inter SemiBold 20px`
  - Subtítulos y labels: `Inter Medium 14px`
  - Cuerpo de texto: `Inter Regular 14px`
  - Texto secundario/captions: `Inter Regular 12px`
  - Montos y cifras financieras: `Inter Bold 18–24px` (los números deben verse grandes y prominentes)

---

### Diseño Responsive y Adaptativo

Flutter permite crear interfaces adaptativas con `LayoutBuilder`, `MediaQuery` y el paquete `flutter_adaptive_scaffold`. Para FinanTrack se define el siguiente comportamiento por plataforma:

- **Móvil (Android/iOS)**: Navegación inferior (`BottomNavigationBar`) con 5 tabs principales. Listas verticales. Formularios de pantalla completa.
- **Web y Desktop (Windows)**: Barra lateral (`NavigationRail` o `Drawer` permanente) con todos los módulos visibles. Layout de dos columnas donde el panel izquierdo muestra listas y el derecho el detalle. Tablas para listados de transacciones.

---

### Navegación Intuitiva

- La navegación principal sigue el principio de **3 clics máximo**: el usuario no debe necesitar más de 3 interacciones para llegar a cualquier función importante.
- Se usa `go_router` para manejar rutas declarativas, incluyendo rutas anidadas y redirección según el estado de autenticación.
- Los formularios de creación/edición se abren como pantallas completas en móvil y como paneles laterales o dialogs en Web/Desktop.
- Los errores, confirmaciones y mensajes de éxito se muestran con `SnackBar` o `Dialog` según la gravedad.

---

### Componentes Visuales Clave

- **Dashboard Card**: Cards con sombra suave que muestran el resumen de una cuenta o proyecto (saldo, porcentaje de presupuesto usado, etc.).
- **Transaction Tile**: Fila de lista con ícono de categoría (con el color definido), monto con color rojo o verde según tipo, y fecha formateada.
- **Budget Progress Bar**: Barra de progreso visual (planificado vs ejecutado) con colores que cambian según el porcentaje de ejecución (verde < 80%, naranja 80-99%, rojo ≥ 100%).
- **Empty State**: Cuando una lista está vacía, se muestra una ilustración amigable y un botón de acción primaria ("Crear primera cuenta", "Registrar transacción").
- **Loading Skeleton**: En lugar de spinners, se muestran skeletons (placeholders animados) mientras carga la información de Firestore.

---

## 5. Planeación del Desarrollo

### Fase 1 — Configuración del Entorno

#### Objetivos
Tener un entorno de desarrollo completamente funcional donde se pueda compilar y ejecutar la aplicación en Android, Web y Windows desde VS Code.

#### Herramientas
Flutter SDK, VS Code, extensiones de Flutter y Dart para VS Code, Android Studio (solo para el emulador AVD), Chrome, Git.

#### Qué se desarrollará
- Instalación de Flutter SDK y verificación con `flutter doctor`.
- Configuración de variables de entorno (PATH).
- Creación del proyecto con `flutter create finantrack --platforms android,ios,web,windows`.
- Configuración del `.gitignore` adecuado para Flutter.
- Primer commit al repositorio de GitHub.
- Verificación de que la app de ejemplo corre correctamente en las 4 plataformas.

#### Buenas Prácticas
- Usar siempre la versión **stable** de Flutter, no beta ni master.
- Verificar con `flutter doctor -v` que no haya ningún warning crítico.
- Configurar `analysis_options.yaml` con reglas de linting desde el inicio (paquete `flutter_lints`).

#### Posibles Problemas
- **Java SDK incompatible con Android Gradle**: asegurarse de que el JDK instalado sea compatible con la versión de Gradle de Flutter.
- **Licencias de Android no aceptadas**: ejecutar `flutter doctor --android-licenses`.
- **Windows Desktop no habilitado**: ejecutar `flutter config --enable-windows-desktop`.

#### Soluciones Recomendadas
Seguir la documentación oficial de Flutter (docs.flutter.dev) paso a paso. Si hay conflictos con múltiples versiones de Java, usar `fvm` (Flutter Version Manager) para aislar la versión de Flutter por proyecto.

---

### Fase 2 — Configuración de Firebase

#### Objetivos
Conectar el proyecto Flutter con Firebase, habilitando Firestore, Authentication y Storage en la consola de Firebase.

#### Herramientas
Firebase Console (console.firebase.google.com), Firebase CLI, FlutterFire CLI, Node.js.

#### Qué se desarrollará
- Crear un proyecto en Firebase Console llamado "finantrack-dev".
- Instalar Firebase CLI con `npm install -g firebase-tools` y autenticarse con `firebase login`.
- Instalar FlutterFire CLI con `dart pub global activate flutterfire_cli`.
- Ejecutar `flutterfire configure` para generar el archivo `firebase_options.dart` automáticamente con la configuración de todas las plataformas.
- Habilitar Firestore en modo de prueba (test mode) desde la consola.
- Habilitar Authentication con el método de Email/Password.
- Habilitar Firebase Storage.
- Inicializar Firebase en `main.dart` con `await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform)`.

#### Buenas Prácticas
- Usar un proyecto Firebase separado para desarrollo y uno para producción. Aunque este proyecto es estándar (no producción), crear el hábito desde el inicio.
- Guardar el archivo `firebase_options.dart` en el repositorio (contiene config pública), pero **nunca** subir claves privadas ni archivos `google-services.json` con permisos de Admin.
- Añadir `google-services.json` (Android) y `GoogleService-Info.plist` (iOS) al `.gitignore` en proyectos que usen credenciales sensibles en producción.

#### Posibles Problemas
- **FlutterFire CLI no encuentra el proyecto Firebase**: asegurarse de estar autenticado con la misma cuenta de Google usada en Firebase Console.
- **Error en iOS con `GoogleService-Info.plist`**: el archivo debe colocarse dentro del directorio `ios/Runner/` y añadirse al target en Xcode.

#### Soluciones Recomendadas
Seguir el proceso de `flutterfire configure` que es el método oficial y automatizado. Si hay errores, regenerar el archivo borrando `firebase_options.dart` y ejecutando el comando nuevamente.

---

### Fase 3 — Diseño de Base de Datos Firestore

#### Objetivos
Definir la estructura exacta de colecciones y documentos en Firestore para todas las entidades del sistema, de manera que sea eficiente, escalable y consistente con las reglas de seguridad que se definirán.

#### Herramientas
Firebase Console (Firestore), documentación de Firestore, papel o herramienta de diagramas (draw.io, Miro).

#### Qué se desarrollará

La estructura de Firestore sigue el modelo de colecciones y subcolecciones. Se propone la siguiente jerarquía:

```
/users/{userId}
  - nombre, email, rol

/projects/{projectId}
  - nombre, descripcion, fecha_inicio, fecha_fin, estado, presupuesto_total, owner_id

/projects/{projectId}/categories/{categoryId}
  - nombre, tipo (ingreso/gasto), color

/projects/{projectId}/accounts/{accountId}
  - nombre, tipo, moneda, saldo_inicial, saldo_actual

/projects/{projectId}/transactions/{transactionId}
  - cuenta_origen_id, cuenta_destino_id, categoria_id, usuario_id,
    monto, fecha, tipo, descripcion, estado

/projects/{projectId}/budgets/{budgetId}
  - categoria_id, monto_planificado, monto_ejecutado, periodo_inicio, periodo_fin

/providers/{providerId}
  - nombre, rfc, contacto, moneda

/invoices/{invoiceId}
  - proveedor_id, transaccion_id, numero, fecha_emision,
    fecha_vencimiento, monto_total, estado
```

El diseño coloca categorías, cuentas, transacciones y presupuestos como **subcolecciones de proyectos**, lo que facilita las consultas por proyecto y permite aplicar reglas de seguridad granulares por proyecto.

#### Buenas Prácticas
- **Desnormalización consciente**: en Firestore es normal duplicar ciertos campos (como el nombre de la categoría dentro de la transacción) para evitar múltiples lecturas en listas grandes. Hacerlo de forma deliberada y documentada.
- **Timestamps de Firestore**: usar `Timestamp` de Firestore en lugar de strings de fecha para facilitar ordenamiento y filtrado.
- **IDs autogenerados**: usar los IDs autogenerados de Firestore (`doc().id`) en lugar de IDs secuenciales para escalar bien.

#### Posibles Problemas
- **Costos de lectura elevados**: Firestore cobra por lectura de documentos. Si una pantalla carga 1000 transacciones de golpe, el costo sube. Mitigar con paginación desde el inicio.
- **Consultas que requieren índices compuestos**: Firestore requiere índices explícitos para consultas con múltiples campos de filtro y ordenamiento. Crearlos desde la consola cuando la app los solicite en desarrollo.

#### Soluciones Recomendadas
Diseñar las consultas más comunes primero, y luego estructurar Firestore para que esas consultas sean eficientes. Usar la herramienta de emuladores de Firebase (`firebase emulators:start`) para probar localmente sin costos.

---

### Fase 4 — Sistema de Autenticación

#### Objetivos
Implementar el flujo completo de registro e inicio de sesión de usuarios con Firebase Authentication, incluyendo persistencia de sesión y redirección automática según el estado del usuario.

#### Herramientas
`firebase_auth` package, `go_router` para redirecciones, Provider para el estado de sesión.

#### Qué se desarrollará
- Pantalla de Login con campos de email y contraseña, validación de formulario y manejo de errores de Firebase (contraseña incorrecta, usuario no encontrado).
- Pantalla de Registro con email, contraseña, confirmación de contraseña y nombre.
- Al registrarse, se crea automáticamente un documento en `/users/{uid}` en Firestore con los datos del nuevo usuario.
- `AuthProvider` (ChangeNotifier) que escucha el stream `authStateChanges()` de Firebase y mantiene el estado del usuario actual (`currentUser`).
- Configuración de `go_router` con un `redirect` global que revisa si el usuario está autenticado y lo envía al Dashboard o al Login según corresponda.
- Funcionalidad de cierre de sesión desde el perfil.

#### Buenas Prácticas
- Nunca almacenar contraseñas en texto plano ni en Firestore. Firebase Authentication lo maneja de forma segura internamente.
- Mostrar mensajes de error amigables al usuario (no los códigos de error técnicos de Firebase directamente).
- Implementar un estado de "loading" en el botón de login para evitar múltiples envíos mientras se procesa la solicitud.

#### Posibles Problemas
- **Excepción `PigeonUserDetails` en versiones de Firebase Auth**: problema conocido en combinación con ciertos plugins. Solución: mantener las versiones de `firebase_auth` y `firebase_core` sincronizadas y actualizadas.
- **Redirección infinita en go_router**: puede ocurrir si el redirect comprueba el estado de auth antes de que el stream emita. Solución: incluir un estado de loading que no redirija mientras se inicializa.

#### Soluciones Recomendadas
Usar el stream `authStateChanges()` como única fuente de verdad para saber si el usuario está logueado. Implementar una pantalla de "splash" que espere a que Firebase inicialice antes de decidir a dónde navegar.

---

### Fase 5 — Pantallas Principales

#### Objetivos
Construir el esqueleto visual de la aplicación: la estructura de navegación completa, el Dashboard y las pantallas vacías (shell) de cada módulo.

#### Herramientas
`go_router`, `flutter_adaptive_scaffold`, widgets de Material 3 de Flutter.

#### Qué se desarrollará
- **Shell de navegación**: La estructura con `BottomNavigationBar` (móvil) y `NavigationRail` (Web/Desktop) que contiene todas las pantallas principales.
- **Dashboard**: Pantalla principal con cards de resumen: saldo total por proyecto, últimas transacciones, estado del presupuesto. En esta fase puede mostrar datos estáticos de prueba.
- **Pantallas vacías**: Se crean las pantallas de Proyectos, Cuentas, Categorías, Transacciones, Presupuestos, Facturas y Perfil, inicialmente con un texto placeholder.
- **Widgets comunes**: Se construyen los widgets reutilizables: `AppButton`, `AppTextField`, `AppCard`, `AppEmptyState`, `AppLoadingIndicator`, `AppErrorWidget`.
- **Tema completo**: Se configura `ThemeData` con la paleta de colores, tipografía Inter y estilos de componentes.

#### Buenas Prácticas
- Usar `Material 3` desde el inicio (`useMaterial3: true` en ThemeData). Es el diseño más moderno y viene activado por defecto en Flutter 3.16+.
- Crear todos los widgets reutilizables en la carpeta `presentation/common/widgets/` antes de necesitarlos en las pantallas específicas.
- Probar el layout en todas las plataformas desde esta fase, no al final.

#### Posibles Problemas
- **Inconsistencias de layout entre plataformas**: lo que se ve bien en Android puede verse diferente en Web. Solución: probar con `flutter run -d chrome` y `flutter run -d windows` de manera continua.
- **Overflow de texto en pantallas pequeñas**: en móvil, el texto largo puede desbordar. Usar `Flexible` y `Expanded` correctamente en los layouts.

#### Soluciones Recomendadas
Usar `LayoutBuilder` y `MediaQuery.of(context).size` para detectar el tamaño de pantalla y mostrar el layout apropiado. Definir breakpoints claros: `< 600px` es móvil, `600–1200px` es tablet/web, `> 1200px` es desktop.

---

### Fase 6 — CRUD de Entidades

#### Objetivos
Implementar las operaciones de crear, leer, actualizar y eliminar para todas las entidades principales: Proyectos, Categorías, Cuentas y Proveedores.

#### Herramientas
Firestore SDK, Provider, formularios de Flutter (Form, TextFormField, validators).

#### Qué se desarrollará

Para cada entidad (Proyecto, Categoría, Cuenta, Proveedor) se implementa:

- **Modelo** (`data/models/`): clase con `fromJson(Map)` y `toJson()` para Firestore.
- **Entidad** (`domain/entities/`): clase Dart pura con los campos del negocio.
- **Repositorio** (`domain/repositories/`): interfaz con métodos `create`, `getAll`, `getById`, `update`, `delete`.
- **Repositorio concreto** (`data/repositories/`): implementación que usa Firestore.
- **Casos de uso** (`domain/usecases/`): un caso de uso por operación (CreateProject, GetProjects, UpdateProject, DeleteProject).
- **Provider** (`presentation/providers/`): `ChangeNotifier` que llama a los casos de uso y notifica a la UI.
- **Pantalla de listado**: lista con tiles, botón de agregar y opción de eliminar/editar por swipe o menú contextual.
- **Formulario de creación/edición**: pantalla con todos los campos validados.

#### Buenas Prácticas
- Implementar soft delete (campo `deleted: true`) en lugar de borrar documentos físicamente, para mantener la integridad referencial (por ejemplo, si se elimina una categoría que tiene transacciones).
- Usar transacciones de Firestore (`runTransaction`) cuando una operación afecta múltiples documentos simultáneamente (por ejemplo, actualizar el saldo de una cuenta al crear una transacción).

#### Posibles Problemas
- **Perder el contexto del proyecto activo**: la app maneja múltiples proyectos, y es fácil que los CRUD no sepan a qué proyecto pertenecen los datos. Solución: mantener un `ProjectProvider` que almacene el proyecto seleccionado actualmente y que los demás providers lo consuman.

#### Soluciones Recomendadas
Crear un `ProjectProvider` central que actúe como contexto global del proyecto activo. Todos los providers de categorías, cuentas, transacciones y presupuestos deben escuchar el proyecto seleccionado para hacer sus consultas.

---

### Fase 7 — Gestión de Transacciones

#### Objetivos
Implementar el módulo de transacciones, que es el más complejo de la aplicación, incluyendo la actualización automática del saldo de las cuentas involucradas.

#### Herramientas
Firestore SDK (runTransaction), Provider, DatePicker de Flutter.

#### Qué se desarrollará
- **Formulario de transacción**: selección de tipo (ingreso, gasto, transferencia), cuenta origen, cuenta destino (para transferencias), categoría, monto, fecha, descripción y estado (pendiente, completada, cancelada).
- **Lógica de actualización de saldos**: al crear/editar/eliminar una transacción, se actualiza automáticamente el `saldo_actual` de las cuentas afectadas usando `runTransaction` de Firestore para garantizar atomicidad.
- **Listado de transacciones**: con filtros por fecha, tipo, categoría y cuenta. Paginación con `limit()` y `startAfterDocument()`.
- **Historial de transacciones por cuenta**: pantalla de detalle de cuenta que muestra solo las transacciones de esa cuenta.
- **Actualización del `monto_ejecutado` en presupuestos**: al registrar una transacción de tipo gasto en una categoría que tiene presupuesto activo, se actualiza el campo `monto_ejecutado` del presupuesto correspondiente.

#### Buenas Prácticas
- **Usar `runTransaction` siempre que se modifiquen múltiples documentos en una sola operación** (por ejemplo, crear transacción + actualizar saldo + actualizar presupuesto). Esto garantiza que o todo se guarda o nada se guarda.
- Validar que el monto sea positivo y que la cuenta origen tenga saldo suficiente (si se requiere en el negocio) antes de enviar a Firestore.

#### Posibles Problemas
- **Condiciones de carrera**: si dos usuarios crean transacciones simultáneamente sobre la misma cuenta, el saldo puede quedar inconsistente. Solución: `runTransaction` de Firestore lee el valor más reciente antes de actualizar, garantizando consistencia.
- **Formulario muy largo en móvil**: el formulario de transacción tiene muchos campos. Solución: dividirlo en secciones con un `Stepper` o un scroll suave con secciones claramente separadas.

#### Soluciones Recomendadas
Diseñar el formulario de transacción como una hoja deslizable (`DraggableScrollableSheet` o `BottomSheet`) en móvil para una experiencia más natural, y como un dialog o panel lateral en Web/Desktop.

---

### Fase 8 — Facturación y Presupuestos

#### Objetivos
Implementar los módulos de presupuestos por período y facturación, incluyendo la vinculación entre facturas y transacciones.

#### Herramientas
Firestore, Firebase Storage (para archivos adjuntos), `file_picker` package, Provider.

#### Qué se desarrollará

**Módulo de Presupuestos**:
- CRUD de presupuestos con selección de proyecto, categoría, período y monto planificado.
- Vista de resumen de presupuesto: card con barra de progreso que muestra el `monto_planificado` vs `monto_ejecutado`.
- Actualización automática del `monto_ejecutado` al registrar transacciones (implementada en Fase 7).
- Alertas visuales cuando un presupuesto supera el 80% de ejecución.

**Módulo de Facturas**:
- CRUD de facturas con campos de número, proveedor, fecha de emisión, fecha de vencimiento, monto y estado.
- Vinculación opcional de una factura a una transacción existente.
- Listado de facturas con filtros por estado (pendiente, pagada, vencida).
- Cálculo automático del estado "vencida" comparando la fecha de vencimiento con la fecha actual.
- Subida de archivo adjunto (PDF/imagen) a Firebase Storage y almacenamiento de la URL en el documento de Firestore.

#### Buenas Prácticas
- Los presupuestos son por período. Si se crean presupuestos mensuales, se debe poder filtrar por mes y año. Diseñar la consulta de Firestore con `where('periodo_inicio', isGreaterThanOrEqualTo, ...)`.
- Las facturas vencidas deben detectarse en el cliente Flutter (comparando fechas) o con una Cloud Function. En modo estándar sin producción, hacerlo en el cliente es suficiente.

#### Posibles Problemas
- **Subida de archivos en Web**: el manejo de archivos en Flutter Web usa `Uint8List` en lugar de rutas de archivo. El paquete `file_picker` lo abstrae correctamente, pero hay que tener cuidado con el tamaño máximo de archivos.
- **Inconsistencia entre `monto_ejecutado` y la suma real de transacciones**: si una transacción se elimina y no se actualiza el presupuesto correctamente. Solución: siempre usar `runTransaction` al modificar transacciones para actualizar el presupuesto de forma atómica.

#### Soluciones Recomendadas
Implementar una función de "recalcular presupuesto" que suma todas las transacciones de la categoría en el período y actualiza el `monto_ejecutado`. Esta función puede ejecutarse manualmente desde la UI de administración en caso de inconsistencias.

---

### Fase 9 — Testing Multiplataforma

#### Objetivos
Verificar que la aplicación funciona correctamente en Android, Web y Windows, y escribir pruebas automatizadas para la lógica de negocio más crítica.

#### Herramientas
Flutter Test, `mocktail` o `mockito` para mocking, Flutter Driver o `integration_test` para pruebas de integración, dispositivos físicos o emuladores.

#### Qué se desarrollará
- **Unit Tests**: pruebas para todos los casos de uso (`domain/usecases/`). Se mockean los repositorios para probar la lógica pura sin necesidad de Firebase.
- **Widget Tests**: pruebas para los widgets críticos: formulario de transacción (validación), Dashboard (que muestre los datos correctamente).
- **Integration Tests**: flujo completo de login → crear proyecto → registrar transacción → verificar saldo actualizado.
- **Pruebas manuales multiplataforma**: checklist de funcionalidades probadas en Android (emulador y dispositivo físico), Chrome (Web) y Windows.

#### Buenas Prácticas
- Separar los unit tests de los widget tests y de los integration tests en carpetas distintas (`test/unit/`, `test/widget/`, `test/integration/`).
- Apuntar a una cobertura mínima del 70% en la capa de dominio (casos de uso y entidades).
- Documentar los casos de prueba manuales en un archivo `TEST_CHECKLIST.md` en el repositorio.

#### Posibles Problemas
- **Firebase en pruebas unitarias**: los unit tests no deben conectarse a Firebase. Si los repositorios están bien abstraídos (con interfaces), se mockean fácilmente.
- **Diferencias de comportamiento entre plataformas**: especialmente en Web, ciertas APIs de Flutter se comportan diferente (como el manejo de archivos o los ScrollControllers). Probar exhaustivamente en Web es indispensable.

#### Soluciones Recomendadas
Usar el emulador de Firebase (`firebase emulators:start`) para los integration tests, de manera que no se consuman créditos de Firebase real y los tests sean reproducibles y aislados.

---

### Fase 10 — Implantación Estándar

#### Objetivos
Generar los builds finales de la aplicación para Android (APK), Web y Windows, y preparar el repositorio para un despliegue organizado.

#### Herramientas
Flutter build tools, GitHub, GitHub Pages o Firebase Hosting (para Web).

#### Qué se desarrollará
- Build de APK para Android: `flutter build apk --release`.
- Build de Flutter Web: `flutter build web --release`.
- Build de Windows: `flutter build windows --release`.
- Configuración de GitHub con ramas: `main` (estable), `develop` (desarrollo activo), `feature/*` (nuevas funciones).
- `README.md` completo con instrucciones de instalación, configuración de Firebase y cómo contribuir.
- Despliegue de la versión Web en Firebase Hosting con `firebase deploy`.

#### Buenas Prácticas
- Configurar las reglas de Firestore apropiadas antes de desplegar (ver Sección 7).
- Versionar la aplicación correctamente en `pubspec.yaml` siguiendo Semantic Versioning (`1.0.0+1`).
- Incluir un `CHANGELOG.md` desde la primera versión.

---

## 6. Dependencias Recomendadas

A continuación se presenta la lista de dependencias recomendadas para el archivo `pubspec.yaml`, organizadas por categoría y con su propósito explicado:

### Firebase y Backend

| Dependencia | Versión | Propósito |
|---|---|---|
| `firebase_core` | ^3.x | Inicialización de Firebase en todas las plataformas |
| `firebase_auth` | ^5.x | Autenticación con email/contraseña y gestión de sesiones |
| `cloud_firestore` | ^5.x | Base de datos NoSQL en tiempo real para todas las entidades |
| `firebase_storage` | ^12.x | Almacenamiento de archivos adjuntos de facturas |

### Manejo de Estado

| Dependencia | Versión | Propósito |
|---|---|---|
| `provider` | ^6.x | Manejo de estado reactivo con ChangeNotifier |

### Navegación

| Dependencia | Versión | Propósito |
|---|---|---|
| `go_router` | ^14.x | Enrutamiento declarativo con soporte de redirecciones y rutas anidadas |

### UI y Diseño

| Dependencia | Versión | Propósito |
|---|---|---|
| `google_fonts` | ^6.x | Fuente Inter y otras fuentes de Google Fonts |
| `flutter_adaptive_scaffold` | ^0.2.x | Layouts adaptativos para móvil, tablet y desktop |
| `fl_chart` | ^0.68.x | Gráficos de barras, líneas y pie para el Dashboard |
| `shimmer` | ^3.x | Efecto skeleton loader mientras se cargan datos |

### Utilidades

| Dependencia | Versión | Propósito |
|---|---|---|
| `intl` | ^0.19.x | Formateo de fechas y números según locale (monedas, fechas) |
| `file_picker` | ^8.x | Selección de archivos PDF/imagen para adjuntar a facturas |
| `uuid` | ^4.x | Generación de UUIDs únicos cuando se necesiten IDs personalizados |
| `equatable` | ^2.x | Comparación de objetos de dominio por valor, no por referencia |
| `dartz` | ^0.10.x | Tipo Either<Failure, Success> para manejo funcional de errores |

### Testing

| Dependencia | Versión | Propósito |
|---|---|---|
| `mocktail` | ^1.x | Creación de mocks para pruebas unitarias sin generación de código |
| `integration_test` | SDK | Pruebas de integración end-to-end incluidas con el SDK de Flutter |
| `flutter_test` | SDK | Framework de testing de widgets incluido con el SDK de Flutter |

---

## 7. Seguridad y Autenticación

### Login y Registro

El flujo de autenticación de FinanTrack utiliza **Firebase Authentication** con email y contraseña. El proceso es:

1. El usuario abre la app y el `AuthProvider` comprueba `FirebaseAuth.instance.authStateChanges()`.
2. Si no hay sesión activa, `go_router` redirige automáticamente a la pantalla de Login.
3. En Login, el usuario ingresa email y contraseña. El formulario valida localmente (campo no vacío, formato de email correcto, contraseña mínimo 6 caracteres) antes de enviar a Firebase.
4. Si la autenticación es exitosa, Firebase emite un evento en el stream y `go_router` redirige al Dashboard.
5. En Registro, además de crear el usuario en Firebase Authentication, se crea el documento `/users/{uid}` en Firestore.

### Manejo de Sesiones

Firebase Authentication maneja la persistencia de sesión automáticamente. El token JWT se almacena de forma segura por el SDK en cada plataforma (Keychain en iOS, EncryptedSharedPreferences en Android, localStorage en Web). La sesión persiste hasta que el usuario cierra sesión explícitamente.

Para cerrar sesión: `FirebaseAuth.instance.signOut()`. Esto limpia el token y el stream emite `null`, lo que hace que `go_router` redirija al Login.

### Seguridad en Firestore: Reglas

Las reglas de seguridad de Firestore son el componente más crítico. Sin reglas adecuadas, cualquier usuario podría leer o modificar los datos de otros. Se propone la siguiente estrategia:

```
// Pseudocódigo de reglas (no es código real de Firestore Rules)

// Solo usuarios autenticados pueden leer/escribir
// Los proyectos solo los puede leer/editar el owner
// Las subcolecciones (cuentas, transacciones) se heredan del acceso al proyecto
// Usuarios con rol 'admin' tienen acceso completo
// Usuarios con rol 'viewer' solo pueden leer, no escribir
```

Las reglas reales se escriben en `firestore.rules` y se despliegan con `firebase deploy --only firestore:rules`. Deben validar que:

- El usuario esté autenticado (`request.auth != null`).
- El `uid` del token coincida con el campo `owner_id` o que el usuario esté en la lista de miembros del proyecto.
- Los tipos de datos sean correctos (montos sean números positivos, fechas sean timestamps, etc.).

### Roles de Usuario

Se propone un sistema de roles simple a nivel de aplicación:

| Rol | Permisos |
|---|---|
| `admin` | CRUD completo de todas las entidades. Gestión de usuarios del proyecto. |
| `editor` | Crear y editar transacciones, facturas y presupuestos. Ver todo. |
| `viewer` | Solo lectura. No puede crear ni editar nada. |

El rol se almacena en el documento `/users/{uid}` en Firestore. Al cargar la app, el `AuthProvider` lee el rol del usuario actual y lo expone globalmente. Los widgets verifican el rol antes de mostrar botones de acción.

---

## 8. Flujo de Navegación

El flujo completo de navegación de FinanTrack se describe a continuación:

### Pantalla de Inicio (Splash)
Al abrir la app, se muestra una pantalla de splash con el logo de FinanTrack mientras Firebase inicializa. Una vez inicializado, se verifica el estado de autenticación y se redirige.

### Login / Registro
Si no hay sesión activa → Login. El usuario puede navegar a Registro desde el link en la pantalla de Login. Desde Login, tras autenticarse, va al Dashboard.

### Dashboard
La pantalla principal muestra un resumen ejecutivo: selector de proyecto activo, saldos de cuentas, últimas transacciones y estado de presupuestos. Desde aquí se puede navegar a cualquier módulo.

### Proyectos
Lista de proyectos del usuario. Al tocar un proyecto, se selecciona como proyecto activo y se muestra su detalle. Desde el detalle se puede ir a Cuentas, Categorías, Presupuestos o Transacciones del proyecto.

### Cuentas
Lista de cuentas del proyecto activo. Al tocar una cuenta, se muestra el historial de transacciones de esa cuenta. Botón "+" para crear nueva cuenta.

### Categorías
Lista de categorías del proyecto activo (ingreso y gasto). CRUD completo. Cada categoría muestra su color identificador.

### Transacciones
Lista de todas las transacciones del proyecto activo, con filtros por fecha, tipo y categoría. Botón "+" para registrar nueva transacción. Al tocar una transacción, se muestra su detalle y opciones de editar/eliminar.

### Presupuestos
Lista de presupuestos del proyecto activo. Vista de progreso (planificado vs ejecutado) por categoría y período. Botón "+" para crear nuevo presupuesto.

### Facturas
Lista de facturas con indicador de estado (pendiente/pagada/vencida). Filtros por estado y proveedor. Al tocar una factura, se muestra detalle con opción de ver archivo adjunto, editar o cambiar estado.

### Proveedores
Lista de proveedores. CRUD completo. Al tocar un proveedor, se muestran sus facturas asociadas.

### Perfil
Datos del usuario actual (nombre, email, rol). Opción de cerrar sesión. En versiones futuras: cambio de contraseña, foto de perfil.

---

## 9. Plan de Implantación

### Configuración Local

Antes de cualquier build, asegurarse de que:

- `flutter doctor` no muestra errores críticos.
- El archivo `firebase_options.dart` está correctamente generado.
- Las variables de entorno o configuraciones de Firebase apuntan al proyecto correcto.
- Todas las dependencias están descargadas con `flutter pub get`.

### Compilación por Plataforma

#### Android — APK de Distribución

```bash
flutter build apk --release
```

El APK se genera en `build/app/outputs/flutter-apk/app-release.apk`. Para distribuirlo sin Google Play, se puede compartir directamente este archivo. Para subir a Play Store, se debe generar un AAB: `flutter build appbundle --release` (requiere keystore de firma).

#### Flutter Web

```bash
flutter build web --release
```

Los archivos se generan en `build/web/`. Se despliegan en Firebase Hosting con:

```bash
firebase init hosting   # Solo la primera vez
firebase deploy --only hosting
```

Firebase Hosting provee HTTPS automático y CDN global, lo que hace que la versión web sea accesible desde cualquier lugar con una URL como `https://finantrack-dev.web.app`.

#### Windows Desktop

```bash
flutter build windows --release
```

El ejecutable se genera en `build/windows/x64/runner/Release/`. Para distribuirlo, se copia esta carpeta completa (contiene el .exe y las DLLs necesarias) o se crea un instalador con NSIS o Inno Setup.

---

### Control de Versiones con Git y GitHub

Se propone la siguiente estrategia de ramas:

| Rama | Propósito |
|---|---|
| `main` | Código estable y compilable. Solo recibe merges de `develop`. |
| `develop` | Rama de integración. Aquí se mergean las features terminadas. |
| `feature/nombre-feature` | Una rama por cada nueva funcionalidad. Se crea desde `develop`. |
| `fix/nombre-bug` | Una rama por corrección de error urgente. |

El flujo es: `feature/*` → Pull Request → revisión → merge a `develop` → cuando está estable → merge a `main` con tag de versión (`v1.0.0`).

Cada commit sigue el estándar **Conventional Commits**: `feat: add transaction form`, `fix: correct balance calculation`, `chore: update firebase dependencies`.

---

## 10. Recomendaciones Profesionales

### Organización del Equipo y el Código

- **Acordar convenciones antes de escribir código**: nombres de archivos, clases, métodos, estructura de carpetas. Es más fácil acordar al inicio que refactorizar después.
- **Code reviews obligatorios**: aunque sea un proyecto personal o de estudiantes, revisar el propio código después de un día ayuda a encontrar errores que no se ven en el momento de escribir.
- **Documentar las decisiones arquitectónicas**: un archivo `ARCHITECTURE.md` en el repositorio que explique por qué se tomaron ciertas decisiones es invaluable cuando vuelves al proyecto después de meses.

### Escalabilidad

- La estructura de Clean Architecture con la que se propone arrancar el proyecto **ya es escalable**. No necesitas sobrediseñar desde el inicio; confía en la separación de capas.
- Si en el futuro se quiere agregar Riverpod en lugar de Provider, solo hay que cambiar la capa de presentación; el dominio y los datos no cambian.
- Si en el futuro se quiere agregar una API REST propia en lugar de Firestore, solo hay que reemplazar los repositorios concretos; el dominio y la presentación no cambian. Eso es la belleza de Clean Architecture.

### Rendimiento

- **Nunca cargues todos los documentos de una colección**. Usa siempre `limit()` y `startAfterDocument()` para paginar. Firestore cobra por lectura; 1000 transacciones cargadas de golpe son 1000 lecturas.
- **Usa `StreamBuilder` con caché**: Firestore tiene persistencia offline activada por defecto. Cuando un usuario abre la app sin internet, verá los últimos datos cargados.
- **Evita rebuilds innecesarios en Flutter**: usa `context.select<T>()` en lugar de `context.watch<T>()` cuando solo necesitas un campo específico de un Provider. Esto limita los rebuilds al widget que los necesita.
- **Lazy loading en imágenes**: si en el futuro se muestran imágenes de comprobantes, usar `cached_network_image` para no recargar la misma imagen múltiples veces.

### Optimización de Firestore

- **Índices compuestos**: cuando una consulta usa múltiples campos con filtros y ordenamiento, Firestore pedirá crear un índice. Créalo desde el link que aparece en el error de la consola de Flutter; es automático.
- **Batched writes**: cuando necesites crear/actualizar múltiples documentos independientes (sin necesidad de transacción), usa `WriteBatch` en lugar de múltiples `set()` separados. Es más eficiente y atómico.
- **Escuchar solo lo necesario**: no pongas un `snapshots()` en toda la colección de transacciones si solo necesitas las últimas 10. Usa `.limit(10).snapshots()`.

### UI/UX

- **Feedback inmediato**: cada acción del usuario debe tener una respuesta visual inmediata. Si toca un botón, debe ver un indicador de carga antes de que llegue la respuesta de Firebase.
- **Modo oscuro desde el inicio**: Flutter lo soporta nativamente. No es una feature de segunda versión; es una expectativa del usuario moderno. Configura ambos temas desde la Fase 5.
- **Accesibilidad básica**: usa `Semantics` widgets en elementos importantes. Asegúrate de que los tamaños de fuente respetan el `textScaleFactor` del sistema.
- **Microinteracciones**: animaciones sutiles en transiciones de pantalla (`Hero`, `AnimatedSwitcher`) hacen que la app se sienta premium sin mucho esfuerzo.

### Firebase — Consideraciones Importantes

- **Las reglas de Firestore en modo "test" expiran en 30 días**. Antes de que expiren, debes configurar reglas reales. Pon un recordatorio.
- **Monitorea el uso de Firebase**: en la consola de Firebase hay un panel de uso. En modo estándar (Spark plan, gratuito), los límites son generosos para desarrollo, pero es bueno saber cuánto estás usando.
- **Firebase Authentication tiene rate limiting**: no puedes crear miles de usuarios en segundos. Para testing con muchos usuarios, usa el emulador local.

### Arquitectura Limpia — Consejo Final

El mayor error que cometen los desarrolladores al aprender Clean Architecture es **sobrecomplicarla** en proyectos pequeños o simplificarla en exceso cuando el proyecto crece. Para FinanTrack, el equilibrio correcto es:

- Una entidad por modelo de negocio (no más, no menos).
- Un caso de uso por operación con nombre de verbo (`GetTransactions`, `CreateTransaction`, `DeleteTransaction`).
- Un repositorio concreto por fuente de datos (Firestore en este caso).
- Un Provider por módulo o pantalla compleja.

Si sigues este principio consistentemente, FinanTrack será una aplicación mantenible, extensible y de la que estarás orgulloso tanto como estudiante como como desarrollador profesional.

---

*Documento generado para el proyecto FinanTrack — Versión 1.0*
*Flutter + Firebase | Arquitectura Clean + MVVM | Multiplataforma*
