# Antipatrones y oportunidades de mejora en ARISCON

## 1. Antipatrones detectados

### 1.1 Big Ball of Mud

ARISCON es el ejemplo más claro de este antipatrón. Se desarrolló por etapas, con varios proveedores, piezas separadas y distintos estándares. El resultado es un sistema donde la lógica de negocio, la interfaz, la seguridad y la persistencia están profundamente acopladas y muy difíciles de evolucionar.

Evidencias:

- Cinco aplicaciones diferentes para el mismo dominio.
- Diferentes proveedores y tiempos de construcción.
- Código complejo y con alta deuda técnica.
- Muchas funcionalidades implementadas en la Web y no en móviles, o viceversa.

### 1.2 Entropy / uncontrolled growth

La solución se fue creciendo sin un marco arquitectónico ni un estándar tecnológico claro. Cada nueva necesidad se resolvió con una implementación ad hoc, sin revisión de arquitectura ni calidad de software.

### 1.3 Copy-paste and custom one-off solutions

Se observa una arquitectura basada en soluciones aisladas y no reutilizables. La seguridad se repite por aplicación, la base de datos se duplicó y la sincronización entre sistemas se hace de manera asíncrona sin una estrategia global.

### 1.4 Lack of architectural standards

No existían estándares de programación, datos y arquitectura antes del rediseño. Esto es una causa directa de la fragmentación tecnológica y funcional.

## 2. Oportunidad de reusabilidad

Sí hay varias oportunidades claras de reutilización:

- Servicio común de autenticación y autorización.
- Modelo común de usuarios, roles y permisos.
- API Gateway para centralizar consumo y políticas.
- Servicios reutilizables para matrícula, asistencias, tareas, notas y reportes.
- Componentes base de UI con diseño corporativo único.
- Módulos de auditoría y logging compartidos.
- Pipelines reutilizables de CI/CD y calidad (SonarQube, pruebas, despliegue).

La arquitectura futura debería buscar componentes reutilizables por dominio y no duplicados por canal (web, móvil, admin, reporting).

## 3. Contratos existentes y faltantes

### 3.1 Contratos que sí existen

- Servicios REST + JSON para la capa de aplicación.
- Integración con sistemas externos (contabilidad, RR. HH., inventario, pizarras virtuales).
- Autenticación por usuario y clave dentro de cada aplicación.
- Contratos funcionales del negocio por módulo (matrícula, notas, tareas, mensajes, reportes).

### 3.2 Contratos que faltan o están deficientes

- Contrato único de seguridad para identidad y acceso.
- Contrato de auditoría para registrar: fecha/hora, usuario, operación, canal, alumno asociado, metadata.
- Contrato de integración para sincronización de datos entre módulos y sistemas.
- Contrato de versionado para APIs, para no romper consumidores.
- Contrato de error y respuesta estandarizada (HTTP codes, mensajes, errores funcionales).

En síntesis, la solución actual tiene servicios y datos, pero no tiene una capa de contratos bien definida ni gobernada.

## 4. ¿Se puede usar otra base de datos no relacional?

Sí, pero de manera selectiva y no para todo el sistema.

### Caso recomendado:

- Base de datos relacional (SQL Server / PostgreSQL):
  - Información académica crítica.
  - Matrículas, notas, tareas, asistencia, historial académico.
  - Requerimientos de integridad transaccional y consistencia.

- Base de datos NoSQL (documental o clave-valor):
  - Logs de auditoría masivos.
  - Notificaciones, mensajes, contenido de eventos, caché, analítica en tiempo real.
  - Almacenamiento de reportes o datos de monitoreo.

### Recomendación arquitectónica

No reemplazar la base relacional como sistema de registro principal, porque la escuela requiere integridad, consistencia y transacciones complejas. La base no relacional puede ser útil para casos de alto volumen, analítica y eventos, pero no debería ser la única base del sistema.

## 5. Recomendación general del rediseño

ARISCON requiere un rediseño de arquitectura basado en:

- Una única identidad compartida.
- Un único modelo de dominio y bases de datos consistentes.
- API Gateway y microservicios o servicios desacoplados por dominio.
- Frontends omnicanal (web, móvil, admin) sobre una capa común.
- Seguridad centralizada y observabilidad.
- Cloud nativa en AWS, con escalado y balanceo de carga.

## 6. Conclusión

Sí existen antipatrones claros: Big Ball of Mud, fragmentación tecnológica, falta de estándares, duplicación de seguridad y base de datos, además de crecimiento no controlado. Sin embargo, la situación también abre una oportunidad estratégica para transformar ARISCON en una arquitectura moderna, reutilizable, gobernada por contratos y preparada para crecer con el futuro del negocio.
