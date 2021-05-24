---
title: "Create a Personal Tab with ASP.NET Core MVC" 
author: laujan
description: A quickstart guide to creating a custom personal tab with ASP. NET Core MVC.
localization_priority: Normal
ms.topic: quickstart 
ms.author: lajanuar
---

# Create a personal tab using C# and ASP.NETCore MVC

This quickstart takes you through the steps to create a custom personal tab with C# and ASP.NetCore MVC, and helps you finalize your app manifest and deploy your tab in Teams using [App Studio for Microsoft Teams](~/concepts/build-and-test/app-studio-overview.md).

## What you'll learn

* Review the source code.
* Update your application.
* Establish a secure tunnel using ngrok.
* Upload your application to Teams using App Studio.

[!INCLUDE [dotnet-core-prereq](~/includes/tabs/dotnet-core-prereq.md)]

## Get the source code

Open a command prompt and create a new directory for your tab project. We have provided a simple project to get you started. To retrieve the source code, download the zip folder and extract the files or clone the following sample repository into your new directory:

``` bash
git clone https://github.com/OfficeDev/microsoft-teams-sample-tabs.git
```

In Visual Studio, navigate to  **File** **Open** and select **project/solution**. Navigate to the tab application directory and open **PersonalTabMVC.sln**.

To build and run your application press **F5** or select **Start Debugging** from the **Debug** menu. In a browser enter the following URLs and verify the application has loaded properly:

http://localhost:44335
http://localhost:44335/privacy
http://localhost:44335/tou

## Review the source code

### Startup.cs

This project was created from an ASP.NETCore 2.2 Web Application empty template with the *Advanced - Configure for HTTPS* check box selected at setup. The MVC services are registered by the dependency injection framework's `ConfigureServices()` method. The empty template does not enable serving static content by default, so the following static files middleware is added to the `Configure()` method:

``` csharp
public void ConfigureServices(IServiceCollection services)
  {
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
  }
public void Configure(IApplicationBuilder app)
  {
    app.UseStaticFiles();
    app.UseMvc();
  }
```

### wwwroot

In ASP.NETCore, the application looks for the static files in this folder.

### AppManifest

This folder contains the following required app package files:

* A **full color icon** measuring 192 x 192 pixels.
* A **transparent outline icon** measuring 32 x 32 pixels.
* A **manifest.json** file that specifies the attributes of your app.

These files must be zipped in an app package to upload your tab to Teams. Microsoft Teams loads the `contentUrl` specified in your manifest, embed it in an <iframe>, and render it in your tab.

### .csproj

In the Visual Studio Solution Explorer window, right-click on the project and select **Edit Project File**. At the bottom of the file, see the code that creates and updates your zip folder when the application builds:

``` xml
<PropertyGroup>
    <PostBuildEvent>powershell.exe Compress-Archive -Path \"$(ProjectDir)AppManifest\*\" -DestinationPath \"$(TargetDir)tab.zip\" -Force</PostBuildEvent>
  </PropertyGroup>

  <ItemGroup>
    <EmbeddedResource Include="AppManifest\icon-outline.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\icon-color.png">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
    <EmbeddedResource Include="AppManifest\manifest.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </EmbeddedResource>
  </ItemGroup>
```

### Models

**PersonalTab.cs** presents a Message object and methods that will be called from *PersonalTabController* when a user selects a button in the **PersonalTab** View.

### Views

#### Home

ASP. NET Core treats files called **Index** as the default or home page for the site. When your browser URL points to the root of the site, **Index.cshtml** will be displayed as the home page for your application.

#### Shared

The partial view markup *_Layout.cshtml* contains the application's overall page structure and shared visual elements. It also reference the Teams Library.

### Controllers

The controllers use the ViewBag property to transfer values dynamically to the Views.

[!INCLUDE [dotnet-update-personal-app](~/includes/tabs/dotnet-update-personal-app.md)]

[!INCLUDE [dotnet-ngrok-intro](~/includes/tabs/dotnet-ngrok-intro.md)]

Open a command prompt in the root of your project directory and run the following command:

    ``` bash
    ngrok http https://localhost:44345 -host-header="localhost:44345"
    ```

Ngrok listens the requests from the internet and routes to your application when it is running on port 44325.  It must resemble `https://y8rPrT2b.ngrok.io/` where *y8rPrT2b* is replaced by your ngrok alpha-numeric HTTPS URL.

You must keep the command prompt while ngrok is running, you need it later to write down the URL.

Verify that **ngrok** is up and running by opening your browser and navigating to your content page through the ngrok HTTPS URL provided in your command prompt window.

> [! TIP]
> You must run your application in Visual Studio and ngrok to complete this quickstart. When you need to stop running your application in Visual Studio to work on it, **keep ngrok running**. It continues to listen and resume routing your application's request when restarts in Visual Studio. When you restart the ngrok service, it returns the new URL, and you need to update all locations that use the old URL.

### Run your application

* In Visual Studio press **F5** or select **Start Debugging** from your application's **Debug** menu.

[!INCLUDE [dotnet-personal-use-appstudio](~/includes/tabs/dotnet-personal-use-appstudio.md)]

## Next step

> [!div class="nextstepaction"]
> [Create a custom channel and group tab using Node.js and the Yeoman Generator for Microsoft Teams](~/tabs/quickstarts/create-channel-group-tab-node-yeoman.md)
