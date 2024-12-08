kubevirt_vm_managea
=========

This role manages the state of a kubevirt vm and optionally configures networking and storage options.

Requirements
------------

This role can run on any Kubernetes distribution using kubevirt and includes support for Openshift

Role Variables
--------------

todo

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

kubevirt.core collection
kubernetes.core collection

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Set the state of an Kubevirt VM
      hosts: k8s_api
      gather_facts: false
      vars:
        create_namespace: false
        set_stat: true
        vm_state: "present"
        vm_labels:
          created_by: "ansible"
        vm_name: kubevirt-manage-testing
        vm_instancetype: "u1.small"
        vm_namespace: "default"
        vm_os_image_datasource: rhel9
        vm_virtualmachineclusterpreference: "rhel.9"
        vm_user_data:  |
          #cloud-config
          chpasswd:
            expire: false
          user: igou
          ssh_authorized_keys:
            - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDumonWRoahxRVNYQT6dt76OkYyRThQ1e0Z/lAAMHcF4ffpZ138fZVWFHipT9f85EOqLkleqLWH6b3yj37+zOOCJ4lGoTSk0oFK92neiWLGV6ayTsvGojdV/cGrSefUP04FqleZirSiwv52FYEVA21vPNweaB70L3m4i7x7+VaHqVvtPh4qT0LnnWa2Yf6Oq6aQU0WUi7Sd388SVczcWVZlJ9L+iibjtir1sm0NUE4Z+sEwHYCOfO2m6YbN809z2GQz1q+DchM0cJhpwBmwH+MIv3wjahM4Khz+XNz4bjousak63BMnZwqROf4jkoQoMrvy3Q/4WZHvivkLTu/Bj51p7TtFPTN1XNHq4kt5qzLE63HsQyhOy9lGdZpLk8cigZe14aQ1NV5WbXm0YSgPIdXTNgpHtXxzUGHjioqEhoMx4q/YBbIHZAFrX8eYorE0nhSzE63HA4cJsjMS56zAs3gk6SaG2Vux04+NwhAOftbQpF8wzwbS0QzdPzw42XKHMVDmQEW/YtPw8XVC15mmHTu6QEYjzBBYU6Noi37PXWOrad2wkq5bInIdlH6VBRuOQ0tw+9VeUlnYUoS9fD8lxcsuGiN3iVaLH8R4kptirEnr0VUBblo3fe1M3YqNiuqXpcB4HJ7sEaKIcyqEetGFRYFmbnvj4iM9BJ5uDb3pgzzmYw== digou@redhat.com
        vm_services:
          - name: service-2
            port: 9090
            target_port: 9090
            protocol: TCP
        # List of route configurations
        vm_routes:
          - name: route-2
            service_name: service-2
            hostname: cockpit-test.apps.sno.igou.systems
            service_port: 9090
            tls:
              insecureEdgeTerminationPolicy: None
              termination: passthrough
      roles:
        - ansible-role-kubevirt_vm_manage

License
-------

MIT

Author Information
------------------

igou.david@gmail.com