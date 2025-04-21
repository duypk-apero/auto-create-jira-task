# Jira Issue Creation Guide

This guide explains how to use the `Create Epic` and `Create Task` files to automatically create Jira Epics and Tasks in your project based on data from a Google Sheet, using the AI assistant within Cursor.

## Prerequisites

*   Access to the Google Sheet containing the task data.
*   A configured Atlassian MCP tool within Cursor (see Configuration section below).
*   The Cursor AI assistant.

## Configuration (Cursor MCP Setup)

For the AI assistant to interact with your Jira instance, you need to configure the Atlassian MCP tool within Cursor. This usually involves editing a JSON configuration file used by Cursor (often named `mcp.json` or similar within Cursor's settings/data directory).

Add or update the section for `"atlassian MCP"` with your Jira details:

```json
{
  "atlassian MCP": {
      "command": "uvx", // Or potentially "pipx" or similar based on your setup
      "args": [
        "mcp-atlassian",
        "--jira-url=YOUR_JIRA_URL",          // Replace with your Jira instance URL (e.g., https://yourcompany.atlassian.net)
        "--jira-username=YOUR_JIRA_EMAIL",   // Replace with your Jira email/username
        "--jira-token=YOUR_JIRA_API_TOKEN"   // Replace with your Jira API Token
        // Add Confluence args here if needed
        // "--confluence-url=YOUR_CONFLUENCE_URL",
        // "--confluence-username=YOUR_CONFLUENCE_EMAIL",
        // "--confluence-token=YOUR_CONFLUENCE_API_TOKEN"
      ]
    }
  // ... other configurations ...
}
```

**Important:**
*   Replace `YOUR_JIRA_URL`, `YOUR_JIRA_EMAIL`, and `YOUR_JIRA_API_TOKEN` with your actual credentials.
*   You need to generate an [API Token from your Atlassian Account Settings](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/). Do not use your password.
*   The specific command (`uvx`, `pipx`, etc.) might vary depending on how MCP tools are managed in your Cursor version.
*   Restart Cursor after saving the configuration file to ensure the changes take effect.

## How to Use (with AI Assistant)

Once configured, follow these steps with the AI assistant:

### 1. Create Epics (`Create Epic` file)

*   **Purpose:** Creates the main Epic issues in Jira.
*   **Instructions:** Use the `@Create Epic` command or provide the `Create Epic` file to the assistant.
*   **Source:** Reads unique Epic names from the "Epic" column in the spreadsheet specified within the `Create Epic` file (currently: `https://docs.google.com/spreadsheets/d/1F1lb7URupPIvMZlkUFWWZUo02wJmRcIxsO1_uXM1Z-I/edit?usp=sharing`).
*   **Project:** Creates Epics in the **AIP666** project.
*   **Process:** The assistant will:
    *   Check if an Epic with the same name already exists in the project.
    *   If not found, create a new Epic, pulling details like priority and dates from the associated tasks in the spreadsheet.

### 2. Create Tasks (`Create Task` file)

*   **Purpose:** Creates Task and Subtask issues under the Epics created in the previous step.
*   **Instructions:** Use the `@Create Task` command or provide the `Create Task` file to the assistant.
*   **IMPORTANT - Template Task:**
    *   Before running, **you MUST edit the `Create Task` file** and replace `TEMPLATE_TASK_KEY = "AIP666-XYZ"` with the **actual Jira Key of a real, existing task** in the `AIP666` project.
    *   This template task is used to automatically find the correct internal Jira IDs for custom fields like "Start Date" and "Story Points", making the process more reliable.
*   **Source:** Reads task details from the spreadsheet specified within the `Create Task` file (currently: `https://docs.google.com/spreadsheets/d/1F1lb7URupPIvMZlkUFWWZUo02wJmRcIxsO1_uXM1Z-I/edit?usp=sharing`).
*   **Project:** Creates Tasks/Subtasks in the **AIP666** project.
*   **Process:** The assistant will:
    *   Fetch the template task to map custom field IDs.
    *   Go through each task row in the spreadsheet.
    *   Check if the task/subtask already exists.
    *   If not found, create the Task/Subtask, mapping spreadsheet columns to Jira fields (using the mapped custom field IDs).
    *   Immediately update the created issue to ensure dates, estimates, and story points are set correctly.
    *   Link Tasks to their parent Epic.
    *   Set the issue status based on the spreadsheet value (if possible).

## Key Reminders

*   **Configuration is Key:** Ensure the Atlassian MCP tool is correctly configured in Cursor first.
*   **Template Task is Crucial:** Always update the `TEMPLATE_TASK_KEY` in `Create Task` before running.
*   **Spreadsheet Links:** Ensure the links in the instruction files point to the correct Google Sheet.
*   **Project Key:** Verify the project key (`AIP666`) is correct for your target project.
*   **Avoid Duplicates:** The process includes checks, but reviewing the results is always a good idea.
*   **Date Format:** Dates in the spreadsheet should ideally be in `DD/MM/YYYY` format for the assistant to convert correctly to Jira's `YYYY-MM-DD`. 