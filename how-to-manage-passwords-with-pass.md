How to manage passwords with pass

#Background

###Why a password manager?

Well, if you found this page and are interessted in pass, you must already have your reasons
to look into password managers. For me, it's a basic concept: Use password only once. Don't (**ever**)
reuse passwords or passphrases for other services. If one service gets compromised, you won't
automatically have to worry about your other services. This makes remebering passwords a bitch,
especially if you don't iterate through numbers of your favorite, easy-to-guess, passwords.
Speaking of which, yes, there are tools out there, that can generate very good dictonaries based
on a bit of social engenieering. So you really should use generated passwords. 

###Why pass?

Well, there is a ton of really good password-managers out there. I mean **a lot**. I used 
[gorilla](https://github.com/zdia/gorilla/wiki) a long time, but at some point, I wanted to 
sync my passwords with my home computer, my laptop and my work computer. (Yes, arguably, keeping
your password safe sync'd to your work computer bears its risks, but it all comes down to whether
you trust your employer to not backdoor your work computer. I use FDE (full-disk-encryption) and 
my password safe is password protected and encrypted, too, but that won't save you, if your 
employer is determined to eavsdrop on you.)

Anyway, if I change a password on one system and maybe add another on my second system, I will run
into problems using gorilla (gorilla uses a single, encrypted file).

So something new was needed.

First, I looked into online, cloud serving password managers. I ran across [lastpass](https://www.lastpass.com), 
which worked really well with all my systems. Unfortunatly, I was left in the lurch, because I 
didn't want to use the browser plugin for firefox, but rather a standalone app. Lastpass provides such
a solution, even for Linux, but it has two drawbacks:

1. It doesn't work on a reasonably new Ubuntu Linux (see below for my ticket to lastpass)

	"I am using Ubuntu 17.04 and pocket_x86 does not start, since it complains about the version ob libpng. libpng was updated to v1-6 but your application needs v1-2.
	
	The same problem also applies to the oder version of ubuntu, since it also uses the newer version of libpng. The problem was also reported on your forum (see here: https://forums.lastpass.com/viewtopic.php?f=14&t=232835&p=780625&hilit=pocket_x64#p780625) where the exact same problem was described more than half a year ago!
	
	Could you please update the dynamically linked application to also allow the new version of libpng? (or give me the source code and I'll update your application..)
	I do not use FIrefox (or Chrome or any other supported browser) and I do not use windows, so right now I'm stranded and can only use your online website to access my passwords."
   
2. It is closed source. 

They replied that they forwarded it to the development branch, but I never heard back and they closed the ticket. 
I didn't try it since then.

You need to trust lastpass. I do have trust issues, but I was willing to give it a try. I stoped using lastpass
since then, because I couldn't even debug the code myself.

So I decided I needed something else. Since I love my Terminal I got intrigued by the idea of [pass](https://www.passwordstore.org).
It has a built-in git support, which works really well and combined with [pass-tomb](https://github.com/roddhjav/pass-tomb#readme)
provides a reasonably secure password store. 

###What makes pass different?

Well, pass uses GPG to encrypt/decrypt your passwords. It saves all your passwords in a directory tree. Each directory
works like a folder in your previous password manager. The directory is stored at ~/.password-store .
You can just enter 

	pass

to get an overview or, for example:

	pass Email/yahoo

to get your email password for yahoo. To get a nice overview see [here](https://www.passwordstore.org).

I didn't like that everyone with access to my (decrypted) HDD can see, where I signed up, so I use the pass-tomb extension, as well.
It encrypts your whole .password-store folder into a tomb file with your GPG key.

###How to set pass up

First I installed pass. It can be found in most package systems of mondern ditros, but I found it won't work
(out of the box) with pass-tomb. So I opted to download it manually and install it manually:

*You will need git and gpg for this to work*

First, lets import the public signing key

        gpg --fetch-keys http://www.zx2c4.com/keys/AB9942E6D4A4CFC3412620A749FC7012A5DE03AE.asc

Now lets download pass in a temporary directory (or wherever you like):

        mkdir tmp
        cd tmp
	git clone https://git.zx2c4.com/password-store
        cd password-store

To verify the integraty of the git branch, run the following command:

        git tag -v 1.7.1

If version 1.7.1 isn't the curent one, run git tag to get an overview of all the tags.

It shoud read somewhere:

        gpg: Good signature from "Jason A. Donenfeld <Jason@zx2c4.com>"

Now, you are ready to set up pass. Just run:

        sudo make install

Thats it. Now you could already start using it. I opted to install the pass extension
for tomb. Acording to the pass-tomb website:

        A pass extension that helps to keep the whole tree of password encrypted inside a tomb.

So, first intsall the dependencies:

	sudo yum install zsh sudo cryptsetup pinentry-curses
or

	sudo apt-get install zsh sudo cryptsetup pinentry-curses

Next, install tomb:

	wget https://files.dyne.org/tomb/Tomb-2.4.tar.gz
	wget https://files.dyne.org/tomb/Tomb-2.4.tar.gz.asc
	gpg --keyserver pool.sks-keyservers.net --recv 4ACB7D10
	gpg --verify Tomb-2.4.tar.gz.asc Tomb-2.4.tar.gz

It shoud read somewhere:

	gpg: Good signature from "Denis Roio (Jaromil) <jaromil@dyne.org>"

Next, untar, cd into the directory and install

	tar xvfz Tomb-2.4.tar.gz
	cd Tomb-2.4
	sudo make install


So, to add it to tomb, do the following (in your tmp directory):

        git clone https://github.com/roddhjav/pass-tomb/
        cd pass-tomb
        sudo make install

Thats it.


###How does it work?

Pass, by itself, uses GPG to encrypt your password. That automatically means, you will need gpg. I assume, you already have
gpg installed. If you haven't got a gpg private key, or want to create a seperate one, just for your passwords (what I did)
you will need to generate one:

	$ gpg --gen-key
	gpg (GnuPG) 1.4.21; Copyright (C) 2015 Free Software Foundation, Inc.
	This is free software: you are free to change and redistribute it.
	There is NO WARRANTY, to the extent permitted by law.
	
	Please select what kind of key you want:
	   (1) RSA and RSA (default)
	   (2) DSA and Elgamal
	   (3) DSA (sign only)
	   (4) RSA (sign only)
	Your selection? 1
	RSA keys may be between 1024 and 4096 bits long.
	What keysize do you want? (2048) 4096
	Requested keysize is 4096 bits
	Please specify how long the key should be valid.
	         0 = key does not expire
	      <n>  = key expires in n days
	      <n>w = key expires in n weeks
	      <n>m = key expires in n months
	      <n>y = key expires in n years
	Key is valid for? (0) 0
	Key does not expire at all
	Is this correct? (y/N) y
	
	You need a user ID to identify your key; the software constructs the user ID
	from the Real Name, Comment and Email Address in this form:
	    "Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>"

	Real name: Your name
	Email address: email@provider.com
	Comment: Password key
	You selected this USER-ID:
	    "Your name (Password key) <email@provider.com>"
	
	Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
	You need a Passphrase to protect your secret key.
	
	We need to generate a lot of random bytes. It is a good idea to perform
	some other action (type on the keyboard, move the mouse, utilize the
	disks) during the prime generation; this gives the random number
	generator a better chance to gain enough entropy.

	gpg: key ABCD1234 marked as ultimately trusted
	public and secret key created and signed.
	
	gpg: checking the trustdb
	gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
	gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
	pub   4096R/ABCD1234 2017-07-17
	      Key fingerprint = ABCD EFGH 1234 5678 3C7D  8558 0B49 5647 CD6F 83A4
	uid                  Your name (Password key) <email@provider.com>
	sub   4096R/EFGH6789 2017-07-17

With that out of the way, you can initialize the password store. I do recommend using pass-tomb
extension. So with pass-tomb, you first need to create a tomb:

	pass tomb <gpg-id>

where gpg id is the ke id of your GPG key you generated earlier.

If you run into an error like this:

	[x] Error : Unable to forge the password tomb.

you will need more information. Do the following:

	rm .password.tomb
	pass tomb -v <gpg-id>

Unfortunatly on my system, tomb detects a swap partion (inside an LVM voulme group which is encrypted)
but won't continue, since in would pose a security risk, if the swap would be unencrypted. Tomb
cannot distingues between that, so it complains:

	  .  tomb  .  tomb lock /home/user/.password.tomb -k /home/user/.password.tomb.key
	  .  tomb  .  An active swap partition is detected...
	  .  tomb [W] This poses a security risk.
	  .  tomb [W] You can deactivate all swap partitions using the command:
	  .  tomb [W]  swapoff -a
	  .  tomb [W] [#163] I may not detect plain swaps on an encrypted volume.
	  .  tomb [W] But if you want to proceed like this, use the -f (force) flag.
	  .  tomb [E] Operation aborted.
	 [x] Error : Unable to forge the password tomb.

so, I turned off my swap partion for a lack of a better option (you can't pass 
the -f flag to tomb using pass-tomb extension and I was too lazy to hack pass-tomb..)

	sudo swapoff -a

now, it should work.

You will need to enter your passphrase from your GPG key. Tomb initializes and is ready to work.

From now on, you will need to enter 
	
	pass open

before accessing your passwords. When open, you can work with it like described above. When you're done
just enter
	
	pass close

For whatever reason, it throws an error when closing, I haven't figured out why, yet, but the tomb
closes nevertheless and detaches. If you found a way to verbose the output of *pass close* please let
me know and send me an email ;-)

###Syncing

So, to setup a git repository, I used my raspberry pi at home. I started a blank repository

	git init --bare ~/.password.git

Next, on your work PC init a git repository:

	pass open
	pass git init
	pass git remote add origin ssh://username@raspberry_ip/absolute/path/to/.password.git/
	pass git push origin master

(Yes I use ssh to connect to my pi. You will therefore need to set it up. I assume you know how to do that.
I reccommend adding your ssh pulic key to the authorized_keys file of your raspberry to avoid needing
to enter the password everytime you snyc).


From your second PC, you just need to do the same, but replace the last line with:

	pass git pull

Remember to first open your pass tomb, otherwise you will sync the wrong directory!

(pass tomb mounts the tomb over your ~/.password-store directory. So when you forget to open 
your tomb before, it will complain about not initilizing the git repository first and obviously
there won't be any passwords, because pass uses the empty not-mounted ~/.password-store directory)

You can further automate the procedure with a git hook to automatically pull when opened and push when you 
change something. But, again, I was lazy and just do *pass git pull* and *pass git push* by hand.


Hope that helped,

Florian

Tags: linux, pass, security, tomb, sync, howto
