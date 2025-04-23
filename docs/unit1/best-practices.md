
#  Buenas Prácticas 

<img src="../images/best.webp" alt="Service Accounts Banner" width="400">


## Introducción

Aplicar buenas prácticas en IAM es fundamental para construir soluciones **seguras, mantenibles y auditables** en Google Cloud, especialmente cuando se trabaja con datos sensibles o se automatizan flujos de Machine Learning.

En este capítulo aprenderás principios y recomendaciones clave para mejorar la seguridad y la gobernanza de accesos.

---

## ¿Por qué son importantes las buenas prácticas?

IAM mal configurado puede abrir puertas a:

- 🛑 Pérdida de datos por accesos indebidos.
- 🚨 Vulnerabilidades en pipelines automatizados.
- 🤯 Dificultad para auditar quién hizo qué y cuándo.
- 🧩 Problemas de escalabilidad y colaboración en equipos grandes.

Aplicar buenas prácticas ayuda a prevenir estos riesgos desde el inicio del proyecto.

---

## Principales recomendaciones

### 🔒 1. Aplica el principio de menor privilegio

- Otorga **solo los permisos necesarios** para realizar una tarea.
- Revisa y elimina roles innecesarios o excesivos.
- Evita el uso de `roles/owner`, `roles/editor` o `roles/viewer` en producción.

---

### 👥 2. Usa cuentas de servicio por flujo de trabajo

- Crea una cuenta por pipeline, función o proceso automatizado.
- Asigna a cada cuenta **solo los roles que necesita**.
- Evita reutilizar cuentas en múltiples contextos.

---

### 🧱 3. Prefiere roles predefinidos

- Usa roles como `roles/aiplatform.user`, `roles/storage.objectViewer`, etc.
- Solo crea roles personalizados si los predefinidos no satisfacen tu caso.
- Documenta cualquier rol personalizado que utilices.

---

### 📦 4. Administra el acceso por proyecto

- Organiza tus proyectos por entorno (ej. `ml-dev`, `ml-prod`).
- Otorga roles a nivel de proyecto, no en recursos individuales si no es necesario.
- Usa etiquetas y carpetas para separar ambientes o líneas de negocio.

---

### 🧾 5. Revisa políticas periódicamente

- Usa `gcloud projects get-iam-policy` para revisar accesos.
- Implementa auditorías trimestrales o automáticas.
- Usa herramientas como **Policy Analyzer** o **IAM Recommender**.

```bash
gcloud projects get-iam-policy mi-proyecto-id
```

---

### 🔁 6. Evita la dependencia de credenciales locales

- Usa autenticación automática con cuentas de servicio en GKE, Cloud Run o Vertex.
- Si necesitas credenciales `.json`, **usa secretos** y no los incluyas en el repositorio.

---

## Herramientas útiles

| Herramienta             | Uso principal                                |
|-------------------------|----------------------------------------------|
| IAM Policy Analyzer     | Detecta roles innecesarios o riesgosos       |
| IAM Recommender         | Sugerencias automáticas de permisos mínimos  |
| Audit Logs              | Revisión de acciones históricas               |
| Cloud Asset Inventory   | Inventario completo de roles y recursos       |

---

## Checklist de implementación segura

- [ ] Cada cuenta de servicio tiene un propósito claro y documentado.
- [ ] No se usan roles amplios como `Editor` u `Owner` en producción.
- [ ] Todas las políticas están revisadas al menos una vez al trimestre.
- [ ] El acceso a recursos de ML (datasets, modelos) está auditado.
- [ ] Las claves de cuentas de servicio están protegidas y rotadas periódicamente.


---

## Recursos adicionales

- [IAM Recommender](https://cloud.google.com/iam/docs/recommender-overview)
- [IAM Policy Troubleshooter](https://cloud.google.com/iam/docs/troubleshooting-access)
- [Cloud Audit Logs](https://cloud.google.com/logging/docs/audit)

---

## Conclusión

Una buena configuración de IAM no solo mejora la seguridad, sino que también facilita la automatización, la colaboración y el monitoreo en tus flujos de Machine Learning.

En la próxima unidad aprenderás cómo almacenar, consultar y preparar datos usando **Cloud Storage y BigQuery**.
