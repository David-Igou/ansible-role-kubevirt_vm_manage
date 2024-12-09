kubevirt_vm_manage
=========

This role manages the state of a kubevirt vm and optionally configures networking and storage options.

Requirements
------------

Currently this role only caters to a default openshift-virtualization installation but will eventually expand support

Role Variables
--------------

### Virtualmachine variables

|Variable Name|Type|Default Value|Required|Description|Example|
|:---|:---:|:---:|:---:|:---|:---|
|`vm_state`|string|"present"|no|The state of the virtual machine|'absent'|
|`vm_name`|string|""|yes|The name of the virtualmachine object|"foobar"|
|`vm_namespace`|string|""|yes|Target namespace to deploy objects|"foobar"|
|`vm_labels`|dictionary|""|no|Labels to add to the virtualmachine object||
|`vm_os_image_datasource`|string|"rhel9"|no|Datasource in `openshift-virtualization-os-images` namespace to use (wip) |"rhel9"|
|`vm_virtualmachineclusterpreference`|string|"rhel.9"|no||"rhel.9"|
|`vm_user_data`|string|`10`|no|||
|`vm_services`|string|"rhel.9"|no||"rhel.9"|
|`vm_routes`|list|`[]`|no|List of routes to create for the virtual machine.||

### `vm_services` Variables

|Variable Name|Type|Default Value|Required|Description|Example|
|:---|:---:|:---:|:---:|:---|:---|
|`name`|string|`""`|yes|The name of the service.|`"my-service"`|
|`type`|string|`"ClusterIP"`|no|The type of the service. Valid values are `ClusterIP`, `NodePort`, or `LoadBalancer`.|`"NodePort"`|
|`ports`|list|`[]`|yes|List of ports to expose. Each port is a dictionary containing `port`, `targetPort`, and optionally `nodePort` and `protocol`. if `port` is not defined, its value defauts to `targetPort`|`[{ port: 80, targetPort: 8080 }]`|
|`annotations`|dictionary|`{}`|no|Annotations to add to the service.|`{ "example.com/key": "value" }`|
|`labels`|dictionary|`{}`|no|Labels to add to the service.|`{ "app": "my-app" }`|

### `vm_routes` Variables

|Variable Name|Type|Default Value|Required|Description|Example|
|:---|:---:|:---:|:---:|:---|:---|
|`name`|string|`""`|yes|The name of the route.|`"my-route"`|
|`service_name`|string|`""`|yes|The name of the service this route will expose.|`"my-service"`|
|`hostname`|string|`""`|no|The external hostname for the route.|`"example.com"`|
|`service_port`|integer|`""`|no|The service port to target. Defaults to the first port in the service.|`80`|
|`tls`|dictionary|`{}`|no|TLS configuration for the route, including `termination` and `insecureEdgeTerminationPolicy`.|`{ "termination": "edge", "insecureEdgeTerminationPolicy": "Redirect" }`|
|`annotations`|dictionary|`{}`|no|Annotations to add to the route.|`{ "haproxy.router.openshift.io/timeout": "1m" }`|


Dependencies
------------

ansible-galaxy collection install -r requirements.yml to be installed
Currently:
  kubernetes.core
  and
  kubevirt.core

Example Playbook
----------------

```yaml
- name: Set the state of an Kubevirt VM
  hosts: sno
  gather_facts: false
  vars:
    create_namespace: false
    set_stat: true
    vm_state: "absent"
    vm_labels:
      created_by: "ansible"
    vm_name: kubevirt-manage-testing
    vm_instancetype: "u1.small"
    vm_namespace: "default"
    vm_os_image_datasource: rhel9
    vm_virtualmachineclusterpreference: "rhel.9"

    vm_services:
    - name: service-1
      type: ClusterIP
      clusterIP: None
      ports:
        - target_port: 9090
    - name: service-2
      type: NodePort
      ports:
        - target_port: 9090

    vm_routes:
      - name: route-1
        service_name: service-1
        hostname: cockpit-test.apps.sno.igou.systems
        tls:
          insecureEdgeTerminationPolicy: Redirect
          termination: passthrough
  roles:
    - ansible-role-kubevirt_vm_manage
```

License
-------

MIT

Author Information
------------------

igou.david@gmail.com