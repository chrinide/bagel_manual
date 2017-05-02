.. index:: basis, df_basis, ecp, geometry, dkh

.. _molecule:

**************************************
Molecule (system specification)
**************************************

Description
===========

Molecule, starting with ``"title" : "molecule"``, is the input block which specifies the basic features of the calculation.
It contains information such as basis sets and geometry for the input system. The user has to specify the contents of this block
in order to run the calculation.

Required keywords
=================

.. topic:: ``geometry``

   | **Description**: specify atoms and their Cartesian coordinates  
   | **Datatype**: vector
   | **Values**:
   |    Vector of atoms provided in the following format ``{ "atom" : "atom symbol",  "xyz" : [x, y, z] }``
        Please see the end of the file for some examples.

.. topic:: ``basis``

   | **Description**: define default basis set used for the system
   | **Datatype**: string
   | **Values**:
   |    Please refer to `Basis sets`_ and `Effective core potential (ECP) basis sets`_ for possible arguments.
        `User defined basis sets`_ are also possible.

.. topic:: ``df_basis``

   | **Description**: basis sets used for density fitting
   | **Datatype**: string
   | **Values**:
   |     Please refer to `Density fitting basis sets`_ for possible arguments

Note that the use of mixed basis sets and/or density fitting basis sets is possible by specifying a different 
basis set other than the default for each atom (see example for `Basis sets`_ below).

Optional keywords
=================

.. topic:: ``angstrom``

   | **Description**: Specify units for atomic coordinates (Angstrom or Bohr)
   | **Default**: false (Angstrom)
   | **Datatype**: bool
   |    ``TRUE``: use Angstrom
   |    ``FALSE``: use Bohr

.. topic:: ``finite_nucleus``

   | **Description**: Represent nucleus as a Gaussian charge distribution with default exponents 
   | **Default**: false 
   | **Datatype**: boolean 
   | **Recommendation**: Improves description of relativistic effects. 

.. topic:: ``molden_file``

   | **Description**: filename of input molden file, which is required if ``"basis" : "molden"`` is specified.
   | **Datatype**: string
   | **Recommendation**: False. However, this can be useful for doing investigative calculations before using active space methods.
 
.. topic:: ``cfmm``

   | **Description**: Option to do RHF-FMM, in which case density fitting is not used, for more details,
                      refer to :ref:`hf` section.
   | **Default**: false 
   | **Datatype**: boolean 
   | **Recommendation**: Use for calculations on very large systems. This is method is particularly effective for long, chain-like molecules. 

.. topic:: ``schwarz_thresh``

   | **Description**: Schwarz screening integral threshold (only used in RHF-FMM by specifying ``"cfmm" : "true"``).
                      For more details, refer to :ref:`hf` section. 
   | **Default**: :math:`1.0\times 10^{-12}`
   | **Datatype**: double 
   | **Recommendation**: Default, looser thresholds reduce accuracy but potentially increase speed. 

.. topic:: ``dkh``

   | **Description**: Option to do second order Douglas-Kroll-Hess (DKH2).
   | **Default**: false 
   | **Datatype**: boolean 
   | **Recommendation**: False, unless you are interested in relativistic effects. DKH2 is a cheaper than using full four component methods, but slightly less accurate. The four-compoment methods should be used for the calculation of electron paramagnetic resonance tensors.    

.. topic:: ``basis_type``

   | **Description**: Can request generation of gauge independent atomic orbitals (GIAO), London orbitals from requested basis.
   | **Default**: Standard if no magnetic field, GIAO if there is a magnetic field.
   | **Datatype**: String 
   | **Recommendation**: Default. Asking for standard orbitals in a calculation with a finite magentic field is equivalent to asking for a common gauge origin, which is less expensive, bit significantly less accurate.
 
.. topic:: ``magnetic_field``

   | **Description**: a vector of external magnetic field. When the magnetic field is non-zero,
                      Gauge-invariant atomic orbitals (GIAO) is used by default.
   | **Default**: ``{{0.0, 0.0, 0.0}}``
   | **Recommendation**: Only use it if you need to; running with a magnetic field of zero, whilst physically equivalent to switching the magnetic field off, may be computationally more expensive. The gauge origin of the vector potential corresponding to the magentic field is at the origin of the co-ordinate system, hence the geometry should be specified such that the paramagnetic centre of the system is located at the origin, particularly if are making use of the :ref:`aniso` tools. At present finite magnetic field is only well tested for :ref:`dhf` and :ref:`hf`, but can potentially used with :ref:`zcasscf`. 

.. topic:: ``tesla``

   | **Description**: unit of the external magnetic field
   | **Default**: false (use atomic unit)

.. topic:: ``basis_type``

   | **Description**: type of basis set used: standard Gaussian or Gauge-invariant atomic orbitals (GIAO).
                      Note that when the magnetic field is non-zero, GIAO is used by default.
   | **Default**: gaussian
   | **Values**: "london" or "giao" (for GIAO), and "gaussian"

==========
Basis sets 
==========

The following basis sets are available in BAGEL library. The basis set name can be used with the ``basis`` keyword.

* sto-3g
* 3-21g  
* 6-31g
* svp
* tzvpp
* qzvpp
* cc-pvdz  
* cc-pvtz  
* cc-pvqz
* cc-pv5z  
* cc-pv6z  
* aug-cc-pvdz
* aug-cc-pvtz
* aug-cc-pvqz
* aug-cc-pv5z
* aug-cc-pv6z
* ano-rcc

==========================
Density fitting basis sets
==========================

The following density fitting basis sets are available in BAGEL library. The basis set name can be used with the ``df_basis`` keyword.

* svp-jkfit
* tzvpp-jkfit
* qzvpp-jkfit
* cc-pvdz-jkfit
* cc-pvtz-jkfit
* cc-pvqz-jkfit
* cc-pv5z-jkfit

========
Examples
========

.. code-block:: javascript 

   { "bagel" : [

   {
     "title" : "molecule",
     "basis" : "svp",
     "df_basis" : "svp-jkfit",
     "angstrom" : false,
     "geometry" : [
         {"atom" : "H", "xyz" : [ -0.22767998367, -0.82511994081,  -2.66609980874]; },
         {"atom" : "O", "xyz" : [  0.18572998668, -0.14718998944,  -3.25788976629]; },
         {"atom" : "H", "xyz" : [  0.03000999785,  0.71438994875,  -2.79590979943]; }
     ]
   },

   {
     "title" : "hf",
     "thresh" : 1.0e-10
   }

   ]}

Example with mixed basis sets and density fitting basis sets:

.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "symmetry" : "C1",
     "basis" : "svp",
     "df_basis" : "svp-jkfit",
     "angstrom" : "false",
     "geometry" : [
       { "atom" : "F",  "xyz" : [ -0.000000,     -0.000000,      2.720616]},
       { "atom" : "H",  "xyz" : [ -0.000000,     -0.000000,      0.305956],
                        "basis" : "cc-pvqz", "df_basis" : "cc-pvqz-jkfit" }
     ]
   },
   
   {
     "title" : "hf",
     "thresh" : 1.0e-8
   }
   
   ]}

Example with running a calculation from a molden file using the keyword ``"basis" : "molden"``
and providing a value for ``"molden_file"``:

.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "symmetry" : "C1",
     "basis" : "molden",
     "df_basis" : "svp-jkfit",
     "cartesian" : true,
     "molden_file" : "hf_write_mol_cart.molden"
   }
   
   ]}

(refer to :ref:`molden` in :ref:`misc` for more details)

Example with external magnetic field using Gauge-invariant atomic orbitals (GIAO):

.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "symmetry" : "C1",
     "basis" : "svp",
     "df_basis" : "svp-jkfit",
     "angstrom" : "false",
     "basis_type" : "giao",
     "tesla" : "false",
     "magnetic_field" : [  0.2000,   0.3000,  -0.1500   ],
     "geometry" : [
       { "atom" : "F",  "xyz" : [ -1.200000,      2.500000,      2.720616]},
       { "atom" : "H",  "xyz" : [ -1.200000,      2.500000,      0.305956]}
     ]
   },
   
   {
     "title" : "hf",
     "thresh" : 1.0e-10
   }

   ]}

====================
Auxiliary basis sets
====================

The following auxiliary basis sets are available in BAGEL library. The basis set name can be used with the ``aux_basis`` keyword
in the method block (refer to :ref:`mp2` for more details).

* cc-pv5z-ri
* cc-pvdz-ri
* cc-pvqz-ri
* cc-pvtz-ri

Example
-------

An example using cc-pvdz-ri in MP2 calculation

.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "basis" : "cc-pvdz",
     "df_basis" : "cc-pvdz-jkfit",
     "angstrom" : "true",
     "geometry" : [
       { "atom" : "C", "xyz" : [ -1.20433891360,  0.54285096106, -0.04748199659] },
       { "atom" : "C", "xyz" : [ -1.20543291352, -0.83826393986,  0.12432899108] },
       { "atom" : "C", "xyz" : [ -0.00000600000, -1.52953889027,  0.20833398505] },
       { "atom" : "C", "xyz" : [  1.20544091352, -0.83825393987,  0.12432799108] },
       { "atom" : "C", "xyz" : [  1.20433091360,  0.54284396106, -0.04748099659] },
       { "atom" : "C", "xyz" : [  0.00000400000,  1.23314191154, -0.13372399041] },
       { "atom" : "H", "xyz" : [ -2.13410484690,  1.07591192282, -0.12500499103] },
       { "atom" : "H", "xyz" : [ -2.13651384673, -1.37179190159,  0.18742198655] },
       { "atom" : "H", "xyz" : [  0.00000000000, -2.59646181374,  0.33932597566] },
       { "atom" : "H", "xyz" : [  2.13651384673, -1.37179290159,  0.18742198655] },
       { "atom" : "H", "xyz" : [  2.13410684690,  1.07591292282, -0.12500599103] },
       { "atom" : "H", "xyz" : [ -0.00000000000,  2.29608983528, -0.28688797942] }
     ]
   },
   
   {
     "title" : "mp2",
     "aux_basis" : "cc-pvdz-ri",
     "frozen" : true
   }
   
   ]}

=========================================
Effective core potential (ECP) basis sets 
=========================================
The following auxiliary basis sets are available in BAGEL library. The basis set name can be used with the ``basis`` keyword.

* ecp10mdf
* ecp28mdf
* ecp46mdf
* ecp60mdf
* ecp78mdf
* def2-SVP-ecp
* def2-SVP-2c-ecp
* lanl2dz-ecp

Note that user-defined ECP basis sets need to contain the keyword "ecp" in the names. 
Refer to `User defined basis sets`_ for more details.

Example
-------

Example for CuH2 using cc-pvtz basis set for H and lanl2dz-ecp for the heavy atom Cu

.. code-block:: javascript 

   { "bagel" : [
   
   {
     "title" : "molecule",
     "symmetry" : "C1",
     "basis" : "lanl2dz-ecp",
     "df_basis" : "svp-jkfit",
     "angstrom" : "true",
     "geometry" : [
       { "atom" : "Cu",  "xyz" : [  0.000000,      0.000000,      0.000000]},
       { "atom" :  "H",  "xyz" : [  0.000000,      0.000000,     -1.560000],
                        "basis" : "cc-pvtz"},
       { "atom" :  "H",  "xyz" : [  0.000000,      0.000000,      1.560000],
                        "basis" : "cc-pvtz"}
     ]
   },
   
   {
     "charge" : "-1",
     "title" : "hf",
     "thresh" : 1.0e-8
   }
   
   ]}

========================
User defined basis sets
========================

The basis set file is in the following format

.. code-block:: javascript 

 {
  "H" : [
    {
      "angular" : "s",
      "prim" : [5.4471780, 0.8245470],
      "cont" : [[0.1562850, 0.9046910]]
    }, {
      "angular" : "s",
      "prim" : [0.1831920],
      "cont" : [[1.0000000]]
    }
  ],
  "He" : [
    {
      "angular" : "s",
      "prim" : [13.6267000, 1.9993500],
      "cont" : [[0.1752300, 0.8934830]]
    }, {
      "angular" : "s",
      "prim" : [0.3829930],
      "cont" : [[1.0000000]]
    }
  ]
 }

The file is essentially one large array, the elements of which are further arrays, each corresponding to the basis set for a given element.
The basis set for associated with each element is then made up of futher arrays, each of which  contains information specifying the properties
of a single basis function.

  * ``angular`` defines the kind of orbital (s,p,d,f...) . 
  * ``prim`` is a array containing the exponents of the primitive orbitals from which the basis funciton is composed.
  * ``cont`` is an array containing the coefficients associated with each of these primitive orbitals.
 
The user can specify their own basis set using the above format, or use one of the predefined basis sets listed in `Basis sets`_. Note that not
all of the the basis sets are defined for all atoms;  an error of form "No such node(X)", where X is the element, typically means that the relevant element was not found in the basis set file. Refer to the EMSL Basis set exchange library for more basis sets (https://bse.pnl.gov/bse/portal).
 
To use a user specified basis the explicit path to the basis set file must be specified in the basis set block.

Example
-------

.. code-block:: javascript 

   { "bagel" : [

   {
     "title" : "molecule",
     "basis" : "/path/to/my/basis",
     "df_basis" : "/path/to/my/basis",
     "angstrom" : false,
     "geometry" : [
         {"atom" : "H", "xyz" : [ -0.22767998367, -0.82511994081,  -2.66609980874]; },
         {"atom" : "O", "xyz" : [  0.18572998668, -0.14718998944,  -3.25788976629]; },
         {"atom" : "H", "xyz" : [  0.03000999785,  0.71438994875,  -2.79590979943]; }
     ]
   },

   {
     "title" : "hf",
     "thresh" : 1.0e-10
   }

   ]}

