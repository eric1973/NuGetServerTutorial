# NuGetServerTutorial
Demonstration of how you can setup your own NuGet Server and consume package feeds.

The setup of a NuGet Repository is very well documented at: https://docs.nuget.org/create/hosting-your-own-nuget-feeds

My tutorial should only lift the burden of creating the solution and to get up and running fast
in a NuGet aware ecosystem.

#Create a local NuGet Server 

		- Make sure you habe IIS up and running.
		- Create an empty ASP.NET Web App
		- In Project properties window goto Web and select "Local IIS" as server target.
		  The project URL is: http://localhost/NuGetServerWebApp. Right beside that click
		  on "Create Virtual Directory".
		- Install the NuGet.Server package into that app		  
		- Add Packages to the Packages folder. To test it I downloaded a random package 
		  from nuget.org and put it into the folder "~/Packages".
		  
		  The package I used is : TestPackage.ReadMePackage 1.2.0
		  
		  "~/Packages" is a Project Folder you see in your ASP.NET Web App Project.
		  To see a copied NuGet Package in the Solution Explorer select "Show All Files" in the
		  Solution Explorer toolbar above the solution.
		  
		  The package path can also be set to a different location via the Web.config:
		  
			<!--
            Change the path to the packages folder. Default is ~/Packages.
            This can be a virtual or physical path.
			-->
			<add key="packagesPath" value=""/>
			
		  But to keep things simply I decided to leave it at the default path "~/Packages" which
		  is the case when the value attribute is left blank.
		  
		- In Web.config you will find the "apiKeySetting":
		
			<!--
				Determines if an Api Key is required to push\delete packages from the server. 
			-->
			<add key="requireApiKey" value="true"/>
			<!-- 
					Set the value here to allow people to push/delete packages from the server.
					NOTE: This is a shared key (password) for all users.
			-->
			<add key="apiKey" value=""/>
			
			That "apikey" is used to publish your own NuGet package to that NuGet Server feed.
			To keep it simply I disabled the requirement of an "apikey" and set the "requireApiKey" to
			false like: (If your network is not secure consider to activate it of course!)
			
			<!--
				Determines if an Api Key is required to push\delete packages from the server. 
			-->
			<add key="requireApiKey" value="false"/>
			<!-- 
					Set the value here to allow people to push/delete packages from the server.
					NOTE: This is a shared key (password) for all users.
			-->
			<add key="apiKey" value=""/>
		
		- Deploy and run your brand new Package Feed. Hit CTRL + F5 and run it.
		  
		  At  http://localhost/NuGetServerWebApp you should see the Web Interface
		  of the NuGet Server with information like:
		  
			You are running NuGet.Server v2.8.60717.93

			Click here to view your packages.

			Repository URLs
			In the package manager settings, add the following URL to the list of Package Sources:
			http://localhost/NuGetServerWebApp/nuget
			
			To enable pushing packages to this feed using the nuget command line tool (nuget.exe). 
			Set the api key appSetting in web.config.
			
			nuget push {package file} -s http://localhost/NuGetServerWebApp/ {apikey}
			
			To add packages to the feed put package files (.nupkg files) in the folder "C:\dev\NuGetTutorial\NuGetServerWebApp\Packages".

			
		- Now is time for the funny part. Add the Repository URL to your NuGet Servers in Visual Studio
		  -> Tools -> Options -> NuGet Package Manager -> Package Sources. 
		  
		  Click on the plus (add) button and name it for example "My local NuGet Server" and in the source
		  textfield enter the repository URL: http://localhost/NuGetServerWebApp/nuget. Click "Update" and 
		  hit "Ok".
		  
		  So now you have a new NuGet Server Source available where you can fetch Nuget packages from.
		  

#Use your newly created local NuGet Server 
		  
		- To test the new local NuGet Server I created a new console app project where I want to use 
		  that sample package I added in the server. 
		  
		  I called it : NuGetConsumerConsoleUI
		  
		  Right-click -> Manage Nuget Packages on the solution. 
		  Then in the NuGet Package Manager window change the Package Source to "My local NuGet Server".
		  When you select "Browse" that package should be found:
		  
		  TestPackage.ReadMePackage 1.2.0
		  
		  Now you can install it in the project "NuGetConsumerConsoleUI" as any other packages you 
		  previously installed from nuget.org source.
		  
		  That's it!
		  
		  
