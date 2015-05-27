# amx-vagrant

Before `vagrant up`, edit the hostname in the Vagrantfile:

`hostname = "projectname.dev"`

If you need to, you can also update the server_ip. By default it is random (to avoid conflicts).

You might want to configure the document root to be `/public`:

	synced_folder = "/usr/share/nginx/html"
	public_folder = "/usr/share/nginx/html/public"

If you can run Bash, you can update your hostfile automatically after vagrant is running:

	chmod +x hostfile.sh
	./hostfile.sh
