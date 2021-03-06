FXOS
====

This section lists the services which are supported with Firepower Extensible Operating System (FXOS) Unicon plugin.
This plugin supports Firepower 2000 series.

  * `execute <#execute>`__
  * `ftd <#execute>`__
  * `fxos <#execute>`__
  * `fxos_mgmt <#execute>`__
  * `expert <#execute>`__
  * `sudo <#execute>`__
  * `disable <#execute>`__
  * `enable <#execute>`__
  * `rommon <#execute>`__
  * `config <#config>`__
  * `switchto <#switchto>`__
  * `reload <#reload>`__

The following generic services are also available:

  * send
  * sendline
  * expect
  * log_user


execute
-------

The services ``ftd``, ``fxos``, ``fxos_mgmt``, ``expert``, ``sudo``, ``disable``, ``enable``,
``rommon`` are aliases to the execute service and are based on the generic execute implementation.
You can use these methods to switch between states and/or execute commands in that state.

When the `execute()` method is used, there is no state change performed, the current state
is left 'as-is' and commands are executed in the state the device is in.

For more information see `execute <generic_services.html#execute>`__

Example usage of the execute services:

.. code-block:: python

        # simple execute call
        output = dev.execute("show version")

        # switch state and execute
        dev.fxos()
        dev.execute('show version')
        dev.execute('another command')

        # switch state and execute combined
        dev.ftd('show version')

        # bring device to rommon and execute command in rommon mode
        # Note: the device will stay in rommon unless the state is switched
        dev.rommon('help')


config
------

For more information see `configure <generic_services.html#configure>`__



switchto
--------

The `switchto` service is a helper method to switch between CLI states. This can be used to switch
to more specific states than e.g. the ``fxos`` method.

The following states are supported:

* `fireos`
* `ftd`
* `fxos`
* `fxos mgmt`
* `expert`
* `sudo`
* `disable`
* `enable`
* `rommon`

===================   ========================    ====================================================
Argument              Type                        Description
===================   ========================    ====================================================
to_state              str or list                 target state(s) to switch to
timeout               int (default 60 sec)        timeout value for the command execution takes.
===================   ========================    ====================================================

The ``fxos`` state allows to specify a `scope` to switch to, e.g. `/system/services`.
See below for an example.

Example usage of the execute services:

.. code-block:: python

        # switch to fxos state
        dev.switchto("fxos")

        # switch to sudo state
        dev.switchto("sudo")

        # switch to specific scope in fxos state
        dev.switchto("fxos scope /system/services")

        # switch via several states
        # this switches to fxos, then ftd and then sudo
        dev.switchto(['fxos', 'ftd', 'sudo'])


reload
------

The reload service executes a device reboot via the ftd prompt. This works with console connections
and with SSH based connections. When SSH is used, the service automatically disconnects and reconnects.
The console output is captured and returned to the caller.

===============   =======================     ================================================================
Argument          Type                        Description
===============   =======================     ================================================================
reload_command    str                         reload command to be issued on device.
                                              default reload_command is "reboot"
reply             Dialog                      additional dialogs/new dialogs which are not handled by default.
timeout           int                         timeout value in sec, Default value is 600 sec
===============   =======================     ================================================================

The following settings can be updated to influence the timers used:

.. code-block::

  # Timeout for console based reboot
  BOOT_TIMEOUT (default: 600 seconds)

  # How many seconds to wait before trying to reconnect after rebooting the device
  RELOAD_WAIT (default: 420 seconds)

  # How many times to try to reconnect
  RELOAD_RECONNECT_ATTEMPTS (default: 3)


Example execution:

.. code-block:: python

  # console output is returned
  output = dev.reload()
