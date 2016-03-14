Container configuration
=======================

Before using *SyliusReviewBundle*, you should configure your review resource with several keys. They'll be automatically processed
to proper configuration and database mapping. Whole configuration should be written under ``sylius_review`` tag in your ``app/config/config.yml`` file.
Under ``sylius_review``, ``classes`` sub-key is required and defines tree of specific reviews configuration. Next tag should be named according
to your "reviewable" entity name (see exemplary configuration at the end of this page).

Configuration details
---------------------

Keys that should be configured:

- ``subject`` (required): entity representing reviewable resource (it must implement ``Sylius\Component\Review\Model\ReviewableInterface``)
- ``review``: tree that describes your review class; it should contain following sub-keys:
    - ``model`` (required): your review model (it must implement ``Sylius\Component\Review\Model\ReviewInterface``)
    - ``controller``: custom controller for your review class, as default it is ``Sylius\Bundle\ResourceBundle\Controller\ResourceController``
    - ``form``: tree that describes review forms, it contains 2 required options:
        - ``default``: form used to add review by reviewers on shop page; the default value is ``Sylius\Bundle\ReviewBundle\Form\Type\ReviewType``
        - ``admin``: form used to add review in administrator panel
    - ``reviewer``: tree that describes reviewer entity, (for example shop customer) that is allowed to write reviewable's reviews:
        - ``model`` (required): your reviewer model; it must implement ``Sylius\Component\Review\Model\ReviewerInterface``

Exemplary configuration
-----------------------

.. code-block:: yaml

    sylius_review:
        classes:
            product:
                subject: Sylius\Component\Core\Model\Product
                review:
                    model: Sylius\Component\Product\Model\ProductReview
                    form:
                        default: Sylius\Bundle\ProductBundle\Form\Type\ProductReviewType
                        admin: Sylius\Bundle\CoreBundle\Form\Type\ProductReviewAdminType
                reviewer:
                    model: Sylius\Component\Core\Model\Customer
