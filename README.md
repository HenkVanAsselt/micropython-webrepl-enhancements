# micropython-webrepl-enhancements
Micropython webrepl enhancements,, using selenium to autoconnect, enter the password and catch problems.

The standard Micropython webrepl client contains the following files:
* webrepl.html
* term.js
* FileSaver.js
* websocket_helper.py
* webrepl_cli.py

In normal use, when webrepl.html is started, one sees the pre-defined URL 'ws://192.168.4.1:8266/' and a [Connect] button.

First, the correct URL has to be entered, and then the [Connect] button has to be pressed.

Solution 1
----------

My first approach was to use a python script which did the following:

1. Ask for the IP address of the ESP32 or 8666 running Micropython webrepl service.
2. modify webrepl.html so the line `<input type="text" name="webrepl_url" id="url" value="ws://192.168.4.1:8266/" />` would represent the required url.
3. Start webrepl.html

Still, one has to press the connect button to initialize the connection, and one has to enter the password manually to be granted access.

Solution 2
----------

The second approach is now using selenium with the chrome webdriver to automate this more.

In the file 'webrepl.py' one can find the following functionality:

1. Start webrepl.html with selenium.
2. Start a session to the ESP32 running the webrepl service by entering the url in the element 'url', followed by sending the RETURN key.
3. Wait for the welcome message in the element 'term' (with a configureable number of retries and interval length)
4. Wait for the password promt in the element 'term' and enter the password. This could not be done with selenium, so another 3rd party package 'keyboard' is used.
5. Check if Access was denied or not.
6. Wait for the repl prompt `>>>`.
7. If the repl prompt is detected, enter a `Ctrl+B` leave the repl raw mode, just in case.

Have fun,
//Henk
