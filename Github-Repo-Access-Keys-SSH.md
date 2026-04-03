# Github Repo Access Keys (SSH)

<p><span>(This page was first created by Dr. Jonathan Cook in January 2023 and was updated by Dr. Huiping Cao in January 2025)</span></p>
<p><span>Github does not support allowing username/password access when using Git commands on your Github-hosted repositories. You have to set up keys to do this. There are alternative ways of doing this, but this page is only going to describe setting up SSH keys. Note that these directions are for Linux/Unix; MacOS should be similar (but you need the XCode tools I think); for possible Windows directions, see the bottom of this page.&nbsp;</span></p>
<h2 id="step-1-generate-keys">Step 1: Generate Keys</h2>
<p><span>In Linux/Unix and other platforms, there is a command-line tool named &ldquo;ssh-keygen&rdquo; that will generate keys for you. These keys use public key cryptography and so they generate a </span><strong>pair</strong><span> of keys: one private key and one public key. These keys work together to provide security, you can find many places on the Web to read about it. I recommend that you accept the default name &ldquo;id_rsa&rdquo; or "id_ed25519" and that you use an empty passphrase when generating them. </span><span>The default name on our CS server at the time of updating this page (January 2025) is "id_ed25519".&nbsp;</span></p>
<p><span>If you are already using SSH keys and want to keep your GitHub key separate, you can name your generated keys something other than the default "</span><span class="s1">id_ed25519"</span><span>, perhaps something like &ldquo;gitkey&rdquo;. If you already have &ldquo;id_ed25519&rdquo; keys you should <strong>not</strong> generate new keys! If you do the following commands and see the same output, <strong>do not</strong> generate new keys!</span></p>
<pre>prompt&gt; cd
prompt&gt; ls .ssh
id_ed25519 id_ed25519.pub
</pre>
<p><span>For "id_rsa" keys, you will see the below information </span></p>
<pre>prompt&gt; cd
prompt&gt; ls .ssh
id_rsa id_rsa.pub
</pre>
<p><span>For example, in one machine, I run the command and see the below output, it means that I can generate new keys. </span></p>
<pre>prompt&gt; ls .ssh
  known_hosts &nbsp; &nbsp;known_hosts.old
</pre>
<p><strong>Be careful</strong><span> when using the commands below; if you already have a default "id_ed25519" key (or &ldquo;id_rsa&rdquo; key) that you are using for other things and you don&rsquo;t enter a new filename, you will overwrite and lose your existing keys! Generating them in a command line terminal at one of our CS server looks like this:</span></p>
<pre>prompt&gt; cd
prompt&gt; ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/local-home-folder/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /local-home-folder/.ssh/id_ed25519
Your public key has been saved in /local-home-folder/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:PUQzQzhPrSfbuB8ShW60/kNP9NnfyFRvw4/VndkPAX8 <i>user-name@server-name</i>
The key's randomart image is:
+--[ED25519 256]--+
|         o*.     |
|        o..=o    |
|         ++..o   |
|         =+o. + E|
|        S B* ..=X|
|         oooo +*X|
|          oo.= **|
|          .o..= =|
|           .o.   |
+----[SHA256]-----+
prompt&gt; ls ~/.ssh
id_ed25519  id_ed25519.pub        (and possibly other stuff)</pre>
<p><span>Generating them in a command line terminal at an iMac looks like this:</span></p>
<pre>prompt&gt; cd
prompt&gt; ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/local-user-name/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/local-user-name/.ssh/id_ed25519
Your public key has been saved in /Users/local-user-name/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:NuTvoat4cxjEKTN/Q0+zNmpYVADir72zOAaYdmkM7Ac <i>local-user-name@local-machine-name</i>
The key's randomart image is:
+--[ED25519 256]--+
|    . ....       |
|   . .    .      |
|.   .. ...       |
| E  +.+oo o      |
|.o+ .*.oSo o     |
|oo.* oo.+o=      |
|. +.. .* +o.     |
|    oo*.+o .     |
|   .oo+Bo..      |
+----[SHA256]-----+
</pre>
<p><span>Note that I just hit Return/Enter on all the input requests. The result of this command is two files: the file &ldquo;id_ed25519&rdquo; (with no extension) contains the private key, and the file &ldquo;id_ed25519.pub&rdquo; contains the public key. These are stored in text so you can look at them easily, in the terminal window or in a text editor.</span></p>
<h3 id="only-if-you-entered-a-custom-name-step-1a-move-your-key-files"><strong>ONLY If You Entered a Custom Name: Step 1a: Move Your Key Files</strong></h3>
<p><strong>NOTE:</strong> if you followed the step above and ran the command inside your <em>.ssh"</em> directory (or folder), then you can skip this step.</p>
<p>Your key files must be moved into your ~/.ssh directory (or folder). &ldquo;~&rdquo; is just a shorthand name for your home directory, so this is a directory named &ldquo;.ssh&rdquo; in your home directory. The name must be exactly that &ndash; dot-s-s-h &ndash; and all lowercase. The SSH tools use this directory as a place to store configuration information, and keys. The Unix command &ldquo;ls&rdquo; which is used to list your files and directories will <strong>not</strong> show this by default &ndash; it skips and directory or file name that begins with dot. You have to do &ldquo;ls -a&rdquo; to list all of these &ldquo;hidden&rdquo; files and directories. If you have never used SSH to access your account, you may not have this directory yet; if not, create it. Then move both of the key files into this directory. This can be done as:</p>
<pre><code>prompt&gt; mv gitkey gitkey.pub ~/.ssh
prompt&gt; cd
prompt&gt; ls .ssh
config  gitkey  gitkey.pub  id_rsa  id_rsa.pub  known\_hosts
</code></pre>
<p>You probably do not yet have a &ldquo;config&rdquo; file and may not have the &ldquo;id*&rdquo; files or &ldquo;known_hosts&rdquo; either.</p>
<h3 id="only-if-you-entered-a-custom-name-step-1b-create-an-ssh-config-file">ONLY If You Entered a Custom Name: Step 1b: Create an SSH Config file</h3>
<p>You probably do not yet have a config file for SSH; by default the tools don&rsquo;t really need one. But if you do not want to use the defaul <span>&ldquo;id_ed25519&rdquo; key or </span>&ldquo;id_rsa&rdquo; key, you will need one for your Github keys. <strong>In your .ssh directory</strong>, use your favorite text editor (such as <em>gedit</em>) to create a plain text file named &ldquo;config&rdquo; &ndash; exactly that, no capitals and no extension &ndash; and place these three lines in it:</p>
<pre><code># Github keys
Host github.com
IdentityFile ~/.ssh/gitkey
</code></pre>
<p>Those lines have to be exact, and so watch for typos. If you named your key differently, use that name instead of &ldquo;gitkey&rdquo;.</p>
<h2 id="step-2-upload-your-public-key-to-github">Step 2: Upload Your Public Key to Github</h2>
<p><span>Log in to your Github account in your browser. In the upper right is your icon and clicking on it gives you your account menu. Choose &ldquo;Settings&rdquo;. On the settings page is a left-hand menu of options. Click on &ldquo;SSH and GPG Keys&rdquo;. This gets you to the page where you can upload your public key. Click on &ldquo;New SSH Key&rdquo; and give the key some name &ldquo;CS Lab Key&rdquo; or something (e.g., if you often use your MacBook, you can put "macbook-key"), and then copy and paste the contents of your &ldquo;id_ed25519.pub&rdquo; file (or other public key file) into the provided text entry field. </span><strong>BE SURE</strong><span> that you have not introduced extra newline characters or anything like that. </span><strong>Your best bet is to open your public key file in a GUI text editor</strong><span> (such as </span><em>gedit</em><span> or <em>vim </em>on Linux or </span><em>Notepad</em><span> on Windows or </span><em>textedit</em> or <em>vim</em><span> on MacOS) and select all and then copy and paste it.</span></p>
<pre>prompt&gt; vim id_ed25519.pub
</pre>
<p><span>When you click on Save in the Github interface, it will let you know if the data does not form a valid public key. Be sure to copy all of the file contents, even the initial name and closing identity &ldquo;email&rdquo; address. These won&rsquo;t be used but are part of the key information.</span></p>
<h2 id="step-3-access-your-repositories">Step 3: Access Your Repositories</h2>
<p>Now you are all set and you can use the git command line tools to access your repositories. But <strong>BE SURE to use the SSH URL</strong> for the repository and not the HTTPS URL; whenever you are at a repo in Github, it will provide you the option of both kinds of URLS. Use the SSH URL (it begins with &ldquo;<a href="mailto:git@github.com">git@github.com</a>&rdquo; rather than &ldquo;https://")</p>
<p>E.g., When I ran the following commands, they work perfectly without asking me to input any password to connect to github repository.&nbsp;</p>
<pre>promt&gt;git clone git@github.com:nmsu-hcao/test.git
promt&gt;cd test
promt&gt;ls
README.md
promt&gt; vim README.md # make some changes to the readme file
promt&gt; git add README.md
<br />promt&gt; git commit
[main 1fe48c4] update README.md
 1 file changed, 4 insertions(+), 1 deletion(-) <br /><br />promt&gt; git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 10 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 323 bytes | 323.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:nmsu-hcao/test.git
   6581f05..1fe48c4  main -&gt; main
</pre>
<h2 id="step-4-set-up-other-computers-you-want-to-use">Step 4: Set Up Other Computers You Want to Use</h2>
<p>With the private key in your CS user files, any computer in the CS Department will use the keys you just created, so you are all set for the CS department. But GitHub allows you to upload multiple public key files, so you can generate a unique key pair for your home computer, laptop, or any other computer you want to use. It would be better, and safer, to generate unique keys for each computer, rather than copying the keys you just generated to each computer, but you can also copy them if you want to.</p>
<h3 id="resources">Resources</h3>
<p>GitHub has its own <a href="https://docs.github.com/en/authentication/connecting-to-github-with-ssh">SSH key instructions</a>.</p>
<h3 id="possible-instructions-for-windows">Possible instructions for Windows</h3>
<p>I don&rsquo;t use Windows and so don&rsquo;t really know what the issues are, but&hellip;</p>
<p><a href="https://serverfault.com/questions/194567/how-do-i-tell-git-for-windows-where-to-find-my-private-rsa-key">This page</a> has several responses for various Windows tools; as you can read, it depends on what tools you are using.</p>
<p><a href="https://inchoo.net/dev-talk/how-to-generate-ssh-keys-for-git-authorization/">This page</a> has step by step instructions specifically for Git for Windows.</p>
<p><a href="https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/">This Atlassian page</a> has some instructions for setting up keys on Windows (but it is for BitBucket, not Github, and you have to be willing to interpret and apply somewhat differently).</p>
<p>Everything they do in Windows on that page says it must be in a &ldquo;Git Bash&rdquo; shell, so apparently the git tools under windows come with a &ldquo;Git Bash&rdquo; shell&hellip;which is in the Git for Windows toolset.</p>
<p>Hope it helps.</p>
<p>&nbsp;</p>
