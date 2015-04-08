### Can I open a view in a new tab? ###

Short answer: No.

Opening a view in a new tab, such as when center-clicking on menu items or right-clicking and selecting the "Open link in new tab" command, doesn't yield the expected result in Kitto. Here's why.

Almost all browser/server interaction in Kitto happens through asynchronous requests and responses. So, clicking on a menu item that opens a view ordinarily causes an async request. When you try to open a link in a new tab (or window), the browser performs a sync request instead, which causes Kitto to re-render the entire page in the new tab or window. In other words, Kitto cannot distinguish between a home/refresh request (like when hitting the F5 key) and and "open link in new tab" request.

So how does Kitto respond to such requests? Prior to re-rendering the page Kitto will free any server-side structures and basically reset the session, thus making the **source** tab or window useless. Any subsequent tries to work in that tab or window are likely to cause errors or unexpected behaviour. This is because the first tab holds references to server side objects that no longer exist in that session. The only valid references are now in the destination tab, and you can only resume working there.

In summary, all browser tab and windows share the same Kitto session, and only the most recently open one is functional. Kitto does not currently offer any means to prevent the user to try to open views in new tabs or windows.