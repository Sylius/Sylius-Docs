The Review
==========

Retrieving reviews
------------------

Retrieving a review from the database should always happen via repository, which always implements ``Sylius\Bundle\ResourceBundle\Model\RepositoryInterface``.
If you are using Doctrine, you're already familiar with this concept, as it extends the native Doctrine ``ObjectRepository`` interface.

Your review repository is always accessible via the ``sylius.repository.{name}_review`` service, where ``{name}`` is the name of reviewable resource,
used as tree key under ``sylius_review`` tag (check out `Container configuration </en/latest/bundles/SyliusReviewBundle/container-configuration.html>`_ chapter).

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.product_review');
    }

Retrieving reviews is as simple as calling proper methods on the repository.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.product_review');

        $review = $repository->find(4); // Get review with id 4, returns null if not found.
        $review = $repository->findOneBy(array('title' => 'Really cool')); // Get one review by defined criteria.

        $review = $repository->findAll(); // Load all the reviews
        $review = $repository->findBy(array('rating' => 5)); // Find reviews matching some custom criteria.
    }

Creating new review object
--------------------------

To create new review instance, you can simply call ``createNew()`` method on the repository.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.product_review');
        $review = $repository->createNew();
    }

.. note::

    Creating a review via this factory method makes the code more testable, and allows you to change the review class easily.

Saving & removing review
-------------------------

To save or remove a review, you can use any ``ObjectManager`` which manages Review. You can always access it via alias ``sylius.manager.{name}_review``.
But it's also perfectly fine if you use ``doctrine.orm.entity_manager`` or other appropriate manager service.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.product_review');
        $manager = $this->container->get('sylius.manager.product_review'); // Alias for appropriate doctrine manager service.

        $review = $repository->createNew();

        $review
            ->setTitle('Foo')
            ->setComment('Nice product')
        ;

        $manager->persist($review);
        $manager->flush(); // Save changes in database.
    }

To remove a review, you also use the manager.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.product_review');
        $manager = $this->container->get('sylius.manager.product_review');

        $review = $repository->find(1);

        $manager->remove($review);
        $manager->flush(); // Save changes in database.
    }

Review state machine
--------------------

States
######

*SyliusReviewBundle* is provided with default state machine configuration for review entity. It's placed in ``state-machine.yml.dist`` file, and should have
``class`` key configured to work properly.
As default, Review object has 3 possible states:

- NEW - newly created review, somebody (probably store manager/administrator) should take care of it
- ACCEPTED - review that was accepted and can be safely displayed on its subject page
- REJECTED - if review is somehow inappropriate, it can be rejected and never be shown to anybody

.. attention::

    You should notice, that in default configuration ``rejected`` review cannot be ``accepted`` anymore! You can of course change it, if such a behaviour
    is inadequate for your application.

Callbacks
#########

*StateMachineBundle* allows you to define callbacks, that will be triggered after specific transitions. In default configuration of *SyliusReviewBundle* state machine,
after accepting review its subject's average rating is recalculated.
It's achieved with running ``updateFromReview`` method on `ReviewableAverageRatingUpdater </en/latest/bundles/SyliusReviewBundle/updaters.html#reviewableaverageratingupdater>`_.
