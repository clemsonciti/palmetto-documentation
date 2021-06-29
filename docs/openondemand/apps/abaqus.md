## ABAQUS/CAE

ABAQUS is a Finite Element Analysis software used
for engineering simulations.
Currently, ABAQUS versions 6.10, 6.13, 6.14 are available on Palmetto cluster
as modules.

~~~
$ module avail abaqus

abaqus/6.10 abaqus/6.13 abaqus/6.14
~~~

To see license usage of ABAQUS-related packages,
you can use the `lmstat` command:

~~~
/software/USR_LOCAL/flexlm/lmstat -a -c /software/USR_LOCAL/flexlm/licenses/abaqus.dat
~~~


### Running ABAQUS interactive viewer

To run the interactive viewer,
you must [log in with X11 tunneling enabled](https://www.palmetto.clemson.edu/palmetto/basic/x11_tunneling/),
and then ask for an interactive session: