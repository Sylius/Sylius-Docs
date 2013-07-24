The Address
===========

Address is very simple model you should use to store all address data in your application.

+-----------+--------------------------------+
| Attribute | Description                    |
+===========+================================+
| id        | Unique id of the address       |
+-----------+--------------------------------+
| firstname |                                |
+-----------+--------------------------------+
| lastname  |                                |
+-----------+--------------------------------+
| company   |                                |
+-----------+--------------------------------+
| country   | Reference to Country model     |
+-----------+--------------------------------+
| province  | Refernce to Province model     |
+-----------+--------------------------------+
| street    |                                |
+-----------+--------------------------------+
| city      |                                |
+-----------+--------------------------------+
| postcode  | Description for search engines |
+-----------+--------------------------------+
| createdAt | Date when address was created  |
+-----------+--------------------------------+
| updatedAt | Date of last address update    |
+-----------+--------------------------------+

Retrieving addresss
-------------------

Retrieving address from database should always happen via repository, which always implements ``Sylius\Bundle\ResourceBundle\Model\RepositoryInterface``.
If you are using Doctrine, you're already familiar with this concept, as it extends the native Doctrine ``ObjectRepository`` interface.

Your address repository is always accessible via ``sylius.repository.address`` service.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.address');
    }

Retrieving addresss is simple as calling proper methods on the repository.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.address');

        $address = $repository->find(4); // Get address with id 4, returns null if not found.
        $address = $repository->findOneBy(array('firstname' => 'John')); // Get one address by defined criteria.

        $addresss = $repository->findAll(); // Load all the addresss!
        $addresss = $repository->findBy(array('country' => $country)); // Find addresss matching some custom criteria.
    }

Address repository also supports paginating addresss. To create a `Pagerfanta instance <https://github.com/whiteoctober/Pagerfanta>`_ use the ``createPaginator`` method.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.address');

        $addresss = $repository->createPaginator();
        $addresss->setMaxPerPage(3);
        $addresss->setCurrentPage($request->query->get('page', 1));

       // Now you can returns addresss to template and iterate over it to get addresss from current page.
    }

Paginator also can be created for specific criteria and with desired sorting.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.address');

        $addresss = $repository->createPaginator(array('city' => 'Boston'), array('createdAt' => 'desc'));
        $addresss->setMaxPerPage(3);
        $addresss->setCurrentPage($request->query->get('page', 1));
    }

Creating new address object
---------------------------

To create new address instance, you can simply call ``createNew()`` method on the repository.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.address');
        $address = $repository->createNew();
    }

.. note::

    Creating address via this factory method makes the code more testable, and allows you to change address class easily.

Saving & removing address
-------------------------

To save or remove a address, you can use any ``ObjectManager`` which manages Address. You can always access it via alias ``sylius.manager.address``.
But it's also perfectly fine if you use ``doctrine.orm.entity_manager`` or other appropriate manager service.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.address');
        $manager = $this->container->get('sylius.manager.address'); // Alias for appropriate doctrine manager service.

        $address = $repository->createNew();

        $address
            ->setFirstname('Paweł')
            ->setLastname('Jędrzejewski')
            ->setCity('Lodz')
            ->setStreet('Foobar 35')
            ->setPostcode('94-532')
        ;

        $manager->persist($address);
        $manager->flush(); // Save changes in database.
    }

To remove a address, you also use the manager.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $repository = $this->container->get('sylius.repository.address');
        $manager = $this->container->get('sylius.manager.address');

        $address = $repository->find(1);

        $manager->remove($address);
        $manager->flush(); // Save changes in database.
    }

Country and Province
--------------------

Every address holds reference to a Country and optionally - to one of the country Provinces. You'll learn about them in next chapter.
