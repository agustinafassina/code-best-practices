## Best practices in Security 🔒
List of good security practices (cross-cutting) so I don't forget.

### Checklist 📋
#### 1. Principios generales 🛡️
- Defensa en profundidad: no confiar en una sola capa (firewall, auth, validacion, logging).
- Principio de menor privilegio en codigo, infra y accesos humanos.
- Fail secure: ante error, denegar acceso en lugar de permitir por defecto.
- Asumir que todo input es hostil hasta validarlo.

#### 2. Autenticacion y autorizacion 🔐
- Passwords: hash fuerte (bcrypt, Argon2); nunca guardar en texto plano.
- MFA en cuentas admin y accesos sensibles.
- Sesiones con timeout; invalidar al logout; cookies HttpOnly, Secure, SameSite.
- JWT: firmar y verificar; TTL corto; refresh tokens con rotacion; no guardar secretos en el payload.
- Autorizar en cada request (no solo en login); verificar ownership del recurso (IDOR).
- OAuth/OIDC: validar state, redirect URIs whitelist, scopes minimos.

#### 3. Secretos y configuracion 🔑
- Nunca commitear `.env`, keys, certificados ni connection strings reales.
- Usar `.gitignore` y secret scanning (GitHub secret scanning, gitleaks, pre-commit).
- Secretos en runtime: Key Vault, Secrets Manager, variables de CI cifradas.
- Rotar secretos tras incidentes o rotacion programada.
- Separar config por ambiente; no reutilizar secretos de prod en dev.

#### 4. Datos sensibles 🗄️
- Clasificar datos (PII, financieros, salud) y tratarlos segun politica.
- Cifrar en transito (TLS 1.2+) y en reposo cuando el dato lo requiera.
- Minimizar datos recolectados; no loguear passwords, tokens ni tarjetas completas.
- Enmascarar PII en logs y en UI cuando no haga falta mostrarla completa.
- Cumplir retencion y borrado (GDPR, derecho al olvido) donde aplique.

#### 5. OWASP y aplicaciones web 🌐
- Mitigar OWASP Top 10: injection, broken auth, XSS, misconfiguration, etc.
- Validar y sanitizar input en servidor (no solo en cliente).
- Usar ORM o queries parametrizadas; nunca concatenar SQL con input de usuario.
- Escapar output en HTML (evitar XSS); cuidado con `dangerouslySetInnerHTML` y templates.
- CSRF: tokens en formularios state-changing; SameSite cookies donde aplique.
- Rate limiting en login, registro y APIs sensibles.

#### 6. APIs 🔌
- HTTPS obligatorio; redirigir HTTP a HTTPS.
- Autenticacion en todos los endpoints excepto los publicos documentados.
- Validar Content-Type y tamano de body; rechazar payloads inesperados.
- No exponer stack traces ni detalles internos en errores de produccion.
- Versionar APIs; deprecar con aviso; documentar con OpenAPI.
- CORS restrictivo: origenes explicitos, no `*` con credenciales.

#### 7. Dependencias y supply chain 📦
- Actualizar dependencias con vulnerabilidades conocidas (Dependabot, Snyk, npm audit).
- Fijar versiones en lockfiles; revisar cambios en major updates.
- Verificar integridad de paquetes; preferir fuentes oficiales.
- Escanear imagenes Docker y artefactos en CI antes de deploy.

#### 8. Infraestructura y red 🏗️
- Cerrar puertos innecesarios; SSH/RDP solo desde bastion o VPN.
- Actualizar SO e imagenes base; parchear CVEs criticos con SLA definido.
- Segmentar redes; bases de datos sin acceso publico.
- WAF y DDoS protection en superficies publicas.
- Revisar Security Groups / NSG y IAM periodicamente.

#### 9. CI/CD y despliegue 🚀
- Pipelines con permisos minimos; OIDC en lugar de keys de larga duracion.
- Firmar commits o artefactos cuando el equipo lo requiera.
- Escaneo SAST/DAST en pipeline para proyectos expuestos.
- Aprobaciones manuales para deploy a prod.
- No imprimir secretos en logs de CI.

#### 10. Logging y monitoreo 📊
- Loguear eventos de seguridad: login fallido, cambios de permisos, accesos admin.
- No loguear datos sensibles en texto plano.
- Alertas ante patrones anomalos (brute force, picos de errores 401/403).
- Tener plan basico de respuesta a incidentes: contener, investigar, rotar secretos, comunicar.
- Backups probados; saber como restaurar antes del incidente.

#### 11. Headers y cliente 🖥️
- Security headers: `Content-Security-Policy`, `X-Content-Type-Options`, `X-Frame-Options` o CSP frame-ancestors, `Strict-Transport-Security`.
- Subresource Integrity (SRI) para scripts CDN cuando sea posible.
- No almacenar tokens en localStorage si hay riesgo XSS; preferir httpOnly cookies para sesiones.

#### 12. Revision y cultura 👥
- Checklist de seguridad en PRs para cambios sensibles (auth, pagos, PII).
- Threat modeling ligero en features nuevas de alto riesgo.
- Capacitar al equipo en phishing y credenciales compartidas.
- Pentest o bug bounty en apps criticas segun presupuesto y exposicion.
