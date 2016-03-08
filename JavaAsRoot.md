_Please note that this feature is currently only available in the Developmental branch_

# How to run Java code using RootTools #

## Introduction ##

There are many reasons why you would want to write your root code in Java rather than C:

  1. You are just more comfortable with Java!
  1. You want to write your root helpers faster
  1. You wish to leverage the Java library's APIs to handle files, etc.
  1. ...

Running Java code is very similar to running a native binary.

## Writing a Java root class ##

Not much to it, really: write your code the way you usually would any other class in your project.

For this code to be considered "root" you will use an annotation. Please follow me as I reveal...

## "The Magic" ##

Let's take the example of a class you would name `MySDCardCrawler`

As usual, the class can be found in a similarly named file: `MySDCardCrawler.java`

To flag this class as being of "root" type, use this annotation:
```
@RootClass.Candidate
public class MySDCardCrawler {
```

At this point, simply building and running your project will not help: you need to turn this class -- and other classes you may have annotated -- into a "root" class.

To do this, you will need to run RootTool's helper: `RootClass` itself.

If you are using RootTools' source code, this can be as easy as right-clicking `RootClass.java` in your IDE of choice and running it. You will however need to ensure that `jar` and `dx` are in your global environment path.

A safer, more straightforward way to run `RootClass` is to use the command line:
```
java -cp <path to RootTools.jar> com.stericson.RootTools.containers.RootClass
```

**Watch your step!**

You will need to run this command at the root of your project, i.e. the folder that contains 'src' and 'res'

If successful, a file called `anbuild.dex`will be created. It will, in fact, be created where your particular IDE would expect to find class files. For instance, in IntelliJ it will be created in `out/production/<project name>` whereas in Eclipse it will be in `classes/bin`

Your last step will be to copy/move `anbuild.dex` to `res\raw` and build your project.

## Installing it on Android ##

This will be done automatically by RootTools. The first time `JavaCommandCapture` is used, it will write the dex file into `<app data path>/files`

## JavaCommandCapture? ##

As usual, the breadth of RootTools' feature set is available to you but the simplest approach is very similar to what we recommend when running native commands. Instead of `CommandCapture` however we use `JavaCommandCapture`

Here is a full example, running a class called `NativeJavaClass`, extracted from `SanityCheckRootTools.java`
```
            Shell shell;
            try {
                shell = RootTools.getShell(true);
                JavaCommandCapture cmd = new JavaCommandCapture(
                        43,
                        false,
                        SanityCheckRootTools.this,
                        "com.stericson.RootToolsTests.NativeJavaClass") {

                    @Override
                    public void commandOutput(int id, String line) {
                        super.commandOutput(id, line);
                        visualUpdate(TestHandler.ACTION_DISPLAY, line + "\n");
                    }
                };
                shell.add(cmd);
            } catch (Exception e) {
                e.printStackTrace();
            }
```

## Why 'source' annotations? ##

This is  not a standard implementation of Java annotations. Instead of `.class` files,` .java` files are directly inspected. This has several advantages:

  * annotations are not, in fact, part of Android's default libraries; this approach allows us to sidestep this limitation
  * performing an annotation check this way is a much lighter process, removing compiler complexity, extra reflection processing, etc.

If you really wish to use JSR-compliant reflection, you can easily get started by un-commenting out reflection-related code we added to the header of `RootClass.java` and downloading and including a jar file called `jsr308-all-1.1.2.jar`