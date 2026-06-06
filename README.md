<div align="center">

# WordPress CMS Architecture Document

## Group Members

| # | Name | Student ID |
| :---: | :--- | :--- |
| 01 | **Umar Ajaib** | 2025272110001 |
| 02 | **Aqsa Fakhar Uddin** | 2025272110002 |

</div>

---

## Project Description

WordPress is a PHP-based Content Management System (CMS). It is used to create, manage, and publish website content such as posts, pages, images, comments, and user accounts. This project explains WordPress from a software architecture point of view.

The main focus of this document is the core WordPress architecture. WordPress is built around the WordPress Core, which handles content management, users, roles, themes, plugins, hooks, and database communication. WordPress follows more than one architecture style. It uses client-server architecture, modular monolith architecture, plugin-based architecture, and event-driven architecture through hooks.

A key idea in WordPress is that the core system should not be changed directly. Extra features are added through plugins, and the website design is changed through themes. WordPress stores its data in a MySQL or MariaDB database. This document explains the context, stakeholders, key drivers, architecture views, data model, design decisions, and relationships between design decisions.

---

## Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Context Diagram and Description](#2-context-diagram-and-description)
- [3. Stakeholders and Their Concerns](#3-stakeholders-and-their-concerns)
- [4. Key Architecture Drivers](#4-key-architecture-drivers)
- [5. Architecture Styles Used in WordPress](#5-architecture-styles-used-in-wordpress)
- [6. Architecture Views Chosen](#6-architecture-views-chosen)
- [7. Key Architecture Design Decisions](#7-key-architecture-design-decisions)
- [8. Relationships Between Architecture Design Decisions](#8-relationships-between-architecture-design-decisions)
- [9. Decision Relationship Diagram](#9-decision-relationship-diagram)
- [10. Conclusion](#10-conclusion)

---

# 1. Project Overview

WordPress is a web-based CMS. It allows users to build and manage websites without advanced programming knowledge. A user can create pages, write blog posts, upload images, manage comments, change themes, and install plugins from the admin dashboard.

WordPress works in a simple flow:

1. A user opens a WordPress website in a web browser.
2. The browser sends a request to the web server.
3. The web server runs PHP.
4. PHP executes the WordPress Core.
5. WordPress Core loads themes, plugins, hooks, and required data.
6. WordPress gets content from the MySQL/MariaDB database.

The most important parts of WordPress architecture are:

| Part | Simple Meaning |
|---|---|
| WordPress Core | The main system or brain of WordPress. |
| Themes | Control the design and layout of the website. |
| Plugins | Add extra features without changing the core. |
| Hooks | Allow plugins and themes to connect with WordPress events. |
| MySQL/MariaDB Database | Stores posts, pages, users, comments, settings, and metadata. |
| Admin Dashboard | Allows users to manage website content and settings. |
| Web Server | Runs WordPress using PHP and serves pages to users. |

The main goal of this document is to explain how WordPress is structured, why this architecture is useful, and what major design decisions are involved.

---

# 2. Context Diagram and Description

## 2.1 Context Diagram

![WordPress CMS Context Diagram](Context%20Diagram.png)

## 2.2 Context Description

The context diagram shows WordPress CMS as the main system. It also shows the people and systems that interact with WordPress.

In this project, we separate two things:

| Type | Meaning | Examples |
|---|---|---|
| External Person | A human user who interacts with WordPress. | Site Visitor, Content Author, Site Administrator, Plugin Developer |
| External System | A technical system that supports WordPress. | Web Browser, Web Server, PHP Runtime, MySQL/MariaDB Database, Email Server |

The main users of WordPress are:

- **Site Visitor:** reads pages and posts on the public website.
- **Content Author:** creates and edits posts, pages, and media.
- **Site Administrator:** manages users, themes, plugins, settings, and security.
- **Plugin Developer:** creates plugins to add new functionality.

The main technical systems are:

- **Web Browser:** sends user requests and displays the final website.
- **Web Server:** receives requests and runs WordPress.
- **PHP Runtime:** executes WordPress code.
- **MySQL/MariaDB Database:** stores WordPress data.
- **Email Server:** sends emails such as password reset or notifications.

Optional integrations such as payment gateways, analytics tools, or SEO tools can be added through plugins. However, they are not part of the core WordPress architecture.

---

# 3. Stakeholders and Their Concerns

Stakeholders are people or groups who care about the WordPress system. Each stakeholder has different concerns.

| Stakeholder | Role in WordPress | Main Concerns |
|---|---|---|
| Site Visitor | Opens the website and reads content. | Website should be fast, easy to use, and secure. |
| Content Author | Creates and edits posts/pages. | Content editing should be simple and user-friendly. |
| Site Administrator | Manages the whole website. | Needs control over users, plugins, themes, backups, and security. |
| Plugin Developer | Builds plugins for extra features. | Needs clear hooks, plugin compatibility, and safe extension points. |
| Theme Developer | Builds website design. | Needs separation between content and presentation. |
| Website Owner | Owns the website or business. | Wants low cost, reliability, easy updates, and business growth. |
| Hosting Provider | Provides server environment. | Needs stable server, PHP support, database support, and good performance. |
| Security Team | Protects the website. | Needs secure login, role control, updates, backups, and monitoring. |

---

# 4. Key Architecture Drivers

Architecture drivers are the important reasons that influence architecture decisions.

| Key Driver | Easy Explanation | Impact on Architecture |
|---|---|---|
| Easy Content Management | Users should create and update content easily. | Requires admin dashboard, editor, media library, posts, and pages. |
| Extensibility | New features should be added without changing the core. | Requires plugins and hooks. |
| Customizable Design | Website look should be flexible and adaptable for different brands. | Requires themes, templates, and styling support. |
| Security | User accounts, roles, and admin area must be protected. | Requires login controls, permissions, secure plugins, and update mechanisms. |
| Performance | Website should load quickly and handle multiple users efficiently. | Requires optimized PHP execution, database queries, caching, and careful plugin selection. |

# 5. Architecture Styles Used in WordPress

WordPress does not use only one architecture style. It combines multiple architecture styles.

## 5.1 Client-Server Architecture

WordPress uses client-server architecture. The user uses a web browser as the client. The WordPress website runs on a server. The browser sends requests, and the server returns web pages.

Simple flow:

```text
Browser → Web Server → PHP → WordPress Core → Database → Web Page
```

## 5.2 Modular Monolith Architecture

WordPress Core is deployed as one main application. However, inside the application, different modules handle different responsibilities.

Examples of modules:

- Posts
- Pages
- Users
- Comments
- Media
- Themes
- Plugins
- Settings

This is called a modular monolith because it is one application but internally divided into modules.

## 5.3 Plugin-Based / Microkernel-Like Architecture

WordPress Core provides the basic services. Extra features are added through plugins. This is similar to microkernel architecture because the core remains small and stable, while plugins add extra functionality.

Example:

- Contact form plugin adds forms.
- SEO plugin adds SEO features.
- Security plugin adds protection.
- WooCommerce plugin adds e-commerce features.

These features are not built directly into the core.

## 5.4 Event-Driven Architecture through Hooks

WordPress uses hooks to allow plugins and themes to interact with the core.

There are two main types of hooks:

| Hook Type | Meaning |
|---|---|
| Actions | Run custom code when a specific event happens. |
| Filters | Modify data before it is displayed or saved. |

Example:

A plugin can use a hook to add extra text at the end of a blog post without changing WordPress Core.

---

# 6. Architecture Views Chosen

## 6.1 Use Case View

The use case view explains the main functions of WordPress from the users’ perspective. It shows what different actors can do in the system.

Main actors include:

- Site Visitor
- Content Author
- Site Administrator
- Plugin Developer
- Website Owner

Main use cases include:

- View website content
- Create and edit posts/pages
- Upload and manage media
- Manage users and roles
- Install and configure themes
- Install and configure plugins
- Extend functionality through hooks
- Manage website settings and security

```mermaid
flowchart LR
    Visitor[Site Visitor]
    Author[Content Author]
    Admin[Site Administrator]
    PluginDev[Plugin Developer]
    Owner[Website Owner]

    WP[WordPress CMS]

    Visitor -->|View pages and posts| WP
    Author -->|Create and edit content| WP
    Author -->|Upload media| WP
    Admin -->|Manage users and roles| WP
    Admin -->|Install themes and plugins| WP
    Admin -->|Manage settings and security| WP
    PluginDev -->|Develop plugins using hooks| WP
    Owner -->|Monitor website value and growth| WP
```

---

## 6.2 Context View

![WordPress CMS Context Diagram](Context%20Diagram.png)

The context view shows the boundary of the WordPress system. It explains which users interact with WordPress and which technical systems support it.

---

## 6.3 Logical / Module View

The logical view shows the internal parts of WordPress.

```mermaid
flowchart TD
    User[User / Browser] --> WebServer[Web Server: Apache or Nginx]
    WebServer --> PHP[PHP Runtime]
    PHP --> Core[WordPress Core]

    Core --> Admin[Admin Dashboard]
    Core --> Content[Content Management]
    Core --> Users[User and Role Management]
    Core --> Media[Media Management]
    Core --> Themes[Theme System]
    Core --> Plugins[Plugin System]
    Core --> Hooks[Hooks: Actions and Filters]
    Core --> DB[(MySQL / MariaDB Database)]

    Themes --> Output[Website Design / Presentation]
    Plugins --> Hooks
    Hooks --> Core
```

Simple explanation:

- The browser sends a request.
- The web server runs PHP.
- PHP loads WordPress Core.
- WordPress Core communicates with themes, plugins, hooks, and database.
- The final page is displayed to the user.

---

## 6.4 Plugin and Hooks View

This view explains how WordPress can be extended without modifying the core.

```mermaid
flowchart LR
    Core[WordPress Core] --> HookSystem[Hook System]
    HookSystem --> Actions[Actions]
    HookSystem --> Filters[Filters]

    PluginA[Plugin A] --> Actions
    PluginB[Plugin B] --> Filters
    Theme[Theme] --> Actions
    Theme --> Filters

    Actions --> Event[Run extra code at events]
    Filters --> Modify[Modify content or data]
```

Simple explanation:

Plugins and themes do not need to change WordPress Core. They connect to WordPress through hooks. This makes WordPress flexible and maintainable.

---

## 6.5 Deployment View

![WordPress CMS Deployment Diagram](./Deployment%20Diagram.png)

The deployment view shows how WordPress runs in a real hosting environment.

| Deployment Element | Responsibility |
|---|---|
| Web Browser | Used by visitors and admins to access WordPress. |
| Web Server | Handles HTTP/HTTPS requests. |
| PHP Runtime | Executes WordPress PHP code. |
| WordPress Core | Runs main CMS logic. |
| MySQL/MariaDB Database | Stores content, users, comments, settings, and metadata. |
| wp-content Folder | Stores themes, plugins, and uploaded files. |

Simple deployment flow:

```text
User Browser
   ↓
Web Server
   ↓
PHP Runtime
   ↓
WordPress Core
   ↓
MySQL/MariaDB Database
```

---

## 6.6 Data Model View

The data model is a key architecture design decision in WordPress. WordPress uses a relational database model with MySQL or MariaDB.

```mermaid
erDiagram
    wp_users ||--o{ wp_posts : writes
    wp_users ||--o{ wp_comments : creates
    wp_posts ||--o{ wp_postmeta : has
    wp_posts ||--o{ wp_comments : receives
    wp_posts ||--o{ wp_term_relationships : categorized_by
    wp_terms ||--o{ wp_term_taxonomy : defines
    wp_term_taxonomy ||--o{ wp_term_relationships : links
    wp_users ||--o{ wp_usermeta : has
```

Important database tables:

| Table | Easy Explanation |
|---|---|
| `wp_posts` | Stores posts, pages, revisions, and media attachments. |
| `wp_postmeta` | Stores extra information about posts and pages. |
| `wp_users` | Stores website users. |
| `wp_usermeta` | Stores extra user information and roles. |
| `wp_comments` | Stores comments. |
| `wp_options` | Stores website settings. |
| `wp_terms` | Stores categories and tags. |
| `wp_term_taxonomy` | Defines category/tag type. |
| `wp_term_relationships` | Links posts with categories and tags. |

This data model is flexible because WordPress can store different types of content using common tables.

---

# 7. Key Architecture Design Decisions

This section documents the main architecture decisions in WordPress. Each decision includes clear **Issue**, **Importance**, **Decision**, **Arguments**, **Positive Impact**, and **Negative Impact** for clarity.

---

## ADD-01: Use Client-Server Architecture

| Template Item | Description |
|---|---|
| Issue | How should users access WordPress CMS? |
| Importance | Visitors, content authors, and administrators need reliable, cross-platform web access. |
| Decision | Use a client-server architecture: the browser acts as the client, and WordPress runs on the server. |
| Arguments | This approach allows users to access WordPress from any device without installing software. Requests are processed securely on the server, separating frontend access from backend logic. |
| Positive Impact | Easy access from multiple devices, simplifies deployment and maintenance, supports scalability for small to medium sites. |
| Negative Impact | Server downtime or network issues can prevent access; performance depends on server configuration and network speed. |

---

## ADD-02: Use Modular Monolith Architecture

| Template Item | Description |
|---|---|
| Issue | Should WordPress be structured as one application or multiple independent services? |
| Importance | Ensures maintainability and simplifies deployment. |
| Decision | Deploy WordPress as a modular monolith: one application with internal modules for posts, users, comments, media, themes, plugins, and settings. |
| Arguments | A monolith keeps deployment simple while separating logical responsibilities internally. Easier for administrators to maintain one cohesive system. |
| Positive Impact | Simplifies deployment, maintainable, modules are internally separated for clarity. |
| Negative Impact | Cannot scale individual modules independently; a failure in one module may impact the whole system. |

---

## ADD-03: Keep WordPress Core Separate from Extensions

| Template Item | Description |
|---|---|
| Issue | How can new features be added safely without affecting WordPress Core? |
| Importance | Maintaining system stability and updateability. |
| Decision | Do not modify WordPress Core directly. Add new features via plugins and themes. |
| Arguments | Separating core and extensions ensures updates do not break customizations and preserves stability. |
| Positive Impact | Maintains core integrity, safer updates, extensibility through plugins/themes. |
| Negative Impact | Poorly coded plugins/themes can still introduce performance, security, or compatibility issues. |

---

## ADD-04: Use Plugin-Based Architecture

| Template Item | Description |
|---|---|
| Issue | How can WordPress support optional features for different websites? |
| Importance | Websites require different functionalities such as SEO, forms, analytics, or e-commerce. |
| Decision | Use plugin-based architecture to allow optional features without modifying core. |
| Arguments | Plugins provide flexibility, customization, and isolation of features without affecting the core system. |
| Positive Impact | Increases flexibility and functionality; users can install only needed features. |
| Negative Impact | Excessive plugins can reduce performance, create conflicts, or introduce vulnerabilities. |

---

## ADD-05: Use Hooks for Event-Driven Extension

| Template Item | Description |
|---|---|
| Issue | How can plugins safely interact with WordPress Core? |
| Importance | Ensures modular extensions can execute code without modifying the core. |
| Decision | Implement hooks (actions and filters) for event-driven communication. |
| Arguments | Hooks allow extensions to react to events and modify behavior safely and flexibly. |
| Positive Impact | Supports extensibility and maintainability, reduces direct code changes in the core. |
| Negative Impact | Poorly written hooks can make debugging difficult and may affect performance. |

---

## ADD-06: Use Theme-Based Presentation Layer

| Template Item | Description |
|---|---|
| Issue | How should website design be managed? |
| Importance | Design must be flexible without changing content or core logic. |
| Decision | Use WordPress themes for layout, templates, and styling. |
| Arguments | Themes separate content from presentation, allowing easy design updates and brand customization. |
| Positive Impact | Improves modifiability, usability, and aesthetic flexibility. |
| Negative Impact | Poorly coded themes can impact performance, accessibility, and security. |

---

## ADD-07: Use MySQL/MariaDB Data Model

| Template Item | Description |
|---|---|
| Issue | How should WordPress store content, users, and metadata? |
| Importance | Database design affects performance, maintainability, and flexibility. |
| Decision | Use relational database (MySQL/MariaDB) as standard WordPress storage. |
| Arguments | Relational DB provides structured storage, easy queries, and consistent data relationships. |
| Positive Impact | Efficient data storage, supports content, users, and plugin metadata; reliable and scalable. |
| Negative Impact | Poorly optimized queries or too much metadata can degrade performance. |

---

## ADD-08: Use Role-Based Access Control

| Template Item | Description |
|---|---|
| Issue | How should WordPress control user permissions securely? |
| Importance | Users have different responsibilities; proper access control is critical for security. |
| Decision | Implement RBAC with predefined roles: Admin, Editor, Author, Contributor, Subscriber. |
| Arguments | RBAC ensures each user has only necessary permissions, reducing risks of unauthorized actions. |
| Positive Impact | Enhances security, workflow control, and accountability. |
| Negative Impact | Misconfigured roles or insecure plugins can still pose security threats. |

---

# 8. Relationships Between Architecture Design Decisions

Architecture decisions in WordPress are not separate from each other. Each decision supports, depends on, or constrains another decision. These relationships help explain how the overall WordPress architecture works as one complete system.

| Relationship ID | Decision A | Relationship | Decision B | Explanation |
|---|---|---|---|---|
| REL-01 | ADD-01 Client-Server Architecture | Enables | ADD-02 Modular Monolith Architecture | Because WordPress runs on a server, it can be deployed as one server-side application with internal modules. |
| REL-02 | ADD-02 Modular Monolith Architecture | Supports | ADD-03 Core-Extension Separation | Since WordPress has one central core, plugins and themes are kept separate to protect the stability of the core system. |
| REL-03 | ADD-03 Core-Extension Separation | Enables | ADD-04 Plugin-Based Architecture | Because the core should not be modified directly, plugins are used to add new features safely. |
| REL-04 | ADD-04 Plugin-Based Architecture | Depends On | ADD-05 Hooks for Event-Driven Extension | Plugins need hooks to communicate with WordPress Core and run functionality at specific events. |
| REL-05 | ADD-05 Hooks for Event-Driven Extension | Supports | ADD-03 Core-Extension Separation | Hooks allow plugins and themes to extend WordPress without changing the core files. |
| REL-06 | ADD-06 Theme-Based Presentation Layer | Supports | ADD-03 Core-Extension Separation | Themes manage design and layout separately, so visual changes do not affect WordPress Core logic. |
| REL-07 | ADD-07 MySQL/MariaDB Data Model | Supports | ADD-04 Plugin-Based Architecture | Plugins can store extra settings, metadata, and configuration data using WordPress database tables. |
| REL-08 | ADD-08 Role-Based Access Control | Constrains | ADD-04 Plugin-Based Architecture | Plugins must follow WordPress user roles and permissions to avoid unauthorized access and security risks. |

These relationships show that WordPress architecture is connected and balanced. The client-server model allows WordPress to run as a server-side application. The modular monolith structure keeps the system simple, while core-extension separation protects the main WordPress Core. Plugins and hooks provide extensibility, themes handle presentation, the database supports content and plugin data, and role-based access control protects the system from unauthorized actions.

---

# 9. Decision Relationship Diagram

```mermaid
graph TD
    A[ADD-01 Client-Server Architecture]
    B[ADD-02 Modular Monolith Architecture]
    C[ADD-03 Core-Extension Separation]
    D[ADD-04 Plugin-Based Architecture]
    E[ADD-05 Hooks for Event-Driven Extension]
    F[ADD-06 Theme-Based Presentation Layer]
    G[ADD-07 MySQL/MariaDB Data Model]
    H[ADD-08 Role-Based Access Control]

    A -->|REL-01 enables| B
    B -->|REL-02 supports| C
    C -->|REL-03 enables| D
    D -->|REL-04 depends on| E
    E -->|REL-05 supports| C
    F -->|REL-06 supports| C
    G -->|REL-07 supports| D
    H -->|REL-08 constrains| D
```

This diagram shows the relationship between the eight architecture design decisions. Each relationship explains how one decision affects another decision in the WordPress architecture.

---

# 10. Conclusion

This document explains the architecture of WordPress CMS in a simple and structured way. It focuses on the real WordPress core architecture instead of optional third-party integrations.

WordPress works as a PHP-based CMS using client-server architecture, modular monolith structure, plugin-based extension, event-driven hooks, theme-based presentation, and MySQL/MariaDB data model.

The most important design idea is that WordPress Core should remain stable and should not be modified directly. Themes are used for design, and plugins are used for extra features. Hooks allow plugins and themes to connect with the core without changing it.

The key architecture decisions and their relationships show how WordPress supports flexibility, maintainability, extensibility, usability, and security.




