# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # https://docs.vagrantup.com

  config.vm.box = "ubuntu/bionic64"
  
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--usbxhci", "on"]
    vb.customize ["usbfilter", "add", "0", "--target", :id, "--name", "FTDI FT232R USB UART", "--vendorid", "0403", "--productid", "6001"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y linux-image-extra-virtual setserial python3-pip
    pip3 install scons
    printf '%s\n' \
      'KERNEL=="ttyUSB[0-9]*", MODE="0666", RUN+="/bin/setserial /dev/%k low_latency"' \
      'KERNEL=="ttyACM[0-9]*", MODE="0666", RUN+="/bin/setserial /dev/%k low_latency"' \
    > /etc/udev/rules.d/50-ttyusb.rules
  SHELL
end
