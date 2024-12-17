#### EF Migration Commands to Create the Database
###### 1. .Net Core CLI:
- dotnet-ef and Micrososft.EntityFrameworkCore.Design for cross-platform command line toooling
	- dotnet tool install --global dotnet-ef
	- dotnet add package Microsoft.EntityFrameworkCore.Design
	- dotnet ef  migratons add initialCreate
	- dotnet ef database update

###### 2. Package Manager Console (PMC) in Visual Studio:
- Micorsoft.EntityFrameworkCore.Tools for PowerShell tooling that works in the Visual Studio Package Manager Console
	- Install-Package Microsoft.EntityFrameworkCore.Tools
	- Add-Migration InitialCreate
	- Update-Database