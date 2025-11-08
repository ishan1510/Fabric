# 🧹 Automating Report Archival and Cleanup in Microsoft Fabric

A complete end-to-end framework to **identify, archive, and delete unused reports** in Microsoft Fabric—helping organizations save compute, reduce refresh failures, and keep workspaces lean.

---

## 🌟 Project Overview

Over time, Fabric workspaces can fill up with unused reports that still trigger dataset refreshes, consuming compute and storage unnecessarily.  
This project provides an **automated lifecycle** for such reports—from detection to archival to cleanup—using **Fabric Notebooks**, **Semantic Link Labs**, and **Fabric APIs**.

The solution runs in two stages:

| Stage | Notebook | Description |
|:------|:----------|:------------|
| **Part 1** | [`NB_Archive_Reports.ipynb`](./NB_Archive_Reports.ipynb) | Detects unused reports (no views in the past X days), copies them to an *Archive Workspace*, logs the operation, and deletes them from the source workspace. |
| **Part 2** | [`NB_Delete_Reports_and_Datasets.ipynb`](./NB_Delete_Reports_and_Datasets.ipynb) | After the retention period expires, deletes reports from the archive workspace and removes datasets that have no remaining dependencies. |

---

## 🧩 Architecture at a Glance

Audit Logs ─▶ Identify unused reports <br>
│ <br>
▼ <br>
Archive Workspace ◀─ Copy reports via Semantic Link Labs <br>
│ <br>
▼ <br>
Source Workspace cleanup (delete reports only)<br>
│ <br>
▼ <br>
Delete report exceeding Retention period (e.g., 60 days) <br>
│ <br>
▼ <br>
Dataset dependency check & cleanup

All logic executes inside Fabric Notebooks, orchestrated through scheduled runs (weekly or monthly).

---

## 🧰 Key Components

- **Fabric Audit Logs** – Source of truth for report usage activity  
- **Fabric APIs** – Perform delete and metadata operations  
- **Semantic Link Labs Library** – Simplifies report copy actions  
- **Azure Key Vault** – Securely stores and retrieves service-principal credentials  
- **Archive History Table** – Tracks what was archived, when, and by whom  

---

## ⚙️ Prerequisites

1. **Fabric Admin Account** with contributor access to all workspaces  
2. **Service Principal** with:
   - `Report.ReadWrite.All` or `Item.ReadWrite.All` permissions  
3. **Audit Logs** captured for at least 60 days  
4. **Archive Workspace** created within Fabric  
5. **Azure Key Vault** configured for tenant/client secrets  

---

## 🚀 How It Works

### 🔹 Part 1 – Archival Flow
1. Import libraries and declare configuration variables.  
2. Fetch secrets from Key Vault.  
3. Query Fabric Audit Logs to identify unused reports.  
4. Copy reports to the Archive Workspace using Semantic Link Labs.  
5. Log archival details in a Delta/SQL table.  
6. Delete original reports from their source workspace.  

### 🔹 Part 2 – Cleanup Flow
1. Identify reports older than the defined retention period.  
2. Delete expired reports from the Archive Workspace via Fabric API.  
3. Locate datasets that no longer have linked reports.  
4. Safely delete orphaned datasets.  
5. Update the log table with deletion timestamps.  

Both notebooks can be scheduled in Fabric Notebook Jobs for full automation.

---

## 📅 Scheduling Recommendation

- **Frequency:** Weekly or Monthly  

---

## 💡 Future Enhancements

- Notification emails to report owners before deletion  
- Stop Dataset refresh for archived reports   
- Dashboard summarizing cost savings and cleanup metrics  

---

## 🧠 Why This Matters

By automating archival and cleanup:

- 🏷 **Save Compute** – Stop refreshing unused datasets  
- ⚡ **Improve Performance** – Reduce load on Fabric capacity  
- 🔒 **Enhance Governance** – Maintain an auditable lifecycle  
- 🧭 **Stay Organized** – Keep your Fabric environment clean and scalable  

---

## 🔗 References & Resources

- [Part 1 Blog – Automating Report Archival in Microsoft Fabric](https://www.ishandeshpande.com/post/automating-unused-report-archival-in-microsoft-fabric)  
- [Part 2 Blog – Automating Report Archival in Microsoft Fabric](https://www.ishandeshpande.com/post/automating-report-archival-part-2)  
- [Semantic Link Labs Documentation](https://github.com/microsoft/semantic-link-labs)  

---

## 💬 Feedback

Have ideas to make this smarter or more efficient?  
Open an issue, start a discussion, or reach out—I’d love to hear your perspective and collaborate on future enhancements!

---

