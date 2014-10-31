Extending through events
========================

The resource bundle allows you to extend the controller through events in your bundle.

You need to declare an EventListener in your bundle like described below :

.. code-block:: xml

    # services.xml

    <?xml version="1.0"?>
    <container xmlns="http://symfony.com/schema/dic/services" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd"
        <parameters>
            <parameter key="app.listener.my_listener.class">My\Bundle\CoreBundle\EventListener\UserListener</parameter>
        </parameters>
        <services>
            <service id="app.listener.my_listener" class="%app.listener.my_listener.class%">
                <tag name="kernel.event_listener" event="app.user.pre_create" method="onUserPreCreate"/>
            </service>
        </services>
    </container>

The resource bundle allow you to extend the controller through events in your own bundle.

You need to declare an EventListener in your bundle like describe bellow :

.. code-block:: yaml

    # services.yml

    services:
        app.listener.my_listener:
            class: My\Bundle\CoreBundle\EventListener\UserListener
            tags:
                - { name: kernel.event_listener, event: app.user.pre_create, method: onUserPreCreate }
                - { name: kernel.event_listener, event: app.user.pre_create, method: onUserCreate }
                - { name: kernel.event_listener, event: app.user.pre_create, method: onUserPostCreate }

.. code-block:: php

    # src/My/Bundle/CoreBundle/EventListener/UserListener.php

    <?php
    namespace My\Bundle\CoreBundle\EventListener;

    use Sylius\Bundle\ResourceBundle\Event\ResourceEvent;

    class UserListener
    {
        public function onUserPreCreate(ResourceEvent $event)
        {
            // You can do here whatever you want on your resource by accessing it with
            // $event->getSubject();
            $resource = $event->getSubject();

            // You can also here stop the propagation of the event to avoid resource being created in database
            // explain message and flashbag etc..
            if ($resource->getName() == 'forbidden') {
                $event->stop('app.error.message', array('%username%' => $resource->getName()));
            }
        }

        public function onUserCreate(ResourceEvent $event)
        {
            // You can do here whatever you want on your resource by accessing it with
            // $event->getSubject();
        }

        public function onUserPostCreate(ResourceEvent $event)
        {
            // You can do here whatever you want on your resource by accessing it with
            // $event->getSubject();
        }
    }

As you can see on the onUserPreCreate() method, you can stop the execution before saving changes to database.
The message passed to the method is put to the flashbag corresponding on the type parameter (the third). Default is "error".

The example above illustrates with the create process but you can do the same during update or delete process.