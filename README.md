# Proyecto Integrador: Sistema de Gestión y Circulación para la Biblioteca de la FEI

| Campo | Detalle |
| :--- | :--- |
| **Institución** | Universidad Veracruzana — Facultad de Estadística e Informática (FEI) |
| **Experiencia Educativa** | Bases de Datos |
| **Ponderación Total** | **35 puntos** (calificación final) |
| **Modalidad** | Células de trabajo (equipos de desarrollo) |
| **Entorno Tecnológico** | Docker (SQL Server) + Visual Studio Code (Notebooks de SQL e instancias locales) + Git / GitHub |

---

## Objetivo General del Proyecto

Diseñar, normalizar, programar y consultar de forma avanzada el sistema de información de la **Biblioteca de la Facultad de Estadística e Informática (FEI)** de la Universidad Veracruzana, aplicando de manera integral los conceptos teóricos y técnicos de la asignatura mediante un flujo de trabajo profesional basado en control de versiones con Git, contenedores Docker y notebooks interactivos en VS Code.

---

## Ponderación y Fases de Evaluación (Total: 35 Puntos)

| Fase | Contenido / Entregables | Ponderación |
| :--- | :--- | :---: |
| **Fase 1: Análisis y Modelado** | Matriz de requerimientos, reglas de negocio, diagrama de Chen, modelos relacionales (inicial y normalizado hasta 4FN) y diccionario de datos. | **5 puntos** |
| **Fase 2: DDL** | Creación de la base de datos, tablas, llaves primarias/foráneas y restricciones (`CHECK`, `UNIQUE`, nomenclatura institucional). | **2 puntos** |
| **Fase 3: DML vía Procedimientos Almacenados** | Poblado verídico de recursos y usuarios (entrevistas a alumnos y maestros), préstamos (15 nov – 15 dic 2026), actualización masiva con `GETDATE()` y sanciones. | **12 puntos** |
| **Fase 4: DQL vía Procedimientos Almacenados** | Programación y ejecución de los 3 reportes requeridos usando `JOIN`, funciones de agregación y agrupamiento (`GROUP BY` / `HAVING`). | **8 puntos** |
| **Fase 5: Seguridad (DCL)** | Script de creación de usuarios, roles asignados y control de permisos. | **2 puntos** |
| **Fase 6: Reporte Final** | Documento integrador oficial (`reporte-final.pdf`) con estructura técnica, evidencias y conclusiones. | **6 puntos** |
| **Total** | | **40 puntos** |

---

## Convenciones y Reglas Técnicas Obligatorias

* **Nomenclatura de base de datos:**
  * Tablas en **Plural PascalCase** (ej. `Usuarios`, `Ejemplares`, `Prestamos`, `Sanciones`).
  * Atributos en **camelCase** iniciando en minúscula (ej. `idUsuario`, `fechaPrestamo`).
  * Llaves foráneas con el sufijo **`_FK`** (ej. `idUsuario_FK`, `folio_FK`).
* **Ejecución de DML y DQL:** Las operaciones de inserción, actualización, sanciones y reportes analíticos se encapsulan y ejecutan mediante los **procedimientos almacenados** definidos en `02_programmability/02_stored_procedures/`.
* **Datos reales y acotados:**
  * Recursos obtenidos con datos verídicos de la biblioteca.
  * Entrevistas reales a compañeros estudiantes y a maestros (sin usar datos propios del equipo en el bloque de estudiantes entrevistados).
  * Préstamos con fechas de inicio entre el **15 de noviembre y el 15 de diciembre de 2026**, calculando vencimientos conforme a las reglas de negocio.

---

## Organización y Estructura Oficial del Repositorio GitHub

Cada célula de trabajo clonará el repositorio base provisto por la cátedra y subirá su avance respetando estrictamente esta estructura:

```text
biblioteca-fei/
├── README.md                                     # Guía general del proyecto, equipo, requisitos y flujo de ejecución
├── .gitignore                                    # Archivos temporales, credenciales y configuraciones locales
│
├── 01_modelado/                                  # Fase 1: Análisis y modelado de datos (5 puntos)
│   ├── matriz-requerimientos.pdf                   # Requerimientos funcionales y técnicos del sistema
│   ├── reglas-negocio.pdf                          # Políticas de préstamo, plazos, multas y restricciones
│   ├── diccionario-datos.xlsx                      # Diccionario estructurado (entidades, atributos, tipos, PK/FK)
│   ├── diagrama-chen.png                           # Modelo conceptual de Chen (con agregaciones)
│   ├── modelo-relacional-inicial.png               # Modelo relacional preliminar
│   └── modelo-relacional-normalizado.png           # Modelo normalizado en 4FN con tablas puente
│
├── 02_programmability/                           # Scripts SQL secuenciales para SQL Server (Docker)
│   ├── 01_ddl/                                   # Fase 2: Estructura de la base de datos (2 puntos)
│   │   ├── 01_create_database.sql                  # Creación de la base de datos BibliotecaFEI
│   │   └── 02_create_tables.sql                    # Tablas (Plural PascalCase, llaves _FK, CHECK y UNIQUE)
│   ├── 02_stored_procedures/                     # Fases 3 y 4: Procedimientos almacenados (DML y DQL) (20 puntos)
│   │   ├── inserts/                                # Altas de catálogo, usuarios, préstamos y sanciones
│   │   │   ├── 01_spi_libros.sql
│   │   │   ├── 02_spi_recursos_electronicos.sql
│   │   │   ├── 03_spi_material_complementario.sql
│   │   │   ├── 04_spi_videos.sql
│   │   │   ├── 05_spi_libros_referencia.sql
│   │   │   ├── 06_spi_libros_literatura.sql
│   │   │   ├── 07_spi_usuarios.sql
│   │   │   ├── 08_spi_prestamos.sql
│   │   │   └── 10_spi_sanciones.sql
│   │   ├── updates/                                # Modificaciones y recálculo de plazos/estatus
│   │   │   └── 09_spa_fecha_prestamo.sql
│   │   └── reportes/                               # Consultas analíticas y auditoría (DQL)
│   │       ├── 11_sps_reporte_prestamos_usuario.sql
│   │       ├── 12_sps_reporte_circulacion.sql
│   │       └── 13_sps_reporte_adeudos_alumnos.sql
│   ├── 03_seguridad/                             # Fase 5: Control de accesos (DCL) (2 puntos)
│   │   └── 01_users_security.sql                   # Usuarios, roles (usrBiblioteca, usrAlumno, usrDocente) y permisos
│   │
├── 03_notebooks/                                 # Ejecución interactiva en VS Code (MSSQL) con salidas visibles
│   ├── 01_poblado_y_actualizacion_dml.ipynb        # Cargas iniciales, actualizaciones con GETDATE() y sanciones
│   ├── 02_reportes_dql.ipynb                       # Ejecución y salidas de los 3 reportes estadísticos y de auditoría
│   └── 03_pruebas_integridad.ipynb                 # Pruebas de llaves foráneas, restricciones y errores esperados
│
└── 04_entrega_final/                             # Fase 6: Cierre del proyecto (6 puntos)
    └── reporte-final.pdf                           # Documento integrador oficial del equipo
```
---

## Flujo de Trabajo Recomendado

1. **Clonar y configurar:** Clonar el repositorio en el equipo de trabajo, verificar `.gitignore` y levantar la instancia de SQL Server en **Docker**.
2. **Modelar y documentar (`01_modelado/`):** Consolidar requerimientos, reglas de negocio, diagramas y diccionario normalizado hasta 4FN.
3. **Construir esquema y lógica (`02_programmability/`):** Ejecutar en orden los scripts DDL (`01_ddl/`), crear los procedimientos almacenados (`02_stored_procedures/`) y configurar seguridad (`03_seguridad/`).
4. **Validar en notebooks (`03_notebooks/`):** Ejecutar las pruebas desde **VS Code** contra SQL Server y guardar cada `.ipynb` con sus salidas (`outputs`) visibles.
5. **Integrar memoria técnica (`04_entrega_final/reporte-final.pdf`):** Elaborar y guardar el reporte final en la ruta oficial antes del cierre.

---

## Requisitos Indispensables de Cierre y Notebooks

### Notebooks Interactivos (`03_notebooks/`)

Los alumnos realizarán las pruebas de ejecución interactiva en contenedores Docker mediante VS Code. Los archivos `.ipynb` ubicados en la carpeta `03_notebooks/` deben subirse obligatoriamente con los **outputs de ejecución visibles en cada celda**, demostrando que los procedimientos almacenados y las consultas arrojaron resultados correctos (o los errores esperados en pruebas de integridad) en el servidor local.

### Documento Integrador (`04_entrega_final/reporte-final.pdf`)

Cada equipo deberá incluir obligatoriamente en `04_entrega_final/reporte-final.pdf` el documento integrador oficial del equipo, elaborado con calidad profesional y estructurado con los siguientes apartados:

1. **Portada institucional**
   * Logotipos oficiales de la Universidad Veracruzana y de la Facultad de Estadística e Informática (FEI).
   * Nombre de la experiencia educativa: **Bases de Datos**.
   * Título del proyecto: *Sistema de Gestión y Control para la Biblioteca de la FEI*.
   * Integrantes del equipo con sus respectivos roles (*Student CEO*, Arquitecto de Base de Datos, Especialista en Consultas DQL/SP, etc.).
   * Fecha de entrega y nombre del docente.

2. **Introducción y justificación**
   * Contexto y problemática del control de acervo, préstamos y usuarios en la Biblioteca de la FEI.
   * Objetivos general y específicos del sistema de base de datos implementado.

3. **Ingeniería y modelado de datos (resumen técnico)**
   * **Matriz de requerimientos y reglas de negocio:** Descripción de políticas de préstamo, plazos y sanciones.
   * **Diagrama de Chen y modelo relacional:** Imágenes de los diagramas conceptuales y del modelo relacional normalizado hasta 4FN, explicando relaciones 1:N y N:M mediante tablas puente.
   * **Diccionario de datos:** Resumen o referencia de tablas principales, tipos de datos, restricciones y llaves foráneas (`_FK`).

4. **Implementación del esquema y lógica (DDL, DML y procedimientos almacenados)**
   * **Creación de la base de datos y tablas (DDL):** Cumplimiento de la nomenclatura institucional (*Plural PascalCase*, *camelCase* y sufijo `_FK`).
   * **Poblado de datos reales (DML):** Metodología para obtener datos verídicos (recursos de la biblioteca, entrevistas a 10 estudiantes y 5 maestros reales, y préstamos entre el 15 de noviembre y el 15 de diciembre de 2026).
   * **Uso de procedimientos almacenados:** Detalle técnico de los procedimientos de inserción, actualización con `GETDATE()` y control de sanciones.

5. **Consultas analíticas y reportes (DQL)**
   * Documentación y explicación analítica de los **3 reportes obligatorios** (historial y conteo de préstamos por usuario, análisis de circulación e inventario de ejemplares por clasificación, y auditoría y adeudos totales por multas en estudiantes), incluyendo el código SQL del procedimiento almacenado correspondiente y una muestra tabulada del resultado.

6. **Evidencias de ejecución (capturas clave)**
   * Capturas claras y legibles del contenedor de **Docker** ejecutando SQL Server.
   * Ejecución de scripts y procedimientos desde **VS Code**.
   * **Notebooks de SQL (`.ipynb`)** con los outputs visibles en las consultas y reportes.

7. **Conclusiones y reflexiones del equipo**
   * Conclusiones individuales y grupales sobre el aprendizaje obtenido.
   * Retos superados en modelado, normalización, programación de procedimientos y diseño de consultas.

---


## Inicio del equipo y uso de esta plantilla

| Integrante | Matrícula | Rol | Responsabilidad |
| --- | --- | --- | --- |
| [Completar] | [Completar] | Student CEO | [Completar] |
| [Completar] | [Completar] | Arquitecto de Base de Datos | [Completar] |
| [Completar] | [Completar] | Especialista en Consultas DQL/SP | [Completar] |

Grupo: [Completar]. Docente: [Completar]. Fecha de entrega: [Completar].

Los archivos suministrados son plantillas pendientes de completar. Los PNG son lienzos de reserva, los PDF son guías y los notebooks aún no tienen ejecuciones. Sustituirlos por el trabajo y las salidas reales del equipo antes de entregar.

### Primeros pasos
1. Completar `docs/configuracion-entorno.md` y configurar Docker, SQL Server y el kernel SQL en VS Code.
2. Completar las fuentes de `docs/plantillas/` y exportar los PDF a las rutas oficiales. Abrir y completar el diccionario XLSX. Crear diagramas y guardar sus fuentes en `docs/diagramas/`.
3. Implementar el DDL. Ejecutar `01_create_database.sql` y después `02_create_tables.sql`.
4. Implementar los procedimientos en orden global 01–13: primero altas 01–08, después actualización 09, sanciones 10 y reportes 11–13, aunque estén en subcarpetas distintas. Definir seguridad y probar cada perfil.
5. Completar los tres notebooks, ejecutar contra la base local y guardar outputs visibles. Las celdas SQL no llevan un kernel ficticio: seleccionar el kernel instalado compatible con SQL Server. MSSQL permite trabajar con scripts, pero la ejecución de `.ipynb` requiere soporte de kernel adicional.
6. Integrar el reporte final con los siete apartados oficiales y revisar `docs/checklist-entrega.md`.

La plantilla contiene seis espacios de actualización y ocho de sanción como apoyo de trabajo. El README original no fija esas cantidades: confirmar la cantidad evaluable con el docente. Los datos de inicio de préstamos sí deben quedar entre 2026-11-15 y 2026-12-15; GETDATE() debe registrarse como fecha real de ejecución, sin alterar artificialmente el reloj.

### Material de apoyo
- `docs/plantillas/`: fuentes editables de los tres PDF.
- `docs/diagramas/`: instrucciones para guardar fuentes de los diagramas.
- `docs/registro-fuentes.csv`: trazabilidad de recursos y entrevistas, con identificadores de referencia.
- `docs/matriz-permisos.csv`: permisos esperados y pruebas por identidad.
- `docs/checklist-entrega.md`: control de los 40 puntos y requisitos de cierre.

### Control de versiones
Cada equipo debe registrar avances con commits descriptivos. No subir contraseñas, archivos de base de datos ni credenciales guardadas en outputs. Los recursos y entrevistas deben ser reales; documentar la procedencia y acordar qué datos identificables se incluyen en el repositorio. El docente publicará el repositorio base; los alumnos clonarán o crearán su copia según la modalidad indicada.

```powershell
git clone <URL_DEL_REPOSITORIO>
cd biblioteca-fei
git status
git add .
git commit -m "Completa fase y evidencias correspondientes"
git push
```
