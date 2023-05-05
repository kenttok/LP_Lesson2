MACHINES = {
  :Lesson2 => {
        :box_name => "centos/7",
	      :disks => {
		            :sata1 => {
			                  :dfile => './sata16.vdi',
			                  :size => 150,
		                    :port => 1
		                      },
		            :sata2 => {
                         :dfile => './sata17.vdi',
                         :size => 150, # Megabytes
			                   :port => 2
		                      },
                :sata3 => {
                          :dfile => './sata18.vdi',
                          :size => 150,
                          :port => 3
                          },
                :sata4 => {
                          :dfile => './sata19.vdi',
                          :size => 150, # Megabytes
                          :port => 4
                          },
                :sata5 => {
                          :dfile => './sata11.vdi',
                          :size => 150, # Megabytes
                          :port => 5

	                         }
                  }
                }
              }

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
 config.vm.synced_folder ".", "/vagrant", disabled: true
      config.vm.define boxname do |box|
      config.vm.post_up_message = "Start2!"
          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s
          box.vm.box_version = boxconfig[:box_version]
          box.vm.provider :virtualbox do |vb|
            	  vb.customize ["modifyvm", :id, "--memory", "1024"]
                  needsController = false
	  boxconfig[:disks].each do |dname, dconf|
          unless File.exist?(dconf[:dfile])
		vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
              config.vm.post_up_message = "Start2!"
              config.vm.post_up_message = dconf[:dfile]
              needsController =  true
          end
		  end
      if needsController == true
        vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
        boxconfig[:disks].each do |dname, dconf|
          vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
        end
      end
          end
 	  box.vm.provision "shell", inline: <<-SHELL
	      mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
	      yum install -y mdadm smartmontools hdparm gdisk
  	  SHELL

      end
  end
end
