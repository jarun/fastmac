# fastmac

> Get a MacOS or Linux shell, for free, in around 2 minutes

I don't have a Mac, but I often want to test my software on a Mac, or build software for folks using Macs. Rather than shelling out thousands of dollars to buy a Mac, it turns out we can use GitHub Actions to give us access to one for free! `fastmac` makes this process as simple as possible. Note that this only gives us access to a terminal shell, not a full GUI. See below for how to get started. Here's a little video that shows all the steps (click it for a full-size version):

<a href="https://files.fast.ai/images/fastmac.png"><img src="https://files.fast.ai/images/fastmac-optimize.gif" width="727" /></a>

**NB**: Please check the [GitHub Actions Terms of Service](https://docs.github.com/en/github/site-policy/github-additional-product-terms#5-actions-and-packages). Note that your repo needs to be public, otherwise you have a strict monthly limit on how many minutes you can use. Note also that according to the TOS the repo that contains these files needs to be the same one where you're developing the project that you're using it for, and specifically that you are using it for the "*production, testing, deployment, or publication of [that] software project*".

## Clone template

First, [click here](https://github.com/fastai/fastmac/generate) to create a copy of this repo in your account. Type `fastmac` under "repository name" and then click "Create repository from template". After about 10 seconds, you'll see a screen that looks just like the one you're looking at now, except that it'll be in your repo copy.

**NB**: Follow the  rest of the instructions in repo copy you just made, not in the `fastai/fastmac` repo.

## Run the `mac` workflow

Next, <a href="../../actions?query=workflow%3Amac">click here</a> to go to the GitHub actions screen for the `mac` workflow, and then click the "Run workflow" dropdown on the right, and then click the green "Run workflow" button that appears.

<img width="365" src="https://user-images.githubusercontent.com/346999/92965396-91320680-f42a-11ea-9bc3-90682e740343.png" />

## Access the shell using ssh or browser

After a few seconds, you'll see a spinning orange circle. Click the "mac" hyperlink next to it.

On the next screen, you'll  see another spinning orange circle, this time with "build" next to it. Click "build".

This will show the progress of your Mac that's getting ready for you. After a while, the "Setup tmate session" section will open, and once it's done installing itself, it will repeatedly print lines like this:
```
WebURL: https://tmate.io/t/rXbusP3qkYsfALDSLMQZVwG3d

SSH: ssh rXbusP3qkYsfALDSLMQZVwG3d@sfo2.tmate.io
```

Copy and paste the ssh line (e.g `ssh rXbusP3qkYsfALDSLMQZVwG3d@sfo2.tmate.io` in this case) into your terminal (Windows users: I strongly recommend you use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) if possible) and press <kbd>Enter</kbd>.

You'll see a welcome message. Press <kbd>q</kbd> to remove it, and you'll be in a Mac shell! The shell already has [brew](https://brew.sh/) installed, so you can easily add any software you need.

Instead of using ssh in your terminal, you can paste the "WebURL" value into your browser, to get a terminal in your browser. Whilst this is adequate if you're in a situation that you can't access a terminal (e.g. you have to do some emergency work on your phone or tablet), it's less reliable than the ssh approach and not everything works.

## Stop your session

Your session will run for up to six hours. When you're finished, you should close it, since otherwise you're taking up a whole computer that someone else could otherwise be using!

To close the session, click the red "Cancel workflow" on the right-hand side of the Actions screen (the one you copied the `ssh` line from).

## Use a Linux (Ubuntu) shell

If you need to access a Linux shell, instead of MacOS, follow all the same steps as above, except <a href="../../actions?query=workflow%3Alinux">click this link</a> instead of the one mentioned above. And click "linux" instead of "mac" to access your session.

## Using ssh to connect to other servers

You can ssh from your fastmac/linux instance to your servers. First you have to set up a GitHub secret containing the ssh private key needed to connect to your server. To set one up, [click here](../../settings/secrets/new) to create a new secret, name it `SSH_KEY` (it must be that exact name), and paste your private key file (e.g. `~/.ssh/id_rsa`) contents as the value. Save your secret, and then when you connect using the fastmac/linux steps, you'll find that your terminal has your key ready for use.

**NB**: anyone who has access to your GitHub account can access your `SSH_KEY` contents by running this action. Therefore, you should only use a key which is not security sensitive, or at the very least ensure that it is password protected with a strong password.

## Auto-configuration of your sessions

In your `fastmac` repo, edit the `script-{linux,mac}.sh` files to add configuration commands that you want run automatically when you create a new session. These are bash scripts that are run whenever a new session is created.

Furthermore, any files that you add to your repo will be available in your sessions. So you can use this to any any data, scripts, information, etc that you want to have access to in your fastmac/linux sessions.

## Behind the scenes

`fastmac` is a very thin wrapper around [tmate](https://tmate.io/), so all the features of tmate are available. tmate itself is based on [tmux](https://github.com/tmux/tmux/wiki), so you have all that functionality too. In practice, that means other people can connect to the same ssh session, and you'll all be sharing the same screen! This can be very handy for debugging and support. The integration with Github Actions is provided by [action-tmate](https://github.com/mxschmitt/action-tmate).
