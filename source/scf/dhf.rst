.. _dhf:

*************
Dirac--Hartree--Fock
*************

Description
===========

The Dirac--Hartree--Fock method performs a self-consistent field orbital optimization and energy calculation
with a four-component relativistic framework.  The Dirac--Coulomb, Dirac--Coulomb--Gaunt, or full Dirac--Coulomb--Breit 
Hamiltonian can be used.  In the BAGEL implementation, density fitting is used for the two-electron integrals, and 
2-spinor basis functions are generated using restricted kinetic balance (RKB).  
External magnetic fields can be applied, in which case the spinor basis functions are generated using restricted magnetic balance (RMB) instead.  

Dirac--Hartree--Fock (DHF) cannot be run with an odd number of electrons in the absence of an external magnetic field, due 
to the presence of multiple degenerate solutions.  For open-shell molecules, it is recommended to run relativistic 
complete active space self-consistent field (ZCASSCF) instead, possibly with a minimal active space.  
DHF can be used to generate guess orbitals by increasing the molecular charge to remove unpaired electrons.  

Command: ``dhf``

Prerequisite
=============

.. topic:: Molecular geometry and AO integrals

   | Generated by "molecule" block in the input file.  

.. topic:: Guess molecular orbitals

   | Description: Usually, these are generated using the reference orbitals from an earlier calculation.  
   |     Non-relativistic orbitals from HF, UHF, ROHF, and CASSCF can be used, as well as relativistic orbitals 
   |     from another DHF or relativistic CASSCF calculation.  If no starting orbitals are provided, the 1-electron 
   |     Hamiltonian will be diagonalized to obtain a starting guess.  
   | Recommendation: Precede DHF with a non-relativistic HF calculation 

Keywords
========

.. topic:: ``gaunt``

   | Description:  Used to specify the form of the 2-electron Hamiltonian used.  The default is to use the Dirac--Coulomb Hamiltonian;
   |     If "gaunt" is set to true, the Gaunt interaction will be added, which accounts for direct spin--spin and spin-other-orbit 
   |     coupling between electrons.  Note that if "gaunt" is set to true, "breit" is also set to true unless otherwise specified by the user.  
   | Default: false
   | Datatype: bool
   | Recommendation:  The default is often fine, unless very strong relativistic effects are expected.  

.. topic:: ``breit``

   | Description:  Used to determine whether the full Breit interaction (including the gauge term) is included in the two-electron Hamiltonian.  
   | Default: value copied from "gaunt"
   | Datatype: bool
   | Recommendation: Use default, unless you wish to include the Gaunt interaction without the additional computational costs of the 
   |      full Breit interaction.

.. topic:: ``robust``

   | Description:  Determines whether or not to use the "robust" density fitting algorithm.  
   | Default: false
   | Datatype: bool
   | Recommendation: use default.

.. topic:: ``maxiter (or maxiter_scf)``

   | Description:  Maximum number of iterations, after which the program will terminate if convergence is not reached.  
   | Default: 100
   | Datatype: int
   | Recommendation: use default.

.. topic:: ``diis_start``

   | Description:  After the specified iteration, we will begin using Pulay's Direct Inversion in the Iterative Subspace (DIIS) algorithm for the 
   |      to update the orbitals.  
   | Default: 1
   | Datatype: int
   | Recommendation: use default.

.. topic:: ``thresh (or thresh_scf)``

   | Description:  Convergence threshold for the root-mean-squared of the error vector.  
   | Default: 1.0e-8
   | Datatype: double
   | Recommendation: The default value is good for production runs; often a looser threshold may be used if generating 
   |     guess orbitals for ZCASSCF.  

.. topic:: ``thresh_overlap``

   | Description:  Overlap threshold used to identify linear dependancy in the atomic basis set.  Increasing this value will 
   |      more aggressively remove linearly dependent basis vectors.  
   | Default: 1.0e-8
   | Datatype: double
   | Recommendation: use default.

.. topic:: ``charge``

   | Description:  Molecular charge.  
   | Default: 0
   | Datatype: int

.. topic:: ``multipole``

   | Description:  Order of multipoles to be used.  At this time, only dipoles are implemented for DHF, but this option is included 
   |      for future extensions and consistency with non-relativistic HF.  
   | Default: 1
   | Datatype: int
   | Recommendation: use default.  

.. topic:: ``pop``

   | Description:  If set to true, population analysis of the molecular orbitals will be printed to a file names dhf.log.  
   | Default: false
   | Datatype: bool
   | Recommendation:  Not needed for SCF calculations, but this feature can be helpful in finding guess active orbitals for ZCASSCF.  

Example
=======
This should be an example that is chemically relevant. There should be text explaining what the example is and why it's interesting.

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
		"thresh" : 1.0e-10
	},

	{
		"title" : "dhf",
		"gaunt" : true,
		"breit" : true
	}

	]}

Some information about the output should also be included. This will not be entire output but enough for the reader to know their calculation worked.

References
==========

+-----------------------------------------------+-----------------------------------------------------------------------+
|          Description of Reference             |                          Reference                                    | 
+===============================================+=======================================================================+
| General text on relativistic electronic       | Marcus Reiher and Alexander Wolf, Relativistic Quantum Chemistry,     |
| structure, including Dirac--Hartree--Fock.    | Wiley-VCH, Weinheim, 2009.                                            |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Original implementation of density fitted     | Matthew S. Kelley and Toru Shiozaki J. Chem. Phys. 2013, 138, 204113. |
| Dirac--Hartree--Fock with RMB spinor basis.   |                                                                       |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Extension to permit external magnetic fields, | Ryan D. Reynolds and Toru Shiozaki Phys. Chem. Chem. Phys. 2015, 17,  |
| including GIAO-RMB atomic basis.              | 14280-14283.                                                          |
+-----------------------------------------------+-----------------------------------------------------------------------+
