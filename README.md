# amx-vagrant

This project is deeply inspired by Vaprobash: https://github.com/fideloper/Vaprobash 

	# Linux:

	$ wget -O Vagrantfile https://goo.gl/k6FNxZ && wget -O provision.sh https://goo.gl/Ktv03C && wget -O hostfile.sh https://goo.gl/HTNNJi

	# Windows (using PowerShell) (untested -- supports goo.gl redirect?) (does not support hostfile.sh):

	$ (new-object System.Net.WebClient).DownloadFile('https://goo.gl/k6FNxZ', 'Vagrantfile')
	$ (new-object System.Net.WebClient).DownloadFile('https://goo.gl/Ktv03C', 'provision.sh')

Edit `Vagrantfile` to fit project requirements

	$ vagrant up

Before `vagrant up`, edit the hostname in the Vagrantfile:

`hostname = "projectname.dev"`

If you need to, you can also update the server_ip.

If you can run Bash (tested on Linux, may or may not work on OSX or Cygwin/Windows Lixux emulators), you can update your hostfile automatically after vagrant is running:

	chmod +x hostfile.sh
	./hostfile.sh
