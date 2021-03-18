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
