.. Licensed under the Apache License: http://www.apache.org/licenses/LICENSE-2.0
.. For details: https://github.com/nedbat/coveragepy/blob/master/NOTICE.txt

.. This file is processed with cog to create the tabbed multi-syntax
   configuration examples.  If those are wrong, the quality checks will fail.
   Running "make prebuild" checks them and produces the output.

.. [[[cog
    from cog_helpers import show_configs
.. ]]]
.. [[[end]]] (checksum: d41d8cd98f00b204e9800998ecf8427e)


.. _plugins:

========
Plug-ins
========

Coverage.py's behavior can be extended with third-party plug-ins.  A plug-in is
a separately installed Python class that you register in your .coveragerc.
Plugins can alter a number of aspects of coverage.py's behavior, including
implementing coverage measurement for non-Python files.

Information about using plug-ins is on this page.  To write a plug-in, see
:ref:`api_plugin`.

.. versionadded:: 4.0


Using plug-ins
--------------

To use a coverage.py plug-in, you install it and configure it.  For this
example, let's say there's a Python package called ``something`` that provides
a coverage.py plug-in called ``something.plugin``.

#. Install the plug-in's package as you would any other Python package:

   .. code-block:: sh

    $ python3 -m pip install something

#. Configure coverage.py to use the plug-in.  You do this by editing (or
   creating) your .coveragerc file, as described in :ref:`config`.  The
   ``plugins`` setting indicates your plug-in.  It's a list of importable
   module names of plug-ins:

   .. [[[cog
        show_configs(
            ini=r"""
                [run]
                plugins =
                    something.plugin
                """,
            toml=r"""
                [tool.coverage.run]
                plugins = [ "something.plugin" ]
                """,
            )
   .. ]]]

   .. tabs::

       .. code-tab:: ini
           :caption: .coveragerc

           [run]
           plugins =
               something.plugin

       .. code-tab:: toml
           :caption: pyproject.toml

           [tool.coverage.run]
           plugins = [ "something.plugin" ]

       .. code-tab:: ini
           :caption: setup.cfg, tox.ini

           [coverage:run]
           plugins =
               something.plugin

   .. [[[end]]] (checksum: 788b15abb3c53370ccae3d9348e65385)

#. If the plug-in needs its own configuration, you can add those settings in
   the .coveragerc file in a section named for the plug-in:

   .. [[[cog
        show_configs(
            ini=r"""
                [something.plugin]
                option1 = True
                option2 = abc.foo
                """,
            toml=r"""
                [tool.coverage.something.plugin]
                option1 = true
                option2 = "abc.foo"
                """,
            )
   .. ]]]

   .. tabs::

       .. code-tab:: ini
           :caption: .coveragerc

           [something.plugin]
           option1 = True
           option2 = abc.foo

       .. code-tab:: toml
           :caption: pyproject.toml

           [tool.coverage.something.plugin]
           option1 = true
           option2 = "abc.foo"

       .. code-tab:: ini
           :caption: setup.cfg, tox.ini

           [coverage:something.plugin]
           option1 = True
           option2 = abc.foo

   .. [[[end]]] (checksum: 71aa2ad856e03d228758fd5026fd3a52)

   Check the documentation for the plug-in for details on the options it takes.

#. Run your tests with coverage.py as you usually would.  If you get a message
   like "Plugin file tracers (something.plugin) aren't supported with
   PyTracer," then you don't have the :ref:`C extension <install_extension>`
   installed.  The C extension is needed for certain plug-ins.


Available plug-ins
------------------

Some coverage.py plug-ins you might find useful:

* `Django template coverage.py plug-in`__: for measuring coverage in Django
  templates.

  .. __: https://pypi.org/project/django_coverage_plugin/

* `Conditional coverage plug-in`__: for measuring coverage based
  on any rules you define!
  Can exclude different lines of code that are only executed
  on different platforms, python versions,
  and with different dependencies installed.

  .. __: https://github.com/wemake-services/coverage-conditional-plugin

* `Mako template coverage plug-in`__: for measuring coverage in Mako templates.
  Doesn't work yet, probably needs some changes in Mako itself.

  .. __: https://bitbucket-archive.softwareheritage.org/projects/ne/ned/coverage-mako-plugin.html
