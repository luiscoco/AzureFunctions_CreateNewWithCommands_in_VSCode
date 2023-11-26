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

Navigate to the Project Directory:

```bash
cd YourFunctionProjectName
```

Create a New Azure Function:

Run the following command to create a new Azure Function:

```bash
func new
```

Follow the prompts to select the template. Choose "HTTP trigger" for a simple example.

## 4. Build the Project.

 Run the following command to build the project:

```bash
dotnet build
```

## 5. Run the Function Locally:

Run the following command to start the function locally:

```bash
func start
```

This will start a local development server.

## 6. Test the Function:

Open a web browser or a tool like Postman and send a request to the locally running function. 

The URL will be displayed in the terminal.

## 7. Publish to Azure (Optional):

If you want to deploy your function to Azure, you can run the following command:

```bash
func azure functionapp publish YourAzureFunctionAppName
```

Replace YourAzureFunctionAppName with the desired name for your Azure Function App.

That's it! You've created a C# Azure Function using Visual Studio Code and the Azure Functions Core Tools. 

Feel free to explore more templates and configurations based on your specific requirements.
