# Preparing Code for Opensourcing

* Most code being opensourced by RDKM is Apache-2.0 licensed.  Such repositories can also contain code under different licenses if those licenses are compatible and their conditions are respected.
* There are exceptions: GStreamer plugins are licensed as LGPLv2.1
* A handful of repositories are GPLv2 licensed.
* GPLv3 code is not allowed for code that could end up in production.
* A few repositories have mixed licensing and their licensing files reflect this.  The CMF (Code Management Facility) team will advise after scanning.
* Meta layers are handled differently.  Metadata is MIT licensed; Other files vary.


## Standard Files

* This varies depending on how the repository is licensed.  There are sample files in this repository: <https://github.com/rdkcentral/sample-licensing>.
* If the repo contains code from an external source, that code must be credited.  In Apache-2 repositoriess, there are three parts to this:
1. In the file where the code is used, either at the top of the file or before the code extract, add:

> Possible source line (not always)
> Copyright YEAR COPYRIGHT_OWNER
> Licensed under the XYZ License.

e.g.

> Code extract from libfoo
> Copyright 2025 John Smith
> Licensed under the MIT License

2. Append a credit to NOTICE:

> Copyright 2025 John Smith
> Licensed under the MIT License

3. Append new license(s) to LICENSE:

Append the full license text.  Do not change the generic license values YEAR and COPYRIGHT_OWNER etc. because this single copy of the license will cover everywhere it applies in the repositories.  The <https://spdx.org> site has a huge collection of licenses.

* LGPLv2 repositories should give extra credits in the COPYING file if needed i.e. where there are code snippets needing acknowledgement.

* Meta layers: External code in patches doesn't need to be credited in NOTICE and LICENSE because of the wording in LICENSE.  (See the sample-licensing repository.)


## Headers

* All significant code files must have a header.  Sample headers are included in the <https://github.com/rdkcentral/sample-licensing> files.
* Makefiles are considered to be code.
* Very short files (say 1-9 lines) are ignored, especially if the content is trivially simple and the header is larger than the contents.
* Metadata files in meta layers are exempt so most recipes and appends don't have headers.  This follows the openembedded approach.
* Copyrights state who owns the code.  So it is likely to be a company e.g. Comcast, or RDK, not an individual.  Check that you use the right form of your company name. So not "Comcast" but "Comcast Cable Communications Management, LLC".
* Conventionally, configuration files have been considered exempt although many formats do permit headers e.g. xml files.


## Patches

* Patches are common in meta layers.  Patches must have a header which gives the source of the changed code.  ("Source: Comcast" means that Comcast engineers wrote the changed code, not that someone from Comcast copied a change from somewhere else.)
* There should be a minimum set of fields, at least Date/From:/Source:.
* This applies to external patches, even if the original does not have a source.
* The source can be a full link, a repository, a reference to a backport from a later version (not an exhaustive list).


## Binary Content

* Github is not optimal for storing binaries and large binaries must be stored elsewhere.
* Small binaries (a few 10s of kb) which do not change may be OK.
* Images are subject to licensing requirements: an image from elsewhere cannot be used unless its license permits.  (Image licenses can be hard to uncover.)  In addition, images must not contain identifiable individuals, company logos, trademarks.
* Icons must also be properly licensed.
* In most case, compressed or archived files e.g. tar or zip etc. should not be stored in rdkcentral as they cannot be properly scanned.


## License Conditions to be Aware of

* LGPLv2.1 code must be dynamically linked.
* Never remove someone else's copyright from code.
* Public domain code may not always be permitted.  CMF Team will advise.
* Some licenses require changes to be marked (GPL, LGPL, Apache-2)
* Code from stackoverflow and other online forums is not normally allowed.


# Scanning

A scan consists of the following:

* Blackduck.  The Blackduck tool's main function is to look for snippets of opensourced code in the repository.  Such code may be OK to use, or it may be OK if credit is given, or it may need to be rewritten.  The output from the tool is interpreted by the RDKM CMF Compliance Team who will provide detailed guidance.

* General Audit: This checks for standard license files, presence of headers, problem copyrights, problem files (binaries, keys, secrets etc.)  Includes a visual inspecion where feasible.

* Problem CCS strings.  The Comcast Cybersecurity (CCS) group has provided a list of strings, mainly related to product-specific handling in code.  Results are usually presented in a file and given the nature of the strings, there are often false positives.  The component owner needs to review.

* Results will be reported via Jira (usually on the CMFSUPPORT ticket).

* The CMF team will rescan after the corrections have been done and give their formal approval for opensourcing.
