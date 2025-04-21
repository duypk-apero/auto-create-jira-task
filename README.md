# Jira Issue Creation Guide

This guide explains how to use the `Create Epic` and `Create Task` files to automatically create Jira Epics and Tasks in your project based on data from a Google Sheet.

## Prerequisites

*   Access to the Google Sheet containing the task data.
*   A connection to your Jira instance (configured for the AI assistant).
*   An AI assistant capable of reading these instruction files and using Jira tools.

## How to Use

Follow these steps with your AI assistant:

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

*   **Template Task is Crucial:** Always update the `TEMPLATE_TASK_KEY` in `Create Task` before running.
*   **Spreadsheet Links:** Ensure the links in the instruction files point to the correct Google Sheet.
*   **Project Key:** Verify the project key (`AIP666`) is correct for your target project.
*   **Avoid Duplicates:** The process includes checks, but reviewing the results is always a good idea.
*   **Date Format:** Dates in the spreadsheet should ideally be in `DD/MM/YYYY` format for the assistant to convert correctly to Jira's `YYYY-MM-DD`. 