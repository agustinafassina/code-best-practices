## Best practices in Terraform 🏗️
List of good practices in Terraform so I don't forget.

### Checklist 📋
#### 1. Naming conventions 🏷️
- Usar nombres descriptivos para recursos, modulos y workspaces.
- Incluir ambiente y proposito en nombres cuando el proveedor lo permita.
- Evitar nombres genericos (test, temp, resource1).
- Documentar el estandar de naming en el README del proyecto.

#### 2. Estructura y organizacion 📂
- Mantener una estructura logica: separar modules, environments, variables, y outputs.
- Usar un directorio raiz con subdirectorios claros (ejemplo: prod/, dev/, modules/).

#### 3. Modulos 🧩
- Crear modulos reutilizables y parametrizables para componentes comunes.
- Evitar duplicacion de codigo; usar modules/ para todo componente reutilizable.
- Documentar los modulos con README y ejemplos.

#### 4. Variables y tipos 📐
- Declarar variables con descripcion (description) y valores predeterminados si corresponde.
- No hardcodear valores sensibles; usar variables o archivos .tfvars.
- Usar variables para inputs y outputs para salidas claramente definidos.

#### 5. Estado remoto ☁️
- Configurar un backend remoto (S3 + DynamoDB, Azure Storage, etc.) para estado centralizado y compartido.
- Habilitar bloqueo de estado para evitar conflictos concurrentes.

#### 6. Versionamiento y proveedores 🔢
- Especificar versiones exactas de proveedores (required_providers) para garantizar reproducibilidad.
- Usar terraform version para controlar versiones de Terraform utilizadas.

#### 7. Inputs y outputs ↔️
- Solo exponer las salidas que sean necesarias.
- Documentar las variables y outputs en los archivos .tf.

#### 8. Plan y apply ✅
- Ejecutar terraform plan antes de apply para revisar cambios.
- Revisar cuidadosamente el plan, especialmente en ambientes productivos.
- Usar -auto-approve solo en automatizaciones controladas.
- Siempre revisar el fichero .tfplan generado por terraform plan antes de aplicar.

#### 9. Seguridad 🔒
- No guardar datos sensibles en codigo; usar variables de entorno o secret management.
- Evitar hardcodear secretos o credenciales en archivos .tf.
- Limitar permisos del usuario/rol que ejecuta Terraform.

#### 10. Variables sensibles 🔑
- Marcar variables sensibles con sensitive = true.
- No mostrarlas en output a menos que sea necesario, y aun asi con precaucion.

#### 11. Ambientes y workspaces 🌍
- Utilizar workspaces si gestionas entornos multiples con la misma infraestructura.
- Separar dev, staging y prod con carpetas o workspaces distintos.

#### 12. Formato y herramientas 🛠️
- Ejecutar terraform fmt -recursive para mantener un estilo coherente.
- Ejecutar terraform validate antes de aplicar cambios.

#### 13. Comentarios y documentacion 📝
- Mantener un README claro con instrucciones, variables y dependencias.
- Documentar modulos con ejemplos de uso y variables requeridas.

#### 14. Limpieza y eliminacion 🗑️
- Usar terraform destroy solo cuando sea necesario, en ambientes controlados.
- Validar los environments en los .tf antes de borrar.
