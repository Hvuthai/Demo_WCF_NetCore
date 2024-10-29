# Demo_WCF_NetCore (Server)

# Step 1: Install CoreWCF Template

Turn on cmd and patse this command:
```bash
dotnet new install CoreWCF.Templates 
```

## Step 2: Create new CoreWCF Service project
Delete "IService" class, Add "ICaculator" interface and "Caculator" class implement "ICaculator"
ICaculator
```bash
    [ServiceContract]
    public interface ICaculator
    {
        [OperationContract]
        double Add(double n1, double n2);
        [OperationContract]
        double Subtract(double n1, double n2);
        [OperationContract]
        double Multiply(double n1, double n2);
        [OperationContract]
        double Divide(double n1, double n2);
    }
```
Caculator
```bash
 public class Caculator : ICaculator
 {
     public double Add(double n1, double n2)
     {
         double result = n1 + n2;
         Console.WriteLine("Received Add({0},{1})", n1, n2);
         // Code added to write output to the console window.
         Console.WriteLine("Return: {0}", result);
         return result;
     }

     public double Subtract(double n1, double n2)
     {
         double result = n1 - n2;
         Console.WriteLine("Received Subtract({0},{1})", n1, n2);
         Console.WriteLine("Return: {0}", result);
         return result;
     }

     public double Multiply(double n1, double n2)
     {
         double result = n1 * n2;
         Console.WriteLine("Received Multiply({0},{1})", n1, n2);
         Console.WriteLine("Return: {0}", result);
         return result;
     }

     public double Divide(double n1, double n2)
     {
         double result = n1 / n2;
         Console.WriteLine("Received Divide({0},{1})", n1, n2);
         Console.WriteLine("Return: {0}", result);
         return result;
     }
 }
```
## Step 3: Add service and config binding type, endpoint.
Open Program.cs
```bash
var builder = WebApplication.CreateBuilder();

builder.Services.AddServiceModelServices();
builder.Services.AddServiceModelMetadata();
builder.Services.AddSingleton<IServiceBehavior, UseRequestHeadersForMetadataAddressBehavior>();

var app = builder.Build();

app.UseServiceModel(serviceBuilder =>
{
    serviceBuilder.AddService<Caculator>();
    serviceBuilder.AddServiceEndpoint<Caculator, ICaculator>(new BasicHttpBinding(BasicHttpSecurityMode.Transport), "/Service.svc");
    var serviceMetadataBehavior = app.Services.GetRequiredService<ServiceMetadataBehavior>();
    serviceMetadataBehavior.HttpsGetEnabled = true;
});

app.Run();
```
Run and chill

# Demo_WCF_NetCore_Client (Client)
## Step 1: Create new console app
## Step 2: Add Service reference
- Right click on the project
- Add -> Add Service Reference -> WCF Web Service -> Next
![image](https://github.com/user-attachments/assets/c4cfdb7e-182a-48fb-b748-ef6c0128100c)
- Enter the Server endpoint and click go
![image](https://github.com/user-attachments/assets/01790180-69c5-4852-bf15-b5d11866942a)
- Click next -> next -> finish -> after the add progess click close
## Step 3: Create client instance and call service
In program.cs
```bash
 CaculatorClient client = new CaculatorClient();
 // Step 2: Call the service operations.
 // Call the Add service operation.
 double value1 = 100.00D;
 double value2 = 15.99D;
 double result = client.AddAsync(value1, value2).Result;
 Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

 // Call the Subtract service operation.
 value1 = 145.00D;
 value2 = 76.54D;
 result = client.SubtractAsync(value1, value2).Result;
 Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

 // Call the Multiply service operation.
 value1 = 9.00D;
 value2 = 81.25D;
 result = client.MultiplyAsync(value1, value2).Result;
 Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

 // Call the Divide service operation.
 value1 = 22.00D;
 value2 = 7.00D;
 result = client.DivideAsync(value1, value2).Result;
 Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);

 // Step 3: Close the client to gracefully close the connection and clean up resources.
 Console.WriteLine("\nPress <Enter> to terminate the client.");
 Console.ReadLine();
 client.Close();
```
## Step 4: Run Server and Client and chill😘
