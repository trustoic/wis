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

- Right click Luna LunaHSMClient.exe (10.4.1), select Run as administrator, and select the following as install options:
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

- run the clientconfig to set up NTLS between the CA server and the HSM: ``clientconfig deploy -server <hsm ip> -client <ca server ip> -partition <myhsmpartition> -user <hsm user name> -v``

Initialise the partition
----------------

- initialize the partition with the command: `partition init -label myhsmpartition`
- insert the Blue and Red PED keys when prompted (and orange assuming this is being done remotely)
- Setup the CO:
    - login as partition SO: `role logi -n partition so`
    - initialise the CO: `role init -n co`
    - create a challenge passsword for the CO: `role createchallenge -n co`
- Set the partition policies:

.. code-block:: console

   partition changepolicy -policy 18 -value 0
   partition changepolicy -policy 20 -value 5
   partition changepolicy -policy 21 -value 1
   partition changepolicy -policy 22 -value 1
   partition changepolicy -policy 23 -value 1


- Login as CO and change the password and PIN
    - Login as the CO: ``role logi -n co``
    - change the CO primary credential (challenge password): ``role changepw -n co -prompt``
    - change the CO secondary credential (PIN): ``role changepw -n co``
    
Configure the KSP
----------------
**to be completed**

