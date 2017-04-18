Web Development Best Practices
========================================
In this lab you will continue improving your web application to follow some of the web development Best Practices that will greatly improve how your application works in the different browsers and mobile devices.

This hands-on lab includes the following exercises:

1. [Correctly Handling Touch with hand.js](#Task1)
1. [Working with CSS](#Task2)
1. [Plugin-free Development](#Task3)


<a name="Task1" />
## Correctly Handling Touch with hand.js
Touch devices are becoming more and more prevalent these days, which is why it is so important to make sure that you write code that supports both desktop and mobile form factors.

In this task you will use **Hand.js**, a framework for unifying touch, pen and mouse events (these are called collectively _pointer events_).

**Hand.js** is a polyfill (downloadable code which provides facilities that are not built into a web browser) that allows you to write touch code only once, even if the browser does not support pointer events.

1. Open the **Sign.html** file, located in the root folder of the project.

2. Find the empty `script` tags and paste the following code inside:

	<!-- mark:2-10 -->
	````JavaScript
    <script>
        var canvas = document.getElementById("drawSurface");
        var context = canvas.getContext("2d");
        context.fillStyle = "rgba(0, 0, 0, 0.5)";

        canvas.addEventListener("mousemove", paint, false);

        function paint(event) {
            context.fillRect(event.clientX, event.clientY, 10, 10);
        }
    </script>
	````

	The preceding code, will draw a black rectangle when the mouse cursor hovers over the canvas area.

	But what if you want to use touch? **Only MouseDown/Up/Click works** with touch. This means that you can only fire the events with touch when you tap on the screen, not when you move your finger on the screen.
	Using touch in the example, it will only draw a unique square at the exact position where you tap the canvas element. As soon as you try to move your finger in the canvas element, the browser will try to pan inside the page because it is the default behavior being defined.

3. Run the application. When it opens, navigate to the Postcard page and try signing by using the mouse, and your fingers.
			![signing-the-postcard](images/signing-the-postcard.png?raw=true)

	_Signing the postcard_

	You then need to override the default behavior of the browser and tell it to redirect the touch events to the JavaScript code rather than trying to interpret it.

1. In **Sign.html**, locate the empty **style** tags and  paste the following CSS rule inside them. This tells the canvas not to react to the default behavior:

	<!-- mark:2-4 -->
	````CSS
    <style>
        #drawSurface {
            touch-action: none; /* Disable touch behaviors, like pan and zoom */
        }
    </style>
	````

	Now, when you move your finger inside the canvas element, it behaves like a mouse pointer. This code only tracks 1 finger because Microsoft Edge maps one finger to simulate a mouse.

5. Run the application again. When it launches, navigate to the postcard page and try signing by using one finger and then two.

	You will notice that multiple fingers are not supported. To support multiple touch points, you can use _Pointer events_, which are intended to abstract any input to a single set of events. You will do so in the next step.

1. In **Sign.html**, locate the line that registers an event listener for **mousemove**. Replace this event with **pointermove**, as shown in the following code:

	<!-- mark:6 -->
	````JavaScript
    <script>
        var canvas = document.getElementById("drawSurface");
        var context = canvas.getContext("2d");
        context.fillStyle = "rgba(0, 0, 0, 0.5)";

        canvas.addEventListener("pointermove", paint, false);

        function paint(event) {
            context.fillRect(event.clientX, event.clientY, 10, 10);
        }
    </script>
	````

	![Unifiying Pen, Touch and Mouse](images/unifiying-pen-touch-and-mouse.png?raw=true)

	_Unifying Pen, Touch & Mouse events_

	Pointer events generally mimic existing mouse events but fire from any pointing input device (mouse, stylus or finger):

	- mousedown => pointerdown
	- mouseenter => pointerenter
	- mousemove => pointermove
	- mouseup => pointerup
	- ...

	However, _Pointer events_ are currently only fully supported in IE 10, 11, and Microsoft Edge.

7. Run the application in Microsoft Edge and notice that multiple fingers work now.

	![signing-the-postcard](images/signing-the-postcard.png?raw=true)

	_Signing the postcard_

	The web application correctly identifies touch events, but only in IE 10, 11, and Edge. If you want to support them in older versions or in other major browsers, you can use **hand.js**.

1. Browse to <https://handjs.codeplex.com/> and download the latest version of the library.

1. Extract the script to the **Scripts** folder of your solution.

1. Include the extracted file in the project, by right-clicking the file and selecting **Include in Project**. **Show All Files** must be enabled to do this.

	![Including hand.js](images/including-hand-js.png?raw=true)

	_Including hand.js_


12. In the **Sign.html** file, add the following reference to the **hand.js** file inside the **head** tags. Notice that the version number may be different.

	````HTML
	<script type="text/javascript" src="Scripts/hand-1.3.8.js"></script>
	````

	> **Note:** We are only referencing the **hand.js** file in the file where we need it. If you have multiple pages where you use pointer events, consider registering a bundle with the script in the **BundleConfig.cs** file and referencing it in your layout view.

13. Run the application. The signing code will now work in every browser and handle touch correctly.

<a name="Task2" />
## Working with CSS

In this task you will learn how to apply some CSS best practices. You will first remove inline styles to classes and apply them to similar elements.

1. Switch back to Visual Studio and expand the **Home** folder under the **Views** folder.

1. Open **Index.cshtml**.

	The file contains a section where styles are defined. A bit futher down you can see the HTML for the navigation bar that is displayed below the video. Notice that the `ul` element has inline styling.

	<!-- mark:1 -->
	````HTML
	<ul data-role="listview" style="margin:10px 0 10px 0;" data-inset="true">
		 <li data-role="list-divider">Navigation</li>
		 <li>@Html.ActionLink("About", "About", "Home")</li>
		 <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
		 <li>@Html.ActionLink("Postcard", "Postcard", "Home")</li>
	</ul>
	````

1. Open **About.cshtml** and notice that the HTML for the navigation bar also contains inline styling, and it is very similar to that of the Index file.

1. Open **Contact.cshtml** and notice that again, the HTML for the navigation bar contains inline styling, and it is very similar to that of the Index and About files.

	To keep a consistent style in the navigation bar, you will now define style classes to be used and replace the inline styles with them.

1. Right-click the **Content** folder and add a new Stylesheet file named **Styles.css**. Add the following rule to it, replacing the default content:

	````CSS
	/* Styles for the navigation bar */
	.navigationlist {
		 margin:10px 0 10px 0;
	}
	````

1. Open **BundleConfig.cs** in the **App_Start** folder and update the line for Content.css in RegisterBundles to look like this:

	````C#
	bundles.Add(new StyleBundle("~/Content/css").Include("~/Content/site.css", "~/Content/styles.css"));

	````

	With this you are including the new stylesheet into all pages of your application. Now you will update all pages to use the new class instead of the inline styles.

1. Update **Index.cshtml**, replacing the `style` attribute of the navigation bar as shown below. Also, give the Navigation header an id.

	<!-- mark:1 -->
	````HTML
	<ul data-role="listview" class="navigationList" data-inset="true">
		 <li data-role="list-divider">Navigation</li>
		 <li>@Html.ActionLink("About", "About", "Home")</li>
		 <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
		 <li>@Html.ActionLink("Postcard", "Postcard", "Home")</li>
	</ul>
	````

1. Similarly, update the HTML for the navigation bar in **About.cshtml** and **Contact.cshtml**.

1. Press **F5** to build and debug the application.

	The website will launch in Edge. Navigate to the About and Contact pages. You should not notice any differences in the way the website is working.

	![Site unchanged after removing inline styles](images/site-unchanged-after-removing-inline-styles.png?raw=true)

	_Navigation bar unchanged after removing inline styles_

	By using classes you can change the style of the navigation bar throughout the website with little effort.

	You will now update the page so that when the viewer hovers over the movie title, the title fades and the synopsis for the movie slides in. To do this you will change the html for the Index page to add the synopsis text. Then you will add styles to make the animation work. In the process you will move the styles that already exist to the Styles.css to keep them in a single location.

1. Open **Index.cshtml**, find the main div, and update it to look as follows:

	<!-- mark:4,9-11 -->
	````HTML
	<p id="hidden-message">
        Coming soon!
    </p>
    <div class="wrapper">
        <div id="movietitle">
            <img id="sintelLogo" src="~/Content/images/Sintel_logo.png" />
            <img id="sintelTitle" src="~/Content/images/Sintel.png" />
        </div>
        <div id="synopsis">
            <p>The film follows a girl named Sintel who is searching for a baby dragon she calls Scales. A flashback reveals that Sintel found Scales with its wing injured and helped care for it, forming a close bond with it. By the time its wing recovered and it was able to fly, Scales was caught by an adult dragon. Sintel has since embarked on a quest to rescue Scales, fending off beasts and warriors along the way. She eventually comes across a cave housing an adult and baby dragon, the latter of which she believes to be Scales. The adult dragon discovers and attacks Sintel, but hesitates to kill her. Sintel slays the dragon, only to recognize the scar on its wing and realize the dragon is an adult Scales, and that she too has aged considerably. Sintel leaves the cave heartbroken, unknowingly followed by Scales's baby.</p>
        </div>
    </div>

	````

1. Scroll up in **Index.cshtml** until you find the `style` element. It contains the styles used in this page. Copy all the content inside the `style` element.

1. Open **Styles.css** and paste the styles copied in the previous step, adding a comment to indicate these are the styles for the Index page. Then add the following styles for the synopsis element and the animation:

	````CSS
	#synopsis {
		padding: 5px 10px 5px 10px;
		font-family: Georgia;
		font-size: 16px;
		position: relative;
		top: 0px;
		left: -100%;
		width: calc( 100% - 30px);
		height: auto;
		display:none;
	}

	.wrapper {
		position: relative;
		overflow: hidden;
		width: calc( 100% - 2px);
		height: auto;
		border: 1px solid black;
	}

	/* Styles for the animation in Index.cshtml */
	.wrapper.animation #synopsis {
		animation: slideOut 3s forwards;
		display: block;
	}

	@keyframes slideOut {
		 0% {left: -100%; color: white;}
		 100% { left: 0px; color: black;}
	}

	.wrapper.animation #movietitle {
		 animation: fadeOut 3s forwards;
	}

	@keyframes fadeOut {
			  0% {opacity: 1; }
			  100% {opacity: 0.1;}
	}
	````

	The styles added use CSS3 Animations to gradually change from one style to another. To accomplish this you need 2 rules:

	* one rule in which the animation is defined:

		````CSS
		@keyframes slideOut {
			 0% {left: -100%; color: white;}
			 100% { left: 0px; color: black;}
		}
		````

		This defines the _slideOut_ animation, which will gradually change the _left_ property of the element it is applied to from -100% (totally hidden) to 0px, giving the appearance of sliding right. It also gradually changes the font color from white to black. As you can see, many properties can be modified. Similarly you can add other steps or percentages in which changes occur (25%, 50%, 75%).

	* a second rule to use the animation:

		````CSS
		.wrapper:hover #synopsis {
			 animation: slideOut 3s forwards;
		}
		````

		This indicates that the synopsis element will have the _slideOut_ animation applied with a duration of 3 seconds, going forward from left to right.

	The resulting **Styles.css** file should look like this:

	````CSS
	/* Styles for the navigation bar */
	.navigationlist {
		  margin:10px 0 10px 0;
	}

	/* Styles for Index.cshtml */
	 .fadeIn {
		  animation: fadeIn 3s forwards;
	 }

	 @keyframes fadeIn {
		  0% {opacity: 0;}
		  100% {opacity: 1;}
	 }

	 #hidden-message{
		  opacity: 0;
		  font-size: 26px;
		  font-weight: bold;
		  text-transform: uppercase;
		  font-family: Garamond;
	 }
	 #movietitle
	 {
		  background-color: black;
		  text-align: center;
		  background-color: black;
		  height: 180px;
	 }

	#synopsis {
		padding: 5px 10px 5px 10px;
		font-family: Georgia;
		font-size: 16px;
		position: relative;
		top: 0px;
		left: -100%;
		width: calc( 100% - 30px);
		height: auto;
		display:none;
	}

	.wrapper {
		position: relative;
		overflow: hidden;
		width: calc( 100% - 2px);
		height: auto;
		border: 1px solid black;
	}

	/* Styles for the animation in Index.cshtml */
	.wrapper.animation #synopsis {
		animation: slideOut 3s forwards;
		display: block;
	}

	@keyframes slideOut {
		  0% {left: -100%; color: white;}
		  100% { left: 0px; color: black;}
	}

	.wrapper.animation #movietitle {
		  animation: fadeOut 3s forwards;
	}

	@keyframes fadeOut {
		 0% {opacity: 1; }
		 100% {opacity: 0.1;}
	}
	````

1. Add the following code at the bottom of the **Index.cshtml** page. This code will add the animation class that will trigger the animation when the user clicks the title.

	````JavaScript
	<script>
	    var wrapper = document.querySelector(".wrapper");
	    wrapper.onclick = function () {
		wrapper.classList.toggle("animation");
	    };
	</script>
	````


1. Press **F5** to build and debug the application in Edge.

	The home page of the website will launch in Edge. Click the title and notice how it expands and how the synopsis text slides in. Then click it again and notice that the synopsis goes away.

	![Animation when hovering over the title IE](images/animation-when-hovering-over-the-title-ie.png?raw=true)

	_Animation visible when hovering over the title_

1. Switch back to Visual Studio and stop debugging.

1. Change the browser in which you are debugging to Chrome.

	![Change browser to Chrome](images/change-browser-to-chrome.png?raw=true)

	_Change debugging browser to Chrome_

1. Press **F5** to debug the application in Google Chrome.

	The home page launches in Chrome. Hover over the title. Notice that the animation does not work like it did in Edge.  

	![Animation not working on Chrome](images/animation-not-working-on-chrome.png?raw=true)

	_Animation not working in Chrome_

	The animation is not working because Chrome does not support the `animation` property as defined in the standard. Instead, Chrome supports the prefixed `-webkit-animation`. The _-webkit-_ prefix is called _CSS vendor prefix_.

	CSS prefixes are used by browser vendors to add support for experimental properties. They are not part of the CSS spec, but are intended to mimic the proposed behavior of existing or future properties.

	The four main vendor prefixes are:

	* **-ms-**: Microsoft
	* **-moz-**: Mozilla
	* **-o-**: Opera
	* **-webkit-**: Safari, Chrome, and other WebKit-based browsers

	You will now update the CSS to add the prefixed properties and verify this fix in Chrome.

1. Open **Styles.css** and edit the rules where the `animation` tag is used to match the following code:

	````CSS
	 .fadeIn {
		  -webkit-animation: fadeIn 3s forwards;
		  animation: fadeIn 3s forwards;
	 }
	````

	````CSS
	.wrapper.animation #synopsis {
		  -webkit-animation: slideOut 3s forwards;
		  animation: slideOut 3s forwards;
		  display: none;
	}
	````

	````CSS
	.wrapper.animation #movietitle {
		  -webkit-animation: fadeOut 3s forwards;
		  animation: fadeOut 3s forwards;
	}
	````

1. Add the following rules as well, which are the definitions for the animations with the vendor prefixes:

	````CSS
	@-webkit-keyframes slideOut {
		  0% {left: -100%; color: white;}
		  100% { left: 0px; color: black;}
	}

	@-webkit-keyframes fadeOut {
		 0% {opacity: 1; }
		 100% {opacity: 0.1;}
	}
	@-webkit-keyframes fadeIn {
		 0% {opacity: 0;}
		 100% {opacity: 1;}
	}
	````

1. Press **F5** to debug the application in Google Chrome.

	The home page launches in Chrome. Click the title. Notice that the animation now works like it did in Edge. Even the "COMING SOON" title flashes when the page loads.

	![Animation working in Chrome with vendor prefix](images/animation-working-in-chrome-with-vendor-prefi.png?raw=true)

	_Animation working in Chrome with vendor prefix_

1. Switch to Visual Studio and stop debugging.

	As you did for Chrome, you can test your website in other browsers and add the other prefixes if needed. The [w3schools CSS Reference](http://www.w3schools.com/cssref/) page can help you determine browser support for the properties you want to use.
	[Here](http://www.w3schools.com/css/css3_animations.asp) is the specification for animations, that indicates this particular property is supported without the prefix in Edge, with the -webkit- prefix for Chrome, Safari and Opera and with the -moz- prefix for Firefox. Thus, you would need to update the styles as follows to support the other browsers.

1. Edit **Styles.css** and update the existing rules to match the following code:

	````CSS
    .fadeIn {
        -webkit-animation: fadeIn 3s forwards;
        -moz-animation: fadeIn 3s forwards;
        -o-animation: fadeIn 3s forwards;
        animation: fadeIn 3s forwards;
    }
	````

	````CSS
	.wrapper.animation #synopsis {
			-webkit-animation: slideOut 3s forwards;
			-moz-animation: slideOut 3s forwards;
			-o-animation: slideOut 3s forwards;
			animation: slideOut 3s forwards;
			display:none;
	}
	````

	````CSS
	.wrapper.animation #movietitle {
			-webkit-animation: fadeOut 3s forwards;
			-moz-animation: fadeOut 3s forwards;
			-o-animation: fadeOut 3s forwards;
			animation: fadeOut 3s forwards;
	}
	````

	These rules need to be added as well:

	````CSS
	@-moz-keyframes slideOut {
		  0% {left: -100%; color: white;}
		  100% { left: 0px; color: black;}
	}

	@-moz-keyframes fadeOut {
		 0% {opacity: 1; }
		 100% {opacity: 0.1;}
	}

	@-moz-keyframes fadeIn {
		 0% {opacity: 0;}
		 100% {opacity: 1;}
	}

	@-o-keyframes slideOut {
		  0% {left: -100%; color: white;}
		  100% { left: 0px; color: black;}
	}

	@-o-keyframes fadeOut {
		 0% {opacity: 1; }
		 100% {opacity: 0.1;}
	}

	@-o-keyframes fadeIn {
		 0% {opacity: 0;}
		 100% {opacity: 1;}
	}
	````

1. Test the website in the other browsers. The animation in the home page should behave similarly in all browsers.

<a name="Task3" />
## Plug-in free Development

From the early history of the web, browser plug-ins have played a vital role by enabling rich online multimedia experiences and complex web application functionality. However, along with these capabilities, plug-ins can come with some disadvantages. Because plug-ins are essentially applications that run inside the browser, they consume additional system resources and expose additional attack surface to security risks. Also, plug-ins are not designed for touch, and because they are separate applications from the browser itself, they don't benefit from any changes from the latest browsers that make websites work smoothly with touch. Finally, plug-ins are based on proprietary technologies and are written with variable code quality, making it difficult to predict or control their support across different browsers and operating systems.

The current trend is to make the web applications plug-in free, favoring the usage of standards-based technologies specified by the [World Wide Web Consortium (W3C)](http://go.microsoft.com/fwlink/p/?LinkID=73527) like the ones comprising HTML5, which offer similar capabilities for various plug-ins. These technologies have strong support across modern web browsers, making it possible for web developers to write the same markup and script that works across all modern browsers, without writing or maintaining any additional code with third-party framework and runtime dependencies.

In this task, you will replace the existing flash video player with the standard HTML 5 video player.

1. Open **Index.html**, located in the **Views/Home** folder.

2. Locate the div element that has the "_myElement_" ID, and the script code that follows. This code, shown below, creates the flash player.

	````HTML
    <div id="myElement">Loading the player...</div>

    <script type="text/javascript">
    jwplayer("myElement").setup({
        file: "http://wams.edgesuite.net/media/SintelTrailer_MP4_from_WAME/sintel_trailer-1080p_3400.mp4",
        width: "100%",
        aspectratio: "16:9",
        primary: "flash"});
    </script>

	````
3. Replace the preceding code with the following one:

	````HTML
	<video id="promoVideo" width="100%" controls src="http://wams.edgesuite.net/media/SintelTrailer_MP4_from_WAME/sintel_trailer-1080p_3400.mp4"></video>

	````

	The new code uses the newly added HTML5 `video` tag. You can find more information about this tag [here](http://www.w3schools.com/tags/tag_video.asp).

4. If you want the video to play automatically after the page loads, locate the following code:

	<!-- mark:3-4 -->
	````JavaScript
	<script>
		window.addEventListener("load", function () {
			setTimeout(function () { jwplayer().play(true); }, 500);
		});
	</script>
	````

	Update it so that it looks like the following code:

	<!-- mark:3-4 -->
	````JavaScript
	<script>
		window.addEventListener("load", function () {
			var player = document.getElementById("promoVideo");
			player.play();
		});
	</script>
	````

	The updated code has the same effect as the previous one: when the page loads this handler causes the video to start playing. The difference is that it is using the new video player object.

	Since you are not using the flash player anymore, you will now remove its references.

1. Open the **_Layout.cshtml** file located in the **Views/Shared** folder. Remove the **Render** statement for the flashplayer, shown below, located inside the **head** tags.

	````JavaScript
	@Scripts.Render("~/bundles/flashplayer")
	````

6. Open the **BundleConfig.cs** file located in the **App_Start** folder. Delete the definition of the flashplayer script bundle, which is shown in the following code.

	````C#
	bundles.Add(new ScriptBundle("~/bundles/flashplayer").Include(
            "~/Scripts/jwplayer/jwplayer.js"));
	````

	> Note: To completely remove the player from the solution you should also delete the nested **jwplayer** folder (in the **Scripts** folder). This folder contains the flash player scripts. Since you will use the jwplayer in a future lab please do not delete the folder.

8. Press **F5** to run the solution to see the new HTML5 video player in action.

	![html5 Video Player](images/html-video-player.png?raw=true)

	_HTML5 video player_

For optimal future proofing and browser compatibility, it is best to develop your site entirely without using plug-ins. In some cases, however, it might not be possible for a website or web app to work completely without plug-ins. In these instances, there are some fallback techniques and mitigation strategies you can follow to ensure the best possible experience for users of plug-in free browsers, such as Microsoft Edge.

### Summary
In this lab you have learned how to continue improving your web application to follow some of the web development Best Practices.
