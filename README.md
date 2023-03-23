_This project is released under the A-GPL License (see enclosed [LICENSE.txt](LICENSE.txt))_

---

0 - Credits
===========
This project is heavily based on the official [ansible-ubuntu](https://github.com/monarc-project/ansible-ubuntu) project, released by the [MONARC Project](https://www.monarc.lu/).

1 - What's this?
================

The goal of this project is to help users deploying a _small_ instance of [Monarc ](https://www.monarc.lu/). By _small_, I mean that you're interested **ONLY** in the [Monarc-FrontOffice component](https://github.com/monarc-project/MonarcAppFO) and you don't want to deploy all the other components (namely: the CFG, BOs and RPX components depicted [here](https://www.monarc.lu/documentation/technical-guide/#global-architecture)).

In short, you simply need to deploy a single FO (FrontOffice), just to start scratching the surface of your RiskAssessment process.

In such a case, this project include a set of _ansible_ playbooks and roles  that you can fire towards a freshly launched Ubuntu _focal_ (20.04), to get back a running Monarc FrontOffice instance, up-and-running.

    Please note that this project is still **UNDER CONSTRUCTION** so, at the moment, it **DOESN'T** work!

2 - How this works?
===================
To get your MonarcFO up-and-running, you simply need to:

1. clone this project;

2. set a MySQL password, by creating a `_vault.yml` file. A sample `_vault.yml.sample` is provided, so you can simply adapt it;

3. update the `hosts` file, by setting proper IP address of the freshly installed Ubuntu _focal_ box. Eg.:
~~~
[verzulli@XPS monarc_deployer]$ cat hosts
[all:vars]
ansible_connection=ssh

[monarc_demo]
172.16.16.202
[verzulli@XPS monarc_deployer]$
~~~

4. check and update the variables defined in `vars.yml`. Eg.:

~~~
[verzulli@XPS monarc_deployer]$ cat vars.yml 
monarcfo_version: "v2.12.5-p4"
monarcfo_release_url: "https://github.com/monarc-project/MonarcAppFO/releases/download/{{ monarcfo_version }}/MonarcAppFO-{{ monarcfo_version }}.tar.gz"
www_vhname: "monarc.garr.lab"
db_user: "mon_user"
instance_name: "monarc@GARRLab"
emailFrom: "monarc-noreply@garrlab.it"
twoFactorAuthEnforced: true
[verzulli@XPS monarc_deployer]$
~~~

5. ensure you have **SSH PASSWORDLESS ACCESS** to your Linux box, and ensure you have NOPASS `sudo` permissions, like this:

~~~
[verzulli@XPS monarc_deployer]$ ssh ubuntu@172.16.16.202 'sudo id'
uid=0(root) gid=0(root) groups=0(root)
[verzulli@XPS monarc_deployer]$ 
~~~

Now everything should be ready to fire _ansible_:

6. launch ansible playbook `monarc_setup_fo.yml`

~~~
[verzulli@XPS monarc_deployer]$ ansible-playbook -u ubuntu -l monarc_demo monarc_setup_fo.yml
~~~

...and go getting a cup of coffee....

When finished, it should print something like:

~~~
[...]
PLAY RECAP *******************************************************************************************************
172.16.16.202              : ok=32   changed=14   unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   

[verzulli@XPS monarc_deployer]$
~~~

and you're running Monarc-FO should be ready in:

`http://172.16.16.102`

Please note that pre-configured `admin` account cretentials, are the one mentioned in the official guide, namely, at the end of [this paragraph](https://www.monarc.lu/documentation/technical-guide/#prepared-virtual-machine)