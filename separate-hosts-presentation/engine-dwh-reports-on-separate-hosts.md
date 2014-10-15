
Engine/DWH/Reports on Separate Hosts
====================================

----

Overview
--------

Allow setting up Engine/DWH/Reports on separate hosts

----

Useful Links
------------

Feature pages:

* [Separate-DWH-Host](http://www.ovirt.org/Features/Separate-DWH-Host)
* [Separate-Reports-Host](http://www.ovirt.org/Features/Separate-Reports-Host)

This presentation:

* [Separate-Reports-Host#Presentation](http://www.ovirt.org/Features/Separate-Reports-Host#Presentation)

----

Why?
----

* Load distribution

    In particular, to keep the engine from suffering potentially high load
    sometimes imposed by DWH and/or Reports.

* Other? TODO

    - Security

    - Control and flexibility

----

Historic Perspective - 3.2
--------------------------

* 3.2 had mostly-monolithic setup scripts for engine/dwh/reports

* dwh and reports had to be setup after the engine was, on same
  host, and relied on it

----

Historic Perspective - 3.3
--------------------------

* engine-setup was rewritten base on [Otopi](http://www.ovirt.org/Features/Otopi_Infra_Migration)

* setup of dwh and reports got more features and became more complex, but
  basic structure remained the same

----

Historic Perspective - 3.4
--------------------------

* dwh and reports setup were completely rewritten, to become
  plugins of engine-setup, instead of being independent utilities.

* Thus, a single invocation of engine-setup
  sets up or upgrades whatever that is installed (by yum).

* Functionality remained mostly the same, where dwh and reports
  rely upon the engine configuration and infrastructure

* This required also a large change in the setup code of the engine,
  which became more modular and was split to several different
  packages.


----

What changed
------------

To allow setting up the engine, dwh and reports on separate hosts,
various changes were required, mostly in the setup code of the
various components:

* Engine

* DWH

* Reports

----

What changed in the engine
--------------------------

* Cleanups of existing mostly-separable code

    E.g. removing unneeded dependencies, moving plugins between
    packages.

* Making the engine itself optional

    `Configure Engine on this host (Yes, No) [Yes]:`

    so that if engine setup plugin is installed, user can still
    choose to not configure it. dwh/reports were already optional

----

What changed in the engine 2
----------------------------

* Make postgresql listen on outside network

* Database credentials

    -   First all components query for their own, then all query
        for those they use, if not already supplied

    -   Refactored out the code asking these questions, to be useable
        by dwh/reports

----

What changed in DWH
-------------------

*   Make setup code not rely on engine code, e.g.

    -   do not use constants from the engine
    -   ask for engine db credentials if needed

*   Verify that Engine and DWH versions are compatible

    -   Already done in 3.3 and before
    -   Dropped in 3.4, as they were upgraded at once
    -   Now needed again, can be on different hosts

*   Prevent two DWH instances to work against a single engine
    (not merged yet)

----

What changed in Reports
-----------------------

*   Ask for both engine and DWH db credentials, if needed.

*   Set our own instance of jboss instead of adding ourself as an app
    to the jboss instance of the engine. Even if on same host.

*   Set apache to proxy to us, similarly to the engine. Only if not
    on same host as engine.

*   Therefore, we need our own pki, which is currently quite complex
    to [set up](http://www.ovirt.org/Features/Separate-Reports-Host#PKI).

----

TODO
----

* More clean up... e.g., hide engine-private data that is currently shared

* Make reports pki setup more comfortable

----

Questions?
----------


