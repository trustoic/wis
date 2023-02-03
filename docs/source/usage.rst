HSM Partition - quick build
=====

.. _installation:

Summary
------------

- Install the Luna client on the CA;
- Create a partition on the HSM;
- Configure the client for NTLS;
- Initialise the partition; and
- Configure the KSP.

.. code-block:: console

   (.venv) $ pip install lumache

Prerequisites and Dependencies:
----------------


- the HSM appliance is already networked and initialized;
- Software, PED Keys are all available

Install the Luna client on the CA
----------------

- Rick click Luna LunaHSMClient.exe (10.4.1), select Run as administrator, and select the following as install options:
    - Install location: C:\Program Files\SafeNet\LunaClient
    - Luna Devices \ Network
    - Luna Devices \ USB
    - Luna Devices \ Backup
    - Luna Devices \ Remote PED
    - Features \ CSP (CAPI) / KSP (CNG)
    - Select the “I agree to the terms of the Thales Licence Agreement”
- Click Install
- Edit the crystoki.ini file to include the ``RSAKeyGenMechRemap=1`` under the ``[MISC]`` section.

Create a partition on the HSM
----------------

- ssh to the HSM
- Login into the HSM as the HSM SO ``hsm login`` (usually need the remote PED)
- Create the partition using the following command where “myhsmpartition” is the partition name: ``partition create -partition <myhsmpartition>``

Configure the client for NTLS
----------------

- run the clientconfig to set up NTLS between the CA server and the HSM:
 this is ``kind``

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

