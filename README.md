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

WordPress is a PHP-based Content Management System (CMS) used to create, manage, and publish website content through a web-based interface. This project studies WordPress from a software architecture perspective, focusing on its core structure rather than optional third-party integrations. The architecture of WordPress is based on a combination of styles, including client-server architecture, modular monolith, event-driven architecture through hooks, and plugin-based architecture similar to a microkernel style. The WordPress Core provides the main system services such as content management, user management, authentication, routing, theme loading, plugin execution, and database communication. Themes control the presentation layer, while plugins extend functionality without modifying the core system. WordPress stores its data in a MySQL/MariaDB relational data model, including posts, pages, users, comments, settings, and metadata. This document explains the context, stakeholders, key drivers, architecture views, data model, and major design decisions of WordPress CMS.

---

## Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Context Diagram and Description](#2-context-diagram-and-description)
- [3. Stakeholders and Their Concerns](#3-stakeholders-and-their-concerns)
- [4. Key Architecture Drivers](#4-key-architecture-drivers)
- [5. Architecture Styles Used in WordPress](#5-architecture-styles-used-in-wordpress)
- [6. Architecture Views Chosen](#6-architecture-views-chosen)
  - [6.1 Context View](#61-context-view)
  - [6.2 Logical / Module View](#62-logical--module-view)
  - [6.3 Plugin and Hooks View](#63-plugin-and-hooks-view)
  - [6.4 Deployment View](#64-deployment-view)
  - [6.5 Data Model View](#65-data-model-view)
- [7. Key Architecture Design Decisions](#7-key-architecture-design-decisions)
- [8. Relationships Between Architecture Design Decisions](#8-relationships-between-architecture-design-decisions)
- [9. Decision Relationship Diagram](#9-decision-relationship-diagram)
- [10. Improvements According to Course Comments](#10-improvements-according-to-course-comments)
- [11. Conclusion](#11-conclusion)

---

## 1. Project Overview

This project presents the software architecture of WordPress CMS. WordPress is not designed as a single simple application; instead, it combines multiple architectural styles to support flexibility, extensibility, and maintainability.

At a high level, WordPress follows a client-server model where users interact with the system through a web browser. The request is processed by a web server such as Apache or Nginx, which runs PHP and executes the WordPress Core. The WordPress Core acts as the central part of the system and coordinates themes, plugins, hooks, user roles, content management, media handling, and database access.

A major design principle of WordPress is that the core should not be changed directly. Instead, new features are added through plugins, and the website appearance is customized through themes. WordPress also uses an event-driven mechanism through hooks, including actions and filters, which allow plugins and themes to extend or modify behavior without touching the core code.

The data model is also an important part of the architecture. WordPress stores posts, pages, users, comments, settings, taxonomies, and metadata in a MySQL/MariaDB database. This relational data model supports flexible content management and allows plugins to store additional information when needed.

The purpose of this document is to explain WordPress architecture in a clear and structured way, including its context diagram, stakeholders, key drivers, selected architecture views, data model, and architecture design decisions.

---

## 2. Context Diagram and Description

### 2.1 Context Diagram

![WordPress CMS Context Diagram](diagrams/wordpress_cms_context_diagram.png)

### 2.2 Context Description

The context view shows WordPress CMS as the central system and explains how external people and external systems interact with it.

In this project, an **external person** means a human actor who directly uses or manages the WordPress system. Examples include site visitors, content authors, site administrators, and plugin developers.

An **external system** means a technical system or software environment that supports WordPress. Examples include the web browser, web server, PHP runtime, MySQL/MariaDB database, and email server. Optional third-party integrations such as payment gateways or analytics tools can be connected to WordPress through plugins, but they are not treated as part of the core WordPress architecture.

The main interactions are:

- Site visitors use a web browser to view published pages and posts.
- Content authors use the admin dashboard to create and edit content.
- Site administrators manage users, roles, themes, plugins, and settings.
- Plugin developers extend functionality using plugins and hooks.
- The web server executes WordPress Core using PHP.
- WordPress stores and retrieves data from MySQL/MariaDB.
- WordPress themes control the presentation of website content.
- WordPress plugins extend functionality without modifying the core system.

This context view helps define the boundary of the WordPress CMS and separates core architecture elements from optional integrations.

---

## 3. Stakeholders and Their Concerns

Stakeholders are people or groups who are interested in the WordPress CMS architecture. Each stakeholder has different concerns related to usability, performance, security, maintainability, extensibility, and reliability.

| Stakeholder | Description | Main Concerns |
|---|---|---|
| Site Visitor | A user who visits the public website to read pages or posts. | Fast page loading, easy navigation, responsive design, secure browsing. |
| Content Author | A user who creates and edits posts, pages, and media content. | Easy content editing, draft saving, media upload, publishing workflow. |
| Site Administrator | A user responsible for managing the WordPress website. | User roles, permissions, themes, plugins, updates, backups, and security. |
| Plugin Developer | A developer who creates plugins to extend WordPress functionality. | Stable hooks, clear extension points, compatibility, and safe plugin behavior. |
| Theme Developer | A developer or designer who creates WordPress themes. | Separation of content and presentation, template flexibility, maintainable design. |
| Website Owner | The person or organization that owns the website. | Low cost, reliability, maintainability, SEO support, business growth. |
| Hosting Provider | Provides server infrastructure for WordPress. | Server performance, uptime, PHP support, database support, and resource usage. |
| Security Team | Responsible for protecting the website from attacks. | Authentication, authorization, secure plugins, updates, backups, and monitoring. |

---

## 4. Key Architecture Drivers

Architecture drivers are the most important factors that influence architecture decisions. In WordPress, the main drivers come from both functional needs and quality requirements.

| Key Driver | Description | Architectural Impact |
|---|---|---|
| Easy Content Management | Users should be able to create, edit, and publish content without advanced programming skills. | Requires admin dashboard, editor, media library, and content management modules. |
| Extensibility | The system should support new features without modifying WordPress Core. | Requires plugin architecture and hooks such as actions and filters. |
| Customizable Presentation | Website appearance should be changeable without changing content or core logic. | Requires theme-based presentation layer. |
| Maintainability | WordPress should remain updateable and manageable over time. | Requires separation between core, themes, plugins, and database. |
| Security | User accounts, admin access, and content must be protected. | Requires authentication, role-based access control, input validation, secure updates, and plugin control. |
| Performance | Website pages should load efficiently for visitors. | Requires caching support, efficient PHP execution, optimized database access, and careful plugin usage. |
| Data Flexibility | WordPress must store different types of content and metadata. | Requires a flexible relational data model using posts, metadata, taxonomies, users, and options tables. |
| Usability | Non-technical users should be able to manage websites easily. | Requires simple admin dashboard, editor, media management, and role-based workflows. |

---

## 5. Architecture Styles Used in WordPress

WordPress uses a combination of architecture styles rather than only one single style.

### 5.1 Client-Server Architecture

WordPress follows client-server architecture because users access the system through a web browser. The browser sends HTTP/HTTPS requests to the web server. The server executes WordPress using PHP and returns generated HTML pages to the browser.

### 5.2 Modular Monolith Architecture

WordPress Core is deployed as one main application, but internally it is organized into modules such as posts, pages, comments, users, media, themes, plugins, and settings. This makes WordPress easier to deploy than a microservices system while still keeping internal responsibilities separated.

### 5.3 Plugin-Based / Microkernel-Like Architecture

WordPress uses a plugin-based architecture that is similar to a microkernel style. The WordPress Core provides essential services, while optional features are added through plugins. The core should not be modified directly. This improves maintainability and updateability.

### 5.4 Event-Driven Architecture through Hooks

WordPress uses hooks as an event-driven mechanism. Hooks allow plugins and themes to execute code at specific points in the WordPress lifecycle. There are two main types of hooks:

- **Actions:** allow custom code to run when a specific event occurs.
- **Filters:** allow data to be modified before it is displayed or stored.

This makes WordPress highly extensible without tightly coupling plugins to the core system.

---

## 6. Architecture Views Chosen

To describe the WordPress CMS architecture clearly, this document uses five architecture views.

| Architecture View | Purpose |
|---|---|
| Context View | Shows WordPress CMS, external people, and external systems. |
| Logical / Module View | Shows the main internal parts of WordPress such as Core, Admin Dashboard, Themes, Plugins, Hooks, and Database Access. |
| Plugin and Hooks View | Explains how plugins extend WordPress functionality using hooks without modifying the core. |
| Deployment View | Shows how WordPress runs on a web server with PHP and connects to MySQL/MariaDB. |
| Data Model View | Shows the main WordPress database tables and how data is stored. |

---

### 6.1 Context View

![WordPress CMS Context Diagram](diagrams/wordpress_cms_context_diagram.png)

The context view explains the system boundary of WordPress CMS. It shows the human actors who use the system and the technical systems that support it. This view is useful for understanding what is inside the WordPress system and what is outside the system boundary.

---

### 6.2 Logical / Module View

The logical/module view shows the major internal modules of WordPress.

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

    Themes --> Output[Website Presentation]
    Plugins --> Hooks
    Hooks --> Core




