---
assets:
  - created_at: 2023-06-01T10:52:07Z
    name: sympa-6.2.72.tar.gz
    size: 13361403
    updated_at: 2023-06-01T10:52:14Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.72/sympa-6.2.72.tar.gz
  - created_at: 2023-06-01T10:52:07Z
    name: sympa-6.2.72.tar.gz.md5
    size: 54
    updated_at: 2023-06-01T10:52:07Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.72/sympa-6.2.72.tar.gz.md5
  - created_at: 2023-06-01T10:52:06Z
    name: sympa-6.2.72.tar.gz.sha256
    size: 86
    updated_at: 2023-06-01T10:52:07Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.72/sympa-6.2.72.tar.gz.sha256
  - created_at: 2023-06-01T10:52:05Z
    name: sympa-6.2.72.tar.gz.sha512
    size: 158
    updated_at: 2023-06-01T10:52:06Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.72/sympa-6.2.72.tar.gz.sha512
created_at: 2023-06-01T10:50:20Z
prerelease: 0
published_at: 2023-06-01T13:31:49Z
tag_name: 6.2.72
title: 'Sympa 6.2.72 released '
---

<!-- Release notes generated using configuration in .github/release.yml at 6.2.72 -->

## What's Changed
### Implemented enhancements
* Update `support/xgettext.pl` by @ikedas in [\#1511](https://github.com/sympa-community/sympa/pull/1511)
* ARC: When ARC seal was added, DKIM signing should be forced (#1450) by @ikedas in [\#1452](https://github.com/sympa-community/sympa/pull/1452)
* ARC: Add Authentication-Results: field (AR), if useful one is not found by @ikedas in [\#1453](https://github.com/sympa-community/sympa/pull/1453)
* Forwarded messages should also be ARC sealed if possible by @ikedas in [\#1457](https://github.com/sympa-community/sympa/pull/1457)
* Web archive: Copy permalink of the message to clipboard. by @ikedas in [\#1442](https://github.com/sympa-community/sympa/pull/1442)
* Deprecate scripts under bin/ (#1386), part 1 of 2 by @ikedas in [\#1403](https://github.com/sympa-community/sympa/pull/1403)
* Deprecate scripts under bin/ (#1386), part 2 of 2 by @ikedas in [\#1405](https://github.com/sympa-community/sympa/pull/1405)
* Update SMTP status codes by @ikedas in [\#1467](https://github.com/sympa-community/sympa/pull/1467)
* Deprecate `conf_2_db` feature (#1424) by @ikedas in [\#1492](https://github.com/sympa-community/sympa/pull/1492)
* WWSympa: New "cgi" authentication simply using credentials provided by HTTP server by @ikedas in [\#1499](https://github.com/sympa-community/sympa/pull/1499)
* Update default of anonymous_header_fields parameter by @ikedas in [\#1502](https://github.com/sympa-community/sympa/pull/1502)
* Improve "set" request handler by @ikedas in [\#1503](https://github.com/sympa-community/sympa/pull/1503)
* Use DSN for the message to notify moderation by @ikedas in [\#1508](https://github.com/sympa-community/sympa/pull/1508)
* Adding translation catalogs for Latvian (lv) by request by @ikedas in [\#1520](https://github.com/sympa-community/sympa/pull/1520)
* Deprecate scripts under bin/ (#1386), fixed by @ikedas in [\#1521](https://github.com/sympa-community/sympa/pull/1521)
* Add --noout option to sympa command line (see #1518) by @ikedas in [\#1527](https://github.com/sympa-community/sympa/pull/1527)
* CLI: Allow hyphens in options by @ikedas in [\#1533](https://github.com/sympa-community/sympa/pull/1533)
* Extend info soap function (#1542) by @ikedas in [\#1546](https://github.com/sympa-community/sympa/pull/1546)
* Extend info soap function by @acasadoual in [\#1545](https://github.com/sympa-community/sympa/pull/1545)
* RFC 8058 One-Click Unsubscribe (#1210) by @ikedas in [\#1538](https://github.com/sympa-community/sympa/pull/1538)
* Delete user account completely (#1459) by @racke in [\#1564](https://github.com/sympa-community/sympa/pull/1564)
* Switch Font Awesome from 4 to 6 Free by @ikedas in [\#1657](https://github.com/sympa-community/sympa/pull/1657)
### Fixed bugs
* Following typo fix in Mail::DKIM::ARC::Signer by @ikedas in [\#1449](https://github.com/sympa-community/sympa/pull/1449)
* Excess header fields are shown in the web archives (#1447) by @ikedas in [\#1448](https://github.com/sympa-community/sympa/pull/1448)
* Loading CSV driver crashes by @ikedas in [\#1435](https://github.com/sympa-community/sympa/pull/1435)
* Import SOAP encoding schema in WSDL by @cgx in [\#1456](https://github.com/sympa-community/sympa/pull/1456)
* WWSympa: Page size cannot be changed on review and reviewbouncing by @ikedas in [\#1466](https://github.com/sympa-community/sympa/pull/1466)
* Let some obsoleted parameters be retired and add convenient spam_status scenarios by @ikedas in [\#1470](https://github.com/sympa-community/sympa/pull/1470)
* Options for welcome_return_path & remind_return_path do not describe their function (#1475) by @ikedas in [\#1476](https://github.com/sympa-community/sympa/pull/1476)
* Template error parsing not detected (#1474) by @ikedas in [\#1477](https://github.com/sympa-community/sympa/pull/1477)
* Deprecate `dkim` authentication method for scenarios by @ikedas in [\#1486](https://github.com/sympa-community/sympa/pull/1486)
* Cannot allow owners to manage editors of a distribution list by @aepli in [\#1489](https://github.com/sympa-community/sympa/pull/1489)
* Templates couldn't access to the fields defined by db_additional_subscriber_fields parameter by @ikedas in [\#1495](https://github.com/sympa-community/sympa/pull/1495)
* dmarc_protection.phrase "From" format INCORRECT (#1498) by @ikedas in [\#1500](https://github.com/sympa-community/sympa/pull/1500)
* include_sql_query requests unuseful parameters for CSV database driver (#1437) by @ikedas in [\#1505](https://github.com/sympa-community/sympa/pull/1505)
* `mailto:` link cannot be detected by some MUAs (#1124) [2] by @ikedas in [\#1507](https://github.com/sympa-community/sympa/pull/1507)
* Exim: Failed to get envelope sender in "Return-path:" field (#1354) by @ikedas in [\#1509](https://github.com/sympa-community/sympa/pull/1509)
* WWSympa crashes by topics.conf with inappropriate format (#1242) by @ikedas in [\#1510](https://github.com/sympa-community/sympa/pull/1510)
* Skip edit for shared document options when disabled (#872) [3] by @ikedas in [\#899](https://github.com/sympa-community/sympa/pull/899)
* Fix SQLite upgrade with lowercase types by @k0lter in [\#1516](https://github.com/sympa-community/sympa/pull/1516)
* Fix various typos in source comments by @k0lter in [\#1517](https://github.com/sympa-community/sympa/pull/1517)
* Inclusion: log mistakenly notes failure of inclusion by @ikedas in [\#1431](https://github.com/sympa-community/sympa/pull/1431)
* Fix 'sympa config' command return code to be 0 when there are no changes by @k0lter in [\#1518](https://github.com/sympa-community/sympa/pull/1518)
* Fix and improve custom attribute (#1535) by @ikedas in [\#1536](https://github.com/sympa-community/sympa/pull/1536)
* Some bugs with DKIM / ARC by @ikedas in [\#1543](https://github.com/sympa-community/sympa/pull/1543)
* WWSympa: `msg` (ex. `arcsearch_id`) crashes by @ikedas in [\#1551](https://github.com/sympa-community/sympa/pull/1551)
* Fix incomplete `sympa.wsdl` by @acasadoual in [\#1549](https://github.com/sympa-community/sympa/pull/1549)
* Sympa::HTMLSanitizer: Avoid bug in HTML::StripScripts, reDoS with style attribute (#1573) by @ikedas in [\#1575](https://github.com/sympa-community/sympa/pull/1575)
* Spurious error log during upgrade on removing nonexisting column by @ikedas in [\#1560](https://github.com/sympa-community/sympa/pull/1560)
* Spurious error log adding subscriber (#1532) by @ikedas in [\#1571](https://github.com/sympa-community/sympa/pull/1571)
* [CVE-2021-32850] Potential XSS in jquery-minicolors (#1561) by @ikedas in [\#1562](https://github.com/sympa-community/sympa/pull/1562)
* Fix description for add CLI command by @racke in [\#1565](https://github.com/sympa-community/sympa/pull/1565)
* "sympa test soap" crashes due to undefined subroutine by @ikedas in [\#1593](https://github.com/sympa-community/sympa/pull/1593)
* Skip initial notification email for new user with nomail reception by @racke in [\#1550](https://github.com/sympa-community/sympa/pull/1550)
### Other changes
* Adding GH workflow to submit the PR for translation by @ikedas in [\#1515](https://github.com/sympa-community/sympa/pull/1515)
* Remove OChangeLog by @ikedas in [\#1471](https://github.com/sympa-community/sympa/pull/1471)
* Adjust branch in support README by @racke in [\#1530](https://github.com/sympa-community/sympa/pull/1530)
* Update copyright notices, add documentation and correct some typos by @ikedas in [\#1552](https://github.com/sympa-community/sympa/pull/1552)
* Improve wording in README.support document. by @racke in [\#1553](https://github.com/sympa-community/sympa/pull/1553)
* Replace obsolete Ansible role in README by @racke in [\#1558](https://github.com/sympa-community/sympa/pull/1558)
* Update support/sync_translation.sh by @ikedas in [\#1556](https://github.com/sympa-community/sympa/pull/1556)
* Adding translation catalogs for Georgian (ka) by request by @ikedas in [\#1596](https://github.com/sympa-community/sympa/pull/1596)

## New Contributors
* @aepli made their first contribution in [\#1489](https://github.com/sympa-community/sympa/pull/1489)
* @acasadoual made their first contribution in [\#1545](https://github.com/sympa-community/sympa/pull/1545)

**Full Changelog**: [6.2.70...6.2.72](https://github.com/sympa-community/sympa/compare/6.2.70...6.2.72)
