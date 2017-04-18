Mobile First Responsive Web Design
========================================
An important part of the solution to Responsive Web Design performance is Mobile First Responsive Web design. Typically, responsive Websites are the same size on mobile as on the desktop. This means most responsive sites are not being built for mobile first. Instead a lot of responsive designs take desktop sites and simply hide elements.

In this lab you will learn about the **Mobile First** and **Responsive Design** concepts and learn some guidelines that can be applied to a web application to achieve them.

This lab includes the following tasks:

1. [Responsive Design Guidelines](#Task1)
	1. [Viewport](#Task11)
	2. [Styles](#Task12)
	3. [The tel: URI Scheme](#Task13)
	4. [Responsive Images](#Task14)

<a name="Task1" />
## Responsive Design Guidelines ##
**Mobile First** is a concept created by [Luke Wroblewski](http://www.lukew.com/presos/preso.asp?26) that highlights the need to prioritize the mobile context when creating user experiences. Mobile First allows websites to **reach more people**, forces designers to **focus on core content and functionality**, and lets designers innovate and **take full advantage of new technologies**.

**Responsive web design** responds to the needs of the users and the devices they are using. The layout changes based on the size and capabilities of the device. For example, on a phone, users might see content in a single column view, while a tablet might show the same content in two columns.

**Progressive Enhancement** is the approach of creating the basic functionality of the application and progressively adding more features based on the capabilities of the device where the application is run.

As you may remember, **Contoso Movies**  was developed as a Mobile application, therefore we can use the **Progressive Enhancement** principle to add support for bigger screens, such as desktop browsers.

The following sections will describe some of the guidelines to achieve a Responsive design.

<a name="Task11" />
### **Viewport** ###

1. Open the **_Layout.cshtml** file located in the **Views/Home** folder.

2. Notice the **viewport meta** tag inside the **head** tag.
	Pages optimized for a variety of devices must include a meta viewport element in the head of the document. A meta viewport tag gives the browser instructions on how to control the page's dimensions and scaling.

	In order to accommodate for sites not optimized for mobile screens, many modern mobile browsers set a larger browser viewport, which allows for better viewing of non-mobile-optimized sites.

3. Inside the **content** property of the **meta** tag add **, initial-scale=1** to establish a 1:1 relationship between CSS pixels and device independent pixels. This is shown in the following code.

	````HTML
	<meta name="viewport" content="width=device-width, initial-scale=1">
	````

	> **Note:** Notice that to separate the parameters inside the **content** property, you must use a comma.

<a name="Task12" />
### **Styles** ###

When writing CSS, you have to keep things lightweight and as fluid as possible. There are lots of devices, and all of them have many different screen sizes. Since screen size is typically unknown, the content should determine how the layout will adjust to its container.

**Separate style sheet for larger screens**

It is recommended that you create two separate CSS files - style.css and enhanced.css - in order to deliver basic styles for screens less than 40.5em and use media queries to serve up enhanced styles for screens larger than 40.5em.

You should use relative units like **ems** and **percentages** to keep styles as fluid and flexible as possible. Relative units are far more compatible with the great variance of screen sizes, pixel density and zoom level.

1. Create a new **enhanced.css** file inside the **Content** folder. The styles for higher resolutions will be stored in this file.

2. In the **_Layout.cshtml** file, add the following code inside the **head** tags.

	````HTML
	<link rel="stylesheet" type="text/css" href="~/Content/enhanced.css" media="screen  and (min-width: 40.5em)" />
	````

	> **Note:** Take into account that the reference to the regular styles (styles.css) is added using CSS bundles.

**Mobile First styles & Media Queries**

Instead of declaring large screen rules first only to override them for smaller screens, it is better to simply define rules as more real estate becomes available. Starting with baseline shared styles and introducing more advanced layout rules when screen size permits keeps code simpler, smaller and more maintainable.

> **Note:** It is important to note that some mobile browsers do not support media queries, so serving base styles by default reaches more devices and browsers.

1. In the recently created **enhance.css** file, add the following styles to support all screen sizes.

	````CSS
	@media screen and (min-width: 28.75em) {
	 #primaryContent {
	     float: right;
	     width: calc(100% - 220px);
	 }

	 .navigationList {
	     float: left;
	     width: 200px;
	 }
	}
	````

	The preceding styles will rearrange the application menu, so it will be rendered on the left side of the page instead of the bottom. This styles will only be applied when the device screen sizes is equal or greater than 28.75em.

2. Run the application in a full-screen browser, and notice how the navigation menu is now located in the left site of the page.

	![The application adapted for bigger screen sizes](images/the-application-adapted-for-bigger-screen-siz.png?raw=true)

	_The application adapted for bigger screen sizes_

3. Resize the page to a smaller size and notice how the application adapts to each size. Note that once the browser size is smaller than 28.75 em as the media query specifies, the navigation menu will go back to the mobile state, at the bottom of the page.

	![The application adapted to smaller screens](images/the-application-adapted-to-smaller-screens.png?raw=true)

	_The application adapted to smaller screens_

<a name="Task13" />
### **The tel: URI Scheme** ###

You should take into account that mobile devices are designed to make phone calls, and some desktop configurations can launch VoIP applications to initiate a phone call. The **tel:** URI scheme includes an easy way for users to facilitate a phone call; you can see an example in the following code.

````HTML
<a href="tel:+18005555555">1-800-555-5555</a>
````

1. Open the **Contact.cshtml** file, located in the **Views/Home** folder.
2. Locate the last **div** element with the class **contact**, and notice the phone number.
3. Replace the phone number with the previous code. The resulting code will look like the following.

	````HTML
	<div class="contact">
		You can call us at
			<a href="tel:+18005555555">
				1-800-555-5555
			</a>
	</div>
	````
4. Run the application, selecting one of the **Windows Phone** emulators.
5. Browse to the **Contact** page.
6. Click the phone number link. The **Edit Number** page will be displayed, showing buttons to either make or cancel the call.

	![Making a call from the Web Application](images/making-a-call-from-the-web-application.png?raw=true)

	_Making a call from the web application_

<a name="Task14" />
### **Responsive Images** ###

The guideline in this topic is to load mobile optimized images by default, and then conditionally load larger images on demand. There are several different techniques for responsive images, both client-side and server side. Although the Contoso Movies app does not have many large images, some of the approaches are described in the _Cross-Browser testing_ lab.

# Summary #

Nowadays, to build web applications that covers the most users, it is recommended to use the **Mobile First** design, which means designing your application using mobile as the baseline, and progressively increasing functionality for bigger screen sizes and devices with more capabilities. This concept is referred to as **Progressive Enhancement**.

Creating a responsive web design implies, among other things:

- **Setting the viewport** to accommodate for sites not optimized for mobile screens.
- **Media queries** allowing designs to adapt by establishing dimension breakpoints.
- **Flexible images and media** which keep content intact in any resolution.
- **Fluid grids** that flow with the devices screen size.
