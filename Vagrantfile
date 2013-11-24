# -*- mode: ruby -*-
# vi: set ft=ruby :

SSH_USERNAME = "ubuntu"
SSH_PRIVATE_KEY_PATH = ENV["AWS_SSH_PRIVATE_KEY"]

AWS_ACCESS_KEY_ID = ENV["AWS_ACCESS_KEY_ID"]
AWS_SECRET_ACCESS_KEY = ENV["AWS_SECRET_ACCESS_KEY"]
AWS_REGION = ENV["AWS_DEFAULT_REGION"]

AWS_SECURITY_GROUP = "default"
AWS_KEYPAIR_NAME = ENV["AWS_KEYPAIR_NAME"]
# AMI ID http://cloud-images.ubuntu.com
# Ubuntu 12.04.3 LTS ap-northeast1 64bit root store:instance
AWS_AMI = "ami-17118c16"
# m1.large => 644.946883305 seconds
# m1.xlarge => 583.072030988 seconds
# c1.xlarge => 559.082035988 seconds
AWS_INSTANCE_TYPE = "c1.xlarge"
AWS_IAM_INSTANCE_PROFILE_NAME = "ec2-berkshelf-ruby-build"

RBENV_INSTALL_PREFIX = "/opt"

SIMPLE_RUBY_BUILD_VERSION = "2.0.0-p353"
SIMPLE_RUBY_BUILD_AWS_S3_BUCKET = "simple-ruby-build"


Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  #  Set the version of chef to install using the vagrant-omnibus plugin
  config.omnibus.chef_version = :latest

  #config.vm.hostname = "berkshelf-ruby-build"

  # Every Vagrant virtual environment requires a box to build off of.
  #config.vm.box = "precise-server-cloudimg-amd64"
  config.vm.box = "aws-dummy"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  #config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"

  # Assign this VM to a host-only network IP, allowing you to access it
  # via the IP. Host-only networks can talk to the host machine as well as
  # any other machines on the same network, but cannot be accessed (through this
  # network interface) by any external networks.
  #config.vm.network :private_network, ip: "33.33.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.

  # config.vm.network :public_network

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  config.ssh.max_tries = 40
  config.ssh.timeout   = 120

  # The path to the Berksfile to use with Vagrant Berkshelf
  # config.berkshelf.berksfile_path = "./Berksfile"

  # Enabling the Berkshelf plugin. To enable this globally, add this configuration
  # option to your ~/.vagrant.d/Vagrantfile file
  config.berkshelf.enabled = true

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to exclusively install and copy to Vagrant's shelf.
  # config.berkshelf.only = []

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.except = []

  config.vm.provider :aws do |aws, override|
    override.ssh.username = SSH_USERNAME
    override.ssh.private_key_path = SSH_PRIVATE_KEY_PATH

    aws.access_key_id = AWS_ACCESS_KEY_ID
    aws.secret_access_key = AWS_SECRET_ACCESS_KEY
    aws.region = AWS_REGION

    aws.security_groups = [ AWS_SECURITY_GROUP ]
    aws.keypair_name = AWS_KEYPAIR_NAME
    aws.ami = AWS_AMI
    aws.instance_type = AWS_INSTANCE_TYPE

    aws.iam_instance_profile_name = AWS_IAM_INSTANCE_PROFILE_NAME

    aws.tags = {
      :Name => "berkshelf-ruby-build",
      :Description => "berkshelf simple ruby-build",
    }
  end

  config.vm.provision :chef_solo do |chef|
    #chef.cookbooks_path = "cookbooks"
    #chef.roles_path = "roles"

    chef.json = {
      :rbenv => {
        :user           => "root",
        :group          => "root",
        :install_prefix => RBENV_INSTALL_PREFIX,
      },
      "simple-ruby-build" => {
        :ruby_version  => SIMPLE_RUBY_BUILD_VERSION,
        :aws_region    => AWS_REGION,
        :aws_s3_bucket => SIMPLE_RUBY_BUILD_AWS_S3_BUCKET,
        :auto_shutodwn => true,
      },
    }

    chef.run_list = [
                     "recipe[python::default]",
                     "recipe[simple-ruby-build::rbenv_dir]",
                     "recipe[rbenv::default]",
                     "recipe[rbenv::ruby_build]",
                     "recipe[rbenv::rbenv_vars]",
                     "recipe[simple-ruby-build::default]",
                    ]
  end
end
