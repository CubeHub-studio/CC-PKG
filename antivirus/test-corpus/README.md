# PKG Antivirus Test Corpus

This directory contains benign test material for the antivirus scanners.

It intentionally does not contain real malware. Real malware samples are unsafe to distribute as part of a package manager repository. Instead, tests should use harmless snippets that reproduce suspicious indicators such as dynamic code loading, shell execution, remote requests, destructive file operations, shutdown/reboot calls, and encoded-data patterns.

The scanner should flag these test cases before installation and never write their files to the computer unless the user explicitly completes the 20-key override.
