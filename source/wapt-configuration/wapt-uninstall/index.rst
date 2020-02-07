.. Reminder for header structure :
   Niveau 1 : ====================
   Niveau 2 : --------------------
   Niveau 3 : ++++++++++++++++++++
   Niveau 4 : """"""""""""""""""""
   Niveau 5 : ^^^^^^^^^^^^^^^^^^^^

Uninstall WAPT agent from clients
=================================

Uninstall WAPT agent properly from Windows / Linux / macOS hosts

Uninstall WAPT agent on Windows
-------------------------------

If you need to uninstall WAPT agents from clients,
the uninstaller is automatically created in the WAPT install location,
by default it is :file:`C:\\Program Files (x86)\\wapt\\unins000.exe`.

* default silent uninstall of WAPT agent can be achieved
  with the following command:

.. code:: bash

   unins000.exe /VERYSILENT

* an additional argument can be passed to :command:`unins000.exe`
  to cleanup everything:

.. code:: bash

   unins000.exe /VERYSILENT /purge_wapt_dir=1


Complete list of command-line arguments for :command:`unins000.exe`:

===================== =====================================================
Settings              Description
===================== =====================================================
``/VERYSILENT``       Launches unins000.exe silently
``/purge_wapt_dir=1`` Purges WAPT directory (removes all folders and files)
===================== =====================================================


Uninstall WAPT agent on Linux
-------------------------------

* default uninstall of WAPT agent can be achieved
  with the following command, depending on your Linux OS:

.. code:: bash

   # Debian / Ubuntu
   apt-get remove --purge tis-waptagent

   # CentOS / Redhat
   yum remove tis-waptagent

* an additional step can be done using these commands (WIP)

.. code:: bash

   rm -f /opt/wapt/

   # Debian / Ubuntu
   rm /etc/apt/sources.list.d/wapt.list

   # CentOS / Redhat
   rm /etc/yum/yum.repos.d/wapt.list


Uninstall WAPT agent on macOS
-------------------------------

* default uninstall of WAPT agent can be achieved
  with the following command

  .. code:: bash

    # List all files to delete
    pkgutil --only-files --files com.tranquilit.tis-waptagent-enterprise > file_list

    # Remove packages
    sudo pkgutil --forget com.tranquilit.tis-waptagent-enterprise



Re-enable Windows Update before uninstalling
-----------------------------------------------------------

In the case you have used WAPT to manage Windows Updates, you might want to re-enable Windows Update default behavior before uninstalling WAPT agent.

To do so, here is an example package to push before uninstalling WAPT agent :

.. code-block:: python

    # -*- coding: utf-8 -*-
    from setuphelpers import *

    uninstallkey = []

    def install():
        print('Disable WAPT WUA')
        inifile_writestring(WAPT.config_filename,'waptwua','enabled','false')

        print('DisableWindowsUpdateAccess registry to 0')
        registry_set(HKEY_LOCAL_MACHINE,r'Software\Policies\Microsoft\Windows\WindowsUpdate','DisableWindowsUpdateAccess',0,REG_DWORD)

        print('AUOptions registry to 0')
        registry_set(HKEY_LOCAL_MACHINE,r'SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update','AUOptions',0,REG_DWORD)

        print('Enable wuauserv')
        run_notfatal('sc config wuauserv start= auto')
        run_notfatal('net start wuauserv')