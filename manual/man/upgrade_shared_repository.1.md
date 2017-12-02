# NAME

upgrade\_shared\_repository, upgrade\_shared\_repository.pl -
Migrating shared repository created by earlier versions

# SYNOPSIS

    upgrade_shared_repository.pl --list LISTNAME --robot DOMAIN --all_lists
      [ --fix_qencode ]

# DESCRIPTION

upgrade\_shared\_repository.pl renames file names in shared repositories
that may be incorrectly encoded because of previous Sympa versions.

- As of Sympa 5.3a.8, file names in shared repository are Q-encoded,
therefore made easier to store on any filesystem with any encoding.
- As of Sympa 6.1b.5, 
Encoding of shared documents was not consistent with recent
version of MIME::EncWords module:
MIME::EncWords::encode\_mimewords() used to encode characters `-!*+/`.
Now these characters are preserved, according to RFC 2047 section 5.
We had to change encoding of shared documents according to new algorithm.

# OPTIONS

- --list LISTNAME --robot DOMAIN
- --all\_lists

    Specifys target list(s).

- --fix\_qencode

    If specified, fixes Q-encoding changed on Sympa 6.1b.5.
    Otherwise, applys Q-encoding introduced by Sympa 5.3a.8.

# HISTORY

upgrade\_shared\_repository.pl appeared as separate executable on Sympa 6.2.17.
