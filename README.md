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
  
services.AddBitrix24Client("Bitrix24:Hook");
```
Or create Bitirx24 connector manually using constructors:
```csharp 
 // Better Approach
 var client = new Bitrix24Client(url); 
```
or
```csharp
 // Bad way
 var client = Bitrix24Client(HttpClient client)  
```
This is a bad way! It is highly recommended to use Microsoft.Extensions.DependencyInjection with ```services.AddHttpClient<IBitrix24Client, Bitrix24Client>```
  
### Usage
```csharp
   var contact = await _client.Crm.Contacts
         .Select(s => s.Id, s => s.Phone, s => s.Email, 
               s => s.AddressCity,
               s => s.Birthdate, s => s.Name,
               s => s.LastName, s => s.SecondName, s => s.DateModify)
         .FirstOrDefaultAsync(x => x.Email == "mrbrown@mail.com")
         .ConfigureAwait(false);
```   
#### Inheritance
```csharp
   public class Bitrix24Customer : Bitrix24Contact
   {
       [Bitrix24Property("UF_PARENT_ID")]
       public int? ParentId { get; set; }

       [Bitrix24Property("UF_GUID")]
       public Guid? CustomerId { get; set; }
   }
        
   var contact = await _client.Crm.Contacts
         .Select<Bitrix24Customer>(s => s.Id, s => s.Phone, s => s.Email, 
               s => s.AddressCity, s => s.ParentId,
               s => s.Birthdate, s => s.Name, s => s.CustomerId,
               s => s.LastName, s => s.SecondName, s => s.DateModify)
         .FirstOrDefaultAsync<Bitrix24Customer>(x => x.Email == "mrbrown@mail.com" && x.ParentId == 1)
         .ConfigureAwait(false);        
``` 
Also, any commands can be executed using ```IBitrix24RequestHandler```:
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
   
