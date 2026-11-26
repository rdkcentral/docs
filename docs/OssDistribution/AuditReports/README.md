# OSS Audit Reports

This directory contains detailed Open Source Software (OSS) audit reports for the RDK Yocto/BitBake project.

## Overview

The OSS audit reports are automatically generated **monthly** and contain detailed audit tables with information about all audited OSS recipes, files, and components across all configured repositories. Reports are versioned using GitHub tags for historical tracking.

## Report Structure

The detailed audit report is maintained as a single file:
- **Filename:** `oss_audit_report.md`
- **Frequency:** Updated automatically on the 1st of each month at 03:00 UTC
- **Versioning:** Each monthly update is tagged

## Report Contents

Each detailed audit report includes:

### 1. Report Header
- Generation timestamp
- Configured repositories count
- Failed repositories count
- Total audited files
- Recipe and append files count
- Files containing patch entries count

### 2. Audit Table
Complete table with the following nine columns:
- **Repository:** GitHub repository name and layer
- **Layer:** Yocto layer designation
- **Recipe:** Recipe name
- **Path:** Full path to the audited file
- **File Type:** File extension (bb, bbappend, conf, inc, patch, service, sh, etc.)
- **SRC_URI:** Source URI reference
- **LICENSE:** License information
- **Patch:** Patch entries and related SRC_URI values
- **Include:** Include file references

### 3. Audit Columns Documentation
Detailed explanation of each column in the audit table.

### 4. Repository Scan Failures (if any)
List of repositories that failed to scan with error details.

## Minimal vs Detailed Reports

### Minimal Report (in meta-oss-reference-release)
**Location:** `meta-oss-reference-release/reports/oss-audit/`

Contains both summary information and detailed audit data:
- Three summary tables (Files per repository, by layer, by type)
- Full audit table with detailed file-by-file information
- Repository scan failures
- Size: ~7.5 KB

**Format:** 
- Markdown: `oss-audit.md` - Contains summary tables plus complete audit table
- JSON: `oss-audit.json` - Contains summary statistics only

### Detailed Report (in rdkcentral/docs)
**Location:** `rdkcentral/docs/docs/OssDistribution/AuditReports/`

Contains detailed audit archive with single maintained file:
- Full audit table with 500+ file entries (same as markdown version in minimal report)
- Complete audit table in Markdown format: `oss_audit_report.md`
- Size: ~4.4 MB per report
- Versioning: GitHub tags
- Historical tracking: Access previous versions via GitHub tag checkout or release history

## Finding Reports

### Current Version
The latest `oss_audit_report.md` file contains the most recent audit data.

### Historical Versions
Previous monthly versions are available via GitHub tags:
- View release history on GitHub releases page

## Using the Reports

### For Quick Review
Use the **minimal report** in the main repository for quick summary statistics.

### For Detailed Analysis
Use the **detailed report** in this directory for complete file-by-file audit information.

### For Automation
Both reports are available in JSON format for dashboard integration and automated processing.

## Report Generation

Reports are generated automatically by GitHub Actions workflow:

**Workflow:** `.github/workflows/oss-audit.yml` (in meta-oss-reference-release)

**Publishing Behavior:**
1. Minimal reports (oss-audit.md, oss-audit.json) are committed to `meta-oss-reference-release` on the develop branch
2. Detailed markdown report (oss_audit_report.md) is committed to `rdkcentral/docs` with monthly GitHub tag
3. Both reports use the same underlying audit data, with different presentation formats

**Process:**
1. Clones all configured repositories
2. Scans for Yocto recipes and supporting files
3. Generates minimal reports (oss-audit.md, oss-audit.json) in meta-oss-reference-release
4. Generates detailed markdown report (oss_audit_report.md) in rdkcentral/docs/docs/OssDistribution/AuditReports/
5. Commits minimal reports to meta-oss-reference-release/develop
6. Commits detailed report to rdkcentral/docs/docs/OssDistribution/AuditReports/ on main branch

## Configuration

Repositories to audit are configured in:
- **File:** `configs/oss_audit_repos.json` (in meta-oss-reference-release)
- **Format:** JSON object (not an array) with the following structure:
  ```json
  {
    "authorized_repositories": [
      {
        "repository": "owner/repo",
        "layer": "layer_name",
        ...
      }
    ],
    "audited_extensions": [...],
    "reporting": {...}
  }
  ```
- **Key Structure:** Object with `authorized_repositories` array containing repository configurations, plus audited_extensions and reporting settings

## Maintenance

### Updating Reports
Reports are automatically updated each month. The oss_audit_report.md file is replaced with the latest audit data.

### Accessing Historical Data
Previous audit data is preserved via GitHub tags:
- Each tag corresponds to a monthly audit snapshot
- Compare versions across different tags to track changes over time

## Related Documentation

- **Minimal Reports:** See [`meta-oss-reference-release/reports/oss-audit/`](https://github.com/rdkcentral/meta-oss-reference-release/tree/develop/reports/oss-audit)
- **Audit Script:** [`scripts/oss_audit.py`](https://github.com/rdkcentral/meta-oss-reference-release/blob/develop/scripts/oss_audit.py)
- **Workflow:** [`oss-audit.yml`](https://github.com/rdkcentral/meta-oss-reference-release/blob/develop/.github/workflows/oss-audit.yml)

## Questions or Issues?

If you have questions about the OSS audit reports or need to modify the audit configuration, please refer to the meta-oss-reference-release repository documentation.

---

**Last Updated:** 2026-11-26  
**Report Type:** Detailed OSS Audit Reports  
**Update Frequency:** Monthly (1st of each month at 03:00 UTC)
