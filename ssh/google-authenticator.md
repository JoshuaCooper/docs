<h1>Configuring sshd to support google authenticator on ubuntu 2204 Jammy Jellyfish</h1>
<pre>
sudo apt update
sudo apt-get install libpam-google-authenticator -y
</pre>
<pre>
sudo vim /etc/pam.d/sshd
</pre>

#### Add the following to /etc/pam.d/sshd
<pre>
@include common-password
auth required pam_google_authenticator.so
</pre>
#### Comment out the following in /etc/pam.d/sshd
<pre>
#@include common-auth
</pre>

<pre>
sudo vim /etc/ssh/sshd_config
</pre>

<pre>
UsePAM yes
KbdInteractiveAuthentication yes # 
ChallengeResponseAuthentication yes
AuthenticationMethods publickey,keyboard-interactive # Add this line
</pre>

<pre>
sudo systemctl restart sshd
</pre>


#### As $USER not as root run the following
<pre>
google-authenticator
</pre>
#### Scan the QR code generator with Google Authenticator or Authy and enter the code generated
#### Choose your own options for the next 4 options, I choose Y/Y/Y/Y 


