Vagrant.configure("2") do |config| 
    def hardenVBox(vm)
        vm.customize ["modifyvm", :id, "--boot1", "disk"]
        vm.customize ["modifyvm", :id, "--boot2", "none"]
        vm.customize ["modifyvm", :id, "--boot3", "none"]
        vm.customize ["modifyvm", :id, "--boot4", "none"]
        vm.customize ["modifyvm", :id, "--mouse", "ps2"]
        vm.customize ["modifyvm", :id, "--audio", "none"]
        vm.customize ["modifyvm", :id, "--usb", "off"]
    end   
    config.vm.define "metasploitable" do |meta|
        meta.vm.box = "e314c/Metasploitable2"
        meta.vm.box_version = "0.0.1"
        meta.vm.synced_folder '.', '/vagrant', disabled: true

        meta.vm.provider "virtualbox" do |v|
            v.memory = 1024
            v.cpus = 2
            hardenVBox(v)
        end
    end

    config.vm.define "pfsense" do |pfsense|
        pfsense.vm.box = "kennyl/pfsense"
        pfsense.vm.box_version = "2.4.0"
        pfsense.vm.synced_folder '.', '/vagrant', disabled: true

        #NAT, adapter 1: built-in, used by Vagrant to provision
        #bridged, adapter 2
        pfsense.vm.network "public_network", type: "dhcp"
        #LAN/host-only, adapter 3
        pfsense.vm.network "private_network", type: "dhcp", ip: "172.16.1.1"
        #OTP1/internal network, adapter 4
        pfsense.vm.network "private_network", type: "dhcp", virtualbox__intnet: true, ip: "172.16.2.1"

        pfsense.vm.provider "virtualbox" do |v|
            v.memory = 512
            v.cpus = 1
            v.gui = true
            hardenVBox(v)
            v.customize ["modifyvm", :id, "--macaddress2", "080027DD12F8"]
            v.customize ["modifyvm", :id, "--macaddress3", "080027B736E4"]
            v.customize ["modifyvm", :id, "--macaddress4", "0800275A6EC4"]
        end
    end

    config.vm.define "kali" do |kali|
        kali.vm.box = "kalilinux/rolling"
        kali.vm.synced_folder '.', '/vagrant', disabled: true

        kali.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 2
            v.gui = true
            hardenVBox(v)
        end
    end

    config.vm.define "splunk" do |splunk|
        splunk.vm.box = "badarsebard/centos-7.5-splunk"
        splunk.vm.box_version = "7.2.3.0"
        splunk.vm.synced_folder '.', '/vagrant', disabled: true

        splunk.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 2
            hardenVBox(v)
        end
    end

    config.vm.define "snort" do |snort|
        snort.vm.box = "ubuntu/xenial64"
        snort.vm.synced_folder '.', '/vagrant', disabled: true

        snort.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 2
            hardenVBox(v)
        end

        #todo: provide a variable to pass in the Oinkcode
        #todo: include the relevant autosnort files in this directory, patched for Vagrant config
        #(need to patch in the o_code into full_autosnort.conf)
    end
end
  