Color Thief
===========

A Python module for grabbing the color palette from an image.

Installation
------------

::

    $ pip install git+https://github.com/niyaziulke/color-thief-py



API
---

.. code:: python

    class ColorThief(object):
        def __init__(self, file):
            """Create one color thief for one image.

            :param file: A filename (string) or a file object. The file object
                         must implement `read()`, `seek()`, and `tell()` methods,
                         and be opened in binary mode.
            """
            pass

        def get_color(self, quality=10):
            """Get the dominant color.

            :param quality: quality settings, 1 is the highest quality, the bigger
                            the number, the faster a color will be returned but
                            the greater the likelihood that it will not be the
                            visually most dominant color
            :return tuple: (r, g, b)
            """
            pass

        def get_palette(self, color_count=10, quality=10):
            """Build a color palette.  We are using the median cut algorithm to
            cluster similar colors.

            :param color_count: the size of the palette, max number of colors
            :param quality: quality settings, 1 is the highest quality, the bigger
                            the number, the faster the palette generation, but the
                            greater the likelihood that colors will be missed.
            :return list: a list of tuple in the form (r, g, b)
            """
            pass
    class ColorThiefArray(object):
        """Color thief from numpy array."""
        def __init__(self, array):
            """Create one color thief for one image.

            :param array: A numpy array of an image.
            """
            self.image = Image.fromarray(file)

        def get_color(self, quality=10):
            """Get the dominant color.

            :param quality: quality settings, 1 is the highest quality, the bigger
                            the number, the faster a color will be returned but
                            the greater the likelihood that it will not be the
                            visually most dominant color
            :return tuple: (r, g, b)
            """
            palette = self.get_palette(5, quality)
            return palette[0]

        def get_palette(self, color_count=10, quality=10):
            """Build a color palette.  We are using the median cut algorithm to
            cluster similar colors.

            :param color_count: the size of the palette, max number of colors
            :param quality: quality settings, 1 is the highest quality, the bigger
                            the number, the faster the palette generation, but the
                            greater the likelihood that colors will be missed.
            :return list: a list of tuple in the form (r, g, b)
            """
            image = self.image.convert('RGBA')
            width, height = image.size
            pixels = image.getdata()
            pixel_count = width * height
            valid_pixels = []
            for i in range(0, pixel_count, quality):
                r, g, b, a = pixels[i]
                # If pixel is mostly opaque and not white
                if a >= 125:
                    if not (r > 250 and g > 250 and b > 250):
                        valid_pixels.append((r, g, b))

            # Send array to quantize function which clusters values
            # using median cut algorithm
            cmap = MMCQ.quantize(valid_pixels, color_count)
            return cmap.palette
Thanks
------

Thanks to Lokesh Dhakar for his `original work
<https://github.com/lokesh/color-thief/>`_.

Forked from `fengsp's repo
<https://github.com/fengsp/color-thief-py/blob/master/colorthief.py/>`_.

