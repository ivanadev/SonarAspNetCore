# SonarAspNetCore
Install, configure and execute sonar in applications .net core

### Installing and Configuring

1. Install Java JDK - https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

2. Create path Sonar, let's say in C:\Projects\Sonar

3. Donwload scanner .net core 2.0+ and unzip to folder created on step 2, let's say in C:\Projects\Sonar\sonar-scanner-msbuild-4.6.0.1930-netcoreapp2.0 - https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+MSBuild

4. Download SonarQube Community Edition Server and unzip to folder created on step 2, let's say in C:\Projects\Sonar\sonarqube-7.7 - https://www.sonarqube.org/downloads/

5. Check if environment variable JAVA_HOME exists. If don't, create new environment variable JAVA_HOME with Java path created on step 1, let's say in C:\Program Files\Java\jdk1.8.0_201

6. (Optional) Create new environment variable SONAR_SCANNER with sonnar-scanner path created on step 3, let's say in C:\_projetos\Sonar\sonar-scanner-msbuild-4.6.0.1930-netcoreapp2.0

7. Create file sonar.bat to application path like according your scanner (Ex: https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+MSBuild). If you follow this tutorial, your sonar.bat file will be something like:

```
dotnet %SONAR_SCANNER%\SonarScanner.MSBuild.dll begin /k:"App.Group:App.Name" /n:"App.Name" /v:"1.0"
dotnet build %CD%\App.Name.sln
dotnet %SONAR_SCANNER%\SonarScanner.MSBuild.dll end

pause
```

###  Executing

8. Configure SonarQube.Analysis file on sonnar-scanner path:
	- sonar.host.url - Host - https://localhost:9000 is default
	- sonar.login - Login if authentication is not anonymous
	- sonar.password - Password if authentication is not anonymous
	
9. Executing StartSonar.bat in C:\_projetos\Sonar\sonarqube-7.7\bin\windows-x86-64 created on step 4 and wait for "SonarQube is up" information. if you want, you can close the console because sonar will continue to run in background

10. Executing sonar.bat created on steep 7

Note: When initialize the server, if the error occurs "wrapper  | Critical error: wait for JVM process failed", means that the wrapper does not found installation of Java. In this case, one possible solution is set path of Java in wrapper.conf file, let's say in C:\Projects\Sonar\sonarqube-7.7\conf\wrapper.conf line wrapper.java.command - Ex: C:\Program Files\Java\jdk1.8.0_201\bin\java)

### OPENCOVER

1. To add OpenCover to SonarQube, download the .msi file , let's say in: https://github.com/OpenCover/opencover/releases,
version 4.7.922.

2.Create a new environment variable named 'OPENCOVER_HOME', and set path as the directory of the OpenCover.Console.exe file.

3.Edit your sonar.bat file:
``
setlocal enableextensions
@cd /d "%~dp0"
set OPEN_COVER=%OPENCOVER_HOME%\OpenCover.Console.exe
dotnet %SONAR_SCANNER%\SonarScanner.MSBuild.dll begin /k:"App.Group:App.Name" /n:"App.Name" /v:"1.*" /d:sonar.cs.opencover.reportsPaths="%CD%\opencover.xml"
dotnet build %CD%\App.Name.sln /t:Rebuild
%OPEN_COVER% -output:"%CD%\opencover.xml" -register:user -target:"c:\program files\dotnet\dotnet.exe" -targetargs:"test %CD%\App.Name.Testes\App.Name.Testes.csproj" -oldstyle

dotnet %SONAR_SCANNER%\SonarScanner.MSBuild.dll end
pause
``
4. Run/Rerun the sonar.bat and StarSonar.bat.


Have fun!
