sa-node-nvm
===========

[![Build Status](https://travis-ci.org/softasap/sa-node-nvm.svg?branch=master)](https://travis-ci.org/softasap/sa-node-nvm)

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

         option_nodejs_install_with_nvm: true,
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


Code licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) or the [MIT License] (http://opensource.org/licenses/MIT).

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)

Join gitter discussion channel at [Gitter](https://gitter.im/softasap)
