---
layout: post
title: Automated deployment from BitBucket to Hostgator
---

Long ago when I was looking for a good hosted Distributed Version Control System (DVCS) it came down to two options; 
GitHub or BitBucket.  I went with Bitbucket because it provided free private repositories.

This is a brief guide on setting up an automated deployment process from BitBucket to Hostgator. To start with I'm 
assuming that you already have BitBucket and Hostgator setup for your project.

# SSH in to Hostgator
Use an SSH client such as Putty (if you're on Windows) to connect to your Hostgator terminal. Do the following to 
generate a public key. You'll need this key later on to enable the connection between BitBucket and HostGator.

{% highlight console %}
mkdir ~/.ssh
chmod 700 ~/.ssh
cd ~/.ssh
ssh-keygen
{% endhighlight %}  

Once you enter ssh-keygen, you'll be prompted for a passphrase. You could enter a phrase, or just leave it blank. In 
the end this will create a set of public/private keys in the .ssh directory.

Copy the public key from id_rsa.pub.  One way to do this is to output the content of this file within Putty, 
selecting that output and right-clicking to copy to the clipboard.

{% highlight console %}
cat ~/.ssh/id_rsa.pub
{% endhighlight %}  

# Add the public key to BitBucket
Log in to BitBucket and go to the 'Manage Account' screen.  Then select 'SSH Keys' and 'Add key'.

Enter in some name for your SSH Key, and paste in your public key (that you obtained above), and save it by clicking 
'Add key'.

# Verify connectivity
Go back to your PuTTY session. Run the following command to verify connectivity between BitBucket and Hostgator
{% highlight console %}
ssh -T git@bitbucket.org
{% endhighlight %}  

You may be prompted with an `Are you sure you want to continue connecting` message. If so, type in `yes`. If all goes 
well, you should see a message similar to the following

{% highlight console %}
The authenticity of host 'bitbucket.org (XXX)' can't be established.
RSA key fingerprint is XXX.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'bitbucket.org,XXX' (RSA) to the list of known hosts.
logged in as XXX.

You can use git or hg to connect to Bitbucket. Shell access is disabled.
{% endhighlight %}

# Pull Code to HostGator 

The good news is git is already available on Hostgator by default.  The approach you take to pull your source code to
 your hosting site depends on whether you're pulling it in to a subfolder or directly under the root.  In either case
  you'll need to get the path to the branch you want to pull.
 
If you're pulling code to a subfolder, just clone your repository and you're good to go to the next step.
{% highlight console %}
git clone git@bitbucket.org:<username>/<repositoryname>.git
{% endhighlight %}

Instead of the above, if you wish to pull the repository directly under the root folder, the above command will give 
you an error saying that the `destination path already exists and is not an empty directory`In 
that case do the following

{% highlight console %}
git init
git remote add origin git@bitbucket.org:<username/<repositoryname>.git
git fetch
{% endhighlight %}

# Automate the process

Create a php file, git-hook.php on your hosting site with the following code. Note that the `exec(`git pull ... )` 
command is exactly the same command you would execute on the PuTTY terminal to pull the code manually.

{% highlight php %}
<?php
echo "Begin: Pull code from BitBucket<br/>";
exec('git pull git@bitbucket.org:<username>/<repositoryname>.git', $output);
foreach ($output as $o) {
    echo $o . '<br/>';
}
echo "End: Pull code from BitBucket<br/>";
{% endhighlight %}

Then go back to BitBucket's setting sfor that repository and click on 'Hooks', then select 'POST' from the drop down 
and click on 'Add Hook'.  In the form that appears enter in the URL to the PHP file you created above and 'Save'.

# Verify the automated deployment process
Now try pushing a change to your repository. WIthin a few seconds this change should now be visible (automatically) 
on your live site!