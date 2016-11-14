
Python Library Usage
====================

.. toctree::
   :hidden:

   terminology
   system
   envelopes
   edge
   queue
   relay
   policies
   integrating
   mda
   msa
   extensions

The :doc:`/api/index` provides great information on how to use each piece of
the *python-slimta* library, but it rarely helps decide when and how to put the
pieces together. These pages describe the components of a *python-slimta*
application, with examples, to help construct your own personalized MTA.

For example, once you've read about the :ref:`SMTP edge service <edge-smtp>`
and decided it's what you want, you can use the
:class:`~slimta.edge.smtp.SmtpEdge` reference page for details on exactly what
arguments it takes and what they mean.

* :doc:`system`
* :doc:`envelopes`

  * :ref:`Bounces <envelope-bounces>`

* :doc:`edge`

  * :ref:`SMTP <edge-smtp>`
  * :ref:`HTTP <edge-http>`

* :doc:`queue`
* :doc:`relay`

  * :ref:`SMTP Smart-Hosting <relay-smtp-smarthost>`
  * :ref:`SMTP MX <relay-smtp-mx>`
  * :ref:`LMTP <relay-lmtp>`
  * :ref:`HTTP <relay-http>`
  * :ref:`External Processes <relay-pipe>`

* :doc:`policies`

  * :ref:`Date Header <policy-add-date-header>`
  * :ref:`Message-Id Header <policy-add-message-id-header>`
  * :ref:`Received Header <policy-add-received-header>`
  * :ref:`Recipient Forwarding <policy-forwarding>`
  * :ref:`SpamAssassin <policy-spamassassin>`

* :doc:`integrating`

  * :doc:`mda`
  * :doc:`msa`

* :doc:`extensions`

  * :ref:`External Process Delivery <pipe-relay-extension>`
  * :ref:`Cloud Storage <cloud-storage>`
  * :ref:`Disk Storage <disk-storage>`
  * :ref:`Redis Storage <redis-storage>`
  * :ref:`Celery Queuing <celery-queue>`
  * :ref:`Sender Policy Framework (SPF) <enforce-spf>`

