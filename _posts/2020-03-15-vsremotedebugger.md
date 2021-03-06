---	
layout: post	
title: Remote SSH Debugging with Visual Studio 2019
image: /img/p1/header.png
---	

With the rise of .NET Core and embedded devices such as the Rasperry Pi, I find it frustrating that Microsoft have not added support for remote debugger so that you can quickly deploy and debug your code on the Pi directly. You can do this with [a bit of effort using VsCode](https://www.hanselman.com/blog/VisualStudioCodeRemoteDevelopmentOverSSHToARaspberryPiIsButter.aspx), however, I do not find VsCode good enough yet for using in a professional environment and especially with large solutions.

Therefore, I developed an extension for Visual Studio 2019 that fills the gap. You can download it from [here](https://github.com/radutomy/VSRemoteDebugger/releases/)

Make sure you have public key authentification set up with your remote system and that your private key on your local machine is in `~\.ssh\id_rsa` and on your local machine `~/.ssh/authorized_keys`

Then, once the extension is installed, in VS 2019 go to `Tools -> Settings -> VsRemoteDebugger` and configure your Raspberry Pi IP address, username and remote output directory. 

![alt text][1]

Open a C# solution and click on `Tools -> Start Remote Debugging`.

![alt text][2]


Patience! When it's running for the first time, the extension is trying to setup the debugging server on the remote machine and visual feedback is not great - depending on your internet connection **it might take up to one minute** to get everything set up correctly.


Source code: [https://github.com/radutomy/VSRemoteDebugger](https://github.com/radutomy/VSRemoteDebugger)

[1]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p1/img1.JPG
[2]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p1/img2.JPG