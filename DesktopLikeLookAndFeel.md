## Question ##

Kitto demos are very desktop-like. Is there a way to make it look and feel more webby?

## Answer ##

Looking very desktop-like is one of the prime goals of Kitto and as such the ExtJS library was selected for the GUI. There is room for doing more web-like stuff, while still keeping the data layer, through the use of templates. We have explored the matter but we have not implemented anything sophisticated yet. A template-based controller to display list of records, to be used as a grid replacement, is quite high on the TODO list though).

What you can do right now is

  * Embed views inside HTML pages through divs/css, frames or iframes;
  * Conversely, host blocks of HTML inside views by means of the HtmlPanel controller.

There will definitely be more at some point.