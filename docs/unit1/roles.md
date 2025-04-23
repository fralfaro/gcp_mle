
# Roles y Políticas 

<img src="../images/roles.png" alt="IAM Roles Banner" width="700">


## Introducción

En IAM, los **roles** definen el conjunto de acciones (permisos) que una identidad puede realizar sobre un recurso. Las **políticas** vinculan esos roles a identidades específicas dentro de un alcance determinado, como un proyecto o bucket.

Comprender esta relación es clave para implementar accesos seguros, eficientes y escalables.

---

## ¿Por qué es importante entender los roles?

Los roles son la forma en que GCP controla el acceso. Usar el rol adecuado:

- Minimiza riesgos de seguridad.
- Mejora la gobernanza.
- Permite delegar tareas sin exponer servicios críticos.

---

## Tipos de roles en IAM

Google Cloud define tres tipos de roles:

### 1. Roles Básicos

| Rol     | Permisos                                                                 |
|---------|--------------------------------------------------------------------------|
| Owner   | Control total sobre todos los recursos (incluye IAM).                   |
| Editor  | Modifica recursos pero no gestiona IAM.                                 |
| Viewer  | Solo lectura sobre recursos.                                            |

> ❗ No recomendados para producción, ya que son muy amplios.

---

### 2. Roles Predefinidos

- Diseñados para servicios específicos.
- Ejemplos:
  - `roles/storage.objectViewer`: lectura de objetos en Cloud Storage.
  - `roles/bigquery.dataEditor`: edición de tablas en BigQuery.
  - `roles/ml.developer`: acceso a entrenamiento y despliegue en Vertex AI.

> ✅ Recomendados para controlar el acceso de manera precisa y segura.

---

### 3. Roles Personalizados

- Creados por el usuario.
- Permiten definir exactamente qué permisos incluir.
- Útiles cuando ningún rol predefinido se ajusta a las necesidades.

```bash
gcloud iam roles create customViewer \
  --project=mi-proyecto-id \
  --title="Custom Viewer" \
  --permissions="storage.buckets.get,storage.objects.list" \
  --stage=GA
```

---

## Políticas IAM

Una política de IAM define **quién tiene qué rol sobre qué recurso**.

### Estructura simplificada:

```json
{
  "bindings": [
    {
      "role": "roles/storage.objectViewer",
      "members": [
        "user:usuario@ejemplo.com"
      ]
    }
  ]
}
```

- `bindings`: lista de asignaciones.
- `role`: el rol otorgado.
- `members`: las identidades que lo reciben.

---

## Ejemplo práctico: acceso granular a BigQuery

```bash
gcloud projects add-iam-policy-binding mi-proyecto-id \
  --member="serviceAccount:mi-pipeline@mi-proyecto.iam.gserviceaccount.com" \
  --role="roles/bigquery.dataViewer"
```

Este comando permite que una cuenta de servicio acceda a datos de BigQuery, sin permitirle editar o borrar.

---

## Buenas prácticas

- 🎯 Usa **roles predefinidos** siempre que sea posible.
- 🔐 Evita `Owner` y `Editor` en producción.
- 🧪 Usa cuentas de servicio separadas por flujo de trabajo.
- 🧩 Usa etiquetas o nombres descriptivos para los roles personalizados.
- 🧾 Documenta las políticas aplicadas por recurso y propósito.

---

## Diagrama de relación

```text
[Principal]
   |
   v
[Rol asignado] -----> [Permisos]
   |
   v
[Recurso (Proyecto, Bucket, Modelo...)]
```

---

## Referencias útiles

- [Tipos de roles en IAM](https://cloud.google.com/iam/docs/understanding-roles)
- [Crear roles personalizados](https://cloud.google.com/iam/docs/creating-custom-roles)
- [IAM Policy JSON](https://cloud.google.com/iam/docs/policies)

---

## Conclusión

Conocer los distintos tipos de roles te permitirá aplicar políticas de acceso más efectivas. Usar roles predefinidos y personalizar solo cuando sea necesario ayuda a mantener tu infraestructura segura y manejable.

En el siguiente capítulo aprenderás a trabajar con **cuentas de servicio**, esenciales para tareas automatizadas y pipelines de ML.
