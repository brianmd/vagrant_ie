# -*- mode: ruby -*-
# vi: set ft=ruby :
# from https://gist.github.com/andreptb/57e388df5e881937e62a
#
# run with box_name=win7-ie11 vagrant up
#     os: xp/vista/win7/win8/win81/win10
#     ie: ie6 (xp only), ie7 (vista only), ie8 (xp/win7), ie9 (win7),
#     ie10 (win7/win8), ie11 (win7/win81), msedge (win10)

# box name into env var, same script can be used with different boxes. Defaults to win7-ie11.
box_name = ENV['box_name'] != nil ? ENV['box_name'].strip : 'win7-ie11'
# box repo into env var, so private repos/cache can be used. Defaults to http://aka.ms
box_repo = ENV['box_repo'] != nil ? ENV['box_repo'].strip : 'http://aka.ms'

puts "\n\nbox_name: #{box_name}\n\n"

Vagrant.configure("2") do |config|
  # If the box is win7-ie11, the convention for the box name is modern.ie/win7-ie11
  config.vm.box = "modern.ie/" + box_name
  # If the box is win7-ie11, the convention for the box url is http://aka.ms/vagrant-win7-ie11
  config.vm.box_url = box_repo + "/vagrant-" + box_name
  # big timeout since windows boot is very slow
  config.vm.boot_timeout = 500

  # rdp forward
  config.vm.network "forwarded_port", guest: 3389, host: 3389, id: "rdp", auto_correct: true

  # winrm config, uses modern.ie default user/password. If other credentials are used must be changed here
  config.vm.communicator = "winrm"
  config.winrm.username = "IEUser"
  config.winrm.password = "Passw0rd!"

  config.vm.provider "virtualbox" do |vb|
    # first setup requires gui to be enabled so scripts can be executed in virtualbox guest screen
    vb.gui = true
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id,  "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
  end
end

