---
layout: post
title: Automated deployment from BitBucket to Hostgator
---

Long ago when I was looking for a good hosted Distributed Version Control System (DVCS), it came down to two options; 
GitHub or BitBucket.  Of the two, I chose Bitbucket primarily because they provided free private repositories.

This post describes how to setup an automated deployment workflow from BitBucket to Hostgator.

### SSH to your Hostgator terminal
Use a SSH client such as PuTTY (if you're on Windows) to connect to your Hostgator terminal. Enter the commands 
below to create a `ssh` folder and generate a public key. You'll need this key later on to enable the connection 
between BitBucket and HostGator.

{% highlight console %}
[~]# mkdir ~/.ssh
[~]# chmod 700 ~/.ssh
[~]# cd ~/.ssh
[~]# ssh-keygen
{% endhighlight %}  

Once you enter `ssh-keygen`, you'll be prompted for a pass phrase. You could either enter a phrase, or just leave it 
blank and hit enter. Once this command runs, it will create a set of public/private keys in the .ssh directory.

Copy the public key from id_rsa.pub.  One way to do this is to output the content of this file within PuTTY, 
selecting that output and right-clicking to copy to the clipboard.

{% highlight console %}
[~]# cat ~/.ssh/id_rsa.pub
{% endhighlight %}  

### Add the public key to BitBucket
Log in to BitBucket and go to the 'Manage Account' screen.  Then select 'SSH Keys' and 'Add key'.

Enter a name for your SSH Key, and paste your public key (that key you obtained above), and save it by 
clicking 'Add key'.

### Verify connectivity
Go back to your PuTTY session. Run the following command to verify connectivity between BitBucket and Hostgator
{% highlight console %}
[~]# ssh -T git@bitbucket.org
{% endhighlight %}  

You may be prompted with an "Are you sure you want to continue connecting" message. If so, type in "yes" to continue. If all goes 
well, you should see a message similar to the following.

{% highlight console %}
The authenticity of host 'bitbucket.org (XXX)' can't be established.
RSA key fingerprint is XXX.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'bitbucket.org,XXX' (RSA) to the list of known hosts.
logged in as XXX.

You can use git or hg to connect to Bitbucket. Shell access is disabled.
{% endhighlight %}

### Pull Code to HostGator 

The good news is git is already available on Hostgator by default.  How you pull your code for the first time 
depends on whether you're pulling it into an existing folder (such as the root folder) or pulling it in to a new 
subfolder. In either case you'll need to have the bitbucket SSH path to your source handy.
 
If you're pulling code to a subfolder, just clone your repository and you're good to go to the next step.
{% highlight console %}
[~]# git clone git@bitbucket.org:<username>/<repositoryname>.git
{% endhighlight %}

Instead, if you're pulling your code to an existing folder such as the root folder, the above command errors out 
with a message that the `destination path already exists and is not an empty directory`. So, do the following

{% highlight console %}
[~]# git init
[~]# git remote add origin git@bitbucket.org:<username/<repositoryname>.git
[~]# git fetch
{% endhighlight %}

After the initial clone / fetch, you should be able to pull your code manually using the following
{% highlight console %}
[~]# git pull git@bitbucket.org:<username>/<repositoryname>.git
{% endhighlight %}

### Automate the process

Create a new php file, git-hook.php, on your hosting site with the code below. Note that the `exec('git pull ... ')` 
command contains the same command that you used above to pull the code manually. Also note that the echo 
statements below don't really matter.  You won't actually see any output unless you manually run the PHP 
code.  So, the only code you really need is the exec statement.

{% highlight php %}
<?php
echo "Begin: Pull code from BitBucket<br/>";
exec('git pull git@bitbucket.org:<username>/<repositoryname>.git', $output);
foreach ($output as $o) {
    echo $o . '<br/>';
}
echo "End: Pull code from BitBucket<br/>";
{% endhighlight %}

Once this file has been saved on HostGator, go back to the Settings page of your BitBucket repository and click
 on 'Hooks'. Then select 'POST' from the drop down and click on 'Add Hook'.  In the form that appears type in the URL
  to the PHP file you created above and click 'Save'.

### Verify the automated deployment process
Now try pushing a change to your repository. Within a few seconds your changes should now be visible (automatically) 
on the live site!