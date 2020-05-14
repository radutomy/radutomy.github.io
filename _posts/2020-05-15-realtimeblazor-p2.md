---	
layout: post	
title: Real Time Page Updates in Blazor (P1)
image: /img/p3/header.png
---	

## This is a 3 part series:
- [Part I convers building the Blazor client and demonstrating how it can be used to process events in real time](https://radutomy.github.io/2020-05-08-realtimeblazor-p1/)
- [In Part II  we demonstrate how we can leverage Azure and Github Pages in order to build a complete CI pipeline that builds and deploys the application automatically](https://radutomy.github.io/2020-05-08-realtimeblazor-p2/)
- Finally, in part III we're adding covering unit testing via Azure

## Introduction

In the previous post we built a Blazor Web Assembly client application that displays the price for a basket of cryptocurrencies in real time. In this post we'll learn how to build the repository using Azure and deploy the page to Github Pages automatically.  

We'll assume you have an Azure dev account and the repository is hosted on GitHub.

Head over to Azure dashboard, click on Pipelines -> New Pipeline

![alt text][1]

Select the repository where the Blazor app is hosted and then continue. On the next page you'll be asked to chose your pipeline configuration. Select 



Demo pagee: [https://radutomy.github.io/RealTimeBlazor/](https://radutomy.github.io/RealTimeBlazor/)
Source code: [https://github.com/radutomy/RealTimeBlazor](https://github.com/radutomy/RealTimeBlazor)

[1]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img1.JPG
[2]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img2.JPG
[3]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img3.JPG
[4]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img4.JPG