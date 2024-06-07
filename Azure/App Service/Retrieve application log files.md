
Methods you can use to retrieve logged information for offline analysis.

## Log file storage locations

***Important:*** The Azure infrastructure used to run Azure Web Apps in Windows isn't the same as for Linux apps, and log files aren't stored in the same locations.

### Linux app log files

For Linux Web Apps, the Azure tools currently support fewer logging options than for Windows apps. Redirections to STDERR and STDOUT are managed through the underlying Docker container that runs the app, and these messages are stored in Docker log files. To see messages logged by underlying processes, such as Apache, you need to open an SSH connection to the Docker container.

## Methods for retrieving log files

How you retrieve log files depends on the type of log file, and on your preferred environment. For file system logs, you can use the Azure CLI or the Kudu console. Kudu is the engine behind many features in Azure App Service related to source control based deployment.

### Kudu

There's an associated Source Control Management (SCM) service site associated with all Azure Web Apps. This site runs the **Kudu** service, and other Site Extensions. It's Kudu that manages deployment and troubleshooting for Azure Web Apps, including options for viewing and downloading log files. The specific functionality available in Kudu, and how you download logs, depends on the type of Web App. For Windows apps, you can browse to the log file location, and then download the logs. For Linux apps, there may be a download link.

One way to access the Kudu console is to navigate to **https://<_app name_>.scm.azurewebsites.net**, and then sign in using _**deployment credentials**_.

You can also access Kudu from the Azure portal. On the App Service menu, under **Development Tools**, select **Advanced Tools**, and then on the Advanced Tools pane, select **Go** to open a new **Kudu Services** tab.

#### Reference https://learn.microsoft.com/en-us/training/modules/capture-application-logs-app-service/6-retrieve-application-log-files-from-an-application
