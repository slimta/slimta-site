#############################
slimta: Mail Transfer Library
#############################

.. toctree::
   :hidden:

   license
   versions
   api/index
   manual/index
   blog/index

.. _MTA: https://en.wikipedia.org/wiki/Message_transfer_agent
.. _contributors: https://github.com/slimta/python-slimta/graphs/contributors

The *slimta* suite was created to provide an MTA (Mail Transfer Agent) taking advantage of new
technologies and methodologies to scale horizontally in a virtualized
environment. Along with that, *slimta* is designed to work either as a
traditional application or as a software library.

.. |buildstatus| image:: https://travis-ci.org/slimta/python-slimta.svg?branch=main

#. :doc:`license`
#. :doc:`manual/terminology`
#. `Continuous Integration <https://travis-ci.org/slimta/>`_ |buildstatus|
#. News

   - :doc:`blog/2020-10-30`
   - :doc:`blog/2016-11-14`
   - :doc:`blog/2016-11-14-2`

.. rst-class:: html-toggle

Python Library
==============

The *python-slimta* project is a Python library offering the building blocks
necessary to create a full-featured MTA_. Most MTAs must be configured, but an
MTA built with *python-slimta* is coded. An MTA built with *python-slimta* can
incorporate any protocol or policy -- custom or built-in. An MTA built with
*python-slimta* can integrate with other Python libraries and take advantage of
Python's great community.

#. `README <https://github.com/slimta/python-slimta/blob/main/README.md>`_
#. `Download <https://github.com/slimta/python-slimta/tags>`_ [`Repository <https://github.com/slimta/python-slimta>`_]
#. `Bug Reports <https://github.com/slimta/python-slimta/issues>`_
#. `Change Log <https://github.com/slimta/python-slimta/blob/main/CHANGELOG.md>`_
#. `Installation <https://github.com/slimta/python-slimta/blob/main/README.md#getting-started>`_
#. :doc:`api/index`
#. :doc:`manual/python-slimta`

.. literalinclude:: python-slimta-sample.txt
   :language: python

.. rst-class:: html-toggle

Traditional Application
=======================

The *slimta* project is a traditional application built on top of the
*python-slimta* library. It allows a more "out-of-the-box" MTA_ that offers all
the useful, built-in features needed for a normal mail system setup.

#. `README <https://github.com/slimta/slimta/blob/main/README.md>`_
#. `Download <https://github.com/slimta/slimta/tags>`_ [`Repository <https://github.com/slimta/slimta>`_]
#. `Bug Reports <https://github.com/slimta/slimta/issues>`_
#. :doc:`Configuration and Setup Guide <manual/slimta>`

.. literalinclude:: slimta-config-sample.txt
   :language: yaml

