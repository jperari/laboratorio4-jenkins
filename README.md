# Capturas de pantalla

## Configuración inicial de Jenkins

- Jenkins arrancado usando docker compose up
  
    ![alt text](capturas/1_Jenkins_arrancado.png)

- Plugin HTML Publisher instalado (se usa en la cobertura de código)

    ![Plugin HTML Publisher](capturas/2_Jenkins_HTML_Publisher_instalado.png)

- Repo en GitHub
  
  - Sube el código del proyecto backend
  
    ![Subida código backend](capturas/3_GitHub_con_codigo_backend.png)
  
  - Sube el código del Jenkinsfile pipeline con pasos obligatorios
  
    ![Jenkinsfile pasos obligatorios](capturas/4_GitHub_añade_Jenkinsfile_pasos_obligatorios.png)
  
  - Token en Github
   
    ![Token GitHub](capturas/5_GitHub_añade_token.png)

- Jenkins
  
  - Creación del proyecto
    
    ![Creación del proyecto](capturas/6_Jenkins_añade_proyecto.png)
    
  - Configuración del proyecto
    
    ![Configuración del proyecto](capturas/7_Jenkins_configura_proyecto.png)
    
  - Escaneo inicial
  
    ![Escaneo inicial](capturas/8_Jenkins_escaneo_inicial_proyecto.png)
    
  - Ejecución del pipeline con pasos obligatorios
    
    ![Ejecución del pipeline con pasos obligatorios](capturas/9_Jenkins_ejecución_pipeline_pasos_obligatorios.png)
    
  - Ejecución del pipeline con pasos obligatorios (stages)
  
    ![Ejecución del pipeline con pasos obligatorios stages](capturas/10_Jenkins_ejecución_pipeline_pasos_obligatorios_stages.png)
    
  - Ejecución del pipeline con pasos obligatorios (stages con detalle)
    
    ![Ejecución del pipeline con pasos obligatorios (stages con detalle)](capturas/11_Jenkins_ejecución_pipeline_pasos_obligatorios_stages_detalle_tests.png)
    
  - Aviso de code quality con error pero sin mensaje "inestable"

    ![Aviso de code quality con error pero sin mensaje "inestable"](capturas/12_Jenkins_aviso_code_quality.png)
    
  - Ejecución de tareas opcionales salvo la de Docker
  
    ![Ejecución de tareas opcionales salvo la de Docker](capturas/13_Jenkins_ejecución_con_tareas_opcionales_salvo_docker.png)
    
  - Informe de cobertura de código

    ![Informe de cobertura de código](capturas/14_Jenkins_coverture_report.png)
    
  - Jenkins en estado inestable
  
    No salía por defecto, he tenido que añadir esto: `-- --max-warnings=0` en el `sh` del paso `Code quality`

    ![Jenkins en estado inestable](capturas/15_Jenkins_estado_inestable.png)
    ![Jenkins en estado inestable - detalle](capturas/16_Jenkins_estado_inestable_detalle.png)

NOTA: Falta la actividad opcional de Docker, si saco un rato la miraré aparte.

