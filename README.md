### Quick Start
```bash
vagrant up --provider=aws
```
### Using OpenStack
```bash
vagrant ssh controller
. /vagrant/openrc
nova service list
nova list
nova image-list
neutron agent-list
neutron net-list
```
You can quickly run a demo script that creates a private tenant network, a floating network with a router and launches an instance running Ubuntu by issuing the following
```bash
/vagrant/demo.sh
```

