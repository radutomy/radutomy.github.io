---	
layout: post	
title: Real Time Page Updates in Blazor (P1)
image: /img/p3/header.png
---	

## This is a 3 part series:
- [Part I convers building the Blazor client and demonstrating how it can be used to process events in real time](https://radutomy.github.io/2020-05-08-realtimeblazor/)
- In Part II  we demonstrate see we can leverage Azure and Github Pages in order to build a complete CI pipeline that leverages the cloud to builds and deploy the application automatically
- Finally, in part III we're adding covering unit testing via Azure

## Introduction

In this post we are showcasing a Blazor web assembly application that receives real time information from a service and updates the page asynchronously based on the price change. For the purpose of this demo we're going to display the price in real time for the 5 most popular cryptocurrencies (Bitcoin, Ethereum, Ripple, Bitcoin Cash and Litecoin)

Let's start by describing the model:

```csharp
interface ICrypto
{
	string Name { get; }
	Ticker Ticker { get; }
	decimal Price { get; set; }
}

public enum Ticker
{
	BTC, ETH, XRP, BCH, LTC
}
```

Now let's assume we have a service that we can subscribe to and calls us back when there's a price change. The API design would be something in these lines:

```csharp
public interface ICryptoService : IDisposable
{
	event Action<ICrypto> OnPriceUpdate;
	IEnumerable<ICrypto> GetList();
}
```

The service gets registered in the services container

```c#
builder.Services.AddTransient(ts => new CryptoService());
```

Upon initialising the main view, we're injecting the service and requesting the items that need to be served on the page, while also registering the service callback. Here we're using a table, and we're binding the list of items to the table. In order to maintain a clean coding pattern, the view implements IDisposable which takes care of destroying bindings and freeing up resources when the user navigates away from the page.

As the price for the items change and the service issues callbacks, we have to update the list in order to reflect the new value and then request the DOM to rerender the table so that the new values are displayed correctly.

```c#
@page "/"

@using RealTimeBlazor.Services
@using RealTimeBlazor.Data
@inject CryptoService CryptoService
@implements IDisposable

<MatTable Items="@_cryptos" ShowPaging="false">
    <MatTableHeader>
        <th>Name</th>
        <th>Ticker</th>
        <th>Price</th>
    </MatTableHeader>
    <MatTableRow>
        <td>@context.Name</td>
        <td>@context.Ticker</td>
        <td>$@context.Price</td>
    </MatTableRow>
</MatTable>

@code {

    private IEnumerable<ICrypto> _cryptos;

    protected override void OnInitialized()
    {
        CryptoService.OnPriceUpdate += PriceUpdate;
        _cryptos = CryptoService.GetList();
    }

    private async void PriceUpdate(ICrypto newValue)
    {
        var toUpdate = _cryptos.FirstOrDefault(c => c.Ticker == newValue.Ticker);
        toUpdate.Price = newValue.Price;

        await InvokeAsync(StateHasChanged);
    }

    public void Dispose()
    {
        CryptoService.OnPriceUpdate -= PriceUpdate;
    }
}

```

One important note is that rerendering the UI via invoking `StateHasChanged` is not an atomic operation - the DOM has complete control over when the rerendering takes place.

Demo pagee: [https://radutomy.github.io/RealTimeBlazor/](https://radutomy.github.io/RealTimeBlazor/)
Source code: [https://github.com/radutomy/RealTimeBlazor](https://github.com/radutomy/RealTimeBlazor)