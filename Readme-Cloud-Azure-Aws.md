## Best practices in Azure and AWS ☁️
List of good practices in Azure and AWS so I don't forget.

### Checklist 📋
#### 1. Naming conventions 🏷️
- Usar un patron consistente: `{org}-{env}-{region}-{service}-{resource}` (ej: acme-prod-eus2-app-api).
- Evitar nombres genericos (test, temp, resource1); incluir proposito y ambiente.
- Documentar el estandar de naming en el README del proyecto o en un ADR.
- Respetar limites de longitud y caracteres de cada servicio (Azure y AWS varian).

#### 2. Tags y metadata 📐
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
- **Azure**: Conditional Access para politicas de acceso (ubicacion, dispositivo, riesgo).
- **AWS**: usar SCPs en Organizations para denegar acciones peligrosas a nivel cuenta.
- Cuenta break-glass documentada para emergencias; auditar su uso.
- Access keys solo si no hay alternativa; rotacion automatica y maximo 90 dias.

#### 5. Red y seguridad perimetral 🌐
- Segmentar con VNet (Azure) o VPC (AWS): subnets publicas vs privadas.
- Colocar workloads en subnets privadas; exponer solo load balancers / API Gateway / App Gateway.
- Security Groups (AWS) y NSG (Azure): deny by default, abrir solo puertos necesarios.
- Usar Private Endpoints / PrivateLink para PaaS (Storage, Key Vault, RDS, etc.) sin salir a internet publica.
- WAF delante de APIs y sitios publicos (Azure Front Door / Application Gateway, AWS WAF + ALB/CloudFront).
- Salida a internet via NAT Gateway (AWS) o NAT/Firewall (Azure); evitar IPs publicas en workloads internos.
- Habilitar proteccion DDoS donde aplique (Azure DDoS Protection, AWS Shield).
- Documentar flujos de red (diagrama): que habla con que y por que puerto.

#### 6. Secretos y claves 🔑
- **Azure**: Key Vault para secretos, certificados y keys; referenciar desde App Service / Functions / AKS.
- **AWS**: Secrets Manager o Parameter Store (SecureString); integrar con Lambda, ECS, etc.
- Nunca commitear connection strings, API keys ni .env con secretos reales.
- Habilitar soft-delete y purge protection en Key Vault; rotation automatica donde el servicio lo permita.
- Cifrar datos en reposo (SSE, Azure Storage encryption, RDS encryption).
- Evaluar CMK (customer-managed keys) cuando compliance o control de rotacion lo requieran.
- Separar Key Vault / KMS por ambiente (dev vs prod).

#### 7. Compute y contenedores ⚙️
- **Azure**: App Service / Container Apps / AKS segun complejidad; evitar VMs si no hace falta.
- **AWS**: Lambda para eventos; ECS/Fargate o EKS para contenedores; EC2 solo cuando sea necesario.
- Imagenes minimas y actualizadas; escanear vulnerabilidades en registry (ACR, ECR).
- Health checks y auto-scaling configurados; limits de CPU/memoria definidos.
- Distribuir cargas en multiples Availability Zones (multi-AZ) para alta disponibilidad.
- **AWS**: habilitar termination protection en instancias criticas de prod.
- **Azure**: usar Availability Zones en recursos soportados (SQL, VMs, AKS).
- Serverless: definir timeout, concurrencia y reserved concurrency en Lambda; throttling en API Gateway.

#### 8. Storage y bases de datos 💾
- Bloquear acceso publico por defecto (Azure Storage public access disabled, S3 Block Public Access).
- Habilitar Block Public Access a nivel de cuenta AWS; deshabilitar acceso publico anonimo en Storage Accounts.
- Versionado y lifecycle policies para backups y retencion de costos.
- Backups automaticos en SQL (Azure SQL, RDS); probar restores periodicamente.
- Usar connection strings desde Key Vault / Secrets Manager, no en codigo.
- Bases de datos en subnets privadas; sin endpoint publico salvo necesidad documentada.
- Considerar read replicas para lectura intensiva; documentar lag aceptable.

#### 9. Monitoreo y logging 📊
- **Azure**: Log Analytics, Application Insights, Azure Monitor alerts.
- **AWS**: CloudWatch Logs, metrics, alarms; X-Ray o equivalente para tracing.
- Centralizar logs; retencion alineada a compliance.
- Alertas en errores 5xx, latencia, disco, costos anomalos y fallos de backup.
- Dashboards por ambiente y servicio critico.
- Habilitar Diagnostic Settings (Azure) y logging en todos los recursos criticos (no solo los obvios).
- **AWS**: CloudTrail en todas las regiones; alertas ante cambios IAM, SG y recursos publicos.
- Exportar logs a SIEM o storage centralizado para auditoria e investigacion.

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
- Documentar arquitectura (diagrama) y dependencias entre recursos y servicios.

#### 14. Certificados y TLS 🔏
- **AWS**: ACM para certificados en ALB, CloudFront, API Gateway; renovacion automatica.
- **Azure**: certificados en Key Vault o App Service Managed Certificate.
- TLS 1.2+ minimo; deshabilitar protocolos y ciphers obsoletos.
- Terminar TLS en el load balancer o CDN; redirigir HTTP a HTTPS.
- Alertar antes del vencimiento si el certificado no es managed.

#### 15. DNS, CDN y trafico 🧭
- **AWS**: Route53 para DNS; health checks en records criticos.
- **Azure**: Azure DNS o integracion con proveedor externo; Front Door para CDN + WAF.
- Usar CDN (CloudFront, Azure Front Door) para assets estaticos y cache.
- TTL bajo en records que puedan cambiar en failover o migracion.
- Documentar zona DNS, registros y responsables.

#### 16. Proteccion de recursos 🛡️
- **Azure**: Resource Locks (CanNotDelete / ReadOnly) en recursos criticos de prod.
- **AWS**: termination protection en EC2/RDS; deletion protection en recursos soportados.
- S3: versioning + MFA Delete en buckets criticos cuando aplique.
- Evitar `force_destroy` en Terraform en recursos de prod.
- Revisar periodicamente recursos expuestos a internet (public buckets, open SG rules).

#### 17. Cuotas y limites de servicio 📏
- Revisar quotas/limites antes de go-live (Lambda concurrency, API rate, vCPUs, IPs).
- Solicitar aumentos con anticipacion en prod; no descubrir el limite en un incidente.
- **AWS**: Service Quotas dashboard; **Azure**: subscription limits por region.
- Documentar limites conocidos y plan de escalado cuando se acerquen.

#### 18. Kubernetes en cloud (AKS / EKS) ☸️
- RBAC habilitado; least privilege para service accounts y usuarios del cluster.
- Network Policies para limitar trafico entre pods.
- Integrar con identidad del cloud (Azure AD, IAM Roles for Service Accounts).
- Node pools separados por workload (system vs apps); auto-scaling de nodos configurado.
- Secrets via CSI driver + Key Vault / Secrets Manager, no en manifests en plain text.
- Habilitar audit logs del cluster y escaneo de imagenes en el pipeline.
