# PROMPT V2 — FINANTRACK
Actúa como un arquitecto senior de software especializado en Flutter, Dart y Firebase.  
Tu objetivo es ayudarme a diseñar y planificar profesionalmente una aplicación multiplataforma llamada “FinanTrack”, enfocada en administración financiera, control de presupuestos, cuentas, transacciones y facturación.
La aplicación será desarrollada en Flutter utilizando VS Code y Firebase en configuración estándar (NO producción), compatible con:
- Android
- Web
- Windows
- iOS
NO quiero código todavía.  
Primero necesito un plan de implementación completo, profesional y humano en formato Markdown.
La aplicación debe estar basada en la siguiente estructura lógica y entidades:
## Entidades principales
### Proyecto
- id
- nombre
- descripcion
- fecha_inicio
- fecha_fin
- estado
- presupuesto_total
### Categoria
- id
- proyecto_id
- nombre
- tipo
- color
### Cuenta
- id
- proyecto_id
- nombre
- tipo
- moneda
- saldo_inicial
- saldo_actual

### Usuario
- id
- nombre
- email
- rol
### Transaccion
- id
- cuenta_origen_id
- cuenta_destino_id
- categoria_id
- usuario_id
- monto
- fecha
- tipo
- descripcion
- estado
### Presupuesto
- id
- proyecto_id
- categoria_id
- monto_planificado
- monto_ejecutado
- periodo_inicio
- periodo_fin
### Proveedor
- id
- nombre
- rfc
- contacto
- moneda
### Factura
- id
- proveedor_id
- transaccion_id
- numero
- fecha_emision
- fecha_vencimiento
- monto_total
- estado
# Necesito que generes:
## 1. Descripción general del sistema
Explica de manera profesional y humana cómo funcionará FinanTrack y cuál es su objetivo principal.
## 2. Arquitectura del proyecto
Define:
- Arquitectura recomendada en Flutter
- Organización de carpetas
- Separación frontend/backend
- Manejo de estados
- Estructura escalable
- Buenas prácticas
## 3. Tecnologías necesarias
Explica:
- Flutter
- Dart
- Firebase
- Firestore
- Firebase Authentication
- Firebase Storage
- Provider
- Dependencias necesarias
- Herramientas de desarrollo
## 4. Diseño UI/UX
Describe:
- Estilo visual moderno
- Diseño responsive
- Navegación intuitiva
- Paleta de colores
- Experiencia de usuario
- Diseño para Android/Web/Desktop
## 5. Planeación del desarrollo
Quiero un procedimiento PASO A PASO y humano:
### Fase 1 — Configuración del entorno
### Fase 2 — Configuración de Firebase
### Fase 3 — Diseño de base de datos Firestore
### Fase 4 — Sistema de autenticación
### Fase 5 — Pantallas principales
### Fase 6 — CRUD de entidades
### Fase 7 — Gestión de transacciones
### Fase 8 — Facturación y presupuestos
### Fase 9 — Testing multiplataforma
### Fase 10 — Implantación estándar
Cada fase debe incluir:
- Objetivos
- Herramientas
- Qué se desarrollará
- Buenas prácticas
- Posibles problemas
- Soluciones recomendadas
## 6. Dependencias recomendadas
Genera una lista profesional de dependencias para pubspec.yaml y explica para qué sirve cada una.
## 7. Seguridad y autenticación
Explica:
- Login
- Registro
- Manejo de sesiones
- Seguridad básica en Firebase
- Reglas Firestore
- Roles de usuario
## 8. Flujo de navegación
Describe cómo navegará el usuario entre:
- Login
- Dashboard
- Cuentas
- Categorías
- Presupuestos
- Transacciones
- Facturas
- Perfil
## 9. Plan de implantación
Genera un plan profesional para desplegar la aplicación en modo estándar (NO producción).
Incluye:
- Configuración local
- Compilación
- APK
- Flutter Web
- Windows Desktop
- GitHub
- Control de versiones
## 10. Recomendaciones profesionales
Quiero recomendaciones reales como si fueras un desarrollador senior:
- Organización
- Escalabilidad
- Rendimiento
- Optimización
- UI/UX
- Firebase
- Firestore
- Arquitectura limpia
# IMPORTANTE
- NO generar código todavía
- TODO debe estar en Markdown
- Explica de manera clara, humana y profesional
- Usa lenguaje técnico pero entendible para estudiantes
- El resultado debe parecer un documento profesional de planificación de software
- No resumir
- Sé detallado

