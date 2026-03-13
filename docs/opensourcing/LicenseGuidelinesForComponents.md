# Licensing and Copyright Guidelines <a id="licensing-and-copyright-guidelines"></a>
Audience: External Community

This page provides Licensing and Copyright Guidelines for adding code to the RDK Code Management Repository.

A list of approved licenses and unapproved licenses for included snippets may be accessed [here](https://github.com/rdkcentral/docs/wiki/ROG_LicenseApprovals) 

**AI Generated Code:** Contributions generated in whole or in part by AI are not considered “original creations” (required by Section 5 of the CLA) and therefore may not be accepted. Note this is currently under review by legal.

---
## Table of Contents <a id="table-of-contents"></a>

1. [Components](#1-components)
   1. [General License and Recipe Guidelines](#1.1-general-license-and-recipe-guidelines)
   2. [Apache V2.0 Licensed Components](#1.2-apache-v2-0-licensed-components)
   3. [Other Licensed Components](#1.3-other-licensed-components)


---
## 1. Components <a id="1-components"></a>

### 1.1 General License and Recipe Guidelines <a id="1.1-general-license-and-recipe-guidelines"></a>

#### 1.1.1 License Guidelines <a id="1.1.1-license-guidelines"></a>
- License files need to include all Licenses (and their associated copyrights if so required by the license) which are included in the component. 
- License files must be included at the top-level directory of a component and these license files are referenced in Meta Layer recipes.
- Some components use sublicensing, whereby specific sub directories are licensed differently and have their own license files. The RDKCentral team will advise when sublicensing is required.
- The top-level license file should contain the complete set of all licenses applicable to the component.
- The formats of the license files and copyright headers required for components distributed by RDK Management under the Apache license, LGPL or GPL, differ slightly and may be viewed in the sample licensing repository [here](https://github.com/rdkcentral/sample-licensing)

#### 1.1.2 Copyright Header Guidelines <a id="1.1.2-copyright-header-guidelines"></a>
- The following describes rules that apply for copyright headers in source code files (excluding patch files).

   - The copyright header of the code owner should be present at the top of all source code files.
   - The copyright header should clearly state the license under which the code is distributed.
   - In the case of modified files, if the original copyright header is not an RDK Management copyright header the original should be retained and a new RDK Management copyright header should be added.

#### 1.1.3 Recipe Guidelines <a id="1.1.3-recipe-guidelines"></a>
Recipes have two lines that pertain to Licenses:

>LICENSE=\<List of all licenses used by the component.

>LIC_FILES_CHKSUM=\<Checksum of component top level license file.>

_Note: Where sub directory license files are included, then the recipe related to those sub-directories should refer to the licenses and license files in the sub directories._

For more information on Recipe License Fields see:

https://docs.yoctoproject.org/dev/contributor-guide/recipe-style-guide.html#recipe-license-fields

Example 1 Apache + Others
>LICENSE = "Apache-2.0 & ISC & MIT"
>LIC_FILES_CHKSUM = "file://LICENSE;md5=1d8db96e7ee90f3821eb5e7e913a7b2a"

Example 2 Commercial License & Others
>LICENSE = "commercial-gSOAP & gSOAP-1.3b & Comcast & Dimark & MIT & FSF-Unlimited"
>LIC_FILES_CHKSUM = "file://LICENSE;md5=cbbc1e700af12508df6d6ee261c46176"
>
#### 1.1.4 How to give credit for an external code snippet <a id="1.1.4-how-to-give-credit-for-an-external-code-snippet"></a>

1. Either at the top of the file under the header, add:

```
"Uses code from <source> which is:
Copyright YEAR Copyright Owner
Licensed under the NAME License,"
```
   > or above where the code is used, (if this is only place in the file):

```
"optional  extra info e.g. URL
Copyright YEAR Copyright Owner
Licensed under the NAME License"
```

   > Give the version of a license if there is more than one version in existence:
```
"Licensed under the Apache License, Version 2.0"
```

2. Append these same lines to the end of NOTICE at top level in the component.

3. Check if the license is already present in top level LICENSE and if not, append it.  Give a heading with the name of the license and version if necessary.  Use generic YEAR and COPYRGHT OWNER values in LICENSE, so that the license will apply to any code with this license.  Specific year and copyright owner must be given in NOTICE and in the file containing the source code.

4. Check if a change is required to a license checksum (LIC_FILES_CHKSUM) in any recipes because of the change to LICENSE.

---
### 1.2 Apache V2.0 Licensed Components <a id="1.2-apache-v2-0-licensed-components"></a>

#### 1.2.1 Apache V2.0 License Files <a id="1.2.1-apache-v2-0-license-files"></a>

- LICENSE: This contains the full text of all licenses used in the component. Therefore, at a minimum, the text of the Apache 2.0 license should be present in the LICENSE file.
- NOTICE: This lists the applicable license for the component plus all applicable copyright notices,  together with a reference to  applicable licenses e.g.
  >Copyright 2019 RDK Management, LLC
  
  >Licensed under the Apache license, Version 2.0
- COPYING: This file is just a link to the LICENSE file.
- CONTRIBUTING.md:  This outlines how the community can make a contribution. 

See:
[Apache 2.0 License Files](https://github.com/rdkcentral/sample-licensing/blob/main/standard_files/other-apache)

Noe:The Copyright Notice in the NOTICE file need only have the earliest copyright date – it is not necessary to keep updating the copyright year in the notice file for subsequent changes under the copyright.

Files under Apache 2.0 may also contain a contributor's Apache 2.0 header. 

#### 1.2.2 Apache V2.0 Guidelines for Components <a id="1.2.2-apache-v2-0-guidelines-for-components"></a>
- All RDKM copyright headers within the component must be Apache 2.0.
- New files:
    - New files contributed may include the new contributor's Apache 2.0 copyright header with the addition of the RDKM Apache 2.0 header.  It is acceptable to shorten the new contributor's header to just the two lines of attribution (Copyright <YEAR> <NAME>/Licensed under the Apache License, Version 2.0).
- Modified files:
    - If a contributor makes any modifications to a file, then the file must have an RDKM Apache 2 Copyright Header.
    - Modified files may also be updated to include the new contributor's Apache 2.0 copyright header but retaining the original attribution.
- Unmodified files:
    - The RDKM Header is not needed on top of open source files that have not been modified by contributors.
    - When code from an open source component is used the Copyright Header needs to be retained if the License requires it.
- NOTICE files must be updated to include new copyrights.

_Note: The spdx abreviated Apache Header is not accepted by Apache who require the full version. So the following would be rejected:_

    Copyright YYYY ABC Corporation, LLC
    
    SPDX-License-Identifier: Apache-2.0

### 1.3. Other Licensed Components <a id="1.3-other-licensed-components"></a>

See the following sample repositories for guidelines on other supported licensing: 

[LGPLv2.1 License Files](https://github.com/rdkcentral/sample-licensing/blob/main/standard_files/lgpl)

[GPLv2 License Files](https://github.com/rdkcentral/sample-licensing/blob/main/standard_files/gpl)

[Meta Layer License Files](https://github.com/rdkcentral/sample-licensing/tree/main/standard_files/metadata)

[Comcast Apache License Files](https://github.com/rdkcentral/sample-licensing/tree/main/standard_files/comcast-apache)

