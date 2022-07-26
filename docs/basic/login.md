## Logging in

Any user with a Palmetto Cluster account can log-in using
[SSH (Secure Shell)](https://en.wikipedia.org/wiki/Comparison_of_SSH_clients).
Mac OS X and Linux systems come with an SSH client installed,
while Windows users will need to download one.

### Mac OS X and Linux users

Mac OS X or Linux users may open a Terminal, and
type in the following command:

```
$ ssh username@login.palmetto.clemson.edu
```

where `username` is your Clemson user ID.
You will be prompted for both your password and DUO authentication.

#### Linux Video Instructions

<iframe src="https://drive.google.com/file/d/1HIUWbTd4gwW-nqHLH2C-VmWslrNc4Mk0/preview" width="670" height="376" ></iframe>
<br />

#### Mac Video Instructions

<iframe src="https://drive.google.com/file/d/1n1BCPCDIJCqoNBV24yb-Mp20MhD7t2Qt/preview" width="670" height="376" ></iframe>
<br />

### Windows

MobaXterm is the recommended SSH client for Windows and can be downloaded
from [MobaXterm's download page](http://mobaxterm.mobatek.net/download.html).
This software is recommended because it is free and comes with:

- A built-in file transfer client, which allows you to exchange files and folders
  between your own computer and Palmetto in a convenient manner.
- An X11 server which allows you to run graphical programs on Palmetto cluster
- A graphical port-forwarding interface to support easy access to web-based
  programs launched inside Palmetto

_If you select the installation version of MobaXterm, you will need to unzip the downloaded
file before running the installation program. Windows sometimes allow users to run the
installation from inside the zipped file, resulting in missing an additional utility file (still
inside the zipped file)._

After downloading and installing MobaXterm, users can log-in by following these steps:

1.  Launch the MobaXterm program

    <img src="../../images/basic/login/mobaxterm_01.png" style="width:1000px">

2.  On the top-left corner of MobaXterm, click the **Session** button. Confirm that the
    following settings are set for **Basic SSH settings** and **Advanced SSH settings**:

        Parameter           |   Value
        --------------------|-------------------------------------
        Remote host         | `login.palmetto.clemson.edu`
        Port                |  22
        X11-Forwarding      | enabled
        Compression         | enabled
        Remote environment  | Interactive shell
        SSH-browser type    | **SCP (enhanced speed)**

        <img src="../../images/basic/login/mobaxterm_02.png" style="width:1000px">

3.  Click **OK** and a new session window will be opened, where you will be
    prompted for your Palmetto password and the DUO authentication.

    <img src="../../images/basic/login/mobaxterm_03.png" style="width:1000px">

4.  After being authenticated, you will login to the **login001** node.

    <img src="../../images/basic/login/mobaxterm_04.png" style="width:1000px">

    All settings for this session are saved, and for future logins, you
    can select this session from the **Recent sessions** form of the main MobaXterm
    window as well as the **Saved sessions** tab of the side window. The side
    window can be displayed or hidden by clicking on the blue double-arrow sign
    on the top left of MobaXterm.

    MobaXterm also comes with a built-in file browser and transfer GUI
    (SSH-browser). This GUI is accessible via the **SCP** tab of the side
    window. Using the Upload (green arrow pointing up) and
    and Download (blue arrow pointing down) buttons at the top of the SCP tab,
    you can easily transfer files between Palmetto and your local computer.

#### Windows Video Instructions

<iframe src="https://drive.google.com/file/d/1iikVYeVkUC__Qu6TInhEOJqIdR02RAVp/preview" width="670" height="376" ></iframe>

<br />

### Two-Factor Authentication (2FA)

All connections to Palmetto require 2FA. If you are not enrolled in 2FA yet,
you may enroll using the link <https://2fa.clemson.edu/>.

After you enter your login name and password, Palmetto will ask you to provide
additional authentication for 2FA via one of the following three options for registerd devices
(smart phone or tablet):

```
Using keyboard-interactive authentication.
Duo two-factor login for $user

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-XXXX
 2. Phone call to XXX-XXX-XXXX
 3. SMS passcodes to XXX-XXX-XXXX

Passcode or option (1-3):
```

- Option 1: response to Duo Push to your device by clicking **Approve**
- Option 2: listen to the automatic call from system and select any key on your device.
- Option 3: enter a passcode that is shown in your DUO app.

```
Passcode or option (1-3): 1234567
```

More information can be found at [here](https://ccit.clemson.edu/support/current-students/two-factor-authentication-2fa/)
