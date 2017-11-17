OpenStack Projects
==================

This role can be used to register projects, users and related resources in
OpenStack using the os\_\* modules.

Requirements
------------

The OpenStack keystone API should be accessible from the target host.

Role Variables
--------------

`os_projects_venv` is a path to a directory in which to create a
virtualenv.

`os_projects_auth_type` is an authentication type compatible with
the `auth_type` argument of `os_*` Ansible modules.

`os_projects_auth` is a dict containing authentication information
compatible with the `auth` argument of `os_*` Ansible modules.

`os_projects` is a list of projects to register.
Each item should be a dict containing the following items:
- `name`: The name of the project.
- `description`: A description of the project.
- `project_domain`: The domain in which to register the project.
- `user_domain`: The domain in which to register users.
- `users`: List of users to register. Each user should be a dict containing
  the following items:
  - `name`: The name of the user.
  - `password`: The user's password.
  - `roles`: A list of roles to assign to the user.
  - `domain_roles`: A list of roles to assign to the user on the domain.
  - `openrc_file`: Path to an environment file to create.
- `keypairs`: List of SSH key pairs to register with Nova. Each key pair
  should be a dict containing the following items:
  - `name`: The name of the keypair.
  - `public_key`: The SSH public key contents. Optional.
  - `public_key_file`: Path to the SSH public key on the control host.
- `quotas`: Dict mapping quota names to their values.

Dependencies
------------

This role depends on the `stackhpc.os-shade` and `stackhpc.os-openstackclient`
roles.

Example Playbook
----------------

The following playbook registers an OpenStack project, users and related
resources.

    ---
    - name: Ensure OpenStack projects are registered
      hosts: keystone
      roles:
        - role: stackhpc.os-projects
          os_projects_venv: "~/os-projects-venv"
          os_projects_auth_type: "password"
          os_projects_auth:
            project_name: <keystone project>
            username: <keystone user>
            password: <keystone password>
            auth_url: <keystone auth URL>
          os_projects:
            - name: project1
              description: An example project
              project_domain: default
              user_domain: default
              users:
                - name: user1
                  password: correcthorsebatterystaple
                  roles:
                    - admin
                  domain_roles:
                    - admin
                  openrc_file: /home/user/user1.openrc
              keypairs:
                - name: keypair1
                  public_key_file: /path/to/key
              quotas:
                ram: -1
              
Author Information
------------------

- Mark Goddard (<mark@stackhpc.com>)
