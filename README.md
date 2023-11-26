# AzureFunctions: How to create a new Azure Function with Commands in VSCode

Creating an Azure Function in C# with Visual Studio Code involves a few steps.

Make sure you have the necessary tools installed, such as the Azure Functions Core Tools and the Azure Functions extension for Visual Studio Code.

Here's a step-by-step guide:

## 1. Install Prerequisites:

Install .NET SDK (if not already installed).

Install Azure Functions Core Tools.

Install Azure Functions Extension for VSCode:

## 2. Open Visual Studio Code and install extension.

Go to the Extensions view (you can press Ctrl + Shift + X), and search for "**Azure Functions**"

Install the extension published by Microsoft.

## 3. Create a new Azure Functions Project:

Open a terminal in Visual Studio Code.

Run the following command to create a new Azure Functions project:

```bash
func init YourFunctionProjectName --dotnet
```

Replace YourFunctionProjectName with the desired name for your project.

![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/8cb470a0-a54f-4003-8d48-e2a5b105bf50)

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



![image](https://github.com/luiscoco/AzureFunctions_CreateNewWithCommands_in_VSCode/assets/32194879/01c2ccdd-eb78-4829-ae07-e4999cc27dd6)


If you want to deploy your function to Azure, you can run the following command:

```bash
func azure functionapp publish YourAzureFunctionAppName
```

Replace YourAzureFunctionAppName with the desired name for your Azure Function App.

That's it! You've created a C# Azure Function using Visual Studio Code and the Azure Functions Core Tools. 

Feel free to explore more templates and configurations based on your specific requirements.
