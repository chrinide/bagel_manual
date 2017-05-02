.. _hf:

*************
Hartree--Fock
*************

Description
===========

SCF can be run by specifying the following values to the keyword ``title``:

* Restricted HF for closed-shell systems: ``rhf`` or ``hf``
* Restricted open-shell HF: ``rohf``
* Unrestricted HF: ``uhf``

Except for restricted Hartree--Fock, density fitting is used by default. Density fitting basis has to be
specified in the Molecule block (see Molecule section).

RHF can be run with ECP basis sets and and fast multipole method (FMM). For RHF-FMM, ``"cfmm" : "true"``
has to be specified in the Molecule block (see Molecule section).

Keywords
========
The default values are recommended unless mentioned otherwise.

.. topic:: ``thresh`` or ``thresh_scf``

   | **Description**: SCF convergence threshold for the root-mean-squared of the error vector.
   | **Default**: :math:`1.0\times 10^{-8}`
   | **Datatype**: double

.. topic:: ``maxiter`` and ``maxiter_scf``

   | **Description**: number of iterations and number of SCF interations, after which the program will terminate if convergence is not reached.
   | **Default**: :math:`100`
   | **Datatype**: int 

.. topic:: ``diis_start`` and ``diis_size``
   | **Description**: after the specified iteration, we will begin using Pulay’s Direct Inversion in the Iterative Subspace (DIIS)
                      algorithm for the to update the orbitals.
   | **Default**: :math:`1`
   | **Datatype**: int 


.. topic:: ``thresh_overlap``

   | **Description**: Overlap threshold used to identify linear dependancy in the atomic basis set.
                      Increasing this value will more aggressively remove linearly dependent basis vectors.
   | **Default**: :math:`1.0\times 10^{-8}`
   | **Datatype**: double

.. topic:: ``df`` (only for RHF) 

   | **Description**: whether to use density fitting, which is always strongly recommended.
   | **Default** : true (except for FMM)
   | **Datatype**: bool 

.. topic:: ``multipole``

   | **Description**: rank of Cartesian multipole moments printed out.
   | **Default** : :math:`1` (dipoles)
   | **Values** : :math:`1, 2`
   | **Datatype**: int 

.. topic:: ``dma``

   | **Description**: options to print out multipole moments from distributed multipole analysis.
   | **Default** : :math:`0` (not print out)
   | **Values** : :math:`0, 1, 2, 3`
   | **Datatype**: int 


.. topic:: ``charge``

   | **Description**: molecular charge
   | **Default** : :math:`0`
   | **Datatype**: int 

.. topic:: ``nact`` and ``nocc``

   | **Description**: number of unpaired electrons and occupied orbitals

.. topic:: ``restart``

   | **Description**: to restart the calculation from an archive file
   | **Default**: false
   | **Datatype**: bool

Keywords for RHF-FMM
====================

.. topic:: ``ns``

   | **Description**: level of descritization which controls the number of lowest-level boxes in one dimension for FMM
   | **Default**: :math:`4`
   | **Datatype**: int 

.. topic:: ``ws``

   | **Description**: well-separatedness index, which is the number of boxes that must separate
                      two collections of charges before they are considered distant 
                      and can interact through multipole expansions
   | **Default**: :math:`2`
   | **Datatype**: int 

.. topic:: ``lmax``

   | **Description**: order of the multipole expansions in FMM-J
   | **Default**: :math:`10`
   | **Datatype**: int 

.. topic:: ``exchange``

   | **Description**: whether to include far-field exchange using occ-RI-FMM
   | **Default**: false
   | **Datatype**: int 

.. topic:: ``lmax_exchange``

   | **Description**: order of the multipole expansions in FMM-K
   | **Default**: :math:`2`
   | **Datatype**: int 

.. topic:: ``fmm_thresh``

   | **Description**: integral screening threshold used in FMM
   | **Default**: ``thresh_overlap``
   | **Datatype**: double 

Examples
=======
Below are some examples for SCF calculations using RHF, ROHF, UHF, and RHF-FMM.

RHF
---

.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "basis" : "svp",
     "df_basis" : "svp-jkfit",
     "angstrom" : "false",
     "geometry" : [
       { "atom" : "F",  "xyz" : [ -0.000000,     -0.000000,      2.720616]},
       { "atom" : "H",  "xyz" : [ -0.000000,     -0.000000,      0.305956]}
     ]
   },
   
   {
     "title" : "hf",
     "df" : "true",
     "thresh" : 1.0e-8
   }
   
   ]}

The converged SCF energy is :math:`-99.84772354` after :math:`11` iterations.

ROHF
----
.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "symmetry" : "C1",
     "basis" : "svp",
     "df_basis" : "svp-jkfit",
     "angstrom" : "false",
     "geometry" : [
       { "atom" : "C",  "xyz" : [   -0.000000,     -0.000000,      3.000000] },
       { "atom" : "H",  "xyz" : [    0.000000,      0.000000,      0.000000] }
     ]
   },
   
   {
     "title" : "rohf",
     "nact" : 1,
     "thresh" : 1.0e-8
   }
   
   ]}

The converged SCF energy is :math:`-38.16810629` after :math:`11` iterations.

UHF
---
.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "symmetry" : "C1",
     "basis" : "svp",
     "df_basis" : "svp-jkfit",
     "angstrom" : false,
     "geometry" : [
       { "atom" : "O",  "xyz" : [  -0.000000,     -0.000000,      1.500000]},
       { "atom" : "H",  "xyz" : [  -0.000000,     -0.000000,      0.000000]}
     ]
   },
   
   {
     "title" : "uhf",
     "nact" : 1,
     "thresh" : 1.0e-8
   }
   
   ]}

The converged SCF energy is :math:`-75.28410147` after :math:`12` iterations.

RHF-FMM
-------
.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",        
     "symmetry" : "C1",        
     "basis" : "svp",
     "angstrom" : "false",        
     "cfmm" : "true",
     "schwarz_thresh" : 1.0e-8,
     "geometry" : [
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,        0.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,        5.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       10.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       15.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       20.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       25.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       30.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       35.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       40.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       45.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       50.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       55.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       60.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       65.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       70.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       75.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       80.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       85.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       90.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,       95.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      100.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      105.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      110.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      115.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      120.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      125.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      130.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      135.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      140.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      145.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      150.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      155.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      160.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      165.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      170.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      175.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      180.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      185.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      190.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      195.000000 ] },
         { "atom" : "He", "xyz" : [      0.000000,     0.000000,      200.000000 ] }
     ]
   },        
   
   {
     "title" : "hf",        
     "df" : "false",        
     "ns" : 4,
     "ws" : 0.0,
     "lmax" : 10,
     "exchange" : "true",
     "lmax_exchange" : 2,
     "fmm_thresh" : 1.0e-12,
     "thresh" : 8.0e-6
   }
   
   ]}

The converged SCF energy is :math:`-117.05967543` after :math:`3` iterations.

References
==========

+-----------------------------------------------+-----------------------------------------------------------------------+
|          Description of Reference             |                          Reference                                    | 
+===============================================+=======================================================================+
| General text on electronic structure theory   | Szabo A. and Ostlund N. S., Modern Quantum Chemistry: Introduction to |
|                                               | Advanced Electronic Structure Theory, Dover Publications              |
+-----------------------------------------------+-----------------------------------------------------------------------+
| References for fast multipole method in       | White, C. A., Johnson B. G., Gill P. M. W., Head-Gordon M.,           |
| quantum chemistry                             | Chem. Phys. Lett. **230**, 8 (1994)                                   |
|                                               +-----------------------------------------------------------------------+
|                                               | Strain M. C., Scuseria G. E., Frisch M. J., Science **271**, 51 (1996)|
+-----------------------------------------------+-----------------------------------------------------------------------+
| Exact exchange evaluation using occ-RI-FMM    |  Le H-.A., Shiozaki T., in preparation                                |
+-----------------------------------------------+-----------------------------------------------------------------------+
