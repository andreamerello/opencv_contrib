Processing images causing optical illusions {#tutorial_bioinspired_retina_illusions}
=============================================================

Goal
----

I will show here how the bioinspired module can reproduce a well-known optical illusion that
our eyes perceive in certain light condition: The Adelson checkerboard.

The Adelson checkerboard
------------------------

Looking at the checkerboard image below, human eyes perceive the "B" square lighter than the
"A" square.
While a real checkerboard would originally have a lighter "B" square, actually in this image the
shadow of the green cylinder casting over the "B" square ends up in making the "A" and "B"
squares actually having the same luminance.

![Adelson checkerboard](images/checkershadow_illusion4med.jpg)

Our visual system does "compensate" for the shadow, making us perceive the "B" square lighter,
as the shadow wouldn't be here.

You may find the original Adelson's explanation [here](http://web.mit.edu/persci/people/adelson/checkershadow_description.html).

Proof: You can convince yourself by using a image manipulation program, cutting out a portion
of the two squares, and looking at them without any background. You can also measure the RGB
values of the two squares with the picker tool.

Reproducing the illusion
------------------------

The bioinspired module does mimic the parvocellular retina process, and it does reproduce
our local contrast adaption.

We can expect the parvo channel output to contains luminance values similar to those we
perceive with our eyes. In this case we expect the "B" square pixels to be really lighter
than the "A" square pixels.

In order to make things working, there is one important thing to the care of: the checkerboard
image, as you may see on your laptop, projects on your retina with dimensions that causes
the local contrast adaption to take into account both the two squares at the same time.
We need opencv to do the same.

This means we have to ensure that the opencv's notion of "local" does match with our image's
dimensions, otherwise the local contrast adaption wouldn't work as expected.

For this reason we have to adjust the hcellsSpatialConstant parameter (that technically
specifies the low spatial cut frequency, or slow luminance changes sensitivity) depending by
the image resolution.


example_bioinspired_retinaDemo -image checkershadow_illusion4med.jpg

.. And in fact that is!

While looking at the resulting image may not tell us so much (indeed we are applying the
parvocellular process two time, one in our PC, one in our eyes), measures done with the
image manipulation program and the picker tool, show that the "B" square does really have
higher luminance than the "A" square.
