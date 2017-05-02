.. _dimer_asd:

*********
Dimer ASD
*********


Description
===========
The active space decomposition algorithm for molecular dimers allows for efficient computation for the dimer's complete-active-space wave functions. The current algorithm only works for dimer molecules those fragments are not covalently bonded. Full-CI and restricted-active-space-CI can be used to obtain the fragment state wave functions. ASD calculation starts with dimer molecule construction, see (add ref Dimer) section for more information.


Dimer Construction
==================
.. toctree::
   :maxdepth: 1

   dimer.rst

Keywords
========

.. topic:: ``active orbitals``
   
   | **Description**: assign active orbitals


Example
=======
Give an example here


Sample input
------------

.. code-block:: javascript

   { "bagel" : [
   
   {
     "title" : "molecule",
     "symmetry" : "C1",
     "basis" : "sto-3g",
     "df_basis" : "svp",
     "angstrom" : false,
     "cartesian" : false,
     "geometry" : [
       {"atom" :"C", "xyz" : [    0.00000000000000,     0.00000000000000,     2.64112304663605] },
       {"atom" :"C", "xyz" : [    2.28770766388446,     0.00000000000000,     1.32067631141874] },
       {"atom" :"C", "xyz" : [    2.28770047235649,     0.00000000000000,    -1.32071294538560] },
       {"atom" :"C", "xyz" : [    0.00000000000000,     0.00000000000000,    -2.64114665444819] },
       {"atom" :"C", "xyz" : [   -2.28770047235649,     0.00000000000000,    -1.32071294538560] },
       {"atom" :"C", "xyz" : [   -2.28770766388446,     0.00000000000000,     1.32067631141874] },
       {"atom" :"H", "xyz" : [    4.07221260176630,     0.00000000000000,     2.35164689765998] },
       {"atom" :"H", "xyz" : [    4.07221517814719,     0.00000000000000,    -2.35163163881380] },
       {"atom" :"H", "xyz" : [    0.00000000000000,     0.00000000000000,    -4.70191324441092] },
       {"atom" :"H", "xyz" : [   -4.07221517814719,     0.00000000000000,    -2.35163163881380] },
       {"atom" :"H", "xyz" : [   -4.07221260176630,     0.00000000000000,     2.35164689765998] },
       {"atom" :"H", "xyz" : [    0.00000000000000,     0.00000000000000,     4.70197960246451] }
     ]
   },
   
   {
     "title" : "hf"
   },
   
   {
     "title" : "dimerize",
     "angstrom" : true,
     "translate" : [0.0, 4.0, 0.0],
     "dimer_active" : [17, 20, 21, 22, 23, 24],
     "hf" : {
       "thresh" : 1.0e-12
     },
     "localization" : {
       "max_iter" : 50,
       "thresh" : 1.0e-8
     }
   },
   
   {
     "title" : "asd",
     "method" : "cas",
     "store_matrix" : false,
     "space" : [
       { "charge" : 0, "spin" : 0, "nstate" : 3},
       { "charge" : 0, "spin" : 2, "nstate" : 3},
       { "charge" : 1, "spin" : 1, "nstate" : 2},
       { "charge" :-1, "spin" : 1, "nstate" : 2}
     ],
     "fci" : {
       "thresh" : 1.0e-6,
       "algorithm" : "kh",
       "nguess" : 400
     },
     "nstates" : 2
   }
   
   ]}

 
Reference
=========