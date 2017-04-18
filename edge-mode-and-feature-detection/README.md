Edge Mode and Feature Detection
========================================
You are messaged by a colleague requesting your assistance. He is working on a Mobile-First web application to serve as a promotional site for Contoso Industries. He needs you to check his code looking for common mistakes. After all, you are the expert when it comes to web development!

In this lab you will learn about HTML5 document support in Microsoft's new Edge browser, and how document declarations differ from Internet Explorer to Edge. Additionally, you will improve the application's feature detection mechanism.

This lab includes the following tasks:

1. [HTML5 Document Setup](#Task1)
1. [Feature Detection](#Task2)

<a name="Task1" />
## HTML5 Document Setup

In this exercise you will learn about the deprecation of legacy Document Modes from Internet Explorer in favor of standard support for HTML5 Document Types in Microsoft Edge.

The first step is to open our colleague's application.

1. Start Visual Studio and open [ContosoIndustries/ContosoIndustries.sln](../code/end/ContosoIndustries/ContosoIndustries.sln).

	![Open Solution](images/open-solution.png?raw=true)

	_Open the solution_

1. Make sure "Launch Windows App" is selected in the Start button located on the Visual Studio toolbar.

	![Launch Windows App option](./images/launch-edge.png)

	_The first time you launch the app, select Microsoft Edge from the list of options_

1. Press **F5** to build and debug the application. Take time to familiarize yourself with the application. Once you have become acquainted with the app, close the app and stop debugging.

	![App running in Microsoft Edge](images/app-in-edge.png?raw=true)

	_App running in Microsoft Edge_

	**Compatibility Note:** Go back to Visual Studio and open **Layout.cshtml** under **Views\Shared**.

	You will notice the document declaration `<!DOCTYPE html>` at the top of the page. This is the standard `DOCTYPE` declaration for HTML5 apps. Microsoft Edge provides first class support for HTML5 documents; non-standard `Document Mode` meta tags like this one are no longer required or supported:

	````XML
	<meta http-equiv="X-UA-Compatible" content="Edge" />
	````

1. Again press **F5** to build and debug the application.

	Visual Studio starts launching the app, but the application throws a JavaScript error and fails to start correctly. As you can see, the origin of this error is in **Index.cshtml** because **attachEvent** is not supported.

	![JavaScript error running in Edge Mode](images/javascript-error-running-in-edge-mode.png?raw=true)

	_JavaScript error while running the app in Edge Mode_

1. Click Break in the dialog box and stop debugging.

1. Open the **Index.cshtml** file under **Views\Home** and scroll until you find the offending code:

	````JavaScript
	<script>
		 window.attachEvent("onload", function () {
			setTimeout(function () { jwplayer().play(true); }, 500);
		 });
	</script>
	````

	The application is using **attachEvent** to subscribe to the **load** event. This non-standard mechanism for adding events was deprecated in IE 11 and entirely removed, thus it is absent from Microsoft Edge. As you can see, the error surfaces now that the app is running in Edge.

	You need to update the code to call **addEventListener** in order to register an event handler.

1. Update the code to match the following:
	<!-- mark:2 -->
	````JavaScript
	<script>
		 window.addEventListener("load", function () {
			setTimeout(function () { jwplayer().play(true); }, 500);
		 });
	</script>
	````

1. Press **F5** to build and debug the application.

	Notice that the application now launches correctly.

	![App running in Microsoft Edge](images/app-in-edge.png?raw=true)

	_App running in Microsoft Edge_

1. Switch back to Visual Studio and stop debugging.

<a name="Task2" />
## Feature Detection

_Feature Detection_ is the modern way of building a website that looks and behaves its best in the different browsers and browser versions. It replaces the old error-prone approach of "sniffing" the browser type and version and trying to adjust the code based on that.
With Feature Detection you are detecting specific features using the availability of native functions and objects in the user's browser.

In this exercise you will improve the Feature Detection mechanics of the application.


1. Switch back to Visual Studio.
1. Open the **Index.cshtml** file under **Views\Home** and scroll until you find the second `script` section with the following code:
	<!-- mark:4-14 -->
	````JavaScript
	<script>
		 var comingSoonMessage = document.getElementById("hidden-message");

			function isValidBrowser() {
				var myNav = navigator.userAgent.toLowerCase();
				var version;

				if (myNav.indexOf('msie') != -1) {
					 version = parseInt(myNav.split('msie')[1])
					 return version > 8;
				}

				return true;
		  }

		  if (isValidBrowser()) {
				comingSoonMessage.className = "fadeIn";
		  } else {
				comingSoonMessage.className = "";
		  }
	</script>
	````

	Upon inspection, you can see that the code is relying on User Agent Sniffing to infer if CSS Transitions are available. As previously mentioned, this mechanism for establishing the availability of certain features is highly discouraged and was replaced with "Feature Detection". One of the most common tools Feature Detection tools is [Modernizr](http://modernizr.com/), a JavaScript library for Feature Detection. The application already includes Modernizr so you will update the code to consume it.

1. Update the code shown above to match the following:

	<!-- mark:3 -->
	````JavaScript
	<script>
		 var comingSoonMessage = document.getElementById("hidden-message");
		 if (Modernizr.cssanimations) {
			  comingSoonMessage.className = "fadeIn";
		 } else {
			  comingSoonMessage.className = "";
		 }
	</script>
	````

1. Press **F5** to build and debug the application.

	The application opens in Microsoft Edge. Notice how at the top left the message "COMING SOON!" fades in. The behavior you observe is the same, but the code is better.

	![Coming soon fading in](images/coming-soon-fading-in.png?raw=true)

	_Coming soon label fades in_

1. Stop debugging.

You can find out about other features that can be detected using Modernizr [here](http://modernizr.com/docs/).

### Summary
In this lab you have learned about standard HTML5 document support in Microsoft Edge and how to use Feature Detection via Modernizr.
