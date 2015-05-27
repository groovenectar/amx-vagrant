# amx-vagrant

Before `vagrant up`, edit the hostname in the Vagrantfile:

`hostname = "projectname.dev"`

You might want to configure the document root to be `/public`:

	synced_folder = "/usr/share/nginx/html"
	public_folder = "/usr/share/nginx/html/public"
