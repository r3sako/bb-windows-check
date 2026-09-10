# bb-windows-check

Verifies the fix for [AztecProtocol/aztec-packages#25434](https://github.com/AztecProtocol/aztec-packages/issues/25434)
(`bb.exe` opens files in text mode on Windows) on a real Windows machine.

The [workflow](.github/workflows/windows-check.yml):

1. Cross-compiles `bb.exe` from the `v5.2.0` tag with the fix applied, using the same Zig version and
   `amd64-windows` preset as the release build.
2. On a `windows-2025` runner, runs the scenario from the issue with both the official `bb.exe` 5.2.0 and the
   patched one, on the issue's fixtures (`alejoamiras/presto`, `fixtures/noir/square`, checked against their
   sha256 hashes):
   - `prove -k vk`: reads the bb.js key, whose first `0x1A` byte is at offset 983;
   - `prove --write_vk`: writes the proof and the key, compared byte for byte with the bb.js output;
   - `verify`: reads the reference proof, public inputs and key.

The run passes only if the official build fails (the bug is reproduced) and the patched build passes every
check. The results table is on each run's summary page.
