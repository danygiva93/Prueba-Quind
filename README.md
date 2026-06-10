# Prueba-Quind
1. Documento: Libro del Dominio Afiliaciones (Dominio Afiliados - QUIND)
Objetivo General: Centralizar la metadata de negocio, operativa y técnica del dominio de datos de Afiliados. Actúa como el puente entre las definiciones estratégicas de los Data Owners y la implementación técnica.

Detalle por hoja y su aterrizaje en Azure:

  •	1. Info. General: Define el alcance, descripción y los stakeholders principales del dominio (ej. Gerente de Afiliaciones).
  
      En Azure: Se configura como un "Dominio" (Domain) o "Colección" raíz dentro del Data Map de Microsoft Purview para               organizar     los activos por área de negocio.
      
      
  •	2. Glosario de Términos: Catálogo de mínimo 5 términos clave de negocio (BDO, Afiliado, Unicidad, etc.) y sus definiciones.
  
      En Azure: Se importa directamente al Business Glossary de Microsoft Purview, permitiendo etiquetar los activos técnicos         escaneados con estos términos comprensibles para el negocio.
      
      
  •	3. Equipo de dominio & 4. Procesos: Mapea los roles (Business Data Owner, Data Steward, Usuarios) y los procesos de negocio         en los que participan (ej. Solicitud de Afiliación).
  
        En Azure: Se traduce en la asignación de roles (Data Curator, Data Reader) mediante Azure Role-Based Access Control           (RBAC) y políticas de acceso dentro de Purview y Unity Catalog.
        
        
  •	5. Inventario de Fuentes & 6. Entidades_Tablas: Registra los orígenes de los datos como el CRM Afiliados (SQL Server) y el         ERP.
  
      En Azure: Corresponde a los "Data Sources" registrados en Purview. Las tablas se mapean como "Assets" tras configurar           escaneos automáticos sobre Azure Data Lake Storage (ADLS Gen2) o Azure SQL Database.
      
      
  •	7. Inventario de Datos: Es el diccionario a nivel de campo (Id_Afiliado, Numero_Documento, etc.) indicando su tipo de dato y         clasificación de seguridad (Sensible, Privado).
  
      En Azure: Representa el esquema ("Schema") del activo en Purview Data Catalog, donde los campos adquieren etiquetas             automáticas mediante reglas de clasificación (Classification Rules).
      
      
  •	8. Reglas de Calidad: Define las reglas de Unicidad, Completitud y Validez (ej. El correo no puede estar vacío, el teléfono         debe tener 10 dígitos).
  
      En Azure: Se puede implementar utilizando Purview Data Quality para perfilar los datos, o directamente en la capa de             procesamiento (Medallion Architecture) usando Expectations en Databricks Delta Live Tables (DLT).
      
  •	9. Solución de Inconsistencias & 10. Análisis Causa Raíz: Bitácora para rastrear problemas de completitud y validez               detectados.
  
    En Azure: Puede integrarse mediante flujos de trabajo (Workflows) en Purview para asignar tareas de remediación a los Data       Stewards.
    
    
  •	12. Riesgos (Fundamental para Ley 1581): Identifica campos confidenciales (Número_Documento, Correo_Electrónico, Teléfono) y       define el riesgo de fuga o suplantación.
  
    En Azure: Esta hoja es crítica y justifica la creación de reglas de Enmascaramiento Dinámico de Datos (Dynamic Data             Masking) implementadas a través de Databricks Unity Catalog, asegurando que analistas no autorizados solo vean los últimos 4     dígitos de un documento o teléfono en los tableros de consumo.
    


2. Documento: Nivel de Madurez de Calidad de Datos (NMCD - QUIND)
Objetivo General: Evidenciar la ejecución práctica y medición de las reglas de calidad definidas en el libro del dominio. Demuestra la capacidad de perfilar datos identificando el nivel de madurez, unicidad, completitud y validez de la tabla maestra.
Detalle por hoja y su aterrizaje en Azure:

  •	NMCD_Entidad: Un resumen ejecutivo (Scorecard) que muestra el total de registros de la entidad (ej. 200 en CRM Afiliados),         las     inconsistencias detectadas (34) y el porcentaje de madurez general (99.62%).
  
    En Azure: Estos KPIs se visualizarían en un tablero de Power BI conectado a las métricas generadas por Purview Data Quality     o por logs de Databricks DLT.
    
  •	NMCD_DetalleCampos: Desglose campo por campo, evaluando si aplican reglas de Unicidad (ej. Id_Afiliado), Completitud o           Validez, y mostrando el porcentaje de cumplimiento.
  
    En Azure: Al igual que el resumen, se alimenta de reportes automatizados en Databricks, guardando las estadísticas de         calidad en una tabla "Silver" u "Oro" del Data Lake para su monitoreo.
    
•	Afiliados: Tabla de simulación con datos ficticios que contiene las banderas evaluadoras (1=Si, 0=No) y demuestra cómo las         reglas de calidad iteran registro por registro para encontrar nulos o valores inválidos (ej. correos sin formato válido).

    En Azure: Esta sábana de datos simula la tabla real que residiría en formato Parquet o Delta dentro de Azure Data Lake         Storage Gen2.
