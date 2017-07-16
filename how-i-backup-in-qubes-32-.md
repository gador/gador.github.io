How I backup in Qubes 3.2 

##The scenario

So I want to backup my AppVms in Qubes. Obviously it makes sense, to encrypt the resulting
backup file. Not just so a third party will hae problems decoding my private digital life
but also to ensure integraty. See also [here](https://www.qubes-os.org/doc/backup-restore/)

The catch: You should use a high entropy passphrase. But entering that on a regular basis 
didn't seem like it would work for me.

##The setup

I use an external 1 TB HDD attached via usb. The HDD is attached as a block device to a special
*"backup"* AppVM. *backup* is obviously not connected to a NetVM and there is nothing special 
in it. 

##How I use the backup Manager

First, I use the Qubes VM Manager to attach the external HDD as a block device to the *backup* VM. 

In it I run:

	sudo mount /dev/xvdi1 backup/

to mount the HDD in the *backup* folder (which obviously must be created first).

Then, from a dom0 console, I run 

	qvm-run --pass-io vault-nw 'cat /home/user/.backup_key'

So, wait, what? 

	qvm-run --pass-io

is used to copy file(content) between VMs. It can also be used to copy to dom0. 

	vault-nw

is the source VM. In this case, it is my password manager VM.

	'cat /home/user/.backup_key'

This command is executed in the source VM (so in this example *vault-nw*). And the output
is displayed in the dom0 Terminal. Within the vault-nw I exported my laptop backup key to a plain
text file, called *.backup_key*. 

The content is displayed in dom0 Terminal (which is considered ultimatly trusted) and can be copied through
a simple right click.

In the Qubes VM Manager I shutdown every VM that I want to save, then go to System->Backup VMs.
From there I choose all the VMs to backup. Clicking on Next will show the Dialog, where Qubes
wants to know, where to backup to. Here comes the backup VM handy. Here I choose the *backup* VM 
as a target and

	/home/user/backup/

as the backup folder. (You remember? The external HDD was mounted here within the *backup* VM)

Next, I copy/paste the backup key to the dialog and click next. The backup starts.

##Pitfalls

So, the worst case happend and you need to restore your backup. But wait, what was that horrible
long, high entropy passphrase again? Yeah, right, it is in my password-vault VM. But where is that?
In my backup....

So you see, that sucks.

You should therefore have your passphrase somewhere seperate than *within* your backup.

I personally use [pass](https://www.passwordstore.org) and setup a git repository on my homeserver to sync
to. But that is a story for another blog.

If you have questions or remarks, just drop me an email. 

Regards,

Florian

Tags: qubes, backup, howto
