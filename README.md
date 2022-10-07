# Running VSCode server on HiPerGator

This repo provides information on running VSCode Server on HiPerGator, including running Jupyter notebooks. While I work with Research Computing, and this may eventually be transitioned to our official documentation, **this is currently not officially supported by UF Research Computing.**

Why might you want to do this?

* VSCode is a nice IDE with many great features, including [GitHub Copilot](https://github.com/features/copilot) (which most UF users should be able to access for free--via either student or faculty/staff [GitHub Education](https://education.github.com/) plans).
* [HiPerGator](https://www.rc.ufl.edu/get-started/hipergator/) provides a powerful compute environment with thousands of cores, petabytes of storage, and powerful GPUs.
* But, combining the two can be problematic...
  * Remote SSH setup is complicated by MFA and the need to tunnel into a compute server with your running job. This can be worked out, but is a bit cumbersome.
  * You can accidentally end up running scripts on the login servers and have your account suspended.
  * Running VSCode on HiPerGator through OOD is kind of slow and limiting in terms of extensions and updates.
* With VSCode Server (currently in [private beta](https://code.visualstudio.com/blogs/2022/07/07/vscode-server), 2022-10-07), you can startup the server within an interactive job with whatever resources you have requested and connect to it in your web browser. No MFA issues, install your desired extensions, connect to GitHub Copilot, run and debug your code easily!

## Request access to the VSCode Server private beta

As of this writing, VSCode Server is still in private beta. This means that you will need to request access. See: [https://code.visualstudio.com/blogs/2022/07/07/vscode-server#_getting-started](https://code.visualstudio.com/blogs/2022/07/07/vscode-server#_getting-started) for the sign-up form.

## Install VSCode Server in your ~/bin folder

The installation instructions for VSCode Server will not quite work as-is on HiPerGator as the script that is used relies on `sudo`, which users do not have.

The following modifications are needed.

1. Download and keep the `setup.sh` script:

    ```bash
    wget https://aka.ms/install-vscode-server/setup.sh
    ```

1. Edit the `setup.sh` script:
   * Change line 3 (You can replace `$USER` with your username, though it should work as-is):

        ```diff
        - INSTALL_LOCATION=/usr/local/bin/code-server
        + INSTALL_LOCATION=/home/$USER/bin/code-server
       ```

   * Delete or comment out lines 58-67 (note line numbers are included here):

        ```diff
        - 58  if command_exists sudo;
        - 59  then
        - 60    if [ "$DOWNLOAD_WITH" = curl ]; then
        - 61      sudo curl -sSL "$INSTALL_URL" -o "$INSTALL_LOCATION"
        - 62    else
        - 63      sudo wget -qO "$INSTALL_LOCATION" "$INSTALL_URL"
        - 64    fi
        - 65
        - 66    sudo chown $USER $INSTALL_LOCATION
        - 67  else
        ```

   * Delete or comment out line 73:

        ```diff
        - 73  fi
        ```

1. Run the edited `setup.sh`, which will download and install `code-server` at `~/bin/code-server`, which is in your `$PATH`.

## First run: Launch a development session and start VSCode Server

1. See [this page for information on development sessions HiPerGator](Development_and_Testing), but something like this should work: `srun --mem=4gb --time=01:00:00 --pty bash -i`
1. Start VSCode Server: `code-server`
1. Continue to follow steps 2-5 on the [VSCode Server page](https://code.visualstudio.com/blogs/2022/07/07/vscode-server#_getting-started) to authenticate your GitHub account, name your server (HiPerGator might be a good name), and accept terms.
1. You are mostly set! Though you can add additional extensions, like GitHub Copilot, if you want. 

## For daily use

Now that VSCode Server is setup, it should be relatively easy to start. For the most part, the process is:

1. Log into HiPerGator
1. Start a development session, [requesting GPUs if needed](https://help.rc.ufl.edu/doc/GPU_Access#Interactive_Access), and other resources for the time you want to work.
1. For Jupyter notebooks it is important to:

   * Load Jupyter and the module you need for the kernel you want to use, e.g. for Tensorflow: `module load jupyter tensorflow/2.7.0`
   * Export the `XDG_RUNTIME_DIR` to set temp directory (otherwise it tries to use `/run/user/`, which you can't be written to!): `export XDG_RUNTIME_DIR=${SLURM_TMPDIR}`

1. Start VSCode server: `code-server`
1. Connect to the URL provided in your browser.
1. Code away!
