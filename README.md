# Ansible Role: Deployment

An Ansible role that deploy your application just the same way like Capistrano.

## What means _Capistrano way_?

Capistrano is software written in Ruby, which allows you to deploy your application on remote node.
Here is how it works:

1. On remote node you should have in project workdir directories `/shared`, `/repository` and `/deployments`.
2. In `/repository` directory there is a copy of your source repo (GIT/SVN etc.)
3. When you do deploy, capistrano copies repository directory into `/deployments` dir. 
   Name of copied directory is different - it's always current timestamp (like `/deployments/20171108123453`).
4. Next step is to make symlinks of all directories/files from `/shared` directory into the new deployment directory.
   It allows you to share upload dir or logs directory between deployments.
5. Last step is to create or change symlink `/current` to point to the latest deployment from `/deployments`

## Requirements

There aren't any requirements for this role.

## Role variables

Available variables are listed below, along with default values (see `defaults/main.yml`).

##### Workdir for the role. Inside there will be deployed all applications. It can be also customised per project.

    deployment_default_workdir: ""

##### Project owner and group. It can be also customised per project.

    deployment_default_owner: ""
    deployment_default_group: ""

##### Project deployments data

    deployment_projects: []

##### Example of project deployed with almost all defaults
    deployment_projects: []
        - name: project.name
          repository: git@github.com:some/repo

##### Example of project deployed with all options customized
    deployment_projects: []
     - name: project.name
       workdir: /var/www
       repository: git@github.com:some/repo
       version: "HEAD"
       owner: apache
       group: apache
       shared_files:
         - any/path/to/mount/in/dir/deployment/dir
       ssh_opts: ""

## Dependencies

None.

## Example Playbook

    - hosts: webservers
      vars_files:
        - vars/main.yml
      roles:
        - { role: MWojtowicz.deployment }
        
## License

MIT / BSD

## Author Information

This role was created in 2017 by [Michal Wojtowicz](https://mwojtowicz.it/).