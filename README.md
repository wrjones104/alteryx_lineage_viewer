# Alteryx Data Lineage Visualizer

## 1. High-Level Goal

This is a self-contained, collaborative web tool designed to parse Alteryx workflow files (`.yxmd`) and build an interactive data lineage map. It allows a team to visualize all workflow inputs and outputs, see the relationships between them, and understand data dependencies across multiple processes. The entire application runs from a single HTML file and uses a shared Firebase backend for real-time collaboration.

## 2. Key Features & Decisions

* **Client-Side Parsing:** The tool uses JavaScript within the browser to parse the XML structure of Alteryx workflows. There is no server-side component, making the tool highly portable.
* **Shared Backend:** It uses **Google Firestore** as a real-time, cloud-based database. When one team member uploads or updates a workflow, the changes are instantly visible to everyone else using the tool.
* **Interactive Visualization:** Data lineage is rendered as a force-directed network graph using the **D3.js** library.
    * Workflows and data sources (Files, Databases, APIs) are represented as distinct nodes.
    * Arrows indicate the flow of data (input/output).
    * The graph is interactive: nodes can be dragged, and clicking a node opens an inspector panel.
* **Search & Navigation:** Both the Report View and Graph View have search functions. The graph's search allows users to instantly "snap" to and highlight a specific node.
* **Data Source Aliasing:** Users can click on any data source node to provide a user-friendly alias (e.g., renaming a cryptic connection string to "Centric CRM Source"). This alias is saved centrally and used in all views.
* **Workflow Updates:** Re-uploading a workflow with an existing name will automatically delete its old connections and save the new ones, ensuring the lineage map always reflects the latest version.
* **Handling Complex Tools (Annotation Override):** For complex tools like the Python tool or Download tool where I/O is not explicit in the XML, the parser supports a standardized annotation block. By adding a `--- lineage ---` block to any tool's annotation, users can manually define its inputs and outputs, giving them full control over the lineage documentation.

## 3. Setup & Dependencies

This tool is designed to be run from a single HTML file with minimal setup.

### A. Firebase Project Setup

A Google Firebase project is required to act as the central database.

1.  **Create a Project:** Go to the [Firebase Console](https://console.firebase.google.com/). You can either create a new project or add Firebase to an existing Google Cloud project.
2.  **Enable Firestore:** In the Firebase console, navigate to the "Build" section and enable the **Firestore Database**. You can start it in "test mode" for initial setup.
3.  **Enable Authentication:** Navigate to the "Authentication" section. On the "Sign-in method" tab, find the **Anonymous** provider and enable it.

### B. HTML File Configuration

1.  **Get Config Keys:** In your Firebase project settings, find your web app's configuration details. It will be a JavaScript object named `firebaseConfig`.
2.  **Paste into HTML:** Open the `Alteryx Data Lineage Visualizer.html` file. At the top of the script section, you will find a placeholder `firebaseConfig` object. Paste your project's keys there.

### C. External Libraries

The tool uses the following libraries, which are loaded via CDN links in the HTML file, so no local installation is needed:

* TailwindCSS
* D3.js
* Firebase SDK
