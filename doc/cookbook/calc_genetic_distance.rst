****************************
Genetic distance calculation
****************************

Fast pairwise distance estimation
=================================

For a limited number of evolutionary models a fast implementation is
available.

.. jupyter-execute::
    :linenos:

    from cogent3 import available_distances

    available_distances()

Computing genetic distances using the ``Alignment`` object
==========================================================

Abbreviations listed from ``available_distances()`` can be used as values for the ``distance_matrix(calc=<abbreviation>)``.

.. jupyter-execute::
    :linenos:

    from cogent3 import load_aligned_seqs

    aln = load_aligned_seqs('data/primate_brca1.fasta', moltype="dna")
    dists = aln.distance_matrix(calc="tn93", show_progress=False)
    dists

Using the distance calculator directly
======================================

.. jupyter-execute::
    :linenos:

    from cogent3 import load_aligned_seqs, get_distance_calculator

    aln = load_aligned_seqs('data/primate_brca1.fasta')
    dist_calc = get_distance_calculator("tn93", alignment=aln)
    dist_calc

.. jupyter-execute::
    :linenos:

    dist_calc.run(show_progress=False)
    dists = dist_calc.get_pairwise_distances()
    dists

The distance calculation object can provide more information. For instance, the standard errors.

.. jupyter-execute::
    :linenos:

    dist_calc.stderr

Likelihood based pairwise distance estimation
=============================================

The standard ``cogent3`` likelihood function can also be used to estimate distances. Because these require numerical optimisation they can be significantly slower than the fast estimation approach above.

The following will use the F81 nucleotide substitution model and perform numerical optimisation.

.. jupyter-execute::
    :linenos:

    from cogent3 import load_aligned_seqs, get_model
    from cogent3.evolve import distance

    aln = load_aligned_seqs('data/primate_brca1.fasta', moltype="dna")
    d = distance.EstimateDistances(aln, submodel=get_model("F81"))
    d.run(show_progress=False)
    dists = d.get_pairwise_distances()
    dists

All ``cogent3`` substitution models can be used for distance calculation via this approach, with the caveat that identifiability issues mean this is not possible for some non-stationary model classes.