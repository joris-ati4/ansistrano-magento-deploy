Ansistrano Magento Deploy
=========

This role allows the user to deploy a Magento 2 application with Ansistrano.

Requirements
------------



Role Variables
--------------

```
# Variables for the Magento deployment

magento_php_path: php # The PHP executable to use for all command line tasks

magento_composer_path: composer
magento_composer_options: '--verbose --no-progress --no-interaction'

magento_static_content_locales: [] # Variable for all the locales that need to be deployed
magento_static_content_jobs: 1 # Also set the number of conccurent jobs to run. The default is 1
magento_themes: [] # You can also set the themes to run against. By default it'll deploy all themes 

magento_bin_path: 'bin/magento' # the relative (to the magento root) path of the magento executable
```

Dependencies
------------

This role has a dependency to Ansistrano

Example Playbook
----------------

```
---
- name: Deploy Magento to server
  hosts: server
  vars:
    ansistrano_deploy_from: "{{ playbook_dir }}/" # Where my local project is (relative or absolute path)
    ansistrano_deploy_to: "/var/www/html" # Base path to deploy to.
    ansistrano_version_dir: "releases" # Releases folder name
    ansistrano_shared_dir: "shared" # Shared folder name
    ansistrano_current_dir: "current" # Softlink name. You should rarely changed it.
    ansistrano_current_via: "symlink" # Deployment strategy who code should be deployed to current path. Options are symlink or rsync
    ansistrano_keep_releases: 3 # Releases to keep after a new deployment. See "Pruning old releases".

    ansistrano_shared_paths: [
      'var/composer_home',
      'var/log',
      'var/export',
      'var/report',
      'var/import',
      'var/import_history',
      'var/session',
      'var/importexport',
      'var/backups',
      'var/tmp',
      'pub/sitemap',
      'pub/media',
      'pub/static/_cache'
    ]

    ansistrano_shared_files: [
      'auth.json',
      'app/etc/env.php',
      'var/.maintenance.ip',
    ]

    ansistrano_deploy_via: git
    ansistrano_allow_anonymous_stats: yes

    # Variables used in the Git deployment strategy
    ansistrano_git_repo: GIT REPO # Location of the git repository
    ansistrano_git_branch: deploy-ansistrano # What version of the repository to check out. This can be the full 40-character SHA-1 hash, the literal string HEAD, a branch name, or a tag name
    ansistrano_git_ssh_opts: "-o StrictHostKeyChecking=no" # Additional ssh options to be used in Git
    ansistrano_git_depth: 1 # Additional history truncated to the specified number or revisions
    ansistrano_git_executable: /usr/bin/git # Path to git executable to use. If not supplied, the normal mechanism for resolving binary paths will be used.

    # Variables for the Magento deployment
    magento_php_path: /usr/bin/php7.4 # The PHP executable to use for all command line tasks
    magento_composer_path: /usr/local/bin/composer1
    magento_composer_options: '--verbose --no-progress --no-interaction'

    # Variables for the Magento deployment
    magento_static_content_locales: ['fr_FR', 'en_US', 'es_ES'] #Variable
    magento_static_content_jobs: 1 # Also set the number of conccurent jobs to run. The default is 1
    magento_themes: [] # You can also set the themes to run against. By default it'll deploy all themes 
  
  roles:
    - { role: joris_ati4.ansistrano_magento_deploy }
```

License
-------

MIT

Author Information
------------------

Created by Joris Hart @ ATI4 Group