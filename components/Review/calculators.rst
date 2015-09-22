Calculators
===========

AverageRatingCalculator
-----------------------

*Review* component provides one default calculator class, which is as default used to calculate average rating for given ``ReviewableInterface`` object.
``AverageRatingCalculator`` contains following methods:

+---------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| Method                                      | Description                                                                                               |
+=============================================+===========================================================================================================+
| calculate(ReviewableInterface $reviewable)  | Calculates average rating for passed ``ReviewableInterface`` object, based on all accepted reviews rating |
+---------------------------------------------+-----------------------------------------------------------------------------------------------------------+

.. note::

    This model implements ``AverageRatingCalculatorInterface``

.. hint::

    This is just a default implementation of this calculator.
    Of course, you can override it in your application, to implement different, better for your system calculation logic.