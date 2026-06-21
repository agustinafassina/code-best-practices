## Best practices in Azure 🔵
List of good practices in Azure so I don't forget.

### Checklist 📋
#### 1. Naming conventions 🏷️
- Usar un patron consistente: `{org}-{env}-{region}-{service}-{resource}` (ej: acme-prod-eus2-app-api).
- Evitar nombres genericos (test, temp, resource1); incluir proposito y ambiente.
- Documentar el estandar de naming en el README del proyecto o en un ADR.
- Respetar limites de longitud y caracteres de cada servicio Azure.

#### 2. Tags y metadata 📐
- Aplicar tags obligatorios en todos los recursos: Environment, Owner, CostCenter, Project, ManagedBy.
- Usar Azure Policy para exigir tags en creacion.
- Tags para facturacion y reportes de costos; revisar recursos sin tag periodicamente.
- Usar Resource Groups como unidad logica de despliegue y lifecycle.

#### 3. Ambientes y separacion 🌍
- Separar dev, staging y prod con subscriptions distintas cuando sea posible.
- No usar credenciales de prod en local ni en pipelines de dev.
- Limitar acceso cross-environment; evitar peering o trust innecesarios entre ambientes.
- Infra como codigo (Terraform) con workspaces o carpetas por ambiente.

#### 4. IAM y control de acceso 🔐
- Principio de menor privilegio: roles RBAC con permisos minimos necesarios.
- Preferir Managed Identities sobre service principals con secretos.
- Asignar RBAC en scope de recurso o Resource Group, no en subscription completa.
- Evitar rol Owner en operaciones diarias; MFA obligatorio en cuentas privilegiadas.
- Usar Azure AD groups en lugar de asignar permisos usuario por usuario.
- Conditional Access para politicas de acceso (ubicacion, dispositivo, riesgo).
- Cuenta break-glass documentada para emergencias; auditar su uso.
- Auditar permisos periodicamente con Azure AD Access Reviews.

#### 5. Red y seguridad perimetral 🌐
- Segmentar con VNet: subnets publicas vs privadas.
- Colocar workloads en subnets privadas; exponer solo Application Gateway / Front Door / Load Balancer.
- NSG: deny by default, abrir solo puertos necesarios.
- Usar Private Endpoints para PaaS (Storage, Key Vault, SQL, etc.) sin salir a internet publica.
- WAF delante de APIs y sitios publicos (Front Door / Application Gateway).
- Salida a internet via NAT Gateway o Azure Firewall; evitar IPs publicas en workloads internos.
- Habilitar Azure DDoS Protection donde aplique.
- Documentar flujos de red (diagrama): que habla con que y por que puerto.

#### 6. Secretos y claves 🔑
- Key Vault para secretos, certificados y keys; referenciar desde App Service / Functions / AKS.
- Nunca commitear connection strings, API keys ni .env con secretos reales.
- Habilitar soft-delete y purge protection en Key Vault; rotation automatica donde el servicio lo permita.
- Cifrar datos en reposo (Storage encryption, Azure SQL TDE).
- Evaluar CMK (customer-managed keys) cuando compliance o control de rotacion lo requieran.
- Separar Key Vault por ambiente (dev vs prod).

#### 7. Compute y contenedores ⚙️
- App Service / Container Apps / AKS segun complejidad; evitar VMs si no hace falta.
- Imagenes minimas y actualizadas; escanear vulnerabilidades en ACR.
- Health checks y auto-scaling configurados; limits de CPU/memoria definidos.
- Usar Availability Zones en recursos soportados (SQL, VMs, AKS).
- Functions: definir timeout, plan de consumo vs premium, y limites de concurrencia.

#### 8. Storage y bases de datos 💾
- Deshabilitar acceso publico anonimo en Storage Accounts por defecto.
- Versionado y lifecycle policies para backups y retencion de costos.
- Backups automaticos en Azure SQL; probar restores periodicamente.
- Usar connection strings desde Key Vault, no en codigo.
- Bases de datos en subnets privadas; sin endpoint publico salvo necesidad documentada.
- Considerar read replicas para lectura intensiva; documentar lag aceptable.

#### 9. Monitoreo y logging 📊
- Log Analytics, Application Insights, Azure Monitor alerts.
- Centralizar logs; retencion alineada a compliance.
- Alertas en errores 5xx, latencia, disco, costos anomalos y fallos de backup.
- Dashboards por ambiente y servicio critico.
- Habilitar Diagnostic Settings en todos los recursos criticos (no solo los obvios).
- Activity Log centralizado; alertas ante cambios IAM, NSG y recursos publicos.
- Exportar logs a SIEM o storage centralizado para auditoria e investigacion.

#### 10. Costos y optimizacion 💰
- Budgets y alertas de gasto con Azure Cost Management.
- Apagar o escalar a cero recursos de dev fuera de horario laboral cuando aplique.
- Revisar recursos huerfanos (discos, IPs, snapshots) mensualmente.
- Azure Reservations para cargas estables en prod.
- Rightsizing: no sobre-dimensionar desde el dia uno.

#### 11. Disaster recovery y backups 🔄
- Definir RPO/RTO por servicio critico.
- Geo-redundancy donde el negocio lo requiera (GRS, zone-redundant storage).
- Probar restore de backups al menos una vez al trimestre.
- Documentar runbooks de failover y contactos de escalacion.

#### 12. Governance y compliance 📜
- Management Groups, Azure Policy, Landing Zones.
- Activity Log y Diagnostic Settings habilitados para auditoria.
- Revisar Microsoft Defender for Cloud y corregir findings prioritarios.

#### 13. CI/CD e infraestructura 🚀
- Pipelines con identidad federada (OIDC) en GitHub Actions / Azure DevOps.
- Plan/apply de Terraform con aprobacion manual en prod.
- State remoto con locking (Azure Storage + blob lease).
- No aplicar cambios manuales en prod sin reflejarlos en IaC despues.
- Documentar arquitectura (diagrama) y dependencias entre recursos y servicios.

#### 14. Certificados y TLS 🔏
- Certificados en Key Vault o App Service Managed Certificate.
- TLS 1.2+ minimo; deshabilitar protocolos y ciphers obsoletos.
- Terminar TLS en Application Gateway, Front Door o Load Balancer; redirigir HTTP a HTTPS.
- Alertar antes del vencimiento si el certificado no es managed.

#### 15. DNS, CDN y trafico 🧭
- Azure DNS o integracion con proveedor externo.
- Front Door para CDN + WAF en apps publicas.
- TTL bajo en records que puedan cambiar en failover o migracion.
- Documentar zona DNS, registros y responsables.

#### 16. Proteccion de recursos 🛡️
- Resource Locks (CanNotDelete / ReadOnly) en recursos criticos de prod.
- Evitar `force_destroy` en Terraform en recursos de prod.
- Revisar periodicamente recursos expuestos a internet (Storage publico, NSG abiertas).

#### 17. Cuotas y limites de servicio 📏
- Revisar subscription limits por region antes de go-live (vCPUs, IPs, storage).
- Solicitar aumentos con anticipacion en prod; no descubrir el limite en un incidente.
- Documentar limites conocidos y plan de escalado cuando se acerquen.

#### 18. Kubernetes (AKS) ☸️
- RBAC habilitado; least privilege para service accounts y usuarios del cluster.
- Network Policies para limitar trafico entre pods.
- Integrar con Azure AD (Azure RBAC for AKS, workload identity).
- Node pools separados por workload (system vs apps); auto-scaling de nodos configurado.
- Secrets via CSI driver + Key Vault, no en manifests en plain text.
- Habilitar audit logs del cluster y escaneo de imagenes en el pipeline.
