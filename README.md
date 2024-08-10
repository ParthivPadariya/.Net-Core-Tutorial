Creating a First ASP.NET Core Project

## Folder Structure of Project
![image](https://github.com/user-attachments/assets/0714cd91-8c2d-450f-bf15-ee258b6a5ad0)
# https://www.tutorialsteacher.com/core/aspnet-core-application-project-structure

Start execution from Programe.cs
1. Connected Services
   - containe list of external services Ex. Azure, AWS, Google Cloud, and third-party services like authentication providers or databases.
2. Dependencies
   - Containe List of All dependances Like  NuGet packages, project references, and framework dependencies.
3. wwwroot
  - Static files can be stored Like. css, JavaScript, and external library, Image
4. appsettings.json
  -  It allows developers to use JSON format for the configurations instead of code
5. Programe.cs
  - is an entry point of an application  


Inside Programe.cs File
var builder = WebApplication.CreateBuilder(args);
CreateBuilder() method setup the internal web server which is Kestrel.It also specifies the content root and read application settings file appsettings.json

using builder object dependency injection, middleware, and hosting environment



Properies/launchSetting.json
-   containe server setting, used for local development machine for run server means not require when publish our application
-   it contain IIS and Kestrel Web Server Setting default run on Kestral Server.
-   use for run .net core in Visual Studio or CLI
-   ex. how over server started on over machine

appConfig.json
-   store application configration setting such as database connection string, global variable ...


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Middleware
- when client send send req on server then first go into middleware and also server send response then first go into middleware
- Middleware define in **Programe.cs** file
- ![image](https://github.com/user-attachments/assets/a261d235-281c-481a-9bf7-b8fae3b006f1)

- _**using Run**_ method we can create middleware which is execute for a every req ans res.
- When create middleware using Run method then only first middleware execute other not
- In **Run method does not contain next method for calling next middleware**

app.Run(async (context) => {
    await context.Response.WriteAsync("Padariya Parthiv");
});

app.Run(async (context) =>
{
    await context.Response.WriteAsync("Second Middleware");
});
o/p:- Padariya Parthiv
execute first middleware

_**using Use**_ 
diffrence is use method contain next method for calling next middleware

app.Use(async (context, next) =>
{
    await context.Response.WriteAsync("Hello");
    await next(context); // use for execute next middleware
});

app.Run(async (context) => {
    await context.Response.WriteAsync("Padariya Parthiv");
});

o/p:- HelloPadariya Parthiv

so Run does not execute next middleware and use execute multipale middleware.

![image](https://github.com/user-attachments/assets/ffadc282-bb82-4aba-97ef-03122854773b)


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


**Routing**
-   Mapped the URLS request to the request controller and action method
-   ![image](https://github.com/user-attachments/assets/dbe05334-a044-4831-9049-3a4861706d44)
-   First Req go to the routing meachanism(present in Programe.cs) and after send to controller approprite action method default Index method call
-   Ex. http://locahost:2773/Home  -> routing -> Home Contorller -> Default called Index method(/)
-   Routing = URL + HTTP Method (GET,POST,PUT,DELETE)
-   There are 2 Type of Routing
         1) Convention Based Routing
         2) Attribute-Based Routing (When use => apply Routing + API Concept(get,post,...))




1) Convention Based Routing
   this services must be register for run controller routing
   ![image](https://github.com/user-attachments/assets/4556698c-c20c-4810-9023-d1fde6044f1a)

   ![image](https://github.com/user-attachments/assets/305b41ba-c176-4968-abce-d20dc4145719)

   After applyting this we can also access Home/Index, Home/About, Home/Details/2 this urls



2) Attribute Based Routing
   Route Attribute used to define this routing
   ![image](https://github.com/user-attachments/assets/4556698c-c20c-4810-9023-d1fde6044f1a)

   app.MapControllers();

   this Both is use full for Controller based Routing

   inside controller
   ![image](https://github.com/user-attachments/assets/f366f06d-15b1-4b65-8f09-f872c9f5b7c8)
   if any match present with url('/', 'Home', 'Home/Index') then run Index method of Home Controller.

   ![image](https://github.com/user-attachments/assets/bbafefc9-bfa5-43c4-96f3-47d047bdd340)

   [Route("/")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
       return View("~/Views/Home/Index.cshtml");
   }

   if we declare routing this type then method name any does not matter and we give Path of .cshtml file in action method.

   if we declare route above class component then in this class all url start with this rout
   means
   ![image](https://github.com/user-attachments/assets/2665927c-3998-4987-b1d5-546dc2643c98)

   [Route("~/")]  -> ~ represent null url means http://localhost:2372

   we can also pass a token means automaticaly controller name, action method name replace
   **[Route("[controller]")]  -> use for controller
   [Route("[action]")]  -> use for action**

   this is a best way
   ![image](https://github.com/user-attachments/assets/5a1963a3-3f09-45d8-9517-d40251b1590c)


   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


   Map, httpMethod -> MapGet, MepPost, MapPut, MapDelete
   Default Get Method
   ![image](https://github.com/user-attachments/assets/069bfa7b-375e-48e1-a4e4-768fa943087d)
   Above is use for return Single line string

   Better and use full way is in Programe.cs file
   ![image](https://github.com/user-attachments/assets/85e3b61e-6f0c-42b7-9765-ae1e6b43b464)

   ![image](https://github.com/user-attachments/assets/3ddb3dfb-603b-451a-a67f-486d338f10f0)


   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   ##**Controller**
   controller is use to definr set of action method
   is a backbone of MVC
   recommended that class name of a controller ends with suffix 'Controller'
   **Microsoft.AspNetCore.Mvc** contain controller class so extend this namespace when create controller
   ![image](https://github.com/user-attachments/assets/4d5e9071-2eac-4a71-ae82-5baf5eba119a)
   ![image](https://github.com/user-attachments/assets/3398970b-7948-4a1c-a473-930e711375d6)
   ![image](https://github.com/user-attachments/assets/568cb143-3849-4b0a-b01a-c66e54a4d7ab)


   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   ##**Action Method**
   
