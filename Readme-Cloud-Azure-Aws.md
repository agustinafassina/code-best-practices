## Best practices in Azure and AWS ☁️
List of good practices in Azure and AWS so I don't forget.

### Checklist 📋
#### 1. Naming y convenciones 🏷️
- Usar un patron consistente: `{org}-{env}-{region}-{service}-{resource}` (ej: acme-prod-eus2-app-api).
- Evitar nombres genericos (test, temp, resource1); incluir proposito y ambiente.
- Documentar el estandar de naming en el README del proyecto o en un ADR.
- Respetar limites de longitud y caracteres de cada servicio (Azure y AWS varian).

#### 2. Tags y metadata 🔖
- Aplicar tags obligatorios en todos los recursos: Environment, Owner, CostCenter, Project, ManagedBy.
- Usar Azure Policy o AWS Tag Policies para exigir tags en creacion.
- Tags para facturacion y reportes de costos; revisar recursos sin tag periodicamente.
- En Azure: Resource Groups como unidad logica; en AWS: agrupar por cuenta/OU o tags.

#### 3. Ambientes y separacion 🌍
- Separar dev, staging y prod (cuentas/subscriptions distintas cuando sea posible).
- No usar credenciales de prod en local ni en pipelines de dev.
- Limitar acceso cross-environment; evitar peering o trust innecesarios entre ambientes.
- Infra como codigo (Terraform) con workspaces o carpetas por ambiente.

#### 4. IAM y control de acceso 🔐
- Principio de menor privilegio: roles con permisos minimos necesarios.
- **Azure**: preferir Managed Identities sobre service principals con secretos; usar RBAC en scope de recurso o RG.
- **AWS**: preferir IAM roles sobre access keys; usar roles para EC2, Lambda, CI/CD.
- Evitar usuarios root / Owner en operaciones diarias; MFA obligatorio en cuentas privilegiadas.
- Rotar credenciales y keys; auditar permisos con Azure AD / IAM Access Analyzer periodicamente.
- Usar grupos (Azure AD groups / IAM groups) en lugar de asignar permisos usuario por usuario.

#### 5. Red y seguridad perimetral 🌐
- Segmentar con VNet (Azure) o VPC (AWS): subnets publicas vs privadas.
- Colocar workloads en subnets privadas; exponer solo load balancers / API Gateway / App Gateway.
- Security Groups (AWS) y NSG (Azure): deny by default, abrir solo puertos necesarios.
- Usar Private Endpoints / PrivateLink para PaaS (Storage, Key Vault, RDS, etc.) sin salir a internet publica.
- WAF delante de APIs y sitios publicos (Azure Front Door / Application Gateway, AWS WAF + ALB/CloudFront).

#### 6. Secretos y claves 🔑
- **Azure**: Key Vault para secretos, certificados y keys; referenciar desde App Service / Functions / AKS.
- **AWS**: Secrets Manager o Parameter Store (SecureString); integrar con Lambda, ECS, etc.
- Nunca commitear connection strings, API keys ni .env con secretos reales.
- Habilitar soft-delete y purge protection en Key Vault; rotation automatica donde el servicio lo permita.
- Cifrar datos en reposo (SSE, Azure Storage encryption, RDS encryption).

#### 7. Compute y contenedores ⚙️
- **Azure**: App Service / Container Apps / AKS segun complejidad; evitar VMs si no hace falta.
- **AWS**: Lambda para eventos; ECS/Fargate o EKS para contenedores; EC2 solo cuando sea necesario.
- Imagenes minimas y actualizadas; escanear vulnerabilidades en registry (ACR, ECR).
- Health checks y auto-scaling configurados; limits de CPU/memoria definidos.

#### 8. Storage y bases de datos 💾
- Bloquear acceso publico por defecto (Azure Storage public access disabled, S3 Block Public Access).
- Versionado y lifecycle policies para backups y retencion de costos.
- Backups automaticos en SQL (Azure SQL, RDS); probar restores periodicamente.
- Usar connection strings desde Key Vault / Secrets Manager, no en codigo.

#### 9. Monitoreo, logging y alertas 📊
- **Azure**: Log Analytics, Application Insights, Azure Monitor alerts.
- **AWS**: CloudWatch Logs, metrics, alarms; X-Ray o equivalente para tracing.
- Centralizar logs; retencion alineada a compliance.
- Alertas en errores 5xx, latencia, disco, costos anomalos y fallos de backup.
- Dashboards por ambiente y servicio critico.

#### 10. Costos y optimizacion 💰
- Budgets y alertas de gasto (Azure Cost Management, AWS Budgets).
- Apagar o escalar a cero recursos de dev fuera de horario laboral cuando aplique.
- Revisar recursos huerfanos (discos, IPs, snapshots) mensualmente.
- Reserved Instances / Savings Plans / Azure Reservations para cargas estables en prod.
- Rightsizing: no sobre-dimensionar desde el dia uno.

#### 11. Disaster recovery y backups 🔄
- Definir RPO/RTO por servicio critico.
- Geo-redundancy donde el negocio lo requiera (GRS, cross-region replication).
- Probar restore de backups al menos una vez al trimestre.
- Documentar runbooks de failover y contactos de escalacion.

#### 12. Governance y compliance 📜
- **Azure**: Management Groups, Policy, Blueprints / Landing Zones.
- **AWS**: Organizations, SCPs, Control Tower o landing zone equivalente.
- Habilitar CloudTrail (AWS) y Activity Log / Diagnostic Settings (Azure) para auditoria.
- Revisar Security Center (Azure) / Security Hub (AWS) y corregir findings prioritarios.

#### 13. CI/CD e infraestructura 🚀
- Pipelines con identidad federada (OIDC) en lugar de keys largas en GitHub Actions / Azure DevOps.
- Plan/apply de Terraform con aprobacion manual en prod.
- State remoto con locking (S3+DynamoDB, Azure Storage + blob lease).
- No aplicar cambios manuales en prod sin reflejarlos en IaC despues.
