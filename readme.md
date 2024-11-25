
# Console App
```
dotnet new console -n ConsoleApp --use-program-main
cd .\ConsoleApp\
dotnet add package Microsoft.Extensions.Configuration
dotnet add package Microsoft.Extensions.DependencyInjection
dotnet add package Microsoft.Extensions.Hosting
dotnet add package Microsoft.Extensions.Logging
```

```
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

namespace ConsoleApp;

class Program
{
    static async Task Main(string[] args)
    {
        var builder = Host.CreateDefaultBuilder(args)
        .ConfigureServices((hostContext, services) =>
        {
            services.AddTransient<IMyLogic, MyLogic>();
            services.AddSingleton<MyApp>();
        });
        var app = builder.Build();
        await app.Services.GetRequiredService<MyApp>().StartAsync();
        Console.WriteLine("Done! Press any key to exit...");
        Console.ReadKey();
    }
}

public interface IMyLogic
{
    void Say(string message);
}

public class MyLogic : IMyLogic
{
    public void Say(string message)
    {
        Console.WriteLine(message);
    }
}

class MyApp
{
    private ILogger<MyApp> _logger;
    private IConfiguration _config;
    private IMyLogic _logic;

    public MyApp(ILogger<MyApp> logger, IConfiguration config, IMyLogic logic)
    {
        _logger = logger;
        _config = config;
        _logic = logic;
    }

    public Task StartAsync()
    {
        _logger.LogInformation(_config["App:Value1"]);
        _logger.LogInformation(_config["App:Value2"]);
        _logic.Say("Hello World!");
        return Task.CompletedTask;
    }
}
```

### appsettings.json
```
{
  "App": {
    "Value1": "A",
    Value2": 1
  }
}
```

### Now open your ConsoleApp.csproj file with an editor and add the following block before the </Project> statement at the bottom:
```
<ItemGroup>
	<Content Include="appsettings.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## Very rough VS Project Template for a .NET Core Console app with Dependency Injection, Configuration and Logging

![console screenshot of logging and custom config](readme-media/console-logging-workaround.png)

[Developer Community Feature Request](https://developercommunity.visualstudio.com/idea/651671/net-core-console-template-with-di-logging-config.html)

[Twitter discussion](https://twitter.com/spottedmahn/status/1151911538183364609?s=20)

**Reference Blog Posts** I used to Put this Together:

- DI: https://gunnarpeipman.com/net/net-core-dependency-injection/
- Config: https://garywoodfine.com/configuration-api-net-core-console-application/
- Logging: https://www.blinkingcaret.com/2018/02/14/net-core-console-logging/

## ASP.NET Core Console Application - Starting Template
+ https://github.com/ShawnShiSS/aspnetcore-console-app-template

## Console Application Startup Template
```
dotnet tool install -g Volo.Abp.Studio.Cli
abp new Acme.MyConsoleApp -t console --old
```

## C#: Building a Useful, Extensible .NET Console Application Template for Development and Testing
+ https://www.codeproject.com/Articles/816301/Csharp-Building-a-Useful-Extensible-NET-Console-Ap

## Professional Console Apps Done Right
+ https://codingfreaks.de/consoleapp-01/

## .NET Generic Host
+ https://learn.microsoft.com/en-us/dotnet/core/extensions/generic-host?tabs=appbuilder

## Top Level Statements
+ https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/program-structure/top-level-statements
