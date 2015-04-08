#Standard login window.

# Introduction #

By default, if you use standard authentication you will get a standard login window when navigating to the application. You can customize the login window by setting some controller properties ore replacing it with a different controller altogether.

By default, a view named `Login` is used if present. As an example of a stand-alone login view, here is the entire content of a file named `Login.yaml`:

```
Controller: Login
  BorderPanel:
    BodyStyle: background-image: url(%IMAGE(Background)%; font-size:16px;
    NorthView:
      Controller: HtmlPanel
        # Used when window is maximized.
        Height: 180
        BodyStyle: background: none;
        Html: |
          <div style="text-align: center">
            <p/>
            <p><img src="%IMAGE(main_logo)%"></img></p>
            <p><b>Application Name</b></p>
            <p style="margin-left: 10px">A link <a href="mailto:one@domain.com">one@domain.com</a><br>
              Another link <a href="mailto:two@domain.com">two@domain.com</a><br><br>
            </p>
            <p/>
          </div>
  FormPanel:
    BodyStyle: background: none;
  ExtraWidth: 300
  # Used when window is not maximized.
  ExtraHeight: 180
```

As you can see in the example above, the Login controller hosts a [BorderPanel](BorderPanel.md) which allows you to integrate other controllers in it, such as an [HtmlPanel](HtmlPanel.md).

Here is an example of a login view defined inline in `Config.yaml`:

```
Login:
  Controller:
    BorderPanel:
      NorthView:
        Controller: HtmlPanel
          Height: 80
          Html: |
            <div style="text-align: center">
              <p/>
              <p><img src="%IMAGE(hello_kitto_150)%"></img></p>
              <p/>
            </div>
    ExtraWidth: 300
    ExtraHeight: 80
    LocalStorageMode: Password
```

Note the `LocalStorageMode` node which causes the window to store user name and password in the browser's persistent local storage. The stored data is fetched back the next time the application is used on that device. Other possible values for `LocalStorageMode` are `UserName` and empty.