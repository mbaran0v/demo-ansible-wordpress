## WordPress + PHP-FPM + PerconaDB + Nginx

- require Ansible 1.9 and Vagrant 1.7
- expects CentOS/RHEL 7.X hosts

These playbooks deploy a simple all-in-one configuration of the popular WordPress blogging platform and CMS, frontend by the Nginx web server and the PHP-FPM process manager

then run, like this:
	git clone
	vagrant up
	firefox http://localhost:8037/

The ansible playbook will configure PerconaDB, WordPress, Nginx, and PHP-FPM. When the run is complete, you can hit access server to begin the WordPress configuration.
