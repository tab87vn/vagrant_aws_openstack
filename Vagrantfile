# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"


nodes = {
    'controller'  => [1, 200],
    'compute'  => [1, 201],
    'network'  => [1, 202],
}

	
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = "dummy"
	config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
	config.vm.synced_folder "../3", "/vagrant", id: "vagrant-root"
	
	nodes.each do |prefix,(count, ip_start)|
		count.times do |i|
			config.vm.define "#{prefix}".to_sym do |aws2|
				aws2.vm.hostname = "somename"
							
				aws2.vm.provider :aws do |aws, override|  
					aws.access_key_id = ENV['ACCESS_KEY_ID']
					aws.secret_access_key = ENV['SECRET_ACCESS_KEY']
					aws.keypair_name = ENV['KEYPAIR_NAME']
				
					aws.ami = "ami-3d50120d" #Ubuntu 14.04 LTS
					#if prefix == "compute" or prefix == "controller"
						aws.instance_type = "t2.medium"
					#else 
					#	aws.instance_type = "t2.small"
					#end
					
					#aws.subnet_id = "subnet-b1da23d4"
					aws.private_ip_address =  "172.31.28.#{ip_start+i}"					
					#aws.elastic_ip = true
				
					aws.region = "us-west-2"
					aws.availability_zone = "us-west-2a"
					aws.security_groups = ["tab87vn_master_thesis"]
					if prefix == "compute" or prefix == "controller"
						aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 20 }]
					else 
						aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 15 }]			end

					override.ssh.username = "ubuntu"
					override.ssh.private_key_path = "~/myfiles/keys/ingi2145-tab87vn.pem"
					aws.tags = {
					  'user' => 'tab87vn-node#{i}'
					}
				end
				aws2.vm.provision :shell, :path => "#{prefix}.sh"
			end
		end
    end
end