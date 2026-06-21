## Best practices in AWS 🟠
List of good practices in AWS so I don't forget.

### Checklist 📋
#### 1. Naming conventions 🏷️
- Usar un patron consistente: `{org}-{env}-{region}-{service}-{resource}` (ej: acme-prod-us-east-1-app-api).
- Evitar nombres genericos (test, temp, resource1); incluir proposito y ambiente.
- Documentar el estandar de naming en el README del proyecto o en un ADR.
- Respetar limites de longitud y caracteres de cada servicio AWS.

#### 2. Tags y metadata 📐
- Aplicar tags obligatorios en todos los recursos: Environment, Owner, CostCenter, Project, ManagedBy.
- Usar Tag Policies en Organizations para exigir tags en creacion.
- Tags para facturacion y reportes de costos; revisar recursos sin tag periodicamente.
- Agrupar recursos por cuenta/OU o tags para cost allocation y governance.

#### 3. Ambientes y separacion 🌍
- Separar dev, staging y prod con cuentas AWS distintas cuando sea posible.
- No usar credenciales de prod en local ni en pipelines de dev.
- Limitar acceso cross-environment; evitar peering o trust innecesarios entre ambientes.
- Infra como codigo (Terraform) con workspaces o carpetas por ambiente.

#### 4. IAM y control de acceso 🔐
- Principio de menor privilegio: roles IAM con permisos minimos necesarios.
- Preferir IAM roles sobre access keys; usar roles para EC2, Lambda, CI/CD.
- Evitar usuario root en operaciones diarias; MFA obligatorio en cuentas privilegiadas.
- Usar IAM groups en lugar de asignar permisos usuario por usuario.
- SCPs en Organizations para denegar acciones peligrosas a nivel cuenta.
- Cuenta break-glass documentada para emergencias; auditar su uso.
- Access keys solo si no hay alternativa; rotacion automatica y maximo 90 dias.
- Auditar permisos periodicamente con IAM Access Analyzer.

#### 5. Red y seguridad perimetral 🌐
- Segmentar con VPC: subnets publicas vs privadas.
- Colocar workloads en subnets privadas; exponer solo ALB / API Gateway / CloudFront.
- Security Groups: deny by default, abrir solo puertos necesarios.
- Usar VPC Endpoints / PrivateLink para PaaS (S3, Secrets Manager, RDS, etc.) sin salir a internet publica.
- WAF delante de APIs y sitios publicos (WAF + ALB/CloudFront).
- Salida a internet via NAT Gateway; evitar IPs publicas en workloads internos.
- Habilitar AWS Shield donde aplique.
- Documentar flujos de red (diagrama): que habla con que y por que puerto.

#### 6. Secretos y claves 🔑
- Secrets Manager o Parameter Store (SecureString); integrar con Lambda, ECS, etc.
- Nunca commitear connection strings, API keys ni .env con secretos reales.
- Cifrar datos en reposo (SSE-S3, SSE-KMS, RDS encryption).
- Evaluar CMK (customer-managed keys en KMS) cuando compliance o control de rotacion lo requieran.
- Separar KMS keys por ambiente (dev vs prod).

#### 7. Compute y contenedores ⚙️
- Lambda para eventos; ECS/Fargate o EKS para contenedores; EC2 solo cuando sea necesario.
- Imagenes minimas y actualizadas; escanear vulnerabilidades en ECR.
- Health checks y auto-scaling configurados; limits de CPU/memoria definidos.
- Distribuir cargas en multiples Availability Zones (multi-AZ) para alta disponibilidad.
- Habilitar termination protection en instancias criticas de prod.
- Lambda: definir timeout, concurrencia y reserved concurrency; throttling en API Gateway.

#### 8. Storage y bases de datos 💾
- S3 Block Public Access habilitado a nivel de cuenta y bucket.
- Versionado y lifecycle policies para backups y retencion de costos.
- Backups automaticos en RDS; probar restores periodicamente.
- Usar connection strings desde Secrets Manager, no en codigo.
- Bases de datos en subnets privadas; sin endpoint publico salvo necesidad documentada.
- Considerar read replicas para lectura intensiva; documentar lag aceptable.

#### 9. Monitoreo y logging 📊
- CloudWatch Logs, metrics, alarms; X-Ray para tracing.
- Centralizar logs; retencion alineada a compliance.
- Alertas en errores 5xx, latencia, disco, costos anomalos y fallos de backup.
- Dashboards por ambiente y servicio critico.
- CloudTrail en todas las regiones; alertas ante cambios IAM, SG y recursos publicos.
- Exportar logs a SIEM o S3 centralizado para auditoria e investigacion.

#### 10. Costos y optimizacion 💰
- Budgets y alertas de gasto con AWS Budgets.
- Apagar o escalar a cero recursos de dev fuera de horario laboral cuando aplique.
- Revisar recursos huerfanos (discos, IPs, snapshots, Elastic IPs sin usar) mensualmente.
- Reserved Instances / Savings Plans para cargas estables en prod.
- Rightsizing: no sobre-dimensionar desde el dia uno.

#### 11. Disaster recovery y backups 🔄
- Definir RPO/RTO por servicio critico.
- Geo-redundancy donde el negocio lo requiera (cross-region replication, multi-AZ).
- Probar restore de backups al menos una vez al trimestre.
- Documentar runbooks de failover y contactos de escalacion.

#### 12. Governance y compliance 📜
- AWS Organizations, SCPs, Control Tower o landing zone equivalente.
- CloudTrail habilitado para auditoria en todas las regiones.
- Revisar AWS Security Hub y corregir findings prioritarios.

#### 13. CI/CD e infraestructura 🚀
- Pipelines con identidad federada (OIDC) en GitHub Actions / CodePipeline.
- Plan/apply de Terraform con aprobacion manual en prod.
- State remoto con locking (S3 + DynamoDB).
- No aplicar cambios manuales en prod sin reflejarlos en IaC despues.
- Documentar arquitectura (diagrama) y dependencias entre recursos y servicios.

#### 14. Certificados y TLS 🔏
- ACM para certificados en ALB, CloudFront, API Gateway; renovacion automatica.
- TLS 1.2+ minimo; deshabilitar protocolos y ciphers obsoletos.
- Terminar TLS en ALB o CloudFront; redirigir HTTP a HTTPS.
- Alertar antes del vencimiento si el certificado no es managed (certificados importados).

#### 15. DNS, CDN y trafico 🧭
- Route53 para DNS; health checks en records criticos.
- CloudFront para assets estaticos, cache y proteccion perimetral.
- TTL bajo en records que puedan cambiar en failover o migracion.
- Documentar hosted zones, registros y responsables.

#### 16. Proteccion de recursos 🛡️
- Termination protection en EC2/RDS; deletion protection en recursos soportados.
- S3: versioning + MFA Delete en buckets criticos cuando aplique.
- Evitar `force_destroy` en Terraform en recursos de prod.
- Revisar periodicamente recursos expuestos a internet (public buckets, open SG rules).

#### 17. Cuotas y limites de servicio 📏
- Revisar quotas antes de go-live (Lambda concurrency, API rate, vCPUs, Elastic IPs).
- Solicitar aumentos con anticipacion en prod via Service Quotas dashboard.
- Documentar limites conocidos y plan de escalado cuando se acerquen.

#### 18. Kubernetes (EKS) ☸️
- RBAC habilitado; least privilege para service accounts y usuarios del cluster.
- Network Policies para limitar trafico entre pods.
- IAM Roles for Service Accounts (IRSA) para pods.
- Node groups separados por workload (system vs apps); auto-scaling de nodos configurado.
- Secrets via CSI driver + Secrets Manager, no en manifests en plain text.
- Habilitar audit logs del cluster y escaneo de imagenes en el pipeline.
