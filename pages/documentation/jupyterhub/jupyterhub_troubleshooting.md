
# Troubleshooting

This page lists frequently asked questions and frequently encountered problems, and possible solutions to them.

### I can't log in

First, ensure that you have an account on the Palmetto cluster. You can request an account [here](https://citi.sites.clemson.edu/new-account/). If you are repeatedly re-directed to the login page after entering your password correctly, you may need to delete your browser's cookies and try again.

### I can log in, but when I click on "Spawn", nothing happens

This can happen for various reasons:

1. **If the cluster is especially busy**: depending on the resources you request for your Notebook server, it may start immediately, or may be "queued". JupyterHub keeps requests in the queue for up to 5 minutes, after which your request is timed out. You may be successful if you try again after a while.

1. **Due to invalid/excessive resource requests**: for example, asking for a single CPU core and 64 GB of memory is an invalid request, as single core requests are currently limited to 15 GB. It may be useful to consult the [Palmetto user's guide](http://www.palmetto.clemson.edu/palmetto) when requesting resources.

1. If you still can't log in, please e-mail <ithelp@clemson.edu> and let us know.

### I get a "503: Proxy Target Missing" error when trying to spawn a notebook server

Generally, this is OK. Try waiting a few seconds and then refreshing the current page.

### I can't run the MATLAB kernel

The MATLAB kernel requires a small amount of user configuration. Please see the [Configuration](Configuring.html) page for details.
