# amx-vagrant

This project is deeply inspired by Vaprobash: https://github.com/fideloper/Vaprobash 

	$ wget -O Vagrantfile https://goo.gl/OYzBFy && wget -O provision.sh https://goo.gl/Ktv03C && wget -O hostfile.sh https://goo.gl/HTNNJi
	
Edit `Vagrantfile` to fit project requirements

	$ vagrant up

Before `vagrant up`, edit the hostname in the Vagrantfile:

`hostname = "projectname.dev"`

If you need to, you can also update the server_ip.

If you can run Bash (tested on Linux), you can update your hostfile automatically after vagrant is running:

	chmod +x hostfile.sh
	./hostfile.sh
