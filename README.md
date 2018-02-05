sa-node-nvm
===========

[![Build Status](https://travis-ci.org/softasap/sa-node-nvm.svg?branch=master)](https://travis-ci.org/softasap/sa-node-nvm)
[![Includes support for Windows with PS5](https://img.shields.io/badge/Windows-Friendly-blue.svg)](https://img.shields.io/badge/Windows-Friendly-blue.svg)

Installs nvm node version manager, and, optionally nodejs with it. Suitable for development. For binary installation see sa-node role.

nodejs_version: "0.10.38"  # Could be exact node version


Usage example:

```YAML


     - {
         role: "sa-node-nvm",
         nvm_version: "0.31.1"
       }


```

```YAML

     - {

         role: "sa-node-nvm",

         nvm_version: "0.31.1",

         deploy_user: "{{ansible_user_id}}",

         option_install_nodejs_with_nvm: true,
         nodejs_version: "0.12"
         option_integrate_w_bash: true,
         option_integrate_w_zsh: true

       }

```


Example of using nvm in further steps:

```YAML

- name: Detect npm
  shell: 'source /home/{{deploy_user}}/.profile && dirname "`which npm`"'
  args:
     executable: /bin/bash
  register: npm_path_detected_raw

- name: WSI Workplace | Install bower
  npm: name=bower state=present version="{{bower.version}}" global=yes
  become: "{{npm_is_global}}"
  environment:
    PATH: "{{npm_path_detected}}:{{ ansible_env.PATH }}"       # can be different depending on nvm version

```


# Windows support

For windows support we expect, that box is prepared for provisioning with ansible (best used with role  https://github.com/softasap/sa-box-bootstrap-win ,
but if you configured the same setup manually will work too )

Example of the typical windows play:

```YAML

vars:
  - root_dir: ..

  - ansible_connection: winrm
  - ansible_ssh_port: 5986
  - ansible_winrm_server_cert_validation: ignore
  - ansible_winrm_transport: ssl


pre_tasks:
  - debug: msg="Pre tasks section"

  - name: gather facts
    setup:

roles:
   - {
       role: "sa-node-nvm"
     }

```

Don't forget, that this is not the exact copy of linux nvm, thus command switches differ.
In particular - activating nvm on windows is `nvm on`

Usage with ansible galaxy workflow
----------------------------------

If you installed the sa-node-nvm role using the command

`
   ansible-galaxy install softasap.sa-node-nvm
`

the role will be available in the folder library/softasap.sa-node-nvm
Please adjust the path accordingly.

```YAML

     - {
         role: "softasap.sa-node-nvm"
       }

```




Copyright and license
---------------------

Code is dual licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) and the [MIT License] (http://opensource.org/licenses/MIT). Choose the one that suits you best.

Reach us:

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)

Join gitter discussion channel at [Gitter](https://gitter.im/softasap)

Discover other roles at  http://www.softasap.com/roles/registry_generated.html

visit our blog at http://www.softasap.com/blog/archive.html
