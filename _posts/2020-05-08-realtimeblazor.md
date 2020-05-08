---	
layout: post	
title: Real Time Page Updates in Blazor (p1)
image: /img/p3/header.png
---	

## This is a 3 part series:
- Part I is all about building the Blazor client and demonstrating how we can process events in real time
- In Part II  we'll see how we can leverage Azure Pipelines and Github Pages in order to build a complete CI pipeline that leverages the cloud to builds and deploy the application automatically
- Finally, in part III we're adding unit testing via Azure 

## Introduction

### We are going to demo a Blazor web assembly application that receives real time information from a service and updates the page asynchronously based on the price change.

For the purpose of this demo we're going to display the price in real time for the 5 most popular cryptocurrencies (Bitcoin, Ethereum, Ripple, Bitcoin Cash and Litecoin)

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

Now let's assume we have a service that provides us a hook and calls us back when there's a change. The API would look something like this:

```csharp
public interface ICryptoService : IDisposable
{
	event Action<ICrypto> OnPriceUpdate;
	IEnumerable<ICrypto> GetList();
}
```

