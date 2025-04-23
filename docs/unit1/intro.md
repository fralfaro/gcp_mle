
# Introducción a IAM 

<img src="../images/intro.png" alt="Banner IAM" width="700">


## Introducción

El sistema **Identity and Access Management (IAM)** de Google Cloud permite definir **quién puede hacer qué sobre qué recurso**. En proyectos de Machine Learning, esto es esencial para proteger datos, controlar accesos a modelos y automatizar procesos de manera segura.

En este capítulo, aprenderás los componentes principales de IAM, cómo aplicarlos y por qué es fundamental entender esta arquitectura para cualquier proyecto en la nube.

---

## ¿Por qué es importante entender IAM?

Comprender cómo funciona IAM te permitirá:

- Prevenir accesos no autorizados a tus datos o modelos.
- Automatizar flujos de trabajo de manera segura con cuentas de servicio.
- Implementar el principio de menor privilegio y reducir riesgos.
- Diagnosticar errores de permisos y optimizar configuraciones de seguridad.

---

## Componentes principales de IAM

Google Cloud aplica IAM en una jerarquía: organización → carpeta → proyecto → recurso.

A continuación, los elementos clave del sistema:

### 1. Principals (Identidades)

- Representan a quien accede a los recursos.
- Pueden ser: usuarios (`user:`), grupos (`group:`), cuentas de servicio (`serviceAccount:`) o dominios.

### 2. Roles

- Definen un conjunto de permisos.
- Tipos:
  - **Básicos**: Owner, Editor, Viewer.
  - **Predefinidos**: roles específicos por servicio (ej. `roles/storage.objectViewer`).
  - **Personalizados**: creados por el usuario.

### 3. Permisos

- Son acciones sobre recursos, como:
  - `storage.buckets.get`
  - `bigquery.datasets.create`

### 4. Políticas

- Asocian uno o más roles a uno o más principals.
- Se aplican a un recurso específico (proyecto, bucket, dataset, etc.).

---

## Diagrama lógico de IAM en GCP

```text
Organización
 └── Carpeta (opcional)
      └── Proyecto
           └── Recurso (bucket, modelo, tabla...)
```

Las políticas se heredan hacia abajo. Si das un rol a nivel de proyecto, todos los recursos dentro de ese proyecto lo heredarán.

---

## Ejemplo práctico

Otorgar permisos de lectura a un bucket de Storage para un usuario:

```bash
gcloud projects add-iam-policy-binding mi-proyecto-id \
  --member="user:persona@ejemplo.com" \
  --role="roles/storage.objectViewer"
```

Este comando:
- Añade una política de IAM al proyecto.
- Aplica el rol de visualizador de objetos de Storage.
- Asocia ese rol al usuario especificado.

---

## IAM en proyectos de ML

IAM se usa en cada etapa del ciclo de vida de Machine Learning:

- 🔒 **Acceso a datasets** en Storage o BigQuery.
- 🧠 **Entrenamiento de modelos** con Vertex AI (y cuentas de servicio dedicadas).
- 🚀 **Despliegue de modelos** y control de endpoints.
- 🤖 **Automatización** con Workflows, Cloud Functions, y pipelines de Vertex AI.

---

## Recomendaciones clave

- ✅ Aplica el **principio de menor privilegio**.
- 🔐 Usa **cuentas de servicio separadas** para pipelines y tareas automáticas.
- 🧩 Prefiere roles predefinidos antes de crear personalizados.
- 📊 Supervisa accesos con Cloud Audit Logs.

---

## Referencias útiles

- [Documentación oficial de IAM](https://cloud.google.com/iam/docs)
- [Comando `gcloud` para IAM](https://cloud.google.com/sdk/gcloud/reference/projects/add-iam-policy-binding)
- [IAM para Vertex AI](https://cloud.google.com/vertex-ai/docs/general/access-control)

---

## Conclusión

IAM es el primer paso para construir soluciones seguras y escalables en GCP. Con una buena gestión de accesos, no solo proteges tus datos y modelos, sino que también simplificas la automatización y el trabajo colaborativo.

En el siguiente capítulo exploraremos cómo funcionan los **roles y políticas** para asignar permisos de forma granular.
