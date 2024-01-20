# WebAssembly and Server -WASM , WebApi dotnet6.0 and sql server STORE Application
# Introduction
    Steps to setup

## Build the Restful API:
To create the API, open your terminal or command prompt and navigate to the folder where you want to create your project.

Next, create a new solution file:

Copy code
```
dotnet new sln -n BlazorApi
```
Then, create a new class library for the API:



Copy code
```
dotnet new classlib -n BlazorApi.Core -f net6.0
```
After that, create a new Web API project:


Copy code
```
dotnet new webapi -n BlazorApi.Controllers -f net6.0
```

Add both projects to the solution:

Copy code
```
dotnet sln add BlazorApi.Core/BlazorApi.Core.csproj
dotnet sln add BlazorApi.Controllers/BlazorApi.Controllers.csproj
```

Next, reference the BlazorApi.Core project from the BlazorApi.Controllers project:

Copy code
```
dotnet add BlazorApi.Controllers/BlazorApi.Controllers.csproj reference BlazorApi.Core/BlazorApi.Core.csproj
```

Finally, build the solution:

Copy code
```
dotnet build
```

Create a new Blazor Server Application:
To create the Blazor Server Application, run the following command:

Copy code
```
dotnet new blazorserver -n BlazorServerApp -o BlazorServerApp
```

Then, add a reference to the BlazorApi.Core project:

Copy code
```
dotnet add BlazorServerApp/BlazorServerApp.csproj reference BlazorApi.Core/BlazorApi.Core.csproj
```

Create a new Blazor WebAssembly Application:
To create the Blazor WebAssembly Application, run the following command:

Copy code
```
dotnet new blazorwasm -n BlazorWasmApp -o BlazorWasmApp
```

Then, add a reference to the BlazorApi.Core project:

Copy code
```
dotnet add BlazorWasmApp/BlazorWasmApp.csproj reference BlazorApi.Core/BlazorApi.Core.csproj
```

Configure HttpClient for Blazor WebAssembly:
Open the BlazorWasmApp/wwwroot/appsettings.json file and add the following configuration:

Copy code
```
{
 "ApiUrl": "https://localhost:5001"
}
```

Then, open the BlazorWasmApp/Program.cs file and register the HttpClient with the API URL:

csharp
Copy code
```
builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.Configuration["ApiUrl"]) });
```

Consume the Restful API in Blazor Client Application:
Create a new class called ApiService in the BlazorApi.Core project:

csharp
Copy code
```
public class ApiService
{
    private readonly HttpClient _httpClient;

    public ApiService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    // Add methods to call the API
}
```

Inject the ApiService class into the Blazor Server and WebAssembly client applications:

csharp
Copy code
```
@inject ApiService ApiService
Call the API using the ApiService class in the Blazor client applications:
```


csharp
Copy code
```
var data = await ApiService.GetDataAsync();
```

Deploy to Azure:
First, publish the Blazor Server Application:

Copy code
```
dotnet publish BlazorServerApp/BlazorServerApp.csproj -c Release -o ./publish
```

Then, publish the Blazor WebAssembly Application:

Copy code
```
dotnet publish BlazorWasmApp/BlazorWasmApp.csproj -c Release -o ./publish
```

Finally, deploy the applications to Azure using Azure Portal or Azure CLI.

Secure the API:
To secure the API, implement authentication and authorization. One option is to use Azure Active Directory (AAD). 