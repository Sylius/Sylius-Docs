Event listeners
===============

*Symfony* events makes many things easier, when developing web application. Moreover, by using `SyliusResourceBundle </en/latest/bundles/SyliusResourceBundle/index.html>`_
we can use plenty of new and helpful resource events. *SyliusReviewBundle* is shipped with one default event listener, which is ``ReviewDeleteListener``.

ReviewDeleteListener
--------------------

This listener is used to control reviewable average ratings after review deletion.

+-----------------------------------------------+-----------------------------------------------------------------------------+
| Method                                        | Description                                                                 |
+===============================================+=============================================================================+
| recalculateSubjectRating(GenericEvent $event) | Recalculate review subject average rating, if deleted review was "accepted" |
+-----------------------------------------------+-----------------------------------------------------------------------------+

.. note::

    ``ReviewDeleteListener`` is dynamically tagged based on your ``sylius_review`` configuration, so you don't have to worry about
    proper events handling. Everything is done for you!
