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
- [7. Architecture Design Decisions](#7-architecture-design-decisions)
- [8. Conclusion](#8-conclusion)

---

## 1. Project Overview

This project presents the software architecture of WordPress CMS. WordPress is not designed as a single simple application; instead, it combines multiple architectural styles to support flexibility, extensibility, and maintainability.

At a high level, WordPress follows a client-server model where users interact with the system through a web browser. The request is processed by a web server such as Apache or Nginx, which runs PHP and executes the WordPress Core. The WordPress Core acts as the central part of the system and coordinates themes, plugins, hooks, user roles, content management, media handling, and database access.

A major design principle of WordPress is that the core should not be changed directly. Instead, new features are added through plugins, and the website appearance is customized through themes. WordPress also uses an event-driven mechanism through hooks, including actions and filters, which allow plugins and themes to extend or modify behavior without touching the core code.

The data model is also an important part of the architecture. WordPress stores posts, pages, users, comments, settings, taxonomies, and metadata in a MySQL/MariaDB database. This relational data model supports flexible content management and allows plugins to store additional information when needed.

The purpose of this document is to explain WordPress architecture in a clear and structured way, including its context diagram, stakeholders, key drivers, selected architecture views, data model, and architecture design decisions.




