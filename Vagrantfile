# -*- mode: ruby -*-
# vi: set ft=ruby :

hostname = "ameronix.dev"
synced_folder = "/var/www/#{hostname}"
public_folder = "/var/www/#{hostname}/public"

# Set a local private network IP address.
# See http://en.wikipedia.org/wiki/Private_network for explanation
# You can use the following IP ranges:
#   10.0.0.1    - 10.255.255.254
#   172.16.0.1  - 172.31.255.254
#   192.168.0.1 - 192.168.255.254
server_ip             = "172.31.#{Random.new.rand(0..255)}.#{Random.new.rand(1..254)}"
server_cpus           = "1"   # Cores
server_memory         = "384" # MB
server_swap           = "768" # Options: false | int (MB) - Guideline: Between one or two times the server_memory

# UTC        for Universal Coordinated Time
# EST        for Eastern Standard Time
# US/Central for American Central
# US/Eastern for American Eastern
server_timezone  = "UTC"

# Database Configuration
mysql_root_password   = "root"   # We'll assume user "root"
mysql_version         = "5.5"    # Options: 5.5 | 5.6
mysql_enable_remote   = "false"  # remote access enabled when true

# Languages and Packages
php_timezone          = "UTC"    # http://php.net/manual/en/timezones.php
php_version           = "5.5"    # Options: 5.5 | 5.6
ruby_version          = "latest" # Choose what ruby version should be installed (will also be the default version)
ruby_gems             = [        # List any Ruby Gems that you want to install
    #"jekyll",
    "sass",
    "compass",
    "foundation"
]

# To install HHVM instead of PHP, set this to "true"
hhvm                  = "false"

# PHP Options
composer_packages     = [        # List any global Composer packages that you want to install
    #"phpunit/phpunit:4.0.*",
    #"codeception/codeception=*",
    #"phpspec/phpspec:2.0.*@dev",
    #"squizlabs/php_codesniffer:1.5.*",
]

# Very slow in vagrant. Probably should just install it locally with global packages anyway.
# nodejs_version        = "latest"   # By default "latest" will equal the latest stable version
# nodejs_packages       = [          # List any global NodeJS packages that you want to install
#     "grunt-cli",
#     "gulp",
#     "bower",
#     #"yo",
# ]

sphinxsearch_version  = "rel22" # rel20, rel21, rel22, beta, daily, stable

if ARGV[0] == 'up'
    print "Edit Vagrantfile to update hostname and IP"
    print "\n\nProvisioning with hostname \"" + hostname + "\" and IP " + server_ip
    print "\n\nContinue? [y/n]"

    begin
        system("stty raw -echo")
        str = STDIN.getc
    ensure
        system("stty -raw echo")
    end

    print "\n"

    if str != 'Y' && str != 'y'
        exit
    end
end

# Config Github Settings
github_username = "groovenectar"
github_repo     = "Vaprobash"
github_branch   = "master"
github_url      = "https://raw.githubusercontent.com/#{github_username}/#{github_repo}/#{github_branch}"

# Because this:https://developer.github.com/changes/2014-12-08-removing-authorizations-token/
# https://github.com/settings/tokens
github_pat          = ""

Vagrant.configure("2") do |config|
    # Set server to Ubuntu 14.04
    config.vm.box = "ubuntu/trusty64"

    config.vm.define "Vagrant" do |vapro|
    end

    # Resolve "stdin: is not a tty" errors
    config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

    if Vagrant.has_plugin?("vagrant-hostmanager")
        config.hostmanager.enabled = true
        config.hostmanager.manage_host = true
        config.hostmanager.ignore_private_ip = false
        config.hostmanager.include_offline = false
    end

    # Create a hostname, don't forget to put it to the `hosts` file
    # This will point to the server's default virtual host
    # TO DO: Make this work with virtualhost along-side xip.io URL
    config.vm.hostname = hostname
    config.vm.network :private_network, ip: server_ip

    #config.vm.network :forwarded_port, guest: 80, host: 8000

    # Use NFS for the shared folder
    # config.vm.synced_folder ".", "/vagrant", id: "core", :nfs => true, :mount_options => ['nolock,vers=3,udp,noatime']
    config.vm.synced_folder ".", synced_folder, :mount_options => ["dmode=777", "fmode=774"]

    # If using VirtualBox
    config.vm.provider :virtualbox do |vb|
        vb.name = hostname

        # Set server cpus
        vb.customize ["modifyvm", :id, "--cpus", server_cpus]

        # Set server memory
        vb.customize ["modifyvm", :id, "--memory", server_memory]

        # Set the timesync threshold to 10 seconds, instead of the default 20 minutes.
        # If the clock gets more than 15 minutes out of sync (due to your laptop going
        # to sleep for instance, then some 3rd party services will reject requests.
        vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]

        # Prevent VMs running on Ubuntu to lose internet connection
        # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        # vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    # If using VMWare Fusion
    config.vm.provider "vmware_fusion" do |vb, override|
        override.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"

        # Set server memory
        vb.vmx["memsize"] = server_memory
    end

    # If using Vagrant-Cachier
    # http://fgrehm.viewdocs.io/vagrant-cachier
    if Vagrant.has_plugin?("vagrant-cachier")
        # Configure cached packages to be shared between instances of the same base box.
        # Usage docs: http://fgrehm.viewdocs.io/vagrant-cachier/usage
        config.cache.scope = :box

        config.cache.synced_folder_opts = {
            type: :nfs,
            mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
        }
    end

    # Adding vagrant-digitalocean provider - https://github.com/smdahlen/vagrant-digitalocean
    # Needs to ensure that the vagrant plugin is installed
    config.vm.provider :digital_ocean do |provider, override|
        override.ssh.private_key_path = '~/.ssh/id_rsa'
        override.ssh.username = 'vagrant'
        override.vm.box = 'digital_ocean'
        override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"

        provider.token = 'YOUR TOKEN'
        provider.image = 'ubuntu-14-04-x64'
        provider.region = 'nyc2'
        provider.size = '512mb'
    end

    ####
    # Base Items
    ##########

    # Provision Base Packages
    config.vm.provision "shell", path: "#{github_url}/scripts/base.sh", args: [github_url, server_swap, server_timezone]

    # optimize base box
    config.vm.provision "shell", path: "#{github_url}/scripts/base_box_optimizations.sh", privileged: true

    # Provision PHP
    config.vm.provision "shell", path: "#{github_url}/scripts/php.sh", args: [php_timezone, hhvm, php_version]

    ####
    # Web Servers
    ##########

    # Provision Nginx Base
    config.vm.provision "shell", path: "#{github_url}/scripts/nginx.sh", args: [server_ip, public_folder, hostname, github_url]

    # Provision Apache Base
    # config.vm.provision "shell", path: "#{github_url}/scripts/apache.sh", args: [server_ip, public_folder, hostname, github_url]

    ####
    # Databases
    ##########

    # Provision MySQL
    config.vm.provision "shell", path: "#{github_url}/scripts/mysql.sh", args: [mysql_root_password, mysql_version, mysql_enable_remote]

    ####
    # Search Servers
    ##########

    # Install Elasticsearch
    # config.vm.provision "shell", path: "#{github_url}/scripts/elasticsearch.sh"

    # Install SphinxSearch
    # config.vm.provision "shell", path: "#{github_url}/scripts/sphinxsearch.sh", args: [sphinxsearch_version]

    ####
    # Search Server Administration (web-based)
    ##########

    # Install ElasticHQ
    # Admin for: Elasticsearch
    # Works on: Apache2, Nginx
    # config.vm.provision "shell", path: "#{github_url}/scripts/elastichq.sh"

    ####
    # In-Memory Stores
    ##########

    # Install Memcached
    # config.vm.provision "shell", path: "#{github_url}/scripts/memcached.sh"

    ####
    # Additional Languages
    ##########

    # Install Nodejs
    # Very slow in vagrant. Probably should just install it locally with global packages anyway.
    # config.vm.provision "shell", path: "#{github_url}/scripts/nodejs.sh", privileged: false, args: nodejs_packages.unshift(nodejs_version, github_url)
    # Install Ruby Version Manager (RVM)
    # config.vm.provision "shell", path: "#{github_url}/scripts/rvm.sh", privileged: false, args: ruby_gems.unshift(ruby_version)

    ####
    # Frameworks and Tooling
    ##########

    # Provision Composer
    config.vm.provision "shell", path: "#{github_url}/scripts/composer.sh", privileged: false, args: composer_packages.join(" ")

    # Install Screen
    config.vm.provision "shell", path: "#{github_url}/scripts/screen.sh"

    # Install Mailcatcher
    # config.vm.provision "shell", path: "#{github_url}/scripts/mailcatcher.sh"

    # Install git-ftp
    # config.vm.provision "shell", path: "#{github_url}/scripts/git-ftp.sh", privileged: false

    ####
    # Local Scripts
    # Any local scripts you may want to run post-provisioning.
    # Add these to the same directory as the Vagrantfile.
    ##########
    # config.vm.provision "shell", path: "./local-script.sh"
end
