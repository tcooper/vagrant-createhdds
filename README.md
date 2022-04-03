# rocky-linux-createhdds

## Overview

This Vagrant configuration will help you provision a Vagrant machine with the tools required to run createhdds.

If you have VirtualBox Guest Additions installed the createhdds repo will be cloned into the default shared folder causing it to be resident on your host and **not** inside the guest and assets will be written into a directory in the default shared folder. This will persist your assets through reprovisioning cycles of the guest.
