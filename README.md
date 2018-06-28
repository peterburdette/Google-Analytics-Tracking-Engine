# Redirects-Tracking-Engine
The Redirects Tracking Engine makes it easy to collect event data where traditional tracking methods may not have been possible. This tracking engine utilizes Google Tag Manager and Google Analytics to capture events and is run on the client side. All events can be seen in Google Analytics **(Behavior > Events > Top Events)**. 

## Getting Started
### Installation
Add the latest JQuery and Redirects Tracking Engine scripts to the header of your site.

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script type="text/javascript">
// Fetches redirects.txt and stores information in the variable data
$.get('redirects.txt', function(data) {
	// Fetches the parameters of the query string and stores the path in urlParams
	var urlParams = [];
	// Sifts through the string and removes unwanted characters
	(function () {
		var match,
		pl = /\+/g, // Regex for replacing addition symbol with a space
		search = /([^&=]+)=?([^&]*)/g,
		decode = function (s) { return decodeURIComponent(s.replace(pl, '')); },
		query = window.location.search.substring(1);
		while (match = search.exec(query))
		urlParams[decode(match[1])] = decode(match[2]);
	})();
	// Pulls properties from urlParams and stores them in destination array
	var destination = Object.keys(urlParams).map(function(path){return urlParams[path]});
	// Assigns the redirects.txt data to the userData array
	var userData = data.split('\n');
	// Multidimensional array declaration
	var redirects = [];
	// Fetches the total number of objects in the userData array
	var total = userData.length;
	// Counter variable
	var i = 0;
	// Runs through the redirects array to check to see if there is a string match
	while (i < total) {
		// Places userData into the multidimensional redirects array
		redirects[i] = userData[i].split(' => ');
		// Checks for a path match in the redirects array
		if (redirects[i][0] == destination[1]) {
			window.location.href = redirects[i][1];
			return;
		}
		i++;
	}
	// Redirects to safe page if no match is found
	window.location.href = 'https://example.com';
});
```

### Setup Google Tag Manager

Paste this code as high in the `<head>` of the page as possible. Make sure to substitute the filler `GTM-XXXX` with your Google Tag Manager Account ID.
```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXX');</script>
<!-- End Google Tag Manager -->
```

Additionally, paste this code immediately after the opening `<body>` tag. Make sure to substitute the filler `GTM-XXXX` with your Google Tag Manager Account ID here as well.

```html
<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-XXXX"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->
```

#### Create Tags/Triggers/Variables
1. **Create a variable** &mdash; You will need to first create a User-Defined Variable. Go to the Variables page of your Google Tag Manager and in the User-Defined Variables section create a new variable named `Tracking ID`.
<br /><br />
Give this variable your Google Analytics Tracking ID in the Value field and **SAVE**.
<br /><br />
![Variable Configuration Image](assets/variable-configuration.jpg?raw=true "Optional Title")
<br /><br />
**NOTE:** For more information about installing the Google Tag Manager snippet, check out their [Quick Start Guide](https://developers.google.com/tag-manager/quickstart).
<br /><br />
2. **Create a tag** &mdash; You will need to go to the *Tags* page in Google Tag Manager and click the **NEW** button. Fill in the fields so they resemble the screenshot below. Click **SAVE** when finished.
<br /><br />
![Tag Configuration Image](assets/tag-configuration.jpg?raw=true)
<br /><br />
3. **Create a trigger** &mdash; Finally, you will need to create a trigger. Go to the *Triggers* page in Google Tag Manager. Once there click on the **NEW** button and fill in the fields so they resemble the screenshot below. Click **SAVE** when finished.
<br /><br />
![Trigger Configuration Image](assets/trigger-configuration.jpg?raw=true)
<br /><br />
After these configurations have been setup in Google Tag Manager you are ready to add your redirects to the `redirects.txt` file.

### Redirects
To add a redirect open up the `redirects.txt` file. The text `linkedin` is telling the engine what path it needs to look for in the URL. Immediately following is the separator `=>` which shows the engine where it needs to direct the user. New redirects can be added on a new line. There are no limits to the number of redirects that can be added to this file.

<pre>
// Example Redirect
linkedin => https://example.com
</pre>

### Failsafe
In the off-chance that one of the redirects is not working it is good to have a page that the user can be directed to. You can add in your failsafe page by modifying the `window.location.href` location that comes immediately after the loop.

```html
// Redirects to safe page if no match is found
window.location.href = 'https://example.com';
```

## Browser Support

*Supported Browsers* : Chrome, Firefox, Safari, Opera, Edge, IE7+.

## License

The source code can be found on [github](https://github.com/peterburdette/Redirects-Tracking-Engine) and is licensed under the [MIT](http://opensource.org/licenses/mit-license.php) license.

Developed by [Peter Burdette](https://www.linkedin.com/in/peter-burdette-76976552)
