################################
HARDWARIO Documentation Skeleton
################################

.. thumbnail:: _static/index/logo.png
   :width: 100%
   :alt: HARDWARIO logo

Welcome to our documentation skeleton where you some basic info about how to write `HARDWARIO <https://www.hardwario.com>`_ documentation.

**************
Where to Begin
**************

You can edit files in the ``/source`` directory. To show this file you have to add it into some toctree that is shown below.

To add images, add them into the ``/source/_static`` directory and reference them by the ``.. thumbnail::`` directive shown on the top of this document.

To see all the extensions installed for this skeleton you can visit ``requirements.txt``.


To see more on how to make .rst documents you can visit the :doc:`example document </example/example_doc>`.

.. toctree::
   :caption: This will be shown in the menu
   :maxdepth: 2
   :hidden:

   example/example_doc


