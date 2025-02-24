

.. image:: .img/PyGromosToolsBanner.png
   :target: .img/PyGromosToolsBanner.png
   :alt: 


Welcome to PyGromosTools
========================


.. image:: https://github.com/rinikerlab/PyGromosTools/actions/workflows/CI.yaml/badge.svg
   :target: https://github.com/rinikerlab/PyGromosTools/actions/workflows/CI.yaml
   :alt: CI


.. image:: https://codecov.io/gh/rinikerlab/PyGromosTools/branch/main/graph/badge.svg?token=R36KJCEKEC
   :target: https://codecov.io/gh/rinikerlab/PyGromosTools
   :alt: codecov


.. image:: https://img.shields.io/lgtm/grade/python/g/rinikerlab/PyGromosTools.svg?logo=lgtm&logoWidth=18
   :target: https://lgtm.com/projects/g/rinikerlab/PyGromosTools/context:python
   :alt: Language grade: Python


.. image:: https://img.shields.io/badge/Documentation-here-white.svg
   :target: https://rinikerlab.github.io/PyGromosTools/
   :alt: Documentation


General
-------

   The aim of the module is to bring GROMOS to the Python3 World!
   This repository should make it easier to work with GROMOS in Python and should enable the user to write cleaner, more reliable and adaptable code.

   General informations about functions can be found in our wiki and usage example for many general functions and theire relations are shown in jupyter notebooks in the examples in the example folder.

Content

----


* 
  GROMOS wrappers


  * GromosXX wrapper: for simulation execution
  * GromosPP wrapper: for GROMOS++ program usage

* 
  File handling of all GROMOS file types for automated creation/modification/analysis :


  * 
    coordinate files CNF:


    * read and analyse CNF files
    * generate CNF files from RDKit
    * generate CNF files from SDF

    .. code-block:: python

       cnf = Cnf(input_value="file_name")
       print(cnf.GENBOX)

  * 
    topology files:


    * create topologies from a forcefield

      * GROMOS 2016H66 / 54A7
      * OpenForceField
      * SerenityForceField

    * modify topologies

      * add new atoms
      * modify force parameters

    .. code-block:: python

       top = Top(input_value="file_path")
       top.add_new_SOLUTEATOM(ATNM=42)
       print(top)

  * 
    simulation parameter files IMD


    * a wide option of templates provided
    * modify IMD files to fit your simulation

    .. code-block:: pythons

       imd = Imd(input_value="file_path")
       imd.INITIALISE.TEMPI = 137
       print(imd)

  * 
    trajectories (tre, trc, trg, ...)


    * analyse trajectories with Pandas data frames
    * standard analysis like RSMD, RDF, ... for trc
    * auto saving of results for later use as hdf5
    * ene_ana like tools for tre
    * easy to add costume analysis tools

    .. code-block:: python

       trc = Trc(input_value="file_path")
       print(trc.rmsd().mean())

  * 
    replica exchange files:

    .. code-block::

       repdat.dat

  * classes for single blocks of each of these files.

* 
  Automation and file management system ``gromos_system``


  * offers clean file management for simulations
  * offers a high level of automation
  * equiped with simulation queuing system
  * includes many force fields

  .. code-block:: python

     ff=forcefield_system(name="openforcefield")
     gsys = Gromos_System(work_folder="dir", in_smiles="C1CCCCC1", auto_convert=True, Forcefield=ff)
     print(gsys)

* 
  Simulation Submission and Execution :


  * Different Types of Simulation modules, like MD, SD or Emin.
  * Can be executed locally or on a cluster
  * easy to automatize and combine with analysis routines

  Run on a local machine:

  .. code-block:: python

     from pygromos.files.gromos_system import Gromos_System
     from pygromos.hpc_queuing.submission_systems.local import LOCAL as subSystem
     from pygromos.simulations.modules.preset_simulation_modules import emin

     #define file paths
     root_dir = "./example_files/SD_Simulation"
     root_in_dir = root_dir+"/SD_input"
     cnf_path = root_in_dir+"/6J29_unitedatom_optimised_geometry.cnf"
     top_path = root_in_dir + "/6J29.top"
     sys_name = "6J29"

     #Build gromos System:
     grom_system = Gromos_System(in_cnf_path=cnf_path, in_top_path=top_path,
                                 system_name=sys_name, work_folder=root_in_dir)

     #Run Emin
     emin_gromos_system, jobID = emin(in_gromos_system=grom_system, project_dir=root_dir,
                             step_name=step_name, submission_system=subSystem())

  Run on LSF-Cluster:

  .. code-block:: python

     from pygromos.files.gromos_system import Gromos_System
     from pygromos.hpc_queuing.submission_systems.lsf import LSF as subSystem
     from pygromos.simulations.modules.preset_simulation_modules import emin

     #define file paths
     root_dir = "./example_files/SD_Simulation"
     root_in_dir = root_dir+"/SD_input"
     cnf_path = root_in_dir+"/6J29_unitedatom_optimised_geometry.cnf"
     top_path = root_in_dir + "/6J29.top"
     sys_name = "6J29"

     #Build gromos System:
     grom_system = Gromos_System(in_cnf_path=cnf_path, in_top_path=top_path,
                               system_name=sys_name, work_folder=root_in_dir)

     #Run Emin
     sub_system = subSystem(nmpi=4) #allows parallelization
     emin_gromos_system, jobID = emin(in_gromos_system=grom_system, project_dir=root_dir,
                             step_name=step_name, submission_system=sub_system)

* 
  Other utilities:


  * Bash wrappers for GROMOS
  * Amino acid library

General Information
-------------------

Specifications
^^^^^^^^^^^^^^


* Python >=3.7:
* 
  requires: numpy, scipy, pandas, rdkit

* 
  optional: openforcefield for OpenForceField and Serenityff functions

SETUP
^^^^^

see INSTALL.md file for more informations

Copyright
^^^^^^^^^

Copyright (c) 2020, Benjamin Ries, Marc Lehner, Salome Rieder

Acknowledgements
^^^^^^^^^^^^^^^^

Project based on the 
`Computational Molecular Science Python Cookiecutter <https://github.com/molssi/cookiecutter-cms>`_ version 1.3.
