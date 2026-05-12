## Prompt
Actua como administrador de base de datos de proyecto de finanzas, que entidades se necesitan para su gestion
Para gestionar un proyecto de finanzas de manera efectiva, aquí están las entidades principales que necesitarías en la base de datos, junto con un diagrama visual.Aquí el resumen de cada entidad y su propósito:

**Núcleo del proyecto**

- `PROYECTO` — entidad raíz que agrupa todo: presupuesto total, estado, fechas y moneda base.
- `CUENTA` — representa cuentas bancarias, cajas chicas o fondos vinculados al proyecto. Tiene tipo (activo, pasivo, capital) y saldo actualizable.
- `TRANSACCION` — el corazón operativo. Registra ingresos, egresos y transferencias entre cuentas, con fecha, monto, estado y usuario responsable.

**Control presupuestal**

- `CATEGORIA` — clasifica las transacciones (ej. nómina, operaciones, marketing). Permite reportes por rubro.
- `PRESUPUESTO` — compara lo planificado vs. lo ejecutado por categoría y período. Fundamental para control de desviaciones.

**Relaciones externas**

- `PROVEEDOR` — catálogo de terceros que facturan al proyecto, con RFC/datos fiscales y moneda.
- `FACTURA` — vincula un documento fiscal con su transacción de pago y proveedor. Incluye fechas de emisión y vencimiento para gestionar cuentas por pagar.

**Seguridad y trazabilidad**

- `USUARIO` — con roles diferenciados (admin, contador, visualizador) para controlar quién puede registrar o solo consultar.

---

Dependiendo del alcance del proyecto podrías extender esto con entidades como `TIPO_CAMBIO` (si manejas múltiples monedas), `CENTRO_COSTO`, `CONTRATO` o `AUDITORIA_LOG`. ¿Quieres que profundice en alguna entidad o que genere los scripts DDL para algún motor de base de datos?



El script está listo para descargar. Lo que incluye:

**Estructura completa**
- 8 tipos `ENUM` para garantizar integridad en campos de estado y tipo
- 8 tablas con todas sus columnas, tipos exactos y restricciones (`PK`, `FK`, `NOT NULL`, `UNIQUE`, `CHECK`)
- 15 índices secundarios en columnas de búsqueda frecuente (fechas, estados, claves foráneas)

**Reglas de negocio implementadas como constraints**
- Una transacción de tipo `transferencia` exige que ambas cuentas existan y sean distintas
- El `total` de una factura siempre debe ser igual a `subtotal + impuesto`
- El presupuesto no puede tener `periodo_fin` anterior al `periodo_inicio`
- Los montos monetarios no pueden ser negativos

**Extras**
- Trigger `fn_set_updated_at()` que actualiza automáticamente el campo `updated_at` en todas las tablas
- Datos semilla (`INSERT`) con un proyecto demo, dos usuarios, dos cuentas y cuatro categorías para arrancar pruebas de inmediato
- Comentarios en cada tabla y columna clave

El script es compatible con **PostgreSQL 15+**. Si necesitas adaptarlo a MySQL o SQL Server, con gusto genero esa versión.
<img width="1440" height="2134" alt="image" src="https://github.com/user-attachments/assets/4b4d1828-2e70-4cbe-8a15-d77a5653ddfd" />
