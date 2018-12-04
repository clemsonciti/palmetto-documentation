---
title: Rclone
keywords: [rclone]
sidebar: documentation_sidebar
permalink: software_rclone.html
---



`rclone` is a command-line program that can be used
to sync files and folders to and from cloud services
such as Google Drive, Amazon S3, Dropbox, and [many others](http://rclone.org/).

In this example,
we will show how to use `rclone` to sync files to a Google Drive account,
but the official documentation has specific instructions for other services.

## Setting up rclone for use with Google Drive on Palmetto

To use `rclone` with any of the above cloud storage services,
you must perform a one-time configuration.
You can configure `rclone` to work with as many services as you like.

For the one-time configuration, you will need to
[log-in with tunneling enabled]({{site.baseurl}}/userguide_howto_run_graphical_applications.html).
Once logged-in, ask for an interactive job:

~~~
$ qsub -I -X
~~~

Once the job starts, load the `rclone` module:

~~~
$ module add rclone/1.23
~~~

After `rclone` is loaded, you must set up a "remote". In this case,
we will configure a remote for Google Drive. You can create and manage a separate
remote for each cloud storage service you want to use.
Start by entering the following command:

~~~
$ rclone config
~~~

Follow the instructions on the command line -  when recommended to "leave blank", just hit Enter.
When asked if you want to use auto config, say yes (Y).
This will open the Firefox web browser, allowing you to log-in to your Google account.
Once this is done, the browser will close and your remote is configured.

After this, you can kill the job and exit this `ssh` session:

~~~
$ exit
~~~

## Using rclone

Whenever transfering files (using `rclone` or otherwise),
log-in to the `xfer01-ext.palmetto.clemson.edu` node,
and **not** the login node `login.palmetto.clemson.edu`.

Once logged-in, load the `rclone` module:

~~~
$ module add rclone/1.23
~~~

After the above one-time setup, you can use `rclone` to (for example),
copy a file from Palmetto to your Google Drive:

~~~
$ rclone copy /path/to/file/on/palmetto remotename:
~~~

Or if you want to copy to a specific destination on Google Drive:

~~~
$ rclone copy /path/to/file/on/palmetto remotename:/path/to/folder/on/drive
~~~

Additional `rclone` commands can be found [here](http://rclone.org/docs/).

