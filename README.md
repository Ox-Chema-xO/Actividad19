# Actividad19: Orquestador local de entornos de desarrollo simulados con Terraform

#### Integrantes:

- Chacón Roque, Leonardo Alexander 20221002K
- Cóndor Chávez Kevin 20172738B
- Delgado Velarde, Diego Manuel 20222676E

Demostraremos los conceptos y principios fundamentales de IaC utilizando Terraform para gestionar un entorno de desarrollo simulado completamente
local. Aprenderemos a definir, aprovisionar y modificar "infraestructura" (archivos, directorios, scripts de configuración)  de forma reproducible y automatizada.

#### **Prerrequisitos:**

 - Terraform instalado localmente.
 - Python 3 instalado localmente.
 - Conocimientos básicos de la línea de comandos (Bash).
 - Un editor de texto o IDE.


#### Estructura del proyecto (Archivos y directorios)

Puedes revisar las instrucciones adicionales y el código completo y sus modificaciones dependiendo de tu sistema operativo en [proyecto inicial de IaC](https://github.com/kapumota/DS/tree/main/2025-1/Proyecto_iac_local).

```
proyecto_iac_local/
├── main.tf                     # Configuración principal de Terraform
├── variables.tf                # Variables de entrada
├── outputs.tf                  # Salidas del proyecto
├── versions.tf                 # Versiones de Terraform y providers (local, random)
├── terraform.tfvars.example    # Ejemplo de archivo de variables
│
├── modules/
│   ├── application_service/    # Módulo para simular un "servicio"
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── templates/
│   │       └── config.json.tpl # Plantilla de configuración del servicio
│   │
│   └── environment_setup/      # Módulo para la configuración base del entorno
│       ├── main.tf
│       ├── variables.tf
│       └── scripts/
│           └── initial_setup.sh # Script de Bash para tareas iniciales
│
├── scripts/                    # Scripts globales
│   ├── python/
│   │   ├── generate_app_metadata.py # Genera metadatos complejos para apps
│   │   ├── validate_config.py       # Valida archivos de configuración generados
│   │   └── report_status.py         # Genera un reporte del "estado" del entorno
│   └── bash/
│       ├── start_simulated_service.sh # Simula el "arranque" de un servicio
│       └── check_simulated_health.sh  # Simula una "comprobación de salud"
│
└── generated_environment/      # Directorio creado por Terraform
    └── (aquí se crearán archivos y directorios)
```


### Actividad detallada por fases y conceptos

**Fase 0: Preparación e introducción**

1.  **¿Qué es infraestructura?**
      * Explica que, en este contexto local, la "infraestructura" serán directorios, archivos de configuración, scripts y la estructura lógica que los conecta.

         - Servidores/Instancias	(Directorios app1_v1.0.2/, app2_v0.5.0/):	Simulan servidores virtuales.
         - Recursos de Configuración	(Templates .tpl que crean archivos .json)	Son las onfiguraciones de aplicaciones.
         - Servicios de Sistema	(Scripts Bash como start_simulated_service.sh):	Inicio y gestión de servicios.
         - Monitoreo/Logs	(Archivos en logs/):	Es el sistema de observabilidad
         - Procesos Activos	(Archivos .pid):	Es el control de estado de servicios.
         - Metadatos de Infraestructura	(Scripts Python + JSON generado):	Es la información de despliegue.


      * Compara con infraestructura tradicional (servidores físicos, redes) y cloud (VMs, VPCs).

      | Concepto | Tradicional | Cloud | Nuestro Proyecto Local |
      |----------|-------------|-------|-------------------|
      | *Servidor* | HP ProLiant | EC2 instance | Directorio app1_v1.0.2/ |
      | *Red* | Switch físico | VPC/Subnet | Estructura de directorios |
      | *Configuración* | Registry/conf files | User Data | config.json.tpl |
      | *Monitoreo* | SCOM/Nagios | CloudWatch | Archivos de log |
      | *Orquestación* | Scripts manuales | Terraform/ARM | main.tf |
    
3.  **¿Qué es infraestructura como código (IaC)?**
      * **Configuración manual de infraestructura:**
          * Simula la creación manual de `generated_environment/app1/config.json` y `generated_environment/app1/run.sh`. Discute la propensión a errores, la falta de reproducibilidad y la dificultad para escalar.
      * **Infraestructura como código:**
          * Repasa Terraform como la herramienta que nos permitirá definir esta estructura en archivos de código (`.tf`).
          * Revisa un `main.tf` muy simple que solo cree un directorio. Presenta a tus compañeros un ejemplo.
      * **¿Qué NO es infraestructura como código?**
          * Escribe script que modifican infraestructura existente sin un estado deseado definido.
          * Documenta sobre cómo configurar manualmente (aunque es útil, no es IaC).
          * Modifica manualmente los recursos creados por Terraform después de `apply`.
