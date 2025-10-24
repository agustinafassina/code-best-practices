## Best practices in terraform
List of good practices in terraform so I don't forget.

### Checklist
#### 1. Organización de archivos y directorios
- Mantener una estructura lógica: separar modules, environments, variables, y outputs.
- Usar un directorio raíz con subdirectorios claros (ejemplo: prod/, dev/, modules/).

#### 2. Uso de módulos
- Crear módulos reutilizables y parametrizables para componentes comunes.
- Evitar duplicación de código, usar modules/ para todo componente reutilizable.
- Documenta los módulos con README y ejemplos.

#### 3. Variables
- Declarar variables con descripción (description) y valores predeterminados si corresponde.
- No hardcodear valores sensibles; usar variables o archivos .tfvars.
- Usar variable para inputs y output para salidas claramente definidos.

#### 4. Estado remoto
- Configurar un backend remoto (como S3 + DynamoDB, Azure Storage, etc.) para mantener el estado centralizado y compartido.
- Habilitar bloqueo de estado para evitar conflictos concurrentes.

#### 5. Versionamiento y proveedores
- Especificar versiones exactas de proveedores (required_providers) para garantizar reproducibilidad.
- Usar terraform version para controlar versiones de Terraform utilizadas.

#### 6. Inputs y outputs
- Solo exponer las salidas que sean necesarias.
- Documentar las variables y outputs en los archivos .tf.

#### 7. Uso correcto de terraform plan y terraform apply
- Ejecutar terraform plan antes de apply para revisar cambios.
- Revisar cuidadosamente el plan, especialmente en ambientes productivos.
- Usar -auto-approve solo en automatizaciones controladas.

#### 8. Seguridad
- No guardar datos sensibles en código, usar variables de entorno o servicios de secret management.
- Evitar hardcodear secretos o credenciales en archivos .tf.
- Limitar permisos del usuario/rol que ejecuta Terraform.

#### 9. Variables sensibles
- Markear variables sensibles con sensitive = true.
- No mostrarlas en output a menos que sea necesario, y aún así con precaución.

#### 10. Uso de terraform fmt y terraform validate
- Ejecutar terraform fmt -recursive para mantener un estilo coherente.
- Ejecutar terraform validate antes de aplicar cambios.

#### 11. Control de workspace
- Utilizar workspaces si gestionas entornos múltiples con la misma infraestructura.

#### 12. Planificación y revisión
- Siempre revisar el fichero .tfplan generado por terraform plan antes de aplicar.

#### 13. Limpieza y eliminación
- Usar terraform destroy solo cuando sea necesario, en ambientes controlados.
- Validar los environments en los tf antes de borrar

#### 15. Documentación
- Mantén un README claro con instrucciones, variables y dependencias.
