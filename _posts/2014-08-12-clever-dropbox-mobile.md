---
layout: post
title:  Dropbox's clever mobile experience
date:   2014-08-12 15:00:00
---

Dropbox posted a neat explanation of the high-level design and engineering of their [new mobile user experience](https://tech.dropbox.com/2014/08/building-dropboxs-new-user-experience-for-mobile-part-1/). There's a lot of really interesting tech being used here, and it's interesting to be able to see that the strategies they're using to approach some problems are nearly identical to those I've worked with in various projects recently.

The issue of introducing new users to any sort of system is a tricky one; Dropbox however seems to have built up a fairly good solution to guiding users through their rather complex synchronization system. The leveraging of custom QR codes to enable session-identity sharing from a user's phone to their desktop is very clever. I think rapid, controllable data exchange between devices may be the best (ie. non-ridiculous) use of high-data-density barcodes yet. Bluetooth and AirDrop and all those other technologies are so finicky and beyond-reasonable-control. Displaying and scanning a barcode, on the other hand, is very much under the user's control; the user literally *has vision* into the operation of the system, which is&mdash;I believe&mdash;extremely important in data exchange between a user's devices for both usability and security's sake.
