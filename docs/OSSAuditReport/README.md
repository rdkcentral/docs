# OSS Audit Reports

This directory contains detailed Open Source Software (OSS) audit reports for the RDK Yocto/BitBake project.

## Overview

The OSS audit reports are automatically generated **monthly** and contain detailed audit tables with information about all audited OSS recipes, files, and components across all configured repositories.

## Report Structure

Each monthly report is named with a date-based filename:
- **Format:** `oss_audit_report_YYYY-MM-DD.md`
- **Example:** `oss_audit_report_2024-11-20.md`
- **Frequency:** Generated automatically on the 1st of each month at 03:00 UTC

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
Complete table with the following columns:
- **Repository:** GitHub repository name and layer
- **File Path:** Full path to the audited file
- **File Type:** File extension (bb, bbappend, conf, inc, patch, service, sh, etc.)
- **Recipes:** Number of recipes referenced
- **Patches:** Number of patches included
- **Status:** Audit status (success, warning, error)

### 3. Audit Columns Documentation
Detailed explanation of each column in the audit table.

### 4. Repository Scan Failures (if any)
List of repositories that failed to scan with error details.

## Minimal vs Detailed Reports

### Minimal Report (in meta-oss-reference-release)
**Location:** `meta-oss-reference-release/reports/oss-audit/`

Contains summary information:
- 4 summary tables (Files per repository, by layer, by type)
- Repository scan failures
- Size: ~7.5 KB

**Format:** 
- Markdown: `oss-audit.md`
- JSON: `oss-audit.json`

### Detailed Report (in rdkcentral/docs)
**Location:** `rdkcentral/docs/docs/OSS_Audit_Report/`

Contains complete audit information:
- Full audit table with 500+ file entries
- Detailed audit columns documentation
- Monthly date-based filenames
- Size: ~4.4 MB per report

## Finding Reports

### By Month
Reports are organized chronologically with date-based filenames. View any report by selecting it from the file list.

### By Year
For historical analysis, reports from previous years are also maintained for reference.

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

**Process:**
1. Clones all configured repositories
2. Scans for Yocto recipes and supporting files
3. Generates minimal report (meta-oss-reference-release)
4. Generates detailed report (rdkcentral/docs) with date-based filename
5. Commits both reports to their respective repositories

## Configuration

Repositories to audit are configured in:
- File: `configs/oss_audit_repos.json` (in meta-oss-reference-release)
- Format: JSON array with repository information

## Maintenance

### Adding New Reports
Reports are automatically added each month. No manual intervention required.

### Archiving Old Reports
Historical reports are maintained in this directory for long-term reference and trend analysis.

## Related Documentation

- **Minimal Reports:** See [`meta-oss-reference-release/reports/oss-audit/`](https://github.com/rdkcentral/meta-oss-reference-release/tree/develop/reports/oss-audit)
- **Audit Script:** [`scripts/oss_audit.py`](https://github.com/rdkcentral/meta-oss-reference-release/blob/develop/scripts/oss_audit.py)
- **Workflow:** [`oss-audit.yml`](https://github.com/rdkcentral/meta-oss-reference-release/blob/develop/.github/workflows/oss-audit.yml)

## Questions or Issues?

If you have questions about the OSS audit reports or need to modify the audit configuration, please refer to the meta-oss-reference-release repository documentation.

---

**Last Updated:** 2026-11-20  
**Report Type:** Detailed OSS Audit Reports  
**Update Frequency:** Monthly (1st of each month at 03:00 UTC)
