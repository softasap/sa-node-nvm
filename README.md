sa-node-nvm
===========

[![Build Status](https://travis-ci.org/softasap/sa-node-nvm.svg?branch=master)](https://travis-ci.org/softasap/sa-node-nvm)

Installs nvm node version manager, and, optionally nodejs with it. Suitable for development. For binary installation see sa-node role.

nodejs_version: "0.10.38"  # Could be exact node version


Usage example:

<pre>


     - {
         role: "sa-node-nvm",
         nvm_version: "0.31.1"
       }


</pre>

<pre>

     - {

         role: "sa-node-nvm",

         nvm_version: "0.31.1",

         deploy_user: "{{ansible_user_id}}",

         option_install_nodejs_with_nvm: true,
         nodejs_version: "0.12"
         option_integrate_w_bash: true,
         option_integrate_w_zsh: true

       }

</pre>


Example of using nvm in further steps:

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
