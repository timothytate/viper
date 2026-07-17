# VIPER

**VIPER** stands for **Visual Infrastructure Picture & Endpoint Response**. It is a lightweight, browser-based common operating picture (COP) for documenting and visualizing cyber threat-hunting or incident-response activity across a network map.

VIPER is designed to help analysts preserve investigation context by connecting infrastructure, attack steps, MITRE ATT&CK techniques, event details, indicators, and narrative notes in one visual workspace.

## What VIPER Does

VIPER provides a single-page interface for building and reviewing an investigation timeline against a network map. It can be used to:

- Build or import a network topology.
- Add hosts, routers, firewalls, servers, OT devices, cloud/external nodes, and other infrastructure.
- Document attack steps with dates, times, MITRE tactics, techniques, affected nodes, narrative notes, and raw indicators.
- Visualize activity as local execution, discovery, internal movement, staging, external command and control, exfiltration, or remote action.
- Record event-specific details such as process, network, file, authentication, account, service, registry, and process injection data.
- Review activity through a map-based playback view or a chronological ledger view.
- Create investigation reports with observables and tags.
- Export the investigation as a VIPER project JSON file.
- Export a STIX 2.1-style bundle for downstream analysis or sharing.

## Why This Tool Exists

VIPER was created as part of a threat-hunting research project examining whether a structured case-management workflow and a common operating picture can improve analyst situational awareness, indicator identification, and incident scoping compared to traditional unstructured notes.

The tool is not intended to replace a SIEM, packet-capture platform, EDR, or case-management system. Instead, it acts as a lightweight visual layer for preserving what the analyst learned, how events relate to infrastructure, and how the incident progressed over time.

## Repository Contents

Recommended repository layout:

```text
.
├── VIPER_v0.2.html
├── day4research_VIPER_Project.json
├── day6research_VIPER_Project.json
└── README.md
```

### `VIPER_v0.2.html`

The main VIPER application. It is a standalone HTML file that can be opened directly in a browser or hosted with GitHub Pages.

### `day4research_VIPER_Project.json`

A VIPER demo project based on the Day 4 research scenario. This file is already formatted for VIPER import and contains:

- 38 infrastructure nodes.
- 40 documented attack steps.
- A June 19, 2026 scenario timeline.
- Activity including initial Sandcat RAT execution, remote system discovery, lateral movement, and additional remote actions across multiple hosts.
- MITRE ATT&CK references such as `T1018`, `T1021.002`, `T1550.003`, `T1569.002`, `T1059.003`, and `T1071.001`.

### `day6research_VIPER_Project.json`

A VIPER demo project based on the Day 6 research scenario. This file is already formatted for VIPER import and contains:

- 38 infrastructure nodes.
- 29 documented attack steps.
- A June 26, 2026 scenario timeline.
- Activity including Sandcat RAT initial access, SMB-based tool transfer, lateral movement, service creation for persistence, and process injection.
- Network, service, and process-injection event records.

These JSON files are **VIPER project files**, not raw CALDERA full-report exports. They were prepared so they can be loaded directly into VIPER through the **Import Project** button.

## Quick Start

### Option 1: Open Locally

1. Download or clone this repository.
2. Open `VIPER_v0.2.html` in a modern browser such as Chrome, Edge, or Firefox.
3. Use **Import Project** to load one of the demo JSON files.

No server, database, or installation is required.

### Option 2: Host With GitHub Pages

1. Push `VIPER_v0.2.html` and the demo JSON files to a GitHub repository.
2. In GitHub, go to **Settings** > **Pages**.
3. Select the branch and folder that contain the HTML file.
4. Open the published GitHub Pages URL in a browser.
5. Use **Import Project** to load a demo JSON file from your local machine.

## How to Import a Demo Project

1. Open `VIPER_v0.2.html` in your browser.
2. Click **Import Project** in the top-right menu.
3. Select either:
   - `day4research_VIPER_Project.json`
   - `day6research_VIPER_Project.json`
4. VIPER will load the infrastructure map and attack timeline.
5. Use **Map View** to replay the activity visually.
6. Use **Previous Step** and **Next Step** to walk through the attack sequence.
7. Use **Ledger View** to review all steps in chronological order.
8. Use **Export Project** if you make changes and want to save a new copy.

Importing a project replaces the currently loaded workspace in the browser. Export your current project before importing another one if you want to preserve your changes.

## Basic Workflow

### 1. Build Infrastructure

1. Open the **Infrastructure** tab.
2. Add a hostname and primary IP address.
3. Optionally add:
   - Other IPs or aliases.
   - Device type/icon.
   - Operating system.
   - Associated CVEs.
   - Device notes.
4. Click **Save / Add Node**.

### 2. Add Attack Steps

1. Open the **Attack Steps** tab.
2. Add a step title.
3. Enter the date and time.
4. Select a MITRE tactic.
5. Enter the technique or action, such as `T1021.002` or `SMB Share Tool Transfer`.
6. Select a primary node and, when relevant, a secondary node.
7. Select the action visual type.
8. Select an event type and complete the event-specific fields.
9. Add a description or narrative note.
10. Optionally add a raw log or indicator.
11. Click **Add Step**.

### 3. Review the Scenario

Use the main navigation controls:

- **Map View**: Displays the network map and step-by-step visual activity.
- **Ledger View**: Displays the investigation steps as a chronological list.
- **Reports**: Provides a workspace for analyst notes, observables, tags, and markdown export.
- **Previous Step / Next Step**: Walks through the attack sequence.
- **Reset View**: Resets the map zoom and pan position.

### 4. Create Reports

1. Click **Reports**.
2. Click **Create Report**.
3. Add a report title.
4. Add observables or tags such as IPs, hostnames, users, or indicators.
5. Write the report in the living document editor.
6. Use **Download Doc (.md)** to export the report as Markdown.

Reports can be tagged with observables so they can be associated with related infrastructure nodes.

### 5. Export Your Work

Use **Export Project** to save the current VIPER workspace as a JSON file. This file can be imported again later with **Import Project**.

Use **Export STIX** to generate a STIX 2.1-style JSON bundle. VIPER maps infrastructure nodes, IP addresses, vulnerabilities, observed data, and relationships into STIX-style objects for external review or experimentation.

## VIPER Project JSON Format

VIPER project files use a simple JSON structure:

```json
{
  "nodes": [],
  "steps": [],
  "reports": []
}
```

### Nodes

Nodes represent infrastructure on the map. A node can include fields such as:

```json
{
  "id": "node_123",
  "host": "example-host",
  "ip": "10.0.0.10",
  "os": "Windows 10",
  "icon": "icon-laptop",
  "x": "50",
  "y": "50",
  "other_ips": [],
  "cves": [],
  "notes": ""
}
```

### Steps

Steps represent analyst-documented activity in the scenario timeline. A step can include fields such as:

```json
{
  "id": "step_123",
  "title": "SMB Share Tool Transfer",
  "date": "2026-06-26",
  "time": "15:15:44",
  "tactic": "Lateral Movement",
  "action": "T1021.002",
  "desc": "Tool transfer over SMB",
  "raw": "",
  "nodes": ["source_node_id", "destination_node_id"],
  "type": "remote_action",
  "eventType": "Network",
  "eventData": {
    "SrcIP": "10.0.0.10",
    "DstIP": "10.0.0.20",
    "Port": "445",
    "Protocol": "SMB"
  }
}
```

### Reports

Reports represent analyst notes and can include titles, tags, HTML content, and lock status:

```json
{
  "id": "report_123",
  "title": "Incident Summary",
  "tags": ["10.0.0.10", "example-host"],
  "content": "<p>Analyst notes go here.</p>",
  "isLocked": false
}
```

## Demo Ideas

### Day 4 Demo

Load `day4research_VIPER_Project.json`, then use **Next Step** to walk through the scenario from the initial Sandcat RAT activity on `hr-wks-02` through discovery, lateral movement, and follow-on execution across multiple hosts. Use **Ledger View** to show how the same incident can be reviewed as a structured timeline rather than only as map animation.

### Day 6 Demo

Load `day6research_VIPER_Project.json`, then focus on how the tool represents SMB transfer, service creation, persistence, and process injection. This demo is useful for showing how event-specific fields preserve details that are easy to lose in unstructured notes.

## Notes and Limitations

- VIPER is a standalone browser tool.
- It does not require a backend database.
- It does not provide authentication or multi-user synchronization.
- Browser refreshes may clear unsaved work, so use **Export Project** often.
- The included JSON files contain lab scenario data, hostnames, IP addresses, and attack-step details. Review and sanitize them before publishing publicly if needed.
- The current demo projects include no prebuilt reports; use the **Reports** view to create your own demo notes.

## Suggested Use Cases

- Threat-hunting exercise documentation.
- Incident-response tabletop or range exercise visualization.
- Analyst training and after-action review.
- Research demonstrations comparing unstructured notes with structured COP-based analysis.
- Converting scenario findings into a shareable project record.

## License

No license is specified by default. If you intend others to use, modify, or redistribute VIPER, add a license file to the repository.
