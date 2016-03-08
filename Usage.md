# Introduction #

Here is an overview of what methods are provided by the RootTools library and how to use them. Everything below is subject to change at any time, and it fact will be changing. This library is constantly evolving and becoming better. When we change things, we will try and make it as pain free for you as possible.

For more information on methods available within the RootTools Library please look at this Javadoc:

http://roottools.googlecode.com/svn/trunk/Developmental/doc/index.html


---


# Including the Library in Eclipse #

Including the RootTools library into your project so that you can utilize it is simple!

  1. In Eclipse, right click your project, navigate to "Build Path", then to "configure build path"
  1. Now choose "add External Jar"
  1. Navigate to the RootTools .jar file you download and choose it.
  1. Navigate to "Order and Export" make sure the box is ticked next to the library, and move it to the top of the list.
  1. Finish up by choosing "ok"

That's all there is to it! The library should now be ready for use in your Android project. Now that you have the library included into your Android project all you need to do is import it into the desired java file. You can do this by putting "**import com.stericson.RootTools.`*`**"  OR "**import com.stericson.RootTools.RootTools**" at the top of your java file, right under the package name for your Android app.

# Setting Debug Mode #

To turn debug mode on and off, simply do the following in your app:

```
RootTools.debugMode = true; //ON
```

OR

```
RootTools.debugMode = false; //OFF
```

# Using log() #

This method allows you to output debug messages only when debug mode is on. This will allow you to add a debug option to your app, which by default can be left off for performance. However, when you need debugging information, a simple switch can enable it and provide you with detailed logging.

This method handles whether or not to log the information you pass it depending whether or not RootTools.debugMode is on. So you can use this and not have to worry about handling it yourself.

To use the log function you can do the following:

```
RootTools.log("this is a log message); //The TAG for this log will be "RootTools"
```

OR

```
RootTools.log("OurTag", "this is a log message);
```

This method uses log.d by default since this is intended for debug code.

If you want to change the type of logging you can do so like this:

```
log(TAG, msg, 3, null);
```

Here you can pass the TAG you want to use for the log, the message, the type of logging to perform, and the exception if you want.

The types available are as such:

1 = log.v
2 = log.e
3 = log.d

# Checking for busybox #

Busybox is quite possibly the single most useful and versitile tool in the root developer's arsenal. The problem is that not every ROM has busybox installed by default. The RootTools library has a method for checking for busybox. Using it is as simple as:

```
if (RootTools.isBusyboxAvailable()) {
    // busybox exists, do something
} else {
    // do something else
}
```

It is recommended that if busybox is not found that you alert the user to this and give them the a way to install it, usually with the [BusyBox](http://market.android.com/details?id=stericson.busybox) app from the market. RootTools provides an easy way to go about this. You have two options:
```
RootTools.offerBusyBox(activity);
```
or
```
Intent intent = RootTools.offerBusyBox(activity, requestCode);
```

The first method simply launches an intent that will bring up the market page for the BusyBox app. The second method does the same thing, except it uses `startActivityForResult()`, allowing you to get back from the market app what the user did. When using the second method, requestCode is used exactly as it is with `startActivityForResult()`, and the Intent returned is the Intent returned from the same. Both methods take an Activity as their paramater, this should be the activity that is calling the method.

---

# Checking for su #

su is the binary that is used to grant root access. It being on the phone is usually a good sign that the phone is rooted. You can check for the su binary like so:

```
if (RootTools.isRootAvailable()) {
    // su exists, do something
} else {
    // do something else
}
```

Note that this only check that su exists. A more complete check can also be run:
```
if (RootTools.isAccessGiven()) {
    // your app has been granted root access
}
```
`RootTools.isAccessGiven()` not only checks that a device is rooted, it also calls su for your app, requests permission, and returns true if your app was successfully granted root permissions. This can be used as the first check in your app to make sure that you will be granted access when you need it.

Like BusyBox, RootTools includes the capability to launch the market for Superuser. Do note that the Superuser app from the market does not have the capability to root a person's device, so it is not the recommended route to go if you find that a device is not rooted. However it can be called exactly the same way that the busybox methods are:
```
RootTools.offerSuperUser(activity);
```
or
```
Intent intent = RootTools.offerSuperuser(activity, requestCode);
```

---

# Checking the SD Card for available space #

If you have the need to store things on the SD card, it's usually a good idea to make sure that there's room to hold them. RootTools has a simple method for doing so:
```
if (RootTools.hasEnoughSpaceOnSdCard(updateSize)) {
    // there's enough space, go ahead
} else {
    // not enough space, plan b?
}
```
the method takes a long of how much space you need in blocks and returns true if there is enough space, and the SD card is writable. This method will also return false if the SD card is mounted by the user.

---

# Running root commands #
Running a command as root is simple! Here is an example:

```
CommandCapture command = new CommandCapture(0, "echo this is a command", "echo this is another command");
RootTools.getShell(true).add(command).waitForFinish();
```

You can see that with this command you can run as many commands as you want.

If you need to log the output from the commands you are running you can do the following:

```
Command command = new Command(0, "echo this is a command", "echo this is another command")
{
	@Override
	public void output(int id, String line)
	{
            (Do something with the output here)
	}
};
RootTools.getShell(true).add(command);
```

# Everything below is Deprecated #


---



---

RootTools has two methods for simplifying running root commands. One method for when you need to run a single command, another for when you need to run multiple. They are used as follows:
```
try {
    List<String> output = RootTools.sendShell(command);

    String[] commands = new String[] { command1, command2 };
    List<String> otherOutput = RootTools.sendShell(commands);
} catch (IOException e) {
    // something went wrong, deal with it here
}
```

There is no need to bother with input and output streams as it is all handled for you. Every line that is returned from the shell is returned from sendShell in the List. Also, you don't have to use both, only the one that you need.

# The memory-saving way #
Just like XML files can be parsed by a simple parser or a more "economical" but more complex one, you can run root commands using a simple syntax as specified above, or you can inject a callback object that will use very little memory since it will only need to memorize the current line.
This method also allows you to stop reading a command's output if, for instance, you find the line you were looking for or an error message:
```
try {
    RootTools.Result result = new RootTools.Result() {
        @Override
        public void process(String line) throws Exception {
            // Do something with current line;
            // Maybe store it using setData()
        }

        @Override
        public void onFailure(Exception ex) {
            // Do something if we failed while trying to run a command or read its output
            setError(1);
        }

        @Override
        public void onComplete(int diag) {
            // Invoked when we are done reading one or more command's output.
            // Convenient because we are still within the context of our result object.
        }

    };
    RootTools.sendShell(
        new String[] {
                command1,
                command2,
                command3 },
        timeout,
        result
    );
    if(0 != result.getError())
        return;
    // Do something with getData() if needed.
} catch (IOException e) {
    // Handle exception
} catch (InterruptedException e) {
    // Handle exception
} catch (RootTools.RootToolsException e) {
    // Handle exception. This one would have been raised by the developer.
}
```