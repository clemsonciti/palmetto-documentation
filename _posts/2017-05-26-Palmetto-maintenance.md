---
title: "Palmetto cluster bi-annual maintenance"
categories: maintenance 
permalink: palmetto-may2017-maintenance.html
tags: [news]
---

Please save the date for the next bi-annual Palmetto outage and
benchmark which is scheduled to begin on Friday, May 26th.

**Connecting to Palmetto after the outage**   

- Starting on Monday, May 29th all connections to Palmetto will require two-factor authentication (2FA). All users will need to enroll in Clemson 2FA prior to accessing Palmetto after the outage. You may enroll into 2FA using the link <https://2fa.clemson.edu/>. More information about Clemson 2FA implementation may be found [here](http://ccit.clemson.edu/support/faculty-staff/two-factor-authentication/).
- You have to SSH to `login.palmetto.clemson.edu` to connect to the Palmetto cluster, which will require Duo 2FA. Connecting to `user.palmetto.clemson.edu` will not work anymore.  
- 2FA will be required for secure shell (command line) access and JupyterHub web based access 
- Passwordless authentication to connect to Palmetto will be disabled.

For any questions and concerns regarding how 2FA on Palmetto will 
impact your work please contact Palmetto Admin Team using 
<ithelp@clemson.edu> with keyword "Palmetto" in the subject line. 

**Outage schedule**

The whole Palmetto cluster will be unavailable starting Friday,
May 26th. Phases 1-6, and 8c will be put back in service 
as soon as the maintenance is complete (Monday, May 29th or 
earlier).

Remaining phases will stay offline for the following week for 
benchmark and will be put back in service after the benchmark is 
completed (Monday, June 5th).

Please schedule important compute jobs to finish before Friday,
May 26th to make sure the outage will affect your work as little
as possible.

**We would like to remind you also that all scratch systems will
be reinitialized during the maintenance and that all data stored
there will be removed. If you keep any important information on any
of the scratch systems please move it to a safe location prior to
Friday, May 26th.**
