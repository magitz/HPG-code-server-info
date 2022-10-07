# Running VSCode server on HiPerGator

This repo provides information on running VSCode Server on HiPerGator, including running Jupyter notebooks. While I work with Research Computing, and this may eventually be transitioned to our official documentation, **this is currently not officially supported by UF Research Computing.**

Why might you want to do this?

* VSCode is a nice IDE with many great features, including [GitHub Copilot](https://github.com/features/copilot), which most UF users should be able to access for free (via either student or faculty/staff [GitHub Education](https://education.github.com/) plans).
* [HiPerGator](https://www.rc.ufl.edu/get-started/hipergator/) provides a powerful compute environment with thousands of cores, petabytes of storage, and powerful GPUs.
* But, combining the two can be problematic...
  * Remote SSH setup is complicated by MFA and the need to tunnel into a compute server with your running job. This can be worked out, but is a bit cumbersome.
  * Running VSCode on HiPerGator through OOD is kind of slow and limiting in terms of extensions and updates.
* With VSCode Server (currently in [private beta](https://code.visualstudio.com/blogs/2022/07/07/vscode-server), 2022-10-07), you can startup the server within an interactive job with whatever resources you have requested and connect to it in your web browser. No MFA issues, install your desired extensions, connect to GitHub Copilot, run and debug your code easily!




