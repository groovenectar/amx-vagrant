This project is deeply inspired by Vaprobash: https://github.com/fideloper/Vaprobash
and Wes Roberts (@jchook)

	# Linux (and possibly OSX):

	$ curl -L https://goo.gl/FDlS6V > Vagrantfile && curl -L https://goo.gl/Ktv03C > provision.sh && curl -L https://goo.gl/HTNNJi > hostfile.sh
	
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
