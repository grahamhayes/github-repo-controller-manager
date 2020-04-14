2. Edit the ``/etc/repo_controller_manager/repo_controller_manager.conf`` file and complete the following
   actions:

   * In the ``[database]`` section, configure database access:

     .. code-block:: ini

        [database]
        ...
        connection = mysql+pymysql://repo_controller_manager:REPO_CONTROLLER_MANAGER_DBPASS@controller/repo_controller_manager
