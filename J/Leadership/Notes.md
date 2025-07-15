## Customer Obsession
1. Redis Flush mvn package
2. JWT Service

## Agility
1. Legacy Code
2. C#

## Innovation
1. Redis
2. FMS



## FMS

### ⭐ **Situation**

In my previous project, I worked closely with the data engineering team, who were running nightly jobs and pipelines that required a lot of manual intervention. There was no centralized way to monitor which jobs succeeded or failed, or which team owned them, which often caused delays and inefficiencies. On top of that, the information security team didn’t allow the operations team direct access to cloud resources like Azure, which made it even harder for them to investigate or monitor issues on their own.

---

### 🔷 **Task**

My task was to help the data team improve operational visibility and reduce the manual effort involved in managing and monitoring file-based pipelines. The goal was to design and implement a centralized solution that could standardize file submissions, trigger downstream processes, and present a clear, real-time view of pipeline statuses.

---

### 🛠️ **Action**

I collaborated with the data team to understand their workflows and pain points in detail. Based on these discussions, I proposed and developed an in-house web-based dashboard with the following capabilities:

* Provided a standardized interface for uploading two input files along with required metadata.
* Implemented business validation and logic to process the files and deliver the output in `.csv` format.
* Integrated with Azure Blob Storage to store files, and leveraged blob storage triggers to automatically invoke Azure Data Factory (ADF) pipelines for processing.
* Built a centralized monitoring page to display pipeline status (success/failure) along with relevant details such as file name, team owner, submission date, and more, with filters and export-to-CSV/Excel functionality.

I ensured the solution was user-friendly, reliable, and aligned with the business rules.

---

### 📈 **Result**

This solution significantly reduced manual intervention and saved the team a lot of time. It centralized pipeline monitoring, improved visibility into job statuses, and made it easy to identify and resolve failures quickly. The dashboard streamlined the file submission process, standardized validations, and empowered the teams with actionable insights, ultimately improving operational efficiency and team productivity.

---

### Summary

---

> In my previous project, I worked closely with the data engineering team, who were running nightly jobs and pipelines that required a lot of manual intervention. There was no centralized way to monitor which jobs succeeded or failed, or which team owned them, which often caused delays and inefficiencies.
>
> I took the initiative to sit with the team, understand their pain points, and design a solution. I developed an in-house web dashboard that standardized file submissions and automated downstream processing. Users could upload input files with metadata, and the files were validated and processed as per business rules, with the output delivered in `.csv` format. We integrated with Azure Blob Storage and used blob triggers to kick off Azure Data Factory pipelines.
>
> The dashboard also provided a centralized view of all pipeline statuses, with filters, export options, and real-time insights into failures and ownership.
>
> As a result, we eliminated much of the manual work, improved visibility and control over pipelines, and helped the team resolve issues faster, saving a lot of operational time and effort.

---


## Redis Cache

**STAR format**

---

### ⭐ **Situation**

In one of my projects, the onshore team had developed a shared Redis cache library for all modules. The library exposed methods like `GetAsync`, `GetOrPutAsync`, `PutAsync`, and `IsAlive`.

---

### 🔷 **Task**

While integrating this library into our module, we encountered a common use case: we needed to invalidate or remove a specific cache entry by its key. However, the library did not provide a generic method for this, so each consumer team was forced to implement their own cache invalidation logic repeatedly.

---

### 🛠️ **Action**

I identified this gap and proposed to the onshore team that we enhance the shared library by adding a reusable method called `PurgeCacheAsync`, which would accept a key and remove the corresponding cache entry. After getting their approval, I implemented the method directly in the library, ensuring it was thoroughly tested and followed the same design patterns as the existing methods.

---

### 📈 **Result**

By adding `PurgeCacheAsync` to the shared library, we eliminated duplicate code across modules, improved maintainability, and ensured consistent behavior for cache invalidation. This change saved developer time, reduced the risk of errors, and made the library more complete and user-friendly for all teams.

---

### 🎤 **Spoken Version**

> In one of my projects, the onshore team had developed a shared Redis cache library that provided methods like `GetAsync`, `GetOrPutAsync`, `PutAsync`, and `IsAlive`. While integrating this library into our module, I noticed a gap: there was no generic way to invalidate or remove a cache entry by key. As a result, every team consuming the library was writing their own custom invalidation logic, which led to duplication and inconsistencies.
>
> I proposed to the onshore team that we enhance the library by adding a reusable method called `PurgeCacheAsync` to handle key-based invalidation. After getting their approval, I implemented the method in the library, aligned with its existing design patterns, and ensured it was thoroughly tested.
>
> This improvement made the library more robust and user-friendly, eliminated redundant code across modules, and saved developer time while ensuring consistent cache invalidation logic.

---

### 📄 **Resume Bullet Point:**

✅ *Enhanced a shared Redis caching library by identifying and implementing a reusable `PurgeCacheAsync` method for key-based invalidation, eliminating code duplication across modules, improving maintainability, and streamlining developer workflows.*

---


