## EB - Immutable
- Creats a new ASG group with EC2 instances.
- Deploy the updated version of the app on the new EC2 instances.
- Point the ELB to the new ASG and delete the old ASG which will terminate the old EC2 instances.

```
The safest way to deploy for critical applications.

**Note**: In case of failure, you just terminate the new instances since the existing instances still remain.
```

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

##### Improvements include:

* Consistent column widths.
* Clear ✅/🔴/🟢 symbols for Yes/No for quick scanning.
* Added a *Legend* section at the bottom for symbols.
* Replaced some wordiness with clearer phrases.


## EB - In-Place vs Blue/Green Deployment
> Elastic Beanstalk by default performs in-place updates.

> In-Place and Blue/Green Deployment are not definitive in definition and the context can change the scope of what they mean.

```
In-Place could mean within **the scope of Elastic Beanstalk Env**

All the `deployment policies provided by EB could be considered In-Place since they are within the scope of a single EB environment.

- All at once
- Rolling
- Rolling with additional batch
- Immutable
```


```
In-Place could mean within **the scope of the same server** (not replacing the server)

Deployment policies which do not involve the server being replaced

- All at once
- Rolling
```


```
In-Place could mean wihin **the scope of an uninterrupted server**

Traffic is never routed away from the server (taken-of-service). Implements **Zero-downtime** deploys where Blue/Greens occurs on the server.

EB can't do this. Capistrano + Ruby on Rails + Unicorn is famous case fo this method of deployment.
```