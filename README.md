# ops

This creatively named repo contains the Ansible plays that I've written to manage my test
infrastructure on top of Digital Ocean. The idea is to allow me to:

* Spin up a set of droplets according to the specs I want.
* Utilize a dynamic inventory to manage my droplets.
* Configure them properly.

Things that bug me with this:

* The Ansible digital_ocean module doesn't allow you to tag new droplets.
* Therefore: tags cannot be used as groups.
* I need to write a script to fix that during provisioning.
* I can't currently register/deregister against DO DNS. Going to need to script that.
* I can't currently add/remove hosts from the DO load balancer service. More scripting...
* Dynamic inventory grouping is stupid. Yup...more scripting...

If I'd realized how many additional features I'd need when I started, I'd have just spent the
night watching Netflix.

## Usage

If you have nothing as your starting point, do these things from your shell:

```
$ export DO_API_TOKEN='<your digital_ocean API token'
$ export ANSIBLE_HOST_KEY_CHECKING=False
```

These allow the provisioner and the dynamic inventory script to use your Digital Ocean account, and
stop Ansible from asking you to verify SSH host keys on connect. Not awesome, but better than
stacking up failures as your plays execute across multiple machines.

This done, type the following:

```
$ ansible-playbook provisioners/digital_ocean.yml
```

My host naming conventions suck, so edit `vars` as you see fit to change some stuff about.

Once you've provisioned some droplets, it's time to configure them, so:

```
$ ansible-playbook -i inventories/digital_ocean.py site.yml
```

This will do a bunch of stuff that will grow over time, but essentially you'll end up with
patched CentOS 7 droplets that will have been rebooted, Docker installed everywhere, and non-root
users with dynamically generated passwords on each droplet, and your SSH public key written to
authorized_users.

I'd recommend taking a moment to edit the `default/main.yml` for each role. Just a suggestion...

## Copyright

Copyright &copy; 2017 Paul Stevens

## License

Licensed under the MIT License. Go nuts.
