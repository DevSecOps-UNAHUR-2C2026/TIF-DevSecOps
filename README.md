# Trabajo Práctico 18: Centralización de Vulnerabilidades con OWASP DefectDojo

## 🎯 Objetivo
Implementar y configurar **OWASP DefectDojo** como plataforma centralizada para la orquestación, seguimiento y gestión del ciclo de vida de vulnerabilidades dentro del pipeline de DevSecOps, automatizando la ingesta de reportes generados en las etapas de CI/CD.

---

## 🛠️ Herramientas Integradas
El pipeline normaliza y exporta los hallazgos en formatos compatibles (`JSON`/`XML`) para ser subidos mediante la API v2 de DefectDojo:

- **Threat Modeling:** Threagile
- **Secret Scanning:** Gitleaks (`gitleaks.toml`)
- **SAST:** Semgrep
- **SCA / Container Security:** Trivy (`.trivyignore`)
- **DAST:** OWASP ZAP

---

## 🚀 Pipeline y Automatización (`.github/workflows/defectdojo-ci.yml`)
El flujo de trabajo automatizado realiza las siguientes acciones:
1. **Ejecución de escaneos:** Corre cada herramienta de seguridad sobre el código base, dependencias y contenedor.
2. **Generación de artefactos:** Exporta los reportes de vulnerabilidades en sus formatos estándar.
3. **Ingesta en DefectDojo:** Mediante llamadas a la API v2 de DefectDojo (`/api/v2/import-scan/` o `/reimport-scan/`), se asocian los hallazgos al producto y engagement correspondiente utilizando el API Token configurado en los GitHub Secrets.

---

## ⚙️ Variables y Secretos Requeridos
Para la ejecución del pipeline se configuran en el repositorio:
- `DEFECTDOJO_URL`: URL base de la instancia de DefectDojo.
- `DEFECTDOJO_API_KEY`: Token de autenticación para la API v2.
- `DEFECTDOJO_ENGAGEMENT_ID`: ID del engagement activo donde se consolidan los escaneos.

---

## 📋 Estructura del Rep
