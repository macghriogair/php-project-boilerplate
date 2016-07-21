# -*- mode: ruby -*-
# vi: set ft=ruby :

params = {
    :host => "awesome.dev",
    :ip => "192.168.33.12"
}

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: params[:ip]
    config.vm.hostname = params[:host]

    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.synced_folder ".", "/var/www",
        :mount_options => ["dmode=777", "fmode=777"]

    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => true
    #{ :mount_options => ["dmode=777","fmode=666"] }

    config.vm.provision "shell", inline: <<-SHELL
        sudo apt-get -y install supervisor

        mailcatcher --http-ip=0.0.0.0

        sed -i "s/scotchbox\.local/#{params[:host]}/" /etc/apache2/sites-available/scotchbox.local.conf
        service apache2 restart
        echo "Apache Config Done."

    SHELL
end
