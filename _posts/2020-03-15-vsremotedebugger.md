---	
layout: post	
title: Remote SSH Debuggin tool for Visual Studio 2019 (Arm/Raspberry Pi compatible)
image: /img/hello_world.jpeg	
---	

With the rise of .NET Core and embedded devices such as the Rasperry Pi, I find it frustrating that Microsoft have not added support for remote debugger so that you can quickly deploy and debug your code on the Pi directly. You can do this with [a bit of effort using VsCode](https://www.hanselman.com/blog/VisualStudioCodeRemoteDevelopmentOverSSHToARaspberryPiIsButter.aspx), however, I do not find VsCode good enough yet for using in a professional environment and especially with large solutions.

Therefore, I developed an extension for Visual Studio 2019 that fills the gap. You can download it from [here](https://github.com/radutomy/VSRemoteDebugger/releases/)


Setup is simple - once installed, go to `Tools -> Settings -> VsRemoteDebugger` and configure your Raspberry Pi IP address, username and remote output directory. 


![alt text](/img/p1/img1.jpg)

Once the above is configured, open a C# solution and click on `Tools -> Start Remote Debugging`

![alt text](/img/p1/img2.jpg)

This will start the remote debugging process.

