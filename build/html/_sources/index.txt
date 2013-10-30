
#############################
slimta: Mail Transfer Library
#############################

.. toctree::
   :hidden:

   api
   manual
   license

.. _MTA: http://en.wikipedia.org/wiki/Message_transfer_agent

The *slimta* suite was created to be provide MTA that takes advantage of new
technologies and methodologies to scale horizontally in a virtualized
environment. Along with that, *slimta* is designed to work either as a
traditional application or as a software platform that is developed on top of.

.. |buildstatus| image:: http://ci.slimta.org/job/python-slimta/badge/icon

#. :doc:`license`
#. :doc:`manual`
#. `Continuous Integration <http://ci.slimta.org/>`_ |buildstatus|
#. IRC Channel: `#slimta <http://webchat.freenode.net/?channels=%23slimta>`_ on Freenode.

.. rst-class:: html-toggle

Python Library
==============

The *python-slimta* project is a Python library offering the building blocks
necessary to create a full-featured MTA_. Most MTAs must be configured, but an
MTA built with *python-slimta* is coded. An MTA built with *python-slimta* can
incorporate any protocol or policy -- custom or built-in. An MTA built with
*python-slimta* can integrate with other Python libraries and take advantage of
Python's great community.

#. `README <https://github.com/slimta/python-slimta/blob/master/README.md>`_
#. `Download <https://github.com/slimta/python-slimta/tags>`_ [`Repository <https://github.com/slimta/python-slimta>`_]
#. `Bug Reports <https://github.com/slimta/python-slimta/issues>`_
#. `Installation <https://github.com/slimta/python-slimta/blob/master/README.md#getting-started>`_
#. :doc:`api`
#. :doc:`api/extra`

.. literalinclude:: python-slimta-sample.txt
   :language: python

.. rst-class:: html-toggle

Traditional Application
=======================

The *slimta* project is a traditional application built on top of the
*python-slimta* library. It allows a more "out-of-the-box" MTA_ that offers all
the useful, built-in features needed for a normal mail system setup.

#. `README <https://github.com/slimta/slimta/blob/master/README.md>`_
#. `Download <https://github.com/slimta/slimta/tags>`_ [`Repository <https://github.com/slimta/slimta>`_]
#. `Bug Reports <https://github.com/slimta/slimta/issues>`_
#. :doc:`Configuration and Setup Guide <manual/slimta>`

.. literalinclude:: slimta-config-sample.txt
   :language: none

