# Table of Contents
## 1. Project Description	
   - 1.1 Purpose	
   - 1.2 Scope	
   - 1.3 Definitions & Abbreviations	
## 2. System Overview	
  - 2.1 System Description	
  - 2.2 Business Goals	
  - 2.3 Stakeholders	
## 3. Architectural Drivers	
  - 3.1 Functional Requirements	
  - 3.2 Non-Functional Requirements (Quality Attributes)	
## 4. Key Architectural Decisions	
   - 4.1 Architecture Style: Modular Monolith with Plugin Architecture	
   - 4.2 Plugin Hook System (Observer Pattern)	
   - 4.3 Template/Theme Engine	
   - 4.4 Dual-Mode API (Traditional + Headless)	
   ### 4.5 RBAC Security Model	
## 5. System Context (C4 Level 1)	
  ### 5.1 Context Description	
  ### 5.2 External Actors & Systems	
## 6. Container Architecture (C4 Level 2)	
  ### 6.1 Overview	
## 6.2 Container Inventory	
  ### 6.3 Inter-Container Communication	
## 7. Component Architecture (C4 Level 3)	
  ### 7.1 CMS Core API Components	
  ### 7.2 Plugin Architecture Detail	
  ### 7.3 Theme Architecture Detail	
## 8. Data Architecture	
  ### 8.1 Database Schema (Core Tables)	
  ### 8.2 Data Access Patterns	
  ### 8.3 Caching Strategy	
## 9. API Design	
  ### 9.1 REST API Endpoint Summary	
  ### 9.2 API Authentication Flow	
## 10. Security Architecture	
  ### 10.1 Security Controls by Layer	
  ### 10.2 OWASP Compliance Mapping	
## 11. Deployment Architecture	
  ### 11.1 Deployment Overview	
  ### 11.2 Infrastructure Components	
  ### 11.3 CI/CD Pipeline	
## 12. Architecture Quality Scenarios	
## 13. Technology Stack	
## 14. Risks & Mitigations	
## 15. Appendix	
## A. Diagrams Index	
## B. Document Revision History	
## 1 Project Description

This project focuses on designing the software architecture of WordPress, one of the world’s most widely used Content Management Systems (CMS). The main objective of this project is to study, analyze, and redesign WordPress using modern software architecture principles to improve its scalability, maintainability, security, and extensibility while preserving its core features such as plugins, themes, and user-friendly content management.

WordPress is widely used across blogs, business websites, e-commerce platforms, educational systems, and enterprise applications. However, with the growing demand for high performance, API-driven architectures, cloud deployment, and stronger security mechanisms, a well-structured architectural design becomes essential. This project provides a complete architectural blueprint that demonstrates how WordPress can evolve into a modern, enterprise-ready CMS using advanced software engineering practices.

The proposed architecture follows a Modular Monolith with Plugin-Based Design, where the system is deployed as a single application but internally divided into independent modules such as user management, content management, media handling, plugin system, theme engine, API services, and security services. Each module is clearly defined and communicates through well-structured interfaces, ensuring loose coupling and high maintainability.

A key component of the design is the Plugin Hook System, based on the Observer Pattern, which allows developers to extend system functionality without modifying the core code. The Theme Engine manages the presentation layer using template-based rendering, enabling flexible and customizable website designs.

The architecture also supports Dual-Mode Operation, including traditional server-side rendering for SEO optimization and headless CMS capabilities through REST APIs for integration with modern frameworks like React, Vue, and mobile applications.

Security is ensured through Role-Based Access Control (RBAC), authentication mechanisms, encrypted communication, secure file handling, and adherence to OWASP security standards. Performance and scalability are achieved using caching mechanisms (Redis), optimized database design, CDN integration, and containerized deployment using Docker and Kubernetes.

The technology stack may include PHP/Laravel or Node.js for backend development, React/Next.js for frontend interfaces, MySQL for database management, and Elasticsearch for advanced search functionality.

Overall, this project demonstrates a modern architectural redesign of WordPress as a scalable, secure, and extensible CMS platform. It serves as a strong academic and practical reference for understanding large-scale software system design and modern web architecture principles.
![My Project Screenshot](description.png)

. ### 1.1 Purpose

This Software Architecture Design Document (SADD) describes the complete architectural blueprint for a Content Management System (CMS) modeled after WordPress. It documents architectural decisions, system structure, component interactions, deployment strategies, and quality attributes. It serves as the main technical reference for developers, architects, testers, and stakeholders involved in system design and development.

### 1.2 Scope

The CMS covers the following functional areas:

Content creation, editing, publishing, and management (posts, pages, media)
User authentication, authorization, roles, and permissions
Theme and template engine for frontend rendering
Plugin and extension framework for system extensibility
RESTful API for headless CMS and third-party integration
Database persistence and caching mechanisms
Admin dashboard for system configuration and management
SEO tools, analytics integration, and media library management

### 1.3 Definitions & Abbreviations
Term / Acronym	Definition
CMS	Content Management System
API	Application Programming Interface
REST	Representational State Transfer
CDN	Content Delivery Network
MVC	Model-View-Controller design pattern
ORM	Object Relational Mapper
JWT	JSON Web Token for stateless auth
WYSIWYG	What You See Is What You Get (editor)
RBAC	Role-Based Access Control
SaaS	Software as a Service
SSR	Server-Side Rendering
CSR	Client-Side Rendering

### 1.4 References
•	Kashifraz/SE-HyperlocalDeliverySystem-Architecture (Reference Architecture Repo)
•	WordPress Core Architecture Documentation — wordpress.org
•	C4 Model for Software Architecture — c4model.com
•	ISO/IEC 42010:2011 — Software Architecture Description Standard
•	OWASP Top 10 — Web Application Security
 
### 2. System Overview
### 2.1 System Description
The CMS is a full-featured, extensible web-based platform that enables non-technical users to create, edit, organize, and publish digital content. Inspired by WordPress, the system follows a plugin-based monolithic-to-microservices evolution path — starting with a modular monolith and providing clean API boundaries for future service extraction.

The platform supports multiple modes of operation: a traditional server-rendered front-end for SEO-critical use cases, and a headless API mode for decoupled front-end frameworks (React, Vue, mobile apps).

### 2.2 Business Goals
•	Enable rapid content creation and publishing with zero coding requirement
•	Support 10,000+ concurrent users with sub-2-second page load times
•	Provide a plugin marketplace allowing third-party feature extensions
•	Ensure enterprise-grade security and GDPR compliance
•	Deliver a mobile-responsive admin interface

## 2.3 Stakeholders
Stakeholder	Role	Interest
Content Authors	Create and publish content	Easy editor, preview, scheduling
Site Administrators	Manage site settings, users, plugins	Full control, security, backups
Plugin Developers	Extend CMS functionality	Stable APIs, hooks, documentation
Theme Designers	Design site appearance	Template engine, CSS flexibility
End Users (Visitors)	Consume content	Fast, responsive, accessible pages
DevOps/Infra Team	Deploy and scale the platform	Containerization, monitoring, CI/CD
Business Owners	Drive revenue via content	Analytics, SEO, uptime, cost
 
## 3. Architectural Drivers
### 3.1 Functional Requirements
ID	Feature	Description
FR-01	User Authentication	Register, login, logout, password reset via JWT/OAuth
FR-02	Role & Permission Mgmt	RBAC: Super Admin, Admin, Editor, Author, Subscriber
FR-03	Content CRUD	Create, edit, delete, version posts, pages, custom types
FR-04	Rich Text Editor	WYSIWYG block-based editor (Gutenberg-style)
FR-05	Media Library	Upload, tag, crop, search images/videos/documents
FR-06	Taxonomy System	Categories, tags, hierarchical custom taxonomies
FR-07	Theme Engine	Template hierarchy, PHP-like/Twig templates, child themes
FR-08	Plugin Framework	Hook/filter system, plugin activation/deactivation
FR-09	REST API	Full CRUD API for all content entities, JWT auth
FR-10	Search	Full-text content search with filters and facets
FR-11	Comments System	Threaded comments, moderation, spam filtering
FR-12	SEO Management	Meta tags, sitemaps, structured data, Open Graph
FR-13	Content Scheduling	Publish/unpublish at scheduled date/time
FR-14	Multi-site Support	Single installation serving multiple sites/domains

### 3.2 Non-Functional Requirements (Quality Attributes)
Attribute	Requirement	Target
Performance	Page load time	< 2s for front-end pages; < 300ms for API
Scalability	Concurrent users	10,000+ concurrent; horizontal scaling support
Availability	Uptime SLA	99.9% uptime (< 8.7 hrs downtime/year)
Security	Auth & data protection	JWT, HTTPS, OWASP Top 10, CSRF, SQL injection prevention
Maintainability	Code modularity	Plugin/theme system; < 30 min to add new plugin
Extensibility	New feature addition	Hook-based plugin API; backward-compatible versioning
Portability	Deployment targets	Docker-compatible; cloud-agnostic (AWS, GCP, Azure)
Usability	Admin UX	Responsive admin on mobile; WCAG 2.1 AA accessibility
 
# 4. Key Architectural Decisions
### 4.1 Architecture Style: Modular Monolith with Plugin Architecture
The system adopts a Modular Monolith pattern as the core architectural style. All modules are deployed as a single unit but maintain clean internal boundaries through a hook/filter event bus. This mirrors WordPress's proven approach and reduces operational complexity while allowing future service extraction.
•	Pros: Simpler deployment, no network latency between modules, easier transactions
•	Cons: Single scaling unit; mitigated via horizontal replication and caching
•	Migration path: Bounded modules can be extracted to microservices when needed

### 4.2 Plugin Hook System (Observer Pattern)
All extensibility is achieved through an Action Hook and Filter Hook system, inspired by WordPress's apply_filters/do_action mechanism. Plugins register callbacks for named events; the core never depends on plugin code directly.

### 4.3 Template/Theme Engine
Themes follow a hierarchical template resolution (single.php → singular.php → index.php). A Twig-compatible templating engine provides safe, sandboxed rendering without PHP execution in themes.

### 4.4 Dual-Mode API (Traditional + Headless)
The system natively supports both server-rendered HTML (for SEO) and a JSON REST/GraphQL API for headless consumption. This enables decoupled front-ends (Next.js, React Native, mobile apps).

### 4.5 RBAC Security Model
Role-Based Access Control with five built-in roles: Super Admin, Admin, Editor, Author, Subscriber. Permissions are additive and stored in a capability matrix, extensible by plugins.
 
## 5. System Context (C4 Level 1)
### 5.1 Context Description
The System Context diagram shows the CMS as a black box and identifies all external users and systems that interact with it. The CMS sits at the center, interacting with human users (authors, visitors, admins) and external systems (email, CDN, third-party auth, payment, analytics).

Note: A full SVG/visual context diagram is provided as a separate diagram artifact alongside this document.

### 5.2 External Actors & Systems
Actor / System	Type	Interaction
Content Author	Human	Creates and manages content via Admin Dashboard
Site Administrator	Human	Configures plugins, themes, users, settings
Site Visitor	Human	Reads published content via the public web front-end
Plugin Developer	Human	Deploys plugins via Plugin Manager or marketplace
SMTP / Email Service	External	Sends registration, notification, and digest emails
CDN (Cloudflare)	External	Serves static assets and caches public pages globally
OAuth Provider (Google)	External	Provides social login / SSO authentication tokens
Cloud Storage (S3)	External	Stores uploaded media files (images, videos, docs)
Search Engine (Elasticsearch)	External	Indexes and retrieves full-text content search
Analytics (Google Analytics)	External	Tracks page views, sessions, and conversion events
Payment Gateway (Stripe)	External	Handles premium plugin/theme subscription payments
 
## 6. Container Architecture (C4 Level 2)
### 6.1 Overview
The Container diagram decomposes the CMS into deployable units (containers). Each container is independently buildable, has a clearly defined responsibility, and communicates via HTTP/REST or message queues.

### 6.2 Container Inventory
Container	Technology	Port	Responsibility
Web Front-End	Node.js / Next.js	3000	SSR/CSR public-facing site
Admin Dashboard	React SPA	3001	Content authoring & site management UI
CMS Core API Server	PHP/Laravel or Node	8000	Business logic, REST/GraphQL API
Plugin Runtime	PHP/Node sandbox	Internal	Executes plugin hooks and filters
Theme Engine	Twig / Blade	Internal	Template rendering for pages
Job Queue Worker	Redis + Bull / Horizon	Internal	Async: email, indexing, image resize
Primary Database	MySQL 8.x	3306	Content, users, metadata persistence
Cache Layer	Redis 7.x	6379	Sessions, object cache, rate limiting
Search Service	Elasticsearch	9200	Full-text content indexing & search
File Storage Proxy	MinIO / S3 proxy	9000	Media upload, resizing, serving
Notification Service	SMTP / SendGrid API	External	Email notifications and newsletters

### 6.3 Inter-Container Communication
•	Admin Dashboard → CMS Core API: REST/JSON over HTTPS (JWT-authenticated)
•	Web Front-End → CMS Core API: REST + GraphQL for data fetching
•	CMS Core API → MySQL: Direct JDBC/PDO connection (connection pool)
•	CMS Core API → Redis: Object caching, session management
•	CMS Core API → Job Queue: Publishes async jobs (email, search index updates)
•	Job Queue Worker → Elasticsearch: Indexes content changes
•	Job Queue Worker → Notification Service: Sends emails via SMTP/SendGrid
•	Web Front-End → CDN: Static assets served via CDN edge nodes
 
### 7. Component Architecture (C4 Level 3)
## 7.1 CMS Core API Components
Component	Responsibility
AuthController	Handles login, registration, JWT issuance, OAuth callback, password reset
UserManager	User CRUD, role assignment, capability checks (can_user_do())
ContentController	Post/page CRUD, status transitions (draft→publish), versioning
PostTypeRegistry	Registers built-in and custom post types (post, page, attachment, CPT)
TaxonomyManager	Category/tag CRUD, hierarchical taxonomy traversal
MediaManager	Upload handling, MIME validation, image resizing, S3 proxy
PluginLoader	Discovers, validates, activates/deactivates plugins; applies hooks
HookDispatcher	Implements do_action() / apply_filters() event bus
ThemeLoader	Resolves template hierarchy, renders via Twig engine
RouterEngine	URL routing: permalink resolution, rewrite rules
SearchManager	Query builder for Elasticsearch, faceted search, ranking
CommentManager	Threaded comment CRUD, moderation queue, spam scoring (Akismet API)
SettingsManager	Site options CRUD, transients (cached options), multi-site config
SEOManager	Meta tag injection, sitemap.xml generation, canonical URLs
SchedulerService	Cron-based content publishing, cleanup jobs, cache warming
AuditLogger	Records all admin actions to audit_log table for compliance

### 7.2 Plugin Architecture Detail
The plugin system is the cornerstone of extensibility. Plugins interact with core exclusively through the Hook Dispatcher, never via direct coupling:
•	Action Hooks: do_action('cms_save_post', $post_id) — plugins register listeners
•	Filter Hooks: apply_filters('cms_the_content', $content) — plugins modify data
•	Plugin Manifest: Each plugin declares its name, version, required CMS version, and capabilities
•	Plugin Sandbox: Plugin PHP/JS executed in a restricted environment; cannot access core internals directly
•	Plugin Database: Plugins may create own DB tables using provided Migration API

### 7.3 Theme Architecture Detail
The theme system uses a template hierarchy to resolve the correct template for any given URL:
•	index.php → fallback for all content types
•	single-{post-type}.php → single post of a custom type
•	archive-{post-type}.php → archive list of a custom type
•	page-{slug}.php → specific page by slug
•	taxonomy-{taxonomy}-{term}.php → taxonomy archive pages
•	search.php → search results page
 
## 8. Data Architecture
### 8.1 Database Schema (Core Tables)
Table	Key Columns	Purpose
cms_posts	ID, post_author, post_date, post_content, post_title, post_status, post_type, post_name, post_parent	All content entities (posts, pages, media, revisions)
cms_postmeta	meta_id, post_id, meta_key, meta_value	EAV extension for post metadata (custom fields)
cms_users	ID, user_login, user_pass, user_email, user_registered, display_name	All registered users
cms_usermeta	umeta_id, user_id, meta_key, meta_value	User metadata and role capability serialization
cms_terms	term_id, name, slug, term_group	Category/tag term definitions
cms_term_taxonomy	term_taxonomy_id, term_id, taxonomy, parent, count	Links terms to taxonomies (category, tag, etc.)
cms_term_relationships	object_id, term_taxonomy_id, term_order	Many-to-many: posts ↔ terms
cms_comments	comment_ID, comment_post_ID, comment_author, comment_content, comment_parent, comment_approved	Threaded comments on posts
cms_options	option_id, option_name, option_value, autoload	Site configuration key-value store
cms_links	link_id, link_url, link_name, link_category	Blogroll/link management

### 8.2 Data Access Patterns
•	ORM Layer: All DB access through an Active Record / Query Builder abstraction (no raw SQL in business logic)
•	Object Cache: Frequently-accessed queries (post, options) cached in Redis with TTL
•	Transients: Time-limited cached computation results stored in cms_options or Redis
•	Database Abstraction: $wpdb-equivalent wrapper allows plugin authors to run parameterized queries safely

### 8.3 Caching Strategy
Cache Type	Storage	TTL / Strategy
Page Cache	Redis / CDN edge	Full rendered HTML; invalidated on content save
Object Cache	Redis	DB query results; TTL 300s; invalidated on write
Transients	Redis or MySQL	Plugin-managed; explicit TTL per transient
Asset Cache	CDN (Cloudflare)	JS/CSS/images; versioned filenames for cache busting
Search Index	Elasticsearch	Near-real-time; updated on content publish/update
 
## 9. API Design
### 9.1 REST API Endpoint Summary
Method	Endpoint	Description	Auth Required
POST	/api/v1/auth/register	User registration	No
POST	/api/v1/auth/login	Login → JWT token	No
GET	/api/v1/posts	List published posts	No
POST	/api/v1/posts	Create new post	Author+
GET	/api/v1/posts/{id}	Get post by ID	No (if published)
PUT	/api/v1/posts/{id}	Update post	Author+
DELETE	/api/v1/posts/{id}	Delete post	Editor+
GET	/api/v1/media	List media library	Author+
POST	/api/v1/media/upload	Upload file to S3	Author+
GET	/api/v1/categories	List all categories	No
GET	/api/v1/search?q={term}	Full-text search	No
GET	/api/v1/users/me	Current user profile	JWT
GET	/api/v1/plugins	List installed plugins	Admin
POST	/api/v1/plugins/{id}/activate	Activate a plugin	Admin

## 9.2 API Authentication Flow
•	Client POSTs credentials to /api/v1/auth/login
•	Server validates credentials, issues Access Token (15 min) and Refresh Token (7 days)
•	Client attaches Authorization: Bearer {token} header to all protected API requests
•	Server validates JWT signature and expiry on each request via middleware
•	Refresh token endpoint /api/v1/auth/refresh issues new access token without re-login
•	OAuth2 flow available via /api/v1/auth/google for social login
 
## 10. Security Architecture
### 10.1 Security Controls by Layer
Layer	Threat	Control
Network	DDoS, MITM	WAF (Cloudflare), TLS 1.3 everywhere, rate limiting
Authentication	Credential theft, brute force	bcrypt passwords, JWT, MFA, account lockout after 5 failures
Authorization	Privilege escalation	RBAC capability checks on every API route; nonce validation for forms
Input Validation	XSS, SQL Injection	Input sanitization (strip_tags, htmlspecialchars), ORM parameterized queries
CSRF	Cross-site request forgery	Double-submit cookie pattern; nonce tokens on all state-changing forms
File Uploads	Malware, path traversal	MIME type validation, randomized filenames, quarantine scan, S3 isolation
Data at Rest	Database breach	AES-256 encryption for PII fields; encrypted backups
Audit	Insider threats	Audit log for all admin actions with user, IP, timestamp

### 10.2 OWASP Compliance Mapping
•	A01 Broken Access Control → RBAC enforcement + capability matrix
•	A02 Cryptographic Failures → bcrypt, AES-256, TLS 1.3
•	A03 Injection → ORM parameterized queries, input sanitization
•	A04 Insecure Design → Threat model review, secure defaults
•	A05 Security Misconfiguration → Hardened default config, secrets in environment variables
•	A06 Vulnerable Components → Automated dependency scanning (Snyk, npm audit)
•	A07 Auth Failures → JWT, MFA, account lockout, refresh token rotation
 
## 11. Deployment Architecture
### 11.1 Deployment Overview
The CMS is packaged as Docker containers orchestrated by Kubernetes (K8s). The deployment targets a cloud environment (AWS/GCP/Azure) and supports auto-scaling, rolling updates, and zero-downtime deployments.

## 11.2 Infrastructure Components
Component	Technology	Purpose
Load Balancer	AWS ALB / Nginx Ingress	Distribute traffic across API pods; SSL termination
API Pods	Docker + K8s Deployment	CMS Core API; HPA for auto-scaling (min 2, max 20)
Web Front-End Pods	Docker + K8s Deployment	Next.js SSR pods; CDN pass-through for static assets
Worker Pods	Docker + K8s Job/CronJob	Queue workers for email, indexing, scheduled posts
Primary DB	AWS RDS MySQL (Multi-AZ)	Primary write; automated failover to standby
Read Replica	AWS RDS Read Replica	Offload read traffic; reporting queries
Cache Cluster	AWS ElastiCache (Redis)	Shared object/session cache across pods
Search Cluster	AWS OpenSearch / Elastic Cloud	3-node cluster for HA full-text search
Object Storage	AWS S3 + CloudFront	Media files; CDN-distributed worldwide
Container Registry	AWS ECR / Docker Hub	Stores versioned Docker images for all services
Secrets Management	AWS Secrets Manager / Vault	Stores DB credentials, API keys, JWT secrets

### 11.3 CI/CD Pipeline
•	Source Control: GitHub (feature branch → PR → main)
•	CI: GitHub Actions — runs lint, unit tests, integration tests, security scan on every PR
•	Build: Docker image build + push to ECR on merge to main
•	CD: ArgoCD GitOps — detects new image tag, triggers rolling update to K8s
•	Blue-Green / Canary: Production deploys via 10% canary with automatic rollback on error rate > 1%
•	DB Migrations: Flyway/Liquibase migrations run as K8s Job pre-deployment
 
## 12. Architecture Quality Scenarios
Attribute	Stimulus	Environment	Response (Target)
Performance	10,000 concurrent users	Normal load	P95 API response < 300ms; page load < 2s via CDN cache
Scalability	5× traffic spike (launch day)	Peak load	HPA scales API pods from 4 to 20; no downtime, same P95
Availability	Database primary failure	Fault condition	RDS Multi-AZ failover completes in < 60s; requests queued in Redis
Security	SQL injection attempt	Normal operation	ORM blocks query; WAF logs and blocks source IP; no data exposed
Extensibility	New plugin installed	Admin action	Plugin activates in < 5s; no CMS restart required; hook system extends behavior
Maintainability	Core version update	Maintenance window	Rolling update with zero downtime; plugins backward-compatible via versioned hooks
Recoverability	Data corruption event	Disaster recovery	Point-in-time restore from RDS backup in < 15 min; RPO < 1 hour
 
## 13. Technology Stack
Layer	Technology	Rationale
Backend API	PHP 8.2 + Laravel 11 (or Node.js + Express)	Proven CMS ecosystem; mature ORM; plugin ecosystem
Front-End (Public)	Next.js 14 (React)	SSR/SSG for SEO; streaming; ISR for performance
Admin UI	React 18 + TypeScript + Vite	Component-based; fast HMR; accessible UI libraries
Template Engine	Twig 3.x (PHP) / Nunjucks (Node)	Safe, sandboxed templating for theme developers
Primary Database	MySQL 8.x	Battle-tested for CMS workloads; JSON column support
Caching	Redis 7.x	Sub-millisecond object cache; pub/sub for invalidation
Search	Elasticsearch 8.x	Full-text search; BM25 ranking; plugin facets
File Storage	AWS S3 + CloudFront CDN	Infinitely scalable; edge distribution worldwide
Queue	Redis + Bull (Node) / Laravel Horizon	Reliable async job processing with retries
Auth	JWT + Passport.js / Laravel Sanctum	Stateless; works for API and session modes
Container	Docker + Kubernetes (EKS)	Portable, scalable, cloud-native deployment
CI/CD	GitHub Actions + ArgoCD	GitOps workflow; automated test and deploy
Monitoring	Prometheus + Grafana + Sentry	Metrics, dashboards, error tracking
Logging	ELK Stack (Elasticsearch, Logstash, Kibana)	Centralized structured log search
 
## 14. Risks & Mitigations
Risk	Severity	Mitigation
Plugin conflicts breaking core stability	High	Plugin versioning API; isolated sandbox execution; automated compatibility tests
Database single point of failure	High	RDS Multi-AZ; read replicas; automated failover; tested restore procedures
DDoS attack overwhelming the platform	High	Cloudflare WAF + rate limiting; auto-scaling; CDN absorbs edge traffic
SQL Injection via plugin code	Medium	ORM enforced for all DB queries; security code review for plugins; WAF
Theme developer XSS injection	Medium	Twig auto-escaping; Content Security Policy headers; output sanitization
Media storage costs explosion	Low	S3 lifecycle policies; image optimization before storage; storage quotas per site
Search index out of sync with DB	Medium	Transactional outbox pattern; index reconciliation cron job; dead letter queue
 
## 15. Appendix
A. Diagrams Index
•	Figure 1: System Context Diagram (C4 Level 1) — separate diagram file
•	Figure 2: Container Diagram (C4 Level 2) — separate diagram file
•	Figure 3: Component Diagram (C4 Level 3) — separate diagram file
•	Figure 4: Deployment Architecture Diagram — separate diagram file
•	Figure 5: Database Entity Relationship Diagram — separate diagram file
•	Figure 6: Security Architecture Layer Diagram — separate diagram file

B. Document Revision History
Version	Date	Description	Author
0.1	Mar 2025	Initial draft — system context and overview	Architecture Team
0.5	Mar 2025	Added container and component sections	Architecture Team
0.9	Apr 2025	Security, deployment, and API sections	Architecture Team
1.0	Apr 2025	Final review and sign-off	Architecture Team




