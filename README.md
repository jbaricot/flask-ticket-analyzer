# 📊 Analizador y Clasificador de Tickets IT con Flask y Ollama ✨

Este proyecto proporciona una interfaz web simple pero potente para analizar y clasificar tickets de soporte técnico exportados desde un sistema de ticketing (en formato `.xlsx`). Utiliza modelos de lenguaje grandes (LLMs) ejecutados localmente a través de **Ollama** para garantizar la privacidad de los datos, generar resúmenes concisos y asignar categorías a los tickets, con la opción de añadir contexto personalizado para mejorar la precisión.

**¿El problema?** Analizar cientos de tickets manualmente es tedioso y consume mucho tiempo.
**¿La solución?** ¡Aprovechar la IA local para automatizar el resumen y la clasificación inicial!

**(¡Añade una captura de pantalla o GIF animado aquí!)**
[![Screenshot-App](URL_A_TU_IMAGEN_O_GIF.png)](URL_A_TU_IMAGEN_O_GIF.png)
*(Reemplaza `URL_A_TU_IMAGEN_O_GIF.png` con un enlace a una imagen/gif que muestre la interfaz)*

---

## 🚀 Características Principales

*   **Interfaz Web Simple:** Sube tus archivos Excel directamente desde el navegador gracias a Flask.
*   **Procesamiento Local con Ollama:** Utiliza modelos LLM (como Gemma, Llama, Mistral) que se ejecutan en tu propia máquina. ¡Tus datos nunca salen de tu red!
*   **Resumen Automático:** Genera resúmenes concisos de cada ticket, enfocándose en los aspectos técnicos clave.
*   **Clasificación Inteligente:** Asigna automáticamente una categoría principal a cada ticket (ej. "Problema Hardware", "Gestión de Cuentas", "Solicitud Software").
*   **Contexto Personalizado:** ¡Mejora drásticamente la clasificación! Añade información específica de tu entorno (nombres de proyectos, software interno, departamentos) para guiar al modelo.
*   **Procesamiento en Segundo Plano:** Analiza archivos grandes sin bloquear la interfaz web, usando hilos paralelos (`concurrent.futures`) para acelerar el proceso.
*   **Seguimiento del Progreso:** Observa el estado y el progreso del análisis en tiempo real.
*   **Descarga de Resultados:** Obtén un nuevo archivo Excel con las columnas originales más las columnas de "Resumen" y "Clasificación" añadidas.

---

## 🛠️ Tecnologías Utilizadas

*   **Backend:** Python 3
*   **Framework Web:** Flask
*   **Procesamiento de Datos:** Pandas
*   **LLM Local:** Ollama ([https://ollama.com/](https://ollama.com/))
    *   **Modelo Predeterminado:** Gemma 3 (configurable en `processing_logic.py`)
*   **Procesamiento Paralelo:** `concurrent.futures.ThreadPoolExecutor`
*   **Frontend:** HTML, CSS Básico, JavaScript (para polling de estado)
*   **Parsing HTML (Limpieza):** BeautifulSoup4

---

## ⚙️ Instalación y Configuración Local

Sigue estos pasos para ejecutar la aplicación en tu máquina:

1.  **Prerrequisitos:**
    *   Python 3.7+
    *   Git
    *   **Ollama Instalado y Corriendo:** Sigue las instrucciones en [ollama.com](https://ollama.com/).
    *   **Modelo LLM Descargado:** Asegúrate de tener el modelo que quieres usar (por defecto `gemma3`). Ejecuta en tu terminal:
        ```bash
        ollama pull gemma3
        ```

2.  **Clonar el Repositorio:**
    ```bash
    git clone https://github.com/jbaricot/flask-ticket-analyzer.git
    cd flask-ticket-analyzer
    ```

3.  **(Recomendado) Crear un Entorno Virtual:**
    ```bash
    python -m venv venv
    # Linux/macOS:
    source venv/bin/activate
    # Windows (cmd):
    # venv\Scripts\activate.bat
    # Windows (PowerShell):
    # venv\Scripts\Activate.ps1
    ```

4.  **Instalar Dependencias:**
    ```bash
    pip install -r requirements.txt
    ```

5.  **Crear Carpetas Necesarias:** Asegúrate de que existan las carpetas `uploads` y `processed` en la raíz del proyecto:
    ```bash
    mkdir uploads processed
    ```

6.  **Iniciar el Servidor Ollama (en una terminal separada):**
    ```bash
    ollama serve
    ```
    *Deja esta terminal corriendo.*

7.  **Ejecutar la Aplicación Flask (en la terminal del proyecto):**
    ```bash
    python app.py
    ```
    *Verás mensajes indicando que el servidor está corriendo en `http://0.0.0.0:5000`.*

8.  **Acceder:** Abre tu navegador y ve a `http://127.0.0.1:5000`.

---

## 📖 Uso

1.  Abre la aplicación en tu navegador (`http://127.0.0.1:5000`).
2.  **(Opcional pero Recomendado):** En el campo "Contexto Adicional", escribe información relevante que ayude al modelo a clasificar mejor tus tickets (ej. "Proyectos: Zeus, Atenea. Software: SISCONT, NOMINAWEB. Departamentos: Contabilidad, RRHH. Problemas comunes: VPN SAP").
3.  Haz clic en "Examinar" y selecciona tu archivo Excel (`.xlsx`) que contenga los tickets. Asegúrate de que tenga las columnas requeridas (ver `REQUIRED_COLUMNS` en `processing_logic.py`).
4.  Haz clic en "Procesar Archivo".
5.  Serás redirigido a una página que muestra el progreso. El procesamiento puede tardar dependiendo del número de tickets y la potencia de tu hardware.
6.  Una vez completado, aparecerá un enlace para "Descargar Archivo Procesado". Haz clic para obtener el Excel con las nuevas columnas de resumen y clasificación.

---

## 🔧 Configuración

*   **Modelo LLM:** Puedes cambiar el modelo base modificando la constante `LLM_MODEL_GEMMA` en el archivo `processing_logic.py`. Asegúrate de que el modelo elegido esté descargado en Ollama (`ollama pull nombre_modelo`). Podrías necesitar ajustar los *prompts* para otros modelos.
*   **Número de Hilos Paralelos:** Ajusta `MAX_WORKERS` en `processing_logic.py` para controlar cuántos tickets se procesan simultáneamente. Un número mayor puede acelerar el proceso si tu CPU/GPU y Ollama pueden manejarlo, pero también consume más recursos.
*   **Host/Puerto Flask:** Modifica la línea `app.run(...)` al final de `app.py` si necesitas cambiar el puerto o la interfaz donde escucha el servidor web.

---

## 🧠 Notas sobre Ollama y LLMs

*   **Ollama debe estar corriendo (`ollama serve`)** para que la aplicación funcione.
*   El rendimiento (velocidad y calidad de resumen/clasificación) dependerá fuertemente del modelo LLM elegido y del hardware de tu máquina (especialmente la CPU o GPU si Ollama la utiliza).
*   La calidad de la clasificación mejora significativamente al proporcionar un **buen contexto** relevante para tus tickets.

---

## 🌱 Futuras Mejoras (Roadmap)

*   [ ] Implementar una cola de tareas más robusta (Celery/RQ) en lugar de `threading` para manejar mejor reinicios y tareas largas.
*   [ ] Mejorar la gestión del estado de las tareas (ej. usando Redis o una base de datos simple).
*   [ ] Añadir un botón funcional para cancelar procesos en curso desde la interfaz.
*   [ ] Permitir seleccionar el modelo LLM directamente desde la interfaz web.
*   [ ] Implementar un sistema básico de limpieza automática de archivos antiguos en `uploads/` y `processed/`.
*   [ ] Mejorar la interfaz de usuario (quizás con un framework como Bootstrap o Tailwind).
*   [ ] Añadir opción para visualizar algunas estadísticas básicas o gráficos directamente en la web.
*   [ ] Considerar opciones de *fine-tuning* de modelos más pequeños para tareas específicas si la precisión/velocidad se vuelve crítica.

---

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Si tienes ideas, encuentras errores o quieres añadir mejoras:

1.  Abre un "Issue" para discutir el cambio.
2.  Haz un "Fork" del repositorio.
3.  Crea una nueva rama para tu característica (`git checkout -b feature/nueva-caracteristica`).
4.  Haz tus cambios y haz commit (`git commit -m 'Añade nueva característica X'`).
5.  Haz push a tu rama (`git push origin feature/nueva-caracteristica`).
6.  Abre un "Pull Request".

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Consulta el archivo `LICENSE` para más detalles (¡Asegúrate de añadir un archivo LICENSE.md con el texto de la licencia MIT!).

---

¡Gracias por usar o contribuir a este proyecto! 🎉
