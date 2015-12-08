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

The bioinspired module does mimic (also) the parvocellular retina process, and it does reproduce
our local contrast adaption.

NOTE: the checkerboard image, as you may see on your laptop, projects on your retina with
dimensions that causes the local contrast adaption to take into account both the two squares
at the same time.

We need opencv to do the same. This means we have to ensure that the opencv's notion of
"local" does match with our image's dimensions, otherwise the local contrast adaption
wouldn't work as expected.

For this reason we may have to adjust the hcellsSpatialConstant parameter (that technically
specifies the low spatial cut frequency, or slow luminance changes sensitivity) depending by
the image resolution. For the image in this tutorial, the default parameters should be fine.

In order to feed the image to the bioinspired module, we can use either your own code or
the example_bioinspired_retinaDemo example that comes with the bioinspired module.

Running example_bioinspired_retinaDemo -image checkershadow_illusion4med.jpg will cause
our image to be processed in both parvocellular and magnocellular channels (we are interested
just in the first one).

If you choose to use your own code, please note that the parvocellular (and magnocellular)
channel does require some iterations (frames to be processed) before actually get steady.
This is something that you don't need to worry about when you process videos, but you have
to take care of it in order to process a single frame. What you will actually need (and that
the example code does), is to repeatedly feed the retina with the same frame, as you would do
with a still video.

Here there is a small fragment of python code we used to process the image. It does 20
iterations. This is an arbitrary number that we found experimentally to be (more than)
enough

@code{.py}
#import OpenCV module
import cv2

#read the input image
inputImage = cv2.imread('checkershadow_illusion4med.jpg', 1)
#create our retina
retina = cv2.bioinspired.createRetina((inputImage.shape[1], inputImage.shape[0]))

#Uncomment the following line to load retina parameters from a xml file
#retina.setup('MyRetinaParameters.xml')

#feed the retina with several frames, in order to reach 'steady' state
for i in range(20):
    retina.run(inputImage)

#get our processed image :)
retinaOut_parvo = retina.getParvo()

#show the original image
cv2.imshow('image', inputImage)
#show the processed image
cv2.imshow('retina parvo out', retinaOut_parvo)

#wait for a key to be pressed and exit
cv2.waitKey(0)
cv2.destroyAllWindows()

#write the image on a file
cv2.imwrite('checkershadow_parvo.png', retinaOut_parvo)
@endcode

Despite which way you used to get your imaged processed, you should end up
with something like this:

![Parvo output for adelson checkerboard](images/checkershadow_parvo.png)

Analyzing the results
----------------------

We can expect the parvo channel output to contains luminance values similar to
those we perceive with our eyes. In this case we expect the "B" square pixels to be really lighter
than the "A" square pixels.

.. And in fact that is!

While looking at the resulting image may not tell us so much (indeed we are applying the
parvocellular process two time, one in our PC, one in our eyes), measures done with the
image manipulation program and the picker tool, show that the "B" square does really have
higher luminance than the "A" square.
