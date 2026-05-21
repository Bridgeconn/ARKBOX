# ARKBOX — Functional specification (module-wise)

**Document type:** Functional / product documentation  
**Application:** ARKBOX Enterprise  
**Observed version (demo):** v2.2.101 (page title and login footer)  
**Demo base URL:** `https://demo.arkbox.cloud/`  
**Authentication:** Username + password (tenant credentials; store only in secure env such as `.env`, never commit secrets).

This document describes **observed UI behavior and controls** from the demo tenant. It does **not** include screenshots. Destructive actions (delete, clear-all) were not executed to completion; behavior should be validated in your environment.

---

## 1. Global application shell

### 1.1 Purpose

After sign-in, every module shares a **common header**, **primary navigation**, and **notification region** so users move between Dashboard, Resources, projects, vault, and administration without losing session context.

### 1.2 Header — controls and behavior

| Control | Purpose / behavior |
|--------|---------------------|
| **Check Out** (green) + **timer** | Appears as the primary time/session control next to a countdown (e.g. multi-hour window in demo). Label may alternate with **Check In** during load or state transitions; governs an active work or attendance-style session. |
| **Notifications (🔔 + count)** | Opens a **Tasks** popover: shows **running task count** and a message such as **“No running tasks.”** when idle. |
| **Switch to Dark mode** | Toggles light/dark theme for the UI. |
| **User identity pill** | Shows avatar initials, display name, and account source badge (e.g. **LDAP**). |
| **Logout** | Terminates the session and returns to authentication. |

### 1.3 Primary navigation

Horizontal links (active state highlighted, typically orange):

- **Dashboard** → `/`
- **Resources** → `/resources` (resolves to library or marketplace sub-routes)
- **Project Management** → `/projects`
- **Arkbox Vault** → `/vault`
- **Admin** → `/admin`

### 1.4 Accessibility / toasts

- **Region:** `Notifications Alt+T` — used for screen reader announcements and toast-style messages.

---

## 2. Authentication module

### 2.1 Route and purpose

- **URL:** `/login`
- **Purpose:** Validate user identity and issue a session before any protected route.

### 2.2 Page content

- **Marketing copy:** “Unified Enterprise Intelligence”, value props for team/assets/operations, scale, visibility, access.
- **Sign-in panel:** Heading **Sign in**, subtitle **Access the unified enterprise workspace.**

### 2.3 Form fields

| Field | Notes |
|-------|--------|
| **Username** | Text input; placeholder suggests format (e.g. `ark.admin`). |
| **Password** | Masked password input. |
| **Password visibility** | Icon button adjacent to password (unlabeled in tree): toggles show/hide of password characters. |

### 2.4 Actions

| Button / action | Behavior |
|-----------------|----------|
| **Sign in to ARKBOX** | Disabled until both fields satisfy client validation; on submit shows **Authenticating…**, disables inputs, then navigates to `/` on success. |

### 2.5 Footer

- Version string (e.g. **ARKBOX Enterprise v2.2.101**) and **Secure authentication gateway** line.

---

## 3. Dashboard module

### 3.1 Route and purpose

- **URL:** `/`
- **Purpose:** Landing hub: welcome, profile summary, system resource shortcuts, quick entry to Resources and Project Management, and per-project spotlight actions.

### 3.2 Welcome and profile

- **Welcome heading:** Personalized greeting including display name and product tagline (“An ARK with complete solutions”).
- **Profile card:** Gradient-ring avatar with initials; **full name**, **email**, **ACCOUNT** badge (e.g. **LDAP**).
- **Read-only fields:** At minimum **FULL NAME** and **EMAIL ID**; additional profile rows may appear below the fold.

### 3.3 System monitoring (modals)

| Trigger | Modal title | Contents (observed) |
|---------|-------------|---------------------|
| **Open CPU details** | CPU Monitor | Overall **CPU %**, progress bar, **Cores** count, **Uptime**; **temperature** strip with thresholds **Safe &lt; 60°C**, **Warn ≥ 60°C**, **Hot ≥ 75°C**; **load average** strip; footer **Updates every 2s**; **Close**. |
| **Open Memory details** | Memory Monitor | **RAM USAGE** (% , bar, used/total GB, uptime, **Source** e.g. `local_vm`); **SWAP USAGE**; scrollable **breakdown** (total/used style); **Updates every 2s**; **Close**. |

### 3.4 Resource summary shortcut

- **Control name (accessible):** Aggregates **remote** vs **downloaded local** resource counts and invites user to open **Resources** (e.g. “Resources Available Remote … Downloaded Local … Click to open Resources”).
- **Behavior:** Navigates to the Resources area.

### 3.5 Project Management shortcut

- **Open Project Management:** Navigates to `/projects`.

### 3.6 Project spotlight buttons

Each targets a specific project or aggregate view within Project Management context:

- **Show project summary**
- **Show Kurbaru**
- **Show Demo project 4**
- **Show demo project 3**

*(Exact routing/query behavior is inferred: quick-focus or drill-down into the named project or summary view.)*

---

## 4. Resources module

### 4.1 Routes and purpose

- **Base:** `/resources`
- **My Library:** `/resources/library` — downloaded items the user already has locally in-app.
- **Marketplace:** `/resources/marketplace` — browse remote **projects/repos** and ingest files into **Arkbox Vault**.

**Module subtitle:** *My Library shows downloaded items. Marketplace lets you discover and download resources.*

### 4.2 Shared chrome

- **Tabs:** **My Library** | **Marketplace** (tab role; one selected at a time).

---

### 4.3 Page: My Library

**Purpose:** Search, filter, download again or remove local library entries; return to project context.

| Control | Purpose |
|---------|---------|
| **Search…** | Free-text filter over library items. |
| **Clear All** | Bulk clear/remove library content (treat as destructive; confirm dialogs in production). |
| **All types** | Dropdown: **All types**, **Videos**, **Audio**, **Documents**. |
| **← Back to Projects** | Navigates back toward **Project Management** workflow. |
| **Per item: Download** | Re-download or sync behavior for that asset (confirm exact semantics with backend). |
| **Per item: Delete** | Removes the item from the user’s library (destructive). |
| **Content grid** | Thumbnail/card per resource. |

---

### 4.4 Page: Marketplace

**Purpose:** Discover content packaged in remote repositories and pull into vault-backed storage.

**Intro copy (observed):** Browse **projects (repos)** and download resources into **Arkbox Vault**.

| Control | Purpose |
|---------|---------|
| **Sync** | Refreshes the list of marketplace projects/repos from the server. |
| **Project list** | Folder-style rows (demo examples: `it-sops`, `arkbox-training-videos`); selecting a repo scopes the catalog below. |
| **Search marketplace…** | Text search across marketplace items. |
| **All types** | Same asset-type filter family as library. |
| **All** | Aggregate filter chip alongside marketplace listing. |
| **Download to Vault** (per item) | Downloads selected marketplace asset into **Arkbox Vault** (SMB-backed). |

---

## 5. Project Management module

### 5.1 Route and purpose

- **URL:** `/projects`
- **Purpose:** Portfolio of projects; create projects; open a project workspace with tabs for overview, assignments, project admin, languages, stages, and optional work plan.

### 5.2 Project list view

| Control | Purpose |
|---------|---------|
| **Open navigation menu** | Hamburger: toggles navigation drawer on constrained layouts. |
| **Create New Project** | Opens **Create New Project** modal (see §5.4). |
| **Per project: Open Project →** | Opens the **project workspace** for that project. |

**Typical project card fields (observed):**

- Project **name** and **code** badge (e.g. `DP-002`).
- **Status** chip (e.g. **OPEN**).
- **Type** pill (e.g. **Bible Translation**).
- **Date range** (planned start → end).
- **Progress** bar with percentage.

---

### 5.3 Create New Project (modal)

| Control | Purpose |
|---------|---------|
| **Close** / **Cancel** | Dismiss without saving. |
| **Create Project** | Submit new project. |

**Fields (labels as shown in UI):**

| Field | Required | Input notes |
|-------|----------|-------------|
| **Project Name** | Yes | Placeholder: enter project name. |
| **Project Code** | No | Optional short code; example placeholder style `PRJ-001`. |
| **Project Type** | Yes | Selector (type taxonomy). |
| **Project Manager** | Yes | Selector (user). |
| **Planned Start Date** | Yes | Date picker. |
| **Planned End Date** | Yes | Date picker. |
| **Comments** | No | Multi-line notes. |

---

### 5.4 Project workspace (single project)

**Context header**

| Control | Purpose |
|---------|---------|
| **← Back** | Return to project list. |
| **Delete** | Delete entire project (destructive). |
| **STATUS: Open ▼** | Dropdown to change lifecycle state; observed values include **Open**, **In Progress**, **Completed**. |
| **Breadcrumb** | e.g. `Projects / <Project name>`. |
| **Tags** | Code, type, role badge (e.g. **Project Admin**) as quick labels. |

**Ribbon tabs**

| Tab | Purpose |
|-----|---------|
| **Overview** | High-level project summary cards (e.g. PROJECT, CODE). May include **language** selector (e.g. **English (en) ▼**) — can be disabled depending on data/state. |
| **Assignments** | Work distribution; filters **All Stages ▾**, **Opened Only ▾**; row-level **Open ▾** menus; section labels **Stages:** and **View:**. |
| **Admin** | Project-scoped administration (distinct from global **Admin** module). Includes **Edit** for project-level settings/metadata. |
| **Languages** | **+ Add Language** — extend languages supported on the project. |
| **Stages** | **Select language… ▾**; **⚙ Manage Stages** / **Manage Stages** — configure stage pipeline; **Filter by Language:**. |
| **Work Plan** (suffix **OPTIONAL**) | Calendar-based planning: **+ Add New Plan**; navigation **Today**, **Back**, **Next**; **Month ▼** and **Year ▼**; view mode **Month** | **Agenda**; full calendar grid (weekday headers Sun–Sat); day cells are interactive for scheduling. |

---

## 6. Arkbox Vault module

### 6.1 Route and purpose

- **URL:** `/vault`
- **Purpose:** Secure **SMB** file access via **RemoteOps** agents; administrators configure shares and users; end users open **My Vault** folders.

**Module tag:** UI may show **PREVIEW** next to the title.

**Headline copy:** *Secure SMB-powered file access across your Arkbox agents.*

---

### 6.2 Configure Vault

**Section purpose:** *Link SMB shares to agents and set user access permissions.*

**Subsection: Available Agents**

- **Description:** *RemoteOps agents that can host Arkbox Vault SMB shares.*

| Control | Purpose |
|---------|---------|
| **Refresh** | Reloads agent list and status from server. |
| **Take Backup** | Initiates backup of vault configuration or associated data (confirm operational scope with runbooks). |
| **←** (back) | Navigates up in vault drill-down; **disabled** at top level. |

**Per-agent display (observed fields):**

- **AGENT HOSTNAME**
- **IP ADDRESS**
- **OS NAME** (e.g. distribution and version)
- **NUMBER OF SHARE**
- **NUMBER OF USERS**

- **Footer:** e.g. **Showing N agent(s)**.

---

### 6.3 My Vault

**Section purpose:** *Access your assigned vaults and SMB locations.*

| Control | Purpose |
|---------|---------|
| **Back** | Returns to previous vault navigation level when applicable. |
| **Vault tiles** | Named vaults (examples: **BcsVault**, **Test**, **TestShares1**) with connectivity/status badges (e.g. **OFFLINE**). |
| **Interaction model** | Copy in UI: *Click once to select a vault, click again to open it.* |

---

## 7. Admin module

### 7.1 Route and purpose

- **URL:** `/admin` (deep routes may exist, e.g. `/admin/groups`; prefer in-app navigation for consistent shell loading)
- **Purpose:** Tenant-wide configuration: directory, LDAP, backup topology, integrations, SMTP, performance, NTP, logs, HR-style reporting, and project taxonomy dictionaries.

### 7.2 Admin shell controls

| Control | Purpose |
|---------|---------|
| **☰** | Mobile / compact menu affordance. |
| **Collapse** | Collapses the admin sidebar. |
| **Expand** (repeated per group) | Expands a sidebar **section group** to show child links. |

---

### 7.3 Sidebar — Directory & authentication

| Item | Intended content |
|------|-------------------|
| **Users** | User accounts CRUD, search, export, pagination. |
| **Groups** | Security or distribution groups. |
| **Roles** | RBAC role definitions. |
| **LDAP** | Directory integration settings. |

---

### 7.4 Sidebar — Backup & storage cluster

*(Preceded by **Expand**)*

| Item | Intended content |
|------|-------------------|
| **Connections** | Connection definitions for backup or storage integrations. |
| **BackupEndPoints** | Registered backup destinations. |
| **Backups** | Backup jobs / history / restore entry points. |

---

### 7.5 Sidebar — Platform & messaging cluster

*(Preceded by **Expand**)*

| Item | Intended content |
|------|-------------------|
| **Connections** | Secondary connection family (distinct from backup cluster; e.g. app integrations). |
| **SMTP Settings** | Outbound mail server configuration. |
| **Performance (CPU/Mem)** | Host or service performance telemetry configuration/view. |
| **Time Settings (NTP)** | System clock / NTP configuration. |
| **Log Viewer** | Centralized log inspection. |

---

### 7.6 Sidebar — HR / reporting cluster

*(Preceded by **Expand**)*

| Item | Intended content |
|------|-------------------|
| **Leave Management** | Leave requests / balances / approvals (scope per deployment). |
| **Attendance Report** | Attendance reporting. |
| **Schedule Report** | Schedule-related reporting. |

---

### 7.7 Sidebar — Project taxonomy cluster

*(Preceded by **Expand**)*

| Item | Intended content |
|------|-------------------|
| **Type** | Project type dictionary used in **Project Type** fields. |
| **Status** | Status values used in project lifecycle. |
| **Stages** | Stage definitions for assignment/stage views. |
| **Settings** | Additional global project settings. |

---

### 7.8 Users screen (reference pattern for list admin pages)

**Toolbar**

| Control | Purpose |
|---------|---------|
| **+ Create User** | Opens flow to add a user. |
| **Export CSV** | Exports current result set to CSV. |
| **Refresh** | Reloads data from API. |

**Search**

- **Search users…** — filters the list.

**Bulk selection**

- **Select All** — header checkbox.
- **Row checkboxes** — per-user selection (accessibility tree may mark readonly; still used for bulk ops).
- **Selection count** — e.g. “N Selected”.

**Per-user row (card layout)**

- **Username** (emphasized), **Full name** subtitle.
- **Status** badge (e.g. **ACTIVE**).
- **EMAIL**
- **TYPE** (e.g. **LOCAL** vs directory-sourced).
- **Edit** — modify user.
- **Delete** — remove user (destructive).

**Pagination**

- **‹** previous, **›** next, numeric page buttons (**1**, **2**, …).

---

## 8. Cross-cutting behaviors

### 8.1 Session and navigation

- Direct navigation to deep URLs may briefly show transitional header labels (e.g. **Check In**) before the full shell hydrates; prefer primary nav links when documenting training materials.

### 8.2 Notifications and tasks

- **Tasks** popover is distinct from email/SMTP; it tracks **background/running tasks** for the current user or tenant job processor (wording: “running tasks”).

### 8.3 Theming

- **Switch to Dark mode** is global for the session UI state until toggled again.

---

