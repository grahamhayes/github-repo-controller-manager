Prerequisites
-------------

Before you install and configure the git-repo service,
you must create a database, service credentials, and API endpoints.

#. To create the database, complete these steps:

   * Use the database access client to connect to the database
     server as the ``root`` user:

     .. code-block:: console

        $ mysql -u root -p

   * Create the ``repo_controller_manager`` database:

     .. code-block:: none

        CREATE DATABASE repo_controller_manager;

   * Grant proper access to the ``repo_controller_manager`` database:

     .. code-block:: none

        GRANT ALL PRIVILEGES ON repo_controller_manager.* TO 'repo_controller_manager'@'localhost' \
          IDENTIFIED BY 'REPO_CONTROLLER_MANAGER_DBPASS';
        GRANT ALL PRIVILEGES ON repo_controller_manager.* TO 'repo_controller_manager'@'%' \
          IDENTIFIED BY 'REPO_CONTROLLER_MANAGER_DBPASS';

     Replace ``REPO_CONTROLLER_MANAGER_DBPASS`` with a suitable password.

   * Exit the database access client.

     .. code-block:: none

        exit;

#. Source the ``admin`` credentials to gain access to
   admin-only CLI commands:

   .. code-block:: console

      $ . admin-openrc

#. To create the service credentials, complete these steps:

   * Create the ``repo_controller_manager`` user:

     .. code-block:: console

        $ openstack user create --domain default --password-prompt repo_controller_manager

   * Add the ``admin`` role to the ``repo_controller_manager`` user:

     .. code-block:: console

        $ openstack role add --project service --user repo_controller_manager admin

   * Create the repo_controller_manager service entities:

     .. code-block:: console

        $ openstack service create --name repo_controller_manager --description "git-repo" git-repo

#. Create the git-repo service API endpoints:

   .. code-block:: console

      $ openstack endpoint create --region RegionOne \
        git-repo public http://controller:XXXX/vY/%\(tenant_id\)s
      $ openstack endpoint create --region RegionOne \
        git-repo internal http://controller:XXXX/vY/%\(tenant_id\)s
      $ openstack endpoint create --region RegionOne \
        git-repo admin http://controller:XXXX/vY/%\(tenant_id\)s
