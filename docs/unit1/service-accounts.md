
# Cuentas de Servicio 

<img src="../images/account.png" alt="Service Accounts Banner" width="700">



## Introducción

Las **cuentas de servicio** son un tipo especial de identidad en Google Cloud usadas por aplicaciones, pipelines y servicios automatizados para interactuar con recursos sin intervención humana.

Son esenciales para proyectos de Machine Learning que requieren procesamiento autónomo, ejecución de pipelines, entrenamiento o despliegue de modelos.

---

## ¿Por qué usar cuentas de servicio?

Usar cuentas de servicio permite:

- 🔒 Asegurar que solo las aplicaciones autorizadas accedan a los recursos.
- 🔁 Automatizar tareas como cargas de datos o entrenamiento de modelos.
- 🧠 Ejecutar pipelines en Vertex AI o Workflows de forma segura.

---

## Características clave

- Cada cuenta tiene una dirección como:
  ```
  my-pipeline@mi-proyecto.iam.gserviceaccount.com
  ```

- Se pueden otorgar **roles específicos** a cada cuenta según su función.

- Se pueden utilizar desde:
  - Jobs de Dataflow o Dataproc
  - Vertex AI Pipelines
  - Contenedores en Cloud Run o GKE
  - Scripts con SDK de GCP (Python, Go, etc.)

---

## Crear una cuenta de servicio

```bash
gcloud iam service-accounts create mi-pipeline \
  --description="Cuenta para pipeline de entrenamiento" \
  --display-name="pipeline-ml"
```

---

## Asignar un rol

```bash
gcloud projects add-iam-policy-binding mi-proyecto-id \
  --member="serviceAccount:mi-pipeline@mi-proyecto.iam.gserviceaccount.com" \
  --role="roles/bigquery.dataViewer"
```

Esto otorga permiso de lectura sobre BigQuery para esa cuenta de servicio.

---

## Usar una cuenta de servicio desde Python

```python
from google.cloud import storage

# Autenticación explícita con una cuenta de servicio
client = storage.Client.from_service_account_json("mi-cuenta.json")
bucket = client.get_bucket("mi-bucket")
print(bucket.list_blobs())
```

> 💡 Asegúrate de que el archivo `.json` de credenciales **nunca se suba a GitHub**.

---

## Cuentas por entorno

| Entorno / Uso         | Cuenta sugerida                            | Rol típico                        |
|-----------------------|--------------------------------------------|-----------------------------------|
| Vertex AI Pipelines   | `vertex-pipeline@proyecto.iam.gserviceaccount.com` | `roles/aiplatform.user`           |
| Cloud Functions       | `cf-model-trigger@proyecto.iam.gserviceaccount.com` | `roles/storage.objectViewer`      |
| Dataflow              | `dataflow-runner@proyecto.iam.gserviceaccount.com` | `roles/dataflow.worker`           |
| GKE                   | `gke-ingestor@proyecto.iam.gserviceaccount.com` | `roles/container.admin`           |

---

## Buenas prácticas

- 🔐 Usa **una cuenta por proceso o servicio**.
- 🗂️ Organiza cuentas con nombres claros y propósito específico.
- 📄 Registra qué roles tiene cada cuenta y por qué.
- 🚫 Nunca uses cuentas de servicio con roles excesivos como `Owner`.

---

## Diagrama: uso típico

```text
Vertex AI Pipeline
      |
      v
Service Account (vertex-pipeline@...)
      |
      v
BigQuery  --- Cloud Storage --- Vertex Model Registry
```

---

## Referencias útiles

- [Documentación oficial de cuentas de servicio](https://cloud.google.com/iam/docs/service-accounts)
- [Autenticación con Python](https://cloud.google.com/docs/authentication/getting-started)
- [Recomendaciones de seguridad IAM](https://cloud.google.com/iam/docs/iam-best-practices)

---

## Conclusión

Las cuentas de servicio son la base de una arquitectura automatizada y segura en GCP. Usarlas correctamente te permite orquestar flujos de ML sin comprometer la seguridad ni depender de accesos manuales.

En el siguiente capítulo revisaremos **buenas prácticas para diseñar políticas de acceso efectivas y seguras**.
