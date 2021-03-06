.. _debian:

**************
Debian package
**************

The Mopidy Debian package is available from `apt.mopidy.com
<http://apt.mopidy.com/>`__ as well as from Debian, Ubuntu and other
Debian-based Linux distributions.


Installation
============

See :ref:`debian-install`.


Running as a system service
===========================

The Debian package comes with an init script. It starts Mopidy as a system
service running as the ``mopidy`` user, which is created by the package.

The Debian package version 0.18.3-1 and older starts Mopidy as a system
service by default. Version 0.18.3-2 and newer asks if you want to run Mopidy
as a system service, defaulting to not doing so.

If you're running 0.18.3-2 or newer, and you've changed your mind about whether
or not to run Mopidy as a system service, just run the following command to
reconfigure the package::

    sudo dpkg-reconfigure mopidy

If you're running 0.18.3-1 or older, and don't want to use the init script to
run Mopidy as a system service, but instead just run Mopidy manually using your
own user, you need to disable the init script and stop Mopidy by running::

    sudo update-rc.d mopidy disable
    sudo service mopidy stop

This way of disabling the system service is compatible with the improved
0.18.3-2 or newer version of the Debian package, so if you later upgrade to a
newer version, you can change your mind using the ``dpkg-reconfigure`` command
above.


Differences when running as a system service
============================================

If you want to run Mopidy using the init script, there's a few differences
from a regular Mopidy setup you'll want to know about.

- All configuration is in :file:`/etc/mopidy`, not in your user's home
  directory. The main configuration file is :file:`/etc/mopidy/mopidy.conf`.
  You can do all your changes in this file.

- Mopidy extensions installed from Debian packages will sometimes install
  additional configuration files in :file:`/etc/mopidy/extensions.d/`. These
  files just provide different defaults for the extension when run as a system
  service. You can override anything from :file:`/etc/mopidy/extensions.d/` in
  the :file:`/etc/mopidy/mopidy.conf` configuration file.

- The init script runs Mopidy as the ``mopidy`` user. The ``mopidy`` user will
  need read access to any local music you want Mopidy to play.

- To run Mopidy subcommands with the same arguments, and thus the same
  configuration files, as the init script uses, you can use ``sudo service
  mopidy run <subcommand>``. In other words, where you'll usually run::

      mopidy config

  You should instead run the following to inspect the system service's
  configuration::

      sudo service mopidy run config

  The same applies to scanning your local music collection. Where you'll
  normally run::

      mopidy local scan

  You should instead run::

      sudo service mopidy run local scan

- Mopidy is started, stopped, and restarted just like any other system
  service::

      sudo service mopidy start
      sudo service mopidy stop
      sudo service mopidy restart

- You can check if Mopidy is currently running as a system service by running::

      sudo service mopidy status

- Mopidy installed from a Debian package can use both Mopidy extensions
  installed both from Debian packages and extensions installed with pip.

  The other way around does not work: Mopidy installed with pip can use
  extensions installed with pip, but not extensions installed from a Debian
  package. This is because the Debian packages install extensions into
  :file:`/usr/share/mopidy` which is normally not on your ``PYTHONPATH``.
  Thus, your pip-installed Mopidy will not find the Debian package-installed
  extensions.
