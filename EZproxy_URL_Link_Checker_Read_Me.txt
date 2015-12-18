==URL Checker Using ProxyURLPassword==

ProxyURLPassword is an EZproxy feature that allows one to pass EZproxy a structured XML document that contains a URL and a password and EZproxy will return a true or false value based on whether the URL has been setup for proxy in EZproxy. According to the release notes (http://oclc.org/support/services/ezproxy/documentation/changes.en.html) this was developed to support third-party applications, such as LibX. There is little official documentation on this feature, but excellent documentation on how to implement this feature using PHP (http://elibtronic.ca/content/20080821/checking-urls-connecting-ezproxy) and (http://elibtronic.ca/content/20101029/taking-care-campus-users-or-checking-urls-connecting-ezproxy-redux). 

Here I offer a method to implement this feature using HTML and JavaScript. No additional resources are required, and this implementation should work with any release of EZproxy that supports ProxyURLPassword. 

Submitted by Paul Butler, Library Technologies Support Analyst at Ball State University. http://cms.bsu.edu/academics/libraries/about/stafflisting/staffbydepartment/lits/butlerpaul

==The Features==
In addition to supporting the URL lookup, the code provides some additional features. 

* The page checks to confirm the user has input something into the input box, and if they have not, prompts them to do so. 

* The page checks to confirm if the user input begins with http or https, and if it does not, it prepends http:// to the URL before adding the proxy prefix. 

* The page checks to confirm if the URL contains any spaces, and removes any that are identified. 

* If the URL is setup for proxy the page returns a success message and offers the user a hyperlink with the URL and the prepended proxy string. 

* If the URL is NOT setup for proxy the page returns a failure message and offers the user a hyperlink with the EZProxy administrator's email address. 

==A Note of Warning==
One limitation with this implementation of ProxyURLPassword is that the password used to allow URL lookups is displayed in plain text in the source code of the page. This password is not used for any other feature or purpose, and only gives a user access to send a lookup request to HTTP://EZPROXY.YOURLIB.ORG/proxy_url. To mitigate risk you may place the page you provide to end-users in /loggedin, which will require the user to authenticate via EZproxy before accessing the page. More on this is provided below. The overall risk is low, but as with anything, caveat emptor! 

==The Setup==
To implement this feature update config.txt with the following line:  ProxyURLPassword somepassword

Replace "somepassword" with a new password value and restart EZProxy. "somepassword" will be stored in plain text in the HTML page so it should NOT be a password that you use elsewhere. I recommended using a long string of random numbers and letters. Once this is complete navigate to HTTP://EZPROXY.YOURLIB.ORG/proxy_url - replacing EZPROXY.YOURLIB.ORG with your EZProxy address. If the page resolves correctly the feature is working. 

The custom HTML page offered with this documentation should be located with your other EZProxy files. The exact location will determine who will have access to the page. The example URLs below assume the page is called URL_lookup.htm, you may name it whatever you want. 

* Placing the HTML file in /public will allow anyone online to access the page. The resulting URL would be: HTTP://EZPROXY.YOURLIB.ORG/public/URL_lookup.htm

* Placing the HTML file in /limited will require remote users to authenticate to access the page. The resulting URL would be: HTTP://EZPROXY.YOURLIB.ORG/limited/URL_lookup.htm

* Placing the HTML file in /loggedin will force all users to authenticate before accessing the page. The resulting URL would be: HTTP://EZPROXY.YOURLIB.ORG/loggedin/URL_lookup.htm

==The Code==
What follows are the variables and their values that need to be changed in URL_lookup.htm for the code to work properly. Searching the code for CHANGEME will help quickly locate these values. If you do not have an HTML editor you can open URL_lookup.htm with any text editor, such as NotePad, to make these changes. 

"PROXY_STRING" - The value for the variable proxy_string should contain your EZProxy prefix in either form, whichever makes sense locally: HTTP://EZPROXY.YOURLIB.ORG/LOGIN?URL= 
or 
HTTP://www.EZPROXY.YOURLIB.ORG/LOGIN?URL=

"PROXY_URL_LOCATION" - The value for the variable proxy_url_location should contain the location your proxy_url page in either form, whichever makes sense locally: HTTP://EZPROXY.YOURLIB.ORG/proxy_url
or 
HTTP://WWW.EZPROXY.YOURLIB.ORG/proxy_url 

"YOUR_PASSWORD_HERE" - Replace YOUR_PASSWORD_HERE with the same "somepassword" you used in the step above when you added "ProxyURLPassword somepassword" to config.txt. Remember this password will be saved in plain text in the html of this page so do NOT use a password that you use elsewhere. Make the password unique, random, and just used for this purpose. 

"EZPROXY-ADMIN@YOURLIB.ORG" - This value should be replaced with the email address of your EZproxy administrator, or whomever maintains EZProxy stanzas. This will be displayed on the page when the user submits a URL that is not setup for proxy. 

Once the four values above have been updated in the accompanying html file URL_lookup.htm and URL_lookup.htm has been added to EZproxy as outlined in "The Setup" step above you should be able to access the page and start doing URL lookups. 