# Galaxy NG

[![Build Status](https://travis-ci.com/ansible/galaxy_ng.svg?branch=master)](https://travis-ci.com/ansible/galaxy_ng)

A Pulp plugin to support hosting your very own Ansible Galaxy server.

This is a brand new take on Ansible Galaxy, so it will look and feel a bit different than the current [galaxy.ansible.com web site](https://galaxy.ansible.com). Over time we expect to migrate the web site to this codebase, so for now you're looking into the future, and you have an opportunity to help shape that future.

Our mission is to help organizations share Ansible automation and promote a culture of collaboration around Ansible automation development. We'll be providing features that make it easy to create, discover, use and distribute Ansible automation content.

To see what we're currently working on, [view the Roadmap](/ROADMAP.rst). 

To learn more about Pulp, [view the Pulp project page](https://pulpproject.org/).

## OpenAPI Spec

View the latest version of the spec by [clicking here](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/ansible/galaxy_ng/master/openapi/openapi.yaml).

## Contributing

* If you're interested in jumping in and helping out, [view the contributing guide](https://github.com/ansible/galaxy_ng/wiki#contributing-to-galaxyng).
* To setup your development environment, [view the development setup guide](https://github.com/ansible/galaxy_ng/wiki/Development-Setup).
* Chat with us on irc.freenode.net: #ansible-galaxy
* Found a bug or have a feature idea? Please [open an issue](https://github.com/ansible/galaxy_ng/issues/new/choose).

## Installation

The following is a simple quickstart for installing a local Galaxy server. It requires that you have [Ansible](https://github.com/ansible/ansible) installed.

1. Clone the Pulp Installer project:

    ```
    git clone https://github.com/pulp/pulp_installer.git
    ```

2. Set your working directory to the `pulp_installer` directory.

3. Create a playbook called `install.yml` that contains the following:

    * **Warning**: please change the `pulp_default_admin_password`  (Initial password for the Pulp admin).

    ```
    - hosts: all
      vars:
        pulp_settings:
          secret_key: secret
          content_origin: "http://{{ ansible_fqdn }}"
          x_pulp_api_host: "{{ pulp_api_host }}"
          x_pulp_api_port: "{{ pulp_api_port }}"
          x_pulp_api_user: "admin"
          x_pulp_api_password: "{{ pulp_default_admin_password }}"
          x_pulp_api_prefix: "pulp_ansible/galaxy/automation-hub/api"
          galaxy_require_content_approval: "False"
        pulp_default_admin_password: password
        pulp_install_plugins:
          pulp-ansible: {}
          galaxy-ng: {}
          pulp-container: {}
        pulp_api_workers: 4
      roles:
        - pulp_database
        - pulp_workers
        - pulp_resource_manager
        - pulp_webserver
        - pulp_content
        - chouseknecht.ansible_galaxy_config
      environment:
        DJANGO_SETTINGS_MODULE: pulpcore.app.settings
    ```

4. Install Ansible role dependencies by running the following commands to download roles from [Community Galaxy](https://galaxy.ansible.com):

    ``` 
    $ ansible-galaxy install -r requirements.yml
    ```
    
    ```
    $ ansible-galaxy install chouseknecht.ansible_galaxy_config 
    ```

5. Run the `install.yml` playbook

   The following provides an example of how to start playbook execution. It assumes an inventory file called `hosts` exists in the current directory and contains the target host(s) where Galaxy server is to be installed: 

    ``` 
    $ ansible-playbook install.yml -i hosts
    ``` 

6. Access the server on port 80. For example, `http://127.0.0.1`. Login using "admin" as the username, and the value assigned to "pulp_default_admin_password" in your playbook as the password.

7. To setup a namespace and publish your fist collection, [view our user guide](https://github.com/ansible/galaxy_ng/wiki/End-User-Installation#uploading-a-collection).

# License

GNU General Public License v2. View [LICENSE](/LICENSE) for full text.
