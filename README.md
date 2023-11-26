# AzureFunctions: How to create a new Azure Function only with commands 

Creating an Azure Function in C# with Visual Studio Code involves a few steps.

Make sure you have the necessary tools installed, such as the **Azure Functions Core Tools** and the **Azure Functions extension** for Visual Studio Code.

Here's a step-by-step guide:

## 1. Install Prerequisites:

### 1.1. Install .NET SDK (if not already installed): https://dotnet.microsoft.com/es-es/download/dotnet/8.0

### 1.2. Install Azure Functions Core Tools: https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local

![image](https://github.com/luiscoco/AzureFunctions_CreateWithCommands/assets/32194879/17058b9f-f138-480f-8e41-7bebe6de9012)

IMPORTANT NOTE: these are the **Azure Functions Core Tools reference**: 

https://learn.microsoft.com/en-us/azure/azure-functions/functions-core-tools-reference?tabs=v2

### 1.3. Install Azure Functions Extension for VSCode.

## 2. Open Visual Studio Code and install extension.

Go to the Extensions view (you can press Ctrl + Shift + X), and search for "**Azure Functions**"

Install the extension published by Microsoft.

## 3. Create a new Azure Functions Project:

Open a terminal in Visual Studio Code.

Run the following command to create a new Azure Functions project:

```bash
func init YourFunctionProjectName --dotnet
```

This command creates a project that runs on the current Long-Term Support (LTS) version of .NET Core.

Replace YourFunctionProjectName with the desired name for your project.

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/8cb470a0-a54f-4003-8d48-e2a5b105bf50)

For other .NET version, create an app that runs in an isolated worker process from the Functions host, use the **dotnet-isolated** flag:

```
func init MyProjFolder --worker-runtime dotnet-isolated 
```

By default this command creates a project that runs in-process with the Functions host on the current Long-Term Support (LTS) version of .NET Core.

You can use the **--target-framework** option to target a specific supported version of .NET, including .NET Framework.

See: https://dotnet.microsoft.com/es-es/platform/support/policy/dotnet-core#lifecycle

For more information, see the **func init** reference:

https://learn.microsoft.com/en-us/azure/azure-functions/functions-core-tools-reference?tabs=v2#func-init

Navigate to the Project Directory:

```bash
cd YourFunctionProjectName
```

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/7d28a6f1-68e4-4663-aa04-ec9945a6e7ee)

Create a New Azure Function:

Run the following command to create a new Azure Function:

```bash
func new
```

Follow the prompts to select the template. Choose "HTTP trigger" for a simple example.

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/970ba539-6271-4bb5-b918-430923566096)

We set the new Azure Function name:

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/6f7b448d-3021-4fc3-9c94-17828cb70917)

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/a2ecf5da-09c6-40d0-8b0f-849f8f0fa831)

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/2cf70d7d-e909-42f4-8ced-5d42191f52cf)

We also can set in the commands: the Azure **FunctionName**, the template, the **authorization level**, and the **runtime**.

```bash
func init YourFunctionProjectName --dotnet
cd YourFunctionProjectName
func new --name MyNewFunction --template "HTTP trigger" --authlevel "anonymous" --runtime dotnet-isolated
```

## 4. Azure Function C# source code.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace YourFunctionProjectName
{
    public static class myNewFunction
    {
        [FunctionName("myNewFunction")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            string name = req.Query["name"];

            string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
            dynamic data = JsonConvert.DeserializeObject(requestBody);
            name = name ?? data?.name;

            string responseMessage = string.IsNullOrEmpty(name)
                ? "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response."
                : $"Hello, {name}. This HTTP triggered function executed successfully.";

            return new OkObjectResult(responseMessage);
        }
    }
}
```

## 5. Build the Project.

Run the following command to build the project:

```bash
dotnet build
```

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/9307d0f5-71ff-4e77-9eab-1016de935dd1)

## 6. Run the Function Locally:

Run the following command to start the function locally:

```bash
func start
```

This will start a local development server.

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/a67836b2-9685-4f2a-bef7-2351c583e3db)

## 7. Test the Function:

Open a web browser or a tool like Postman and send a request to the locally running function. 

The URL will be displayed in the terminal.

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/6c49ad63-645c-4a24-86a6-40b5fc21377f)

We can also invoke the Azure Function setting the parameter "name"

http://localhost:7071/api/MyNewFunction?name=John

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/ebfd5b30-dd70-43ff-b4d0-e255f136b447)

## 8. Publish to Azure (Optional):

Make sure you have an Azure Function App created in the Azure portal. 

If not, you'll need to create one.

The name you provide here is what you'll use in the func azure functionapp publish command.

For creating a new Azure Function open this service in Azure Portal and press on the "Create Function App" button:

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/628a8bd2-8740-4648-b754-ef0f3532a05d)

The set the input data requested for creating the new Azure Function:

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/2637a558-8168-41b6-b5f0-68bdd121b241)

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/f2129045-46e7-47d5-90ad-83951d2f70b0)

Then press the button "Review + create"

Then press the button "Create"

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/3a9ce4d9-8f2a-41b9-93f7-cfb40e2ca6a1)

Now the new Azure Function deployment is in progress

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/00295205-8762-48e8-ac32-a72e9aa532a7)

When the deployment is finished you can navigate to the new resource, press the "Go to resource" button:

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/22215f7b-73df-4307-9fa3-d105328b5080)

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/d0d64a8c-6f4c-4808-a1df-5ebf7f3c18c3)

If you want to deploy your function to Azure, you can run the following command:

```bash
func azure functionapp publish MyFirstAzureFunctionFromCommandPrompt
```

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/a0a60af2-aad5-43b0-b185-fbdc123975a0)

This error is telling you that there is a mismatch between the **FUNCTIONS_WORKER_RUNTIME** setting in your Azure Function App and your local project. 

In Azure, the app is set to use **dotnetIsolated**, while your local project is set to **dotnet**.

To resolve this, you have a couple of options:

### Update Local Project to Use dotnetIsolated:

Open your YourFunctionProjectName.csproj file.

Change the <TargetFramework> element to **net6.0-isolated** (or the version you are using).

Save the file.

Example:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0-isolated</TargetFramework>
  </PropertyGroup>

</Project>
```

### Pass --force to Update Azure App:

Run the following command, passing the **--force** option:

```bash
func azure functionapp publish MyFirstAzureFunctionFromCommandPrompt --force
```

This will update your Azure Function App to use dotnet as the FUNCTIONS_WORKER_RUNTIME.

Choose the option that best fits your requirements. 

If you're using the isolated process model locally (net6.0-isolated), it's a good idea to update your Azure Function App to match.

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/428fb818-2a00-470d-8f8f-cdf1a844a20a)

After the deployment go to Azure portal and navigate to the new Azure Function

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/400b1a99-5c4f-4072-b300-4ae903a2b85d)

Now we press in the "**Get Function Url**" option

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/31da0a7b-45dd-4823-92e7-21c958216862)

Then we copy and paste the Azure Function Url in the internet web browser:

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/a02fef16-1772-4991-91f8-b6818d2fdb30)

We check the output

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/edcf5354-4a61-4ca1-ad9e-6a38d52dcd1e)
