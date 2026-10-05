# Ean-BiD Proyecto: Entorno Reproducible de Datos

Este repositorio contiene la infraestructura técnica y el análisis de datos para el proyecto de telemedición del acueducto. Está diseñado bajo principios estrictos de reproducibilidad, utilizando contenedores para aislar el entorno de ejecución y garantizando que cualquier miembro del equipo pueda ejecutar los análisis sin problemas de compatibilidad.

**Requisitos previos**
Antes de iniciar, asegúrate de tener instalados y configurados en tu equipo:
1. **Git:** Para clonar el repositorio y gestionar el control de versiones.
2. **Docker Desktop:** (o Docker Engine) para ejecutar nuestros contenedores aislados. Asegúrate de que la aplicación esté abierta y el motor en ejecución ("Engine running").

**Pasos para levantar el entorno**
1. Abre tu terminal y clona el repositorio del proyecto ejecutando: `git clone https://github.com/tu_usuario/Ean-BiD-Proyecto.git`
2. Ingresa a la carpeta raíz del proyecto: `cd Ean-BiD-Proyecto`
3. Crea tu archivo de variables locales. Copia el archivo de plantilla `.env.example`, renómbralo a `.env` e ingresa las credenciales de la base de datos (`DB_USER`, `DB_PASSWORD`, `DB_NAME`).
4. Descarga las imágenes y levanta la infraestructura ejecutando el comando: `docker compose up`
5. Espera unos segundos a que la terminal confirme que los servicios iniciaron.
6. Abre tu navegador web de preferencia y dirígete a `http://localhost:8888`.

**Verificación de configuración**
Deberías ingresar a la interfaz de Jupyter sin que te solicite ninguna contraseña. Para confirmar la red interna, abre el archivo `notebooks/00_verificacion.ipynb`, ejecuta sus celdas y verifica que imprima la versión de PostgreSQL instalada y el mensaje "Conexión exitosa".

**Soporte**
Si el navegador rechaza la conexión, revisa que Docker Desktop esté corriendo. Si el error persiste o tienes un conflicto de puertos, contacta al equipo de soporte de datos para recibir asistencia inmediata.

---

## Arquitectura y Fronteras del Contenedor

Para equilibrar la flexibilidad del desarrollo local con la inmutabilidad de los entornos de producción, el proyecto distribuye sus componentes de la siguiente manera:

| Elemento | ¿Dónde vive en la arquitectura? | Justificación |
| :--- | :--- | :--- |
| **Código de los cuadernos** | Montado como volumen (`./notebooks:/...`) | El código cambia constantemente durante el desarrollo. Al montarlo como volumen, los cambios que hacemos en el editor se reflejan instantáneamente en el contenedor sin reconstruir la imagen. |
| **Librerías de Python** | Dentro de la imagen (vía `requirements.txt`) | Las dependencias deben ser idénticas para cualquier persona. Al instalarlas en la imagen, garantizamos que el entorno de ejecución sea inmutable y predecible. |
| **Datos crudos** | Montados como volumen (`./data:/...`) | Los datos masivos no pertenecen al control de versiones de Git ni a la imagen base de Docker. Se montan desde el host para separar la capacidad de cómputo del almacenamiento físico. |
| **Credenciales** | Inyectadas vía variables (`.env`) | Por seguridad cibernética, las contraseñas nunca deben quedar quemadas en el código (`docker-compose.yml`). Se inyectan localmente para prevenir vulnerabilidades en el repositorio. |

---

## Estructura del Repositorio

El proyecto sigue una convención estricta para garantizar la separación entre datos, código fuente y resultados autogenerados:

```text
Ean-BiD-Proyecto/
├── data/
│   ├── raw/               # Datos originales inmutables (ignorados por Git)
│   └── synthetic/         # Datos generados por código (ignorados por Git)
├── docs/                  # Documentación técnica, memorias y análisis (.md)
├── notebooks/             # Cuadernos de Jupyter de experimentación (.ipynb)
├── resultados/            # Archivos exportados o preprocesados (ignorados por Git)
├── src/                   # Scripts modulares de Python (.py)
├── .env.example           # Plantilla de variables de entorno
├── .gitignore             # Reglas de exclusión para proteger el repositorio
├── docker-compose.yml     # Orquestador de la infraestructura (Jupyter + DB)
├── README.md              # Documentación principal del proyecto
└── requirements.txt       # Anclaje exacto de dependencias de Python
