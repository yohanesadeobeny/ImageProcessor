ImageProcessor
===============

Imageprocessor is a lightweight library written in C# that allows you to manipulate images on-the-fly using .NET 4.0.

It's fast, extensible, easy to use, comes bundled with some great features and is fully open source.

Core plugins at present include:

 - Alpha (Sets opacity)
 - Brightness
 - Contrast
 - Crop
 - Filter (Image filters including sepia, greyscale, blackwhite, lomograph, polaroid, comic, gotham, hisatch, losatch)
 - Flip (Flip the image) 
 - Format (Sets the output format)
 - Quality (The quality to set the output for jpeg files)
 - Reset (Resets the image to its original loaded state)
 - Resize 
 - Rotate (Rotate the image through 360º)
 - Saturation
 - Vignette (Adds a vignette effect to images)
 - Watermark (Set a text watermark)

The library consists of two binaries: **ImageProcessor.dll** and **ImageProcessor.Web.dll**.

ImageProcessor.dll
==================

**ImageProcessor.dll** contains all the core functionality that allows for image manipulation via the `ImageFactory` class. This has a fluent API which allows you to easily chain methods to deliver the desired output. This library contains no web specific componenets so is suitable for both desktop and web applications.

e.g.

    // Read a file and resize it.
    byte[] photoBytes = File.ReadAllBytes(file);
    int quality = 70;
    ImageFormat format = ImageFormat.Jpeg;
	Size size = new Size(150, 0)
        
    using (var inStream = new MemoryStream(photoBytes))
    {
        using (var outStream = new MemoryStream())
        {
            using (ImageFactory imageFactory = new ImageFactory())
            {
                // Load, resize, set the format and quality and save an image.
                imageFactory.Load(inStream)
			    		    .Resize(size)
			    		    .Format(format)
			    		    .Quality(quality)
			    		    .Save(outStream);
            }

			// Do something with the stream.
        }
    }

ImageProcessor.Web.dll
======================

**ImageProcessor.Web.dll** contains an ASP.NET HttpModule which captures internal and external requests automagically processing them based on values captured through querystring parameters.

Using the HttpModule requires no code writing at all. Just install the package references the binaries relevant sections in the web.config will be automatically created.

Image requests suffixed with querystring parameters will then be processed and **cached to the server - up to 12,960,000 images** allowing for easy and efficient parsing of following requests.

The parsing engine for the HttpModule is incredibly flexible and will **allow you to add querystring parameters in any order.**

Installation
============

Installation is simple. A Nuget package for the full web version is available [here][1] and the standalone library [here][2]. 

  [1]: http://nuget.org/packages/ImageProcessor.Web/
  [2]: http://nuget.org/packages/ImageProcessor/

Alternatively you can download and build the project and reference the binaries. Then copy the example configuration values from the demo project into your `web.config` to enable the processor. 

Usage
=====

Heres a few examples of the syntax for processing images.

Alpha Transparency
==================
Imageprocessor can adjust the alpha transparency of png images. Simply pass the desired percentage value (without the '%') to the processor.

e.g.

    <img src="/images.yourimage.jpg?alpha=60" alt="60% alpha image"/>

Brightness
==================
Imageprocessor can adjust the brightness of images. Simply pass the desired value (-100 to 100) to the processor.

e.g.

    <img src="/images.yourimage.jpg?brightness=60" alt="60 brightness"/>
    
Contrast
==================
Imageprocessor can adjust the contrast of images. Simply pass the desired value (-100 to 100) to the processor.

e.g.

    <img src="/images.yourimage.jpg?contrast=60" alt="60 contrast"/>
    
Crop
====

Cropping an image is easy. Simply pass the top-left and bottom-right coordinates and the processor will work out the rest.

e.g.

    <img src="/images.yourimage.jpg?crop=5-5-200-200" alt="your cropped image"/>
    
Filters
=======

Everybody loves adding filter effects to photgraphs so we've baked some popular ones into Imageprocessor.

Current filters include:

  - blackwhite
  - comic (requires full trust)
  - lomograph
  - greyscale
  - polaroid
  - sepia
  - gotham
  - hisatch
  - losatch
  - invert

e.g.

    <img src="/images.yourimage.jpg?filter=polaroid" alt="classic polaroid look"/>

Flip
=======

Imageprocessor can flip your image either horizontally or vertically.

e.g.

    <img src="/images.yourimage.jpg?flip=horizontal" alt="horizontal"/>
    <img src="/images.yourimage.jpg?flip=vertical" alt="vertical"/>


Format
======

Imageprocessor will also allow you to change the format of image on-the-fly. This can be handy for reducing the size of requests.

Supported file format just now are:

  - jpg
  - bmp
  - png
  - gif (requires full trust)

e.g.

    <img src="/images.yourimage.jpg?format=gif" alt="your image as a gif"/>

Quality
======

Whilst Imageprocessor delivers an excellent quality/filesize ratio it also allows you to change the quality of jpegs on-the-fly.

e.g.

    <img src="/images.yourimage.jpg?quality=65" alt="your image at quality 30"/>
    
Resize
======

    <img src="/images.yourimage.jpg?width=200" alt="your resized image"/>

Will resize your image to 200px wide whilst keeping the correct aspect ratio.

Remote files can also be requested and cached by prefixing the src with a value set in the web.config 

e.g.

    <img src="remote.axd/http://url/images.yourimage.jpg?width=200" alt="your resized image"/>

Will resize your remote image to 200px wide whilst keeping the correct aspect ratio.

Changing both width and height requires the following syntax:

    <img src="/images.yourimage.jpg?width=200&height=200" alt="your resized image"/>


Rotate
======

Imageprocessor can rotate your images without clipping. You can also optionally fill the background color for image types without transparency.

e.g.

    <img src="/images.yourimage.jpg?rotate=26" alt="your image rotated"/>
    <img src="/images.yourimage.jpg?rotate=angle-54|bgcolor-fff" alt="your image rotated"/>

Saturation
==================
Imageprocessor can adjust the saturation of images. Simply pass the desired value (-100 to 100) to the processor.

e.g.

    <img src="/images.yourimage.jpg?saturation=60" alt="60 saturation"/>
    
Vignette
======

Imageprocessor can also add a vignette effect to images.

e.g.

    <img src="/images.yourimage.jpg?vignette=true" alt="your image vignetted"/>

Watermark
=========

Imageprocessor can add watermark text to your images with a wide range of options. Each option can be ommited and can be added in any order. Just ensure that they are pipe "|" separated:

  - text
  - size (px)
  - font (any installed font)
  - opacity (1-100)
  - style (bold|italic|regular|strikeout|underline)
  - shadow
  - position (x,y)

e.g.

    <img src="/images.yourimage.jpg?watermark=watermark=text-test text|color-fff|size-36|style-italic|opacity-80|position-30-150|shadow-true|font-arial" alt="watermark"/>
    
Internally it uses a class named `TextLayer` to add the watermark text.
