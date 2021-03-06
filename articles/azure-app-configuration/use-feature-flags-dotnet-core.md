---
title: Zelf studie voor het gebruik van functie vlaggen in een .NET core-app | Microsoft Docs
description: In deze zelf studie leert u hoe u functie vlaggen implementeert in .NET Core-Apps.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 3182961f928a9befc5a55fb6d58e22c74ba81089
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "79473435"
---
# <a name="tutorial-use-feature-flags-in-an-aspnet-core-app"></a>Zelf studie: functie vlaggen gebruiken in een ASP.NET Core-app

De beheer bibliotheken van de .NET-kern functie bieden idiomatisch-ondersteuning voor het implementeren van functie vlaggen in een .NET-of ASP.NET Core-toepassing. Met deze bibliotheken kunt u functie vlaggen declaratief toevoegen aan uw code, zodat u niet alle `if` instructies hiervoor hand matig hoeft te schrijven.

De functie beheer bibliotheken beheren ook de levens cyclus van functie vlaggen achter de schermen. Bijvoorbeeld: de bibliotheken vernieuwen en cache vlag statussen of garanderen dat een vlag status onveranderbaar is tijdens het aanroepen van een aanvraag. Daarnaast biedt de ASP.NET Core-bibliotheek out-of-the-box-integraties, waaronder MVC-controller acties, weer gaven, routes en middleware.

[Met de functie vlaggen toevoegen aan een ASP.net core-app Quick Start kunt](./quickstart-feature-flag-aspnet-core.md) u op verschillende manieren functie vlaggen toevoegen aan een ASP.net core-toepassing. In deze zelf studie worden deze methoden uitgebreid beschreven. Zie de [documentatie over het beheer van ASP.net core-onderdelen](https://go.microsoft.com/fwlink/?linkid=2091410)voor een volledige referentie.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Voeg functie vlaggen toe aan de belangrijkste onderdelen van uw toepassing om de beschik baarheid van functies te bepalen.
> * Integreer met app-configuratie wanneer u deze gebruikt voor het beheren van functie vlaggen.

## <a name="set-up-feature-management"></a>Functie beheer instellen

Voeg een verwijzing naar het `Microsoft.FeatureManagement` NuGet-pakket toe om gebruik te maken van het .net Core-functie beheer.
    
De functie vlaggen van het `IFeatureManager` Framework van het systeem eigen configuratie systeem worden in .net core feature Manager opgehaald. Als gevolg hiervan kunt u de functie vlaggen van uw toepassing definiëren met behulp van een configuratie bron die .NET core ondersteunt, inclusief het lokale bestand *appSettings. json* of omgevings variabelen. `IFeatureManager`is afhankelijk van de invoer van .NET core-afhankelijkheid. U kunt de functie Beheer Services registreren met behulp van standaard conventies:

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
}
```

Standaard haalt functie beheer functie vlaggen op uit de `"FeatureManagement"` sectie van de .net core-configuratie gegevens. In het volgende voor beeld wordt aangegeven dat de functie beheer van een andere `"MyFeatureFlags"` sectie wordt genoemd:

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement(options =>
        {
                options.UseConfiguration(Configuration.GetSection("MyFeatureFlags"));
        });
    }
}
```

Als u filters in uw functie vlaggen gebruikt, moet u een extra bibliotheek toevoegen en registreren. In het volgende voor beeld ziet u hoe u een ingebouwd functie filter gebruikt `PercentageFilter`met de naam:

```csharp
using Microsoft.FeatureManagement;
using Microsoft.FeatureManagement.FeatureFilters;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement()
                .AddFeatureFilter<PercentageFilter>();
    }
}
```

U wordt aangeraden de functie vlaggen buiten de toepassing te blijven en deze afzonderlijk te beheren. Als u dit doet, kunt u de status van de vlaggen op elk gewenst moment wijzigen. deze wijzigingen worden direct doorgevoerd in de toepassing. App-configuratie biedt een centrale locatie voor het organiseren en beheren van al uw functie vlaggen via een speciale portal-gebruikers interface. App-configuratie levert ook rechtstreeks de vlaggen aan uw toepassing via de .NET core-client bibliotheken.

De eenvoudigste manier om uw ASP.NET Core-toepassing te koppelen aan app-configuratie is via `Microsoft.Azure.AppConfiguration.AspNetCore`de configuratie provider. Volg deze stappen om dit NuGet-pakket te gebruiken.

1. Open het *Program.cs* -bestand en voeg de volgende code toe.

   ```csharp
   using Microsoft.Extensions.Configuration.AzureAppConfiguration;

   public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
       WebHost.CreateDefaultBuilder(args)
              .ConfigureAppConfiguration((hostingContext, config) => {
                  var settings = config.Build();
                  config.AddAzureAppConfiguration(options => {
                      options.Connect(settings["ConnectionStrings:AppConfig"])
                             .UseFeatureFlags();
                   });
              })
              .UseStartup<Startup>();
   ```

2. Open *Startup.cs* en werk de `Configure` methode bij om een middleware toe te voegen, zodat de waarden van de functie vlag kunnen worden vernieuwd met een terugkerend interval terwijl de ASP.net core web-app aanvragen blijft ontvangen.

   ```csharp
   public void Configure(IApplicationBuilder app, IHostingEnvironment env)
   {
       app.UseAzureAppConfiguration();
       app.UseMvc();
   }
   ```

De waarden van de functie vlag worden naar verwachting in de loop van de tijd gewijzigd. Standaard worden de waarden van de functie vlag in de cache opgeslagen gedurende een periode van 30 seconden, waardoor een vernieuwings bewerking wordt geactiveerd wanneer de middleware een aanvraag ontvangt, wordt de waarde niet bijgewerkt totdat de waarde in cache verloopt. De volgende code laat zien hoe u de verval tijd van de cache of het polling-interval wijzigt `options.UseFeatureFlags()` in 5 minuten in de aanroep.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.CacheExpirationTime = TimeSpan.FromMinutes(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Declaratie van functie vlaggen

Elke functie vlag bestaat uit twee delen: een naam en een lijst met een of meer filters die worden gebruikt om te evalueren of de status van een functie is *ingeschakeld* (dat wil zeggen, `True`wanneer de waarde ervan is). Een filter definieert een use-case voor wanneer een functie moet worden ingeschakeld.

Wanneer een functie vlag meerdere filters heeft, wordt de filter lijst in de juiste volg orde gepasseerd totdat een van de filters bepaalt dat de functie moet worden ingeschakeld. Op dat moment is de functie vlag *ingeschakeld*en worden alle resterende filter resultaten overgeslagen. Als geen filter aangeeft dat de functie moet worden ingeschakeld, is de functie vlag *uitgeschakeld*.

De functie beheerder ondersteunt *appSettings. json* als een configuratie bron voor functie vlaggen. In het volgende voor beeld ziet u hoe u functie vlaggen kunt instellen in een JSON-bestand:

```JSON
"FeatureManagement": {
    "FeatureA": true, // Feature flag set to on
    "FeatureB": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

Per Conventie wordt de `FeatureManagement` sectie van dit JSON-document gebruikt voor instellingen voor functie vlaggen. Het vorige voor beeld toont drie functie vlaggen waarvoor de filters zijn gedefinieerd `EnabledFor` in de eigenschap:

* `FeatureA`is *ingeschakeld*.
* `FeatureB`is *uit*.
* `FeatureC`Hiermee geeft u een `Percentage` filter op `Parameters` met de naam met een eigenschap. `Percentage`is een configureerbaar filter. In dit voor beeld `Percentage` geeft u een kans van 50 procent `FeatureC` *op*voor de vlag.

## <a name="feature-flag-references"></a>Naslag informatie over functie vlaggen

Zodat u gemakkelijk naar functie vlaggen in code kunt verwijzen, moet u deze definiëren als `enum` variabelen:

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-checks"></a>Controles van functie vlaggen

Het basis patroon van functie beheer is om eerst te controleren of een functie vlag is ingesteld op *aan*. Als dit het geval is, voert het onderdeel beheer de acties uit die de functie bevat. Bijvoorbeeld:

```csharp
IFeatureManager featureManager;
...
if (await featureManager.IsEnabledAsync(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Afhankelijkheidsinjectie

In ASP.NET Core MVC kunt u de functie beheer `IFeatureManager` openen via afhankelijkheids injectie:

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;

    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

## <a name="controller-actions"></a>Controller acties

In MVC-controllers gebruikt u het `FeatureGate` kenmerk om te bepalen of een volledige controller klasse of een specifieke actie is ingeschakeld. De volgende `HomeController` controller moet `FeatureA` zijn *ingeschakeld* voordat er een actie kan worden uitgevoerd die de klasse controller bevat:

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

De volgende `Index` actie moet `FeatureA` worden *ingeschakeld* voordat deze kan worden uitgevoerd:

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Wanneer een MVC-controller of-actie wordt geblokkeerd omdat de vlag voor *off*het bepalen van de `IDisabledFeaturesHandler` functie is uitgeschakeld, wordt een geregistreerde interface aangeroepen. De standaard `IDisabledFeaturesHandler` interface retourneert een 404-status code naar de client zonder antwoord tekst.

## <a name="mvc-views"></a>MVC-weer gaven

In MVC-weer gaven kunt u een `<feature>` tag gebruiken om inhoud weer te geven op basis van het feit of een functie vlag is ingeschakeld:

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

Om alternatieve inhoud weer te geven wanneer niet aan de vereisten `negate` wordt voldaan, kan het kenmerk worden gebruikt.

```html
<feature name="FeatureA" negate="true">
    <p>This will be shown if 'FeatureA' is disabled.</p>
</feature>
```

Het onderdeel `<feature>` label kan ook worden gebruikt voor het weer geven van inhoud als een of alle functies in een lijst zijn ingeschakeld.

```html
<feature name="FeatureA, FeatureB" requirement="All">
    <p>This can only be seen if 'FeatureA' and 'FeatureB' are enabled.</p>
</feature>
<feature name="FeatureA, FeatureB" requirement="Any">
    <p>This can be seen if 'FeatureA', 'FeatureB', or both are enabled.</p>
</feature>
```

## <a name="mvc-filters"></a>MVC-filters

U kunt MVC-filters zo instellen dat ze worden geactiveerd op basis van de status van een functie vlag. Met de volgende code wordt een MVC- `SomeMvcFilter`filter toegevoegd met de naam. Dit filter wordt alleen in de MVC-pijp lijn geactiveerd als `FeatureA` dit is ingeschakeld. Deze mogelijkheid is beperkt tot `IAsyncActionFilter`. 

```csharp
using Microsoft.FeatureManagement.FeatureFilters;

IConfiguration Configuration { get; set;}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options => {
        options.Filters.AddForFeature<SomeMvcFilter>(nameof(MyFeatureFlags.FeatureA));
    });
}
```

## <a name="middleware"></a>Middleware

U kunt ook functie vlaggen gebruiken om toepassings vertakkingen en middleware toe te voegen. Met de volgende code wordt alleen een middleware-onderdeel in de aanvraag pijplijn `FeatureA` ingevoegd wanneer is ingeschakeld:

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

Deze code bouwt voort op de meer generieke mogelijkheden om de hele toepassing te vertakking op basis van een functie vlag:

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u geleerd hoe u functie vlaggen kunt implementeren in uw ASP.NET Core-toepassing `Microsoft.FeatureManagement` met behulp van de bibliotheken. Raadpleeg de volgende bronnen voor meer informatie over de ondersteuning van functie beheer in ASP.NET Core en app-configuratie:

* [Voorbeeld code voor functie vlag ASP.NET Core](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Documentatie voor micro soft. FeatureManagement](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [Functievlaggen beheren](./manage-feature-flags.md)
