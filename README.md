# Push the button

Try it on http://liwe.github.io/app-pushthebutton

This is a exemple of the most basic app with Liwe. The entire script is in the `index.html` file.

## Liwe, the basics

Before all, you have to inject the API script.

```html
<script charset="utf-8" src="http://liwe.co/api/liwe.js"></script>
```

### Liwe object

Everything start with the constructor

```javascript
var myLiwe = new Liwe({
  key: 'k3Y4l1W3g1ThU85aMp1E4pP5',
  maxRemoteConn: 1
});
```

The constructor accept only one parameter: the option object.

In this object, you have to mention your `key` to use the service (you can request a key at liwe.co). Then optionaly, the `maxRemoteConn` attribute to mention the maximum amount of remote which can be simultaneously connected to your instance.

```javascript
myLiwe.on('connect', function (info) {
  promptr('Grab your smartphone and go on ' + info.url);
});
```

Your Liwe instance is receptive to three kind of events via the `on` method:

- `connect`: when your instance is succesfully connected. The callback will get the info object containing all information about the connection. Like the URL for remotes.
- `error`: when somethng fails. The callback gets only one parameter: the error object
- `new_remote`: when a new remote is connected. The callback get the new remote object as parameter

Once all your listeners defined, start the connection

```javascript
myLiwe.connect();
```

At this point, you managed to establish the connection to Liwe. Well done. Otherwise, the error callback will help you.

Now, let's have a look to the remote object.

### Remote object

The remote class got only few methods. Everything will begin by setting up a new user interface via `setUI`. This method take two parameters.

- `uiName` (string): Name of the UI to set on this remote
- `helpNote` (string): Text to display in the info bar of the remote 

```javascript
remote.setUI('button', 'Press the button to change the animation');
```

At the moment, the API is compatible with only 3 UIs:

- `button`: display a simple button
- `gyro`: get the accelerometer/gyroscope information from the device
- `touch`: transform the remote a touchepad screen

Each UI got different kind of event types. Check the [documentation](https://github.com/liwe/liwe/wiki) to know more about it.

Then you have to listen the event you want. In the current exemple, the event listened is `touch`.

```javascript
remote.on('button', function (e) {
  ...
});
```

The event object received by the listener contain different information:

- `type`: type of event. In out case `press` or `release`
- `ui`: UI name where the event come from
- `data`: extra information about the event. This is optionnal, depending on the event. Check the [documentation](https://github.com/liwe/liwe/wiki) for further information.

Then listen in case of disconnection

```javascript
remote.on('disconnect', function (remote) {
  promptr('The remote has been disconnected. Reconnect on ' + myLiwe.info.url);
});
```

In out case, once the current remote is disconnected, a message is displayed on the promptr to explain how to reconnect.

But if you want, you can force a remote to disconnect via the `disconnect` method. There's no usecase here, but if you want to implement a timeout, this might help.

## Troubleshoot

Check the [documentation](https://github.com/liwe/liwe/wiki) for further information.

Have any questions? Any ideas? Feedback? Join the conversation at [![Gitter](https://badges.gitter.im/liwe/liwe.png)](https://gitter.im/liwe/liwe)
