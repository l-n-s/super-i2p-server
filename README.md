# Super I2P server

Automagically deploy [i2pd](http://i2pd.website/) with some common hidden services

Following those instructions you can turn almost any server into i2pd node with 
pre-configured hidden servces.

Supported server OSes:

- Ubuntu 16.04 LTS (or newer)
- Debian 9 
- CentOS 7
- Fedora 27
- probably more in future...

You need to have a root shell on your server with SSH key authentication set up. 

On your personal computer (not server!), Ansible automation software is required:

    sudo apt install ansible

Clone the repo on your personal computer (not server!):

    git clone https://github.com/l-n-s/super-i2p-server.git && cd super-i2p-server

Copy sample inventory file and replace `dummy.server.host.name` in it with 
hostname (or IP address) of your server.

    cp inventory.ini.sample inventory.ini
    vi inventory.ini

Now everything is ready, you can choose which services you would like to have. Available services:

- outproxy: Personal I2P proxy to use regular Internet
- ircd: IRC chat server (ngircd)
- ircbouncer: [ZNC](https://znc.in/) bouncer for better experience with IRC
- xmpp: Decentralized instant messaging server

If you want everything, just run:

    ansible-playbook -i inventory.ini playbook.yml

Otherwise, specify desired services with `--tags` key:

    ansible-playbook -i inventory.ini --tags="outproxy,xmpp" playbook.yml

After `ansible-playbook` will finish, you will have a file in `results` directory
containing information about your I2P services. I'd advise you to change passwords instead of using generated ones.

(Using resulting services is out of scope of this manual, but there will probably be links to other resources in future.)

If you just need to install i2pd without any services:

    ansible-playbook -i inventory.ini --tags="i2pd" --skip-tags="get_info" playbook.yml

If you already have i2pd installed AND if it is managed with init system (like systemd),
you can try to install services without i2pd (on your own risk):

    ansible-playbook -i inventory.ini --tags="ircd,ircbouncer" --skip-tags="i2pd" playbook.yml
    
Keep in mind, that you may have as many servers in your inventory file as you wish, ansible will deploy *Super I2P server* on each one of them:

    [all]
    host.name
    another.host.name
    10.20.30.40

Learn more about Ansible in [docs](https://docs.ansible.com/).

