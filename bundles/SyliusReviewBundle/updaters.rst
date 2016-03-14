Updaters
========

*SyliusReviewBundle* provides some useful classes, that can be used to manage reviews easier and nicer.

ReviewableAverageRatingUpdater
------------------------------

This updater takes care about proper average rating value on reviewable entities. It's recalculated by using ``AverageRatingCalculatorInterface``
implementation injected to updater.
``ReviewableAverageRatingUpdater`` contains following methods:

+-----------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| Method                                        | Description                                                                                               |
+===============================================+===========================================================================================================+
| update(ReviewableInterface $reviewable)       | Update passed reviewable object's average rating                                                          |
+-----------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| updateFromReview(ReviewInterface $reviewable) | Update passed review object subject's average rating                                                      |
+-----------------------------------------------+-----------------------------------------------------------------------------------------------------------+

.. note::

    This class implements ``ReviewableAverageRatingUpdaterInterface``.

.. hint::

    This is just an exemplary implementation, you can use ``ReviewableAverageRatingUpdaterInterface`` and implement your own updater,
    which will be more suitable to your application needs.
