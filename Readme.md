# Task 1: Deploy your ASP.Net Core application to your local IIS server.

I created a new solution

```bash
$ dotnet new sln -n WebApplication1
```

I created an ASP.NET Core MVC project instead of Web API

```bash
$ dotnet new mvc -o WebApplication1.Web
```

I added the MVC project to the solution
```bash
$ dotnet sln add WebApplication1.Web
```

I ran the project for checking everything looks fine

```bash
$ dotnet run --project WebApplication1.Web
```


I configured IIS and granted permission to access the directory where my app is deployed, creating the application pool following the Self Study Material:

https://www.linkedin.com/learning/deploying-asp-dot-net-core-applications-from-fundamentals-to-advanced-deployment-strategies/creating-and-configuring-a-website-in-iis-24458853?autoSkip=true&resume=false&u=106534538

I got the project running on IIS, as shown in the screenshots below:

![alt text](./001-deploy-iis/iis-add-website.png)

![alt text](./001-deploy-iis/iis-configuration.png)

# Task 2: Deploy your ASP.Net Core application to Azure App Service 

## 2.1 Via Visual Studio

I used the same template application from Task 1. First, I created the required Azure App Service:

![alt text](./002.1-deploy-azure/101-az-portal.png)

Then, using Visual Studio 2022, I published the app by selecting the previously created Web App:

![alt text](./002.1-deploy-azure/101-az-vs2022-publish.png)

I pressed Publish, and Visual Studio handled the deployment

![alt text](./002.1-deploy-azure/104-az-finished.png)

## 2.2 Via Azure CLI

First, I registered the Microsoft.Web namespace:

![alt text](./002.2-deploy-azure-cli/202.0-register.png)

From the root of my solution, I ran the following commands:

I created a new resource group called WebApp1 in the Canada Central region

```bash
$ az group create --name WebApp1 --location "Canada Central"
```

I set up an S1-tier App Service Plan named WebApp1Plan under the WebApp1 group

```bash
$ az appservice plan create --name WebApp1Plan --resource-group WebApp1 --location "Canada Central" --sku S1
```

I provisioned a .NET 9 Web App named webapp112345667 on the WebApp1Plan

```bash
$ az webapp create --name webapp112345667 --plan WebApp1Plan --resource-group WebApp1 --runtime "dotnet:9"
```

I built the MVC app in Release mode for .NET 9 and writes the output into the publish folder
```bash
$ dotnet publish -c Release -f net9.0 -o ./publish
```

### I compressed the contents of the publish folder into publish.zip. I made the common mistake of compressing the folder itself instead of its contents

I pushed the ZIP package to Azure and deployed it to the web app

```bash
$ az webapp deploy --resource-group WebApp1 --name webapp112345667 --type zip --src-path publish.zip
```

Finally, I browsed to my newly deployed web app:

```bash
$ az webapp browse --resource-group WebApp1 --name webapp112345667
```

![alt text](./002.2-deploy-azure-cli/203-deployed.png)

![alt text](./002.2-deploy-azure-cli/203-end.png)