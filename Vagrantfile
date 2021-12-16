machines = {
	"elastic01"       => { "ip" => "10",  "memory" => "2048", "cpus" => "1" },
	"elastic02"     => { "ip" => "11",  "memory" => "2048", "cpus" => "1" },
  "elastic03"         => { "ip" => "12", "memory" => "2048", "cpus" => "1" },
  "kibana"         => { "ip" => "13", "memory" => "1024", "cpus" => "1" },
}

Vagrant.configure("2") do |config|
  config.vm.box = "debian/buster64"
  machines.each do |name,conf|
    config.vm.define "#{name}" do |srv|
      srv.vm.hostname = "#{name}.athena.lab"
      srv.vm.network 'private_network', ip: "192.168.63.#{conf["ip"]}"
      srv.vm.provider 'virtualbox' do |vb|
        vb.name = "#{name}"
        vb.memory = "#{conf["memory"]}"
        vb.cpus = "#{conf["cpus"]}"
      end
      srv.vm.provision "ansible" do |ansible|
        ansible.playbook = "provision/main.yml"
        ansible.become = true
      end

      if "#{name}" == "kibana" then
        srv.vm.provision "ansible" do |ansible|
          ansible.playbook = "provision/install_kibana.yml"
          ansible.become = true
        end
      else
        srv.vm.provision "ansible" do |ansible|
          ansible.playbook = "provision/install_elasticsearch.yml"
          ansible.become = true
        end
      end
    end
  end
end
