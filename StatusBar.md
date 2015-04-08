# StatusBar #

The classic status bar for messages and info.

Here's an example:

```
Controller: StatusBar
  Text: <p>Connected user: %Auth:UserName%</p>
ImageName: user
```

And here's the result:

![http://wiki.kitto.googlecode.com/git/StatusBar.png](http://wiki.kitto.googlecode.com/git/StatusBar.png)

Note that the content can be translated:
```
Controller: StatusBar
  Text: <p>_(User: %Auth:UserName%)</p>
ImageName: user
```