---
title: How to submit job to owner's node
keywords: [submit, owner's node]
sidebar: documentation_sidebar
permalink: userguide_howto_submit_job_ownernode.html
---
If you belong to an owner's nodes group, you are eligible to submit your job to that nodes either Interactive mode or in Batch script mode.
## Interactive mode
~~~
qsub -I -q $ownernode
~~~

## Batch mode
~~~
qsub job.sh -q $ownernode
~~~
