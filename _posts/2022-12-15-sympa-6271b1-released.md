---
assets:
  - created_at: 2022-12-15T15:19:13Z
    name: sympa-6.2.71b.1.tar.gz
    size: 12808794
    updated_at: 2022-12-15T15:19:20Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.71b.1/sympa-6.2.71b.1.tar.gz
  - created_at: 2022-12-15T15:19:13Z
    name: sympa-6.2.71b.1.tar.gz.md5
    size: 57
    updated_at: 2022-12-15T15:19:13Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.71b.1/sympa-6.2.71b.1.tar.gz.md5
  - created_at: 2022-12-15T15:19:13Z
    name: sympa-6.2.71b.1.tar.gz.sha256
    size: 89
    updated_at: 2022-12-15T15:19:13Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.71b.1/sympa-6.2.71b.1.tar.gz.sha256
  - created_at: 2022-12-15T15:19:12Z
    name: sympa-6.2.71b.1.tar.gz.sha512
    size: 161
    updated_at: 2022-12-15T15:19:13Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.71b.1/sympa-6.2.71b.1.tar.gz.sha512
created_at: 2022-12-15T15:15:27Z
prerelease: 1
published_at: 2022-12-15T15:20:17Z
tag_name: 6.2.71b.1
title: Sympa 6.2.71b.1 released
---

<!-- Release notes generated using configuration in .github/release.yml at 6.2.71b.1 -->

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
* Extend info soap function by @acasadoual in [\#1545](https://github.com/sympa-community/sympa/pull/1545)
* RFC 8058 One-Click Unsubscribe (#1210) by @ikedas in [\#1538](https://github.com/sympa-community/sympa/pull/1538)
### Fixed bugs
* Following typo fix in Mail::DKIM::ARC::Signer by @ikedas in [\#1449](https://github.com/sympa-community/sympa/pull/1449)
* Excess header fields are shown in the web archives (#1447) by @ikedas in [\#1448](https://github.com/sympa-community/sympa/pull/1448)
* Loading CSV driver crashes by @ikedas in [\#1435](https://github.com/sympa-community/sympa/pull/1435)
* Import SOAP encoding schema in WSDL by @cgx in [\#1456](https://github.com/sympa-community/sympa/pull/1456)
* WWSyjmpa: Page size cannot be changed on review and reviewbouncing by @ikedas in [\#1466](https://github.com/sympa-community/sympa/pull/1466)
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
### Other changes
* Adding GH workflow to submit the PR for translation by @ikedas in [\#1515](https://github.com/sympa-community/sympa/pull/1515)
* Remove OChangeLog by @ikedas in [\#1471](https://github.com/sympa-community/sympa/pull/1471)
* Adjust branch in support README by @racke in [\#1530](https://github.com/sympa-community/sympa/pull/1530)
* Extend info soap function (#1542) by @ikedas in [\#1546](https://github.com/sympa-community/sympa/pull/1546)

## New Contributors
* @aepli made their first contribution in [\#1489](https://github.com/sympa-community/sympa/pull/1489)
* @github-actions made their first contribution in [\#1519](https://github.com/sympa-community/sympa/pull/1519)
* @acasadoual made their first contribution in [\#1545](https://github.com/sympa-community/sympa/pull/1545)

**Full Changelog**: [6.2.70...6.2.71b.1](https://github.com/sympa-community/sympa/compare/6.2.70...6.2.71b.1)
