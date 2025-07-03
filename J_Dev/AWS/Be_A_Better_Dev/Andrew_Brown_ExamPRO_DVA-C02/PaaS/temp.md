## EB - Immutable
- Creats a new ASG group with EC2 instances.
- Deploy the updated version of the app on the new EC2 instances.
- Point the ELB to the new ASG and delete the old ASG which will terminate the old EC2 instances.

```
The safest way to deploy for critical applications.

**Note**: In case of failure, you just terminate the new instances since the existing instances still remain.
```

## EB - Deployment Methods
Here’s the table from your image, formatted in Markdown so you can include it on your wiki page:

```markdown
## EB – Deployment Methods

| Method                      | Impact of Failed Deployment                                                    | Deploy Time       | Downtime     | DNS Change       | Rollback Process                     | Code Deployed to Instances |
|-----------------------------|-------------------------------------------------------------------------------|-------------------|--------------|------------------|--------------------------------------|-----------------------------|
| **All-at-once**             | Downtime                                                                      | ⏱                | Downtime     | No DNS Change    | Manual                               | Existing                    |
| **Rolling**                 | Single batch out of service; any successful batches before failure running new application version | ⏱⏱⏱†         | No downtime  | No DNS Change    | Manual                               | New and Existing            |
| **Rolling with additional batch** | Minimal if first batch fails; otherwise, similar to *Rolling*                    | ⏱⏱⏱†         | No downtime  | No DNS Change    | Manual                               | New and Existing            |
| **Immutable**               | Minimal                                                                        | ⏱⏱              | No downtime  | DNS Change       | Terminate New                        | New                         |
| **Blue/green**              | Minimal                                                                        | ⏱⏱              | No downtime  | DNS Change       | Swap URL                             | New                         |
| **Traffic splitting**       | Percentage of client traffic routed to new version temporarily impacted       | ⏱⏱⏱††        | No downtime  | No DNS Change    | Reroute traffic and terminate new instances | New                         |

† Time may vary  
†† Time varies depending on evaluation time option setting
```

If you’d also like, I can save it as a `.md` file and give you a download link. Let me know!


✅ Certainly — here’s a **cleaner, more readable version** of the table, with clear formatting and spacing for a professional wiki page:

```markdown
## EB – Deployment Methods

| **Method**                   | **Impact of Failed Deployment**                                            | **Deploy Time**       | **Downtime**   | **DNS Change**     | **Rollback Process**                | **Code Deployed to Instances** |
|-------------------------------|----------------------------------------------------------------------------|------------------------|----------------|--------------------|------------------------------------|---------------------------------|
| **All-at-once**              | Downtime                                                                   | ⏱                     | 🔴 Yes         | 🔷 No               | Manual                             | Existing                        |
| **Rolling**                  | Single batch out of service; successful batches before failure remain active | ⏱⏱⏱ †            | 🟢 No          | 🔷 No               | Manual                             | New + Existing                  |
| **Rolling + Additional Batch** | Minimal if first batch fails; otherwise, similar to *Rolling*             | ⏱⏱⏱ †            | 🟢 No          | 🔷 No               | Manual                             | New + Existing                  |
| **Immutable**                | Minimal                                                                    | ⏱⏱                   | 🟢 No          | 🔴 Yes             | Terminate new                      | New                             |
| **Blue/Green**               | Minimal                                                                    | ⏱⏱                   | 🟢 No          | 🔴 Yes             | Swap URL                           | New                             |
| **Traffic Splitting**        | Temporary impact as traffic shifts between versions                        | ⏱⏱⏱ ††          | 🟢 No          | 🔷 No               | Reroute & terminate new           | New                             |

---

### Legend:
- **Deploy Time:**
  - ⏱ = Short
  - ⏱⏱ = Moderate
  - ⏱⏱⏱ = Long
- 🔴 = Yes
- 🟢 = No
- 🔷 = No Change

† Time may vary  
†† Time varies depending on evaluation time option setting
```

Improvements include:

* Consistent column widths.
* Clear ✅/🔴/🟢 symbols for Yes/No for quick scanning.
* Added a *Legend* section at the bottom for symbols.
* Replaced some wordiness with clearer phrases.

If you’d like, I can also save this version as a **`.md` file** for you. Shall I?
