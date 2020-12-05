# Setup command-line work environment

We will begin with a maximize terminal windows. In this case, MobaXterm. 
First, we connect to Palmetto

Next, we load the tmux module. It is best to place this in your ~/.bashrc so that the tmux server version match. 

~~~
$ echo 'module load tmux/3.3' >> ~/.bashrc
$ source ~/.bashrc
$ which tmux
/software/external/tmux/tmux
$ cp /zfs/citi/tmux/.tmux.conf ~/
~~~

## Invoking tmux commands via key combinations

- Commands to navigate/control `tmux` are often bound to key combinations. For the remainder of this document, if 
the keys are connected with a `-`, it means they are to be pressed together. If the keys are connection with a `+`, 
it means they are to be pressed separately in the order from left to right. 
- Example 1: `Ctrl-b + d`. 
  - You are to press `Ctrl` then press `b` while holding `Ctrl`, then *release* both `Ctrl` and `b` before press `d`. 
- Example 2: `Ctrl-b + %`. 
- You are to press `Ctrl` then press `b` while holding `Ctrl`, then *release* both `Ctrl` and `b` before press `Shift` 
and then `5` while holding `Shift` (to create the character `%`). 

## Views in tmux

There are three levels of view in `tmux`: session, windows, and panes. 
- Session is the highest level. You can think of a tmux session as a 
working environment for a specific project. 
- Let's create our tmux session called `workspace`. 

~~~
$ tmux new -s workspace
~~~

<img src="../../images/advanced/cli/tmux_01.png" style="width:600px">  

Within a session there can be multiple windows. 







