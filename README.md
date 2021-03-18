![Nuget](https://img.shields.io/nuget/v/Bitrix24.Connector)
![Nuget](https://img.shields.io/nuget/dt/Bitrix24.Connector)
# Bitrix24 .NET Connector
Bitrix24 Connector is a small simple library built to work with Rest API Bitrix24. Currently under development...
### Getting started
Bitrix24 connector can be registered and used with Microsoft.Extensions.DependencyInjection. The HttpClient instance is injected in DI container (IHttpClientFactory).
To use, with an ```IServiceCollection```:
 ```csharp
  using Bitrix24.Connector.DependecyInjection;
  
  // ....
  
  services.AddBitrix24Connector(url);
 ```
 Or create Bitirx24 connector manually using constructors:
 ```csharp 
 // Better Approach
 var con = new Bitrix24Connector(url); 
 ```
 or
  ```csharp
  // Bad way
  Bitrix24Connector(HttpClient client)  
  ```
  This is a bad way! It is highly recommended to use Microsoft.Extensions.DependencyInjection with ```services.AddHttpClient <Bitrix24Connector>()```
  
  ### Usage
   ```csharp
   // Build
   var query = Bitrix24QueryBuilder
       .Create()
       .AddSelect("TITLE")
       .AddFilter("ID", 12)
       .Build();
    
   // Get deals collection
   var res = await _connector.Deals.ListAsync(query);
   ```
   Также любые команды можно выполнять с помощью ```IBitrix24RequestHandler```:
   ```csharp
    public interface IBitrix24RequestHandler
    {
        Task<string> ExecuteAsync(string command, object query = null);
        Task<Bitrix24Response<T>> ExecuteAsync<T>(string command, object query = null);
        Task<T> ExecuteDefaultAsync<T>(string command, object query = null);
    }
   ``` 
- command - example: user.get
- query - Bitrix24Query or any custom object
```csharp
    var users = await _connector.RequestHandler.ExecuteAsync<IEnumerable<Bitrix24User>>("user.get", query);
 ```
   
