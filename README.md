berkshelf-ruby-build
===================================
Berkshelf + Vagnrat for Build ruby via rbenv + ruby-build, upload rbenv directory to your S3.


Requirements
------------
### Platforms
Ubuntu

### Cookbooks
[`simple-ruby-build`](https://github.com/n0ts/cookbook-simple-ruby-build)


Usage
-----
Install [`Berkshelf`](http://berkshelf.com/) and [`vagrant`](http://www.vagrantup.com/) and [`vagrant-aws`](https://github.com/mitchellh/vagrant-aws) plugin.

```
$ vagrant up --provider=aws
```

Environment variables
-----
See `Vagrantfile` for default values.

`AWS_SSH_PRIVATE_KEY` - Path to your SSH private key
`AWS_ACCESS_KEY_ID` -  your AWS access key
`AWS_SECRET_ACCESS_KEY` - your AWS secret key
`AWS_REGION` - your AWs region
`AWS_KEYPAIR_NAME` - Key Pair Name


Vagrantfile variables
-----
See `Vagrantfile` for default values.

`SSH_USERNAME` - SSH user name, default "ubuntu"
`AWS_AMI` - Your AMI ID, default Ubuntu 12.04.3 LTS / ap-northeast1 / 64bit /root store - instance)
`AWS_SECURITY_GROUP` - Securiy group, default "default"
`AWS_INSTANCE_TYPE` - Instance Type, default "m1.xlarge"
`AWS_IAM_INSTANCE_PROFILE_NAME` - IAM role name, default "ec2-berkshelf-ruby-build"
`SIMPLE_RUBY_BUILD_VERSION` - Install ruby version, default "2.0.0-p247"
`SIMPLE_RUBY_BUILD_AWS_S3_BUCKET` - Copy S3 bucket name, default "simple-ruby-build"


Built times by EC2 Instance type
-----
Recommend instance type `c1.xlarge`.

`m1.large` - 644.946883305 seconds
`m1.xlarge` - 583.072030988 seconds
`c1.xlarge` - 559.082035988 seconds.


License and Authors
-------------------
- Author:: Naoya Nakazawa (<me@n0ts.org>)
