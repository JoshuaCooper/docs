<h3>Configuring sshd to support google authenticator on ubuntu 2204 Jammy Jellyfish</h3>
<pre>
sudo apt update
sudo apt-get install libpam-google-authenticator -y
</pre>
### /etc/pam.d/sshd configuration
#### Add the following to /etc/pam.d/sshd
#### Ensure '@include common-auth' is removed from /etc/pam.d/sshd
<pre>
#@include common-auth
@include common-password
auth required pam_google_authenticator.so
</pre>


<pre>
sudo vim /etc/pam.d/sshd
</pre>

### /etc/ssh/sshd_config configuration
#### Add the following to /etc/ssh/sshd_config
#### KbdInteractiveAuthentication is defaulted 'no' 
<pre>
UsePAM yes
KbdInteractiveAuthentication yes
ChallengeResponseAuthentication yes
AuthenticationMethods publickey,keyboard-interactive
</pre>

<pre>
sudo vim /etc/ssh/sshd_config
</pre>


<pre>
sudo systemctl restart sshd
</pre>


#### As {$USER} not as root run the following
<pre>
google-authenticator
</pre>

#### Scan the QR code generator with Google Authenticator or Authy and enter the code generated
#### Choose your own options for the next 4 options, I choose Y/Y/Y/Y 

<pre>
Do you want me to update your "/home/jcooper/.google_authenticator" file? (y/n) y

Do you want to disallow multiple uses of the same authentication
token? This restricts you to one login about every 30s, but it increases
your chances to notice or even prevent man-in-the-middle attacks (y/n) y

By default, a new token is generated every 30 seconds by the mobile app.
In order to compensate for possible time-skew between the client and the server,
we allow an extra token before and after the current time. This allows for a
time skew of up to 30 seconds between authentication server and client. If you
experience problems with poor time synchronization, you can increase the window
from its default size of 3 permitted codes (one previous code, the current
code, the next code) to 17 permitted codes (the 8 previous codes, the current
code, and the 8 next codes). This will permit for a time skew of up to 4 minutes
between client and server.
Do you want to do so? (y/n) y

If the computer that you are logging into isn't hardened against brute-force
login attempts, you can enable rate-limiting for the authentication module.
By default, this limits attackers to no more than 3 login attempts every 30s.
Do you want to enable rate-limiting? (y/n) y
</pre>

### Done
