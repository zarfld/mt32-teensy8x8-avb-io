# Licenses

## This Repository

The license for the original content in this repository (documentation, design notes, hardware design files, and scripts) has **not yet been finalized**.

Candidates under consideration:

- **Hardware designs** (interposer, input pads, output stages, wing boards): [CERN-OHL-S v2](https://ohwr.org/cern_ohl_s_v2.txt) (strongly reciprocal) or [CERN-OHL-W v2](https://ohwr.org/cern_ohl_w_v2.txt) (weakly reciprocal) or [CERN-OHL-P v2](https://ohwr.org/cern_ohl_p_v2.txt) (permissive).
- **Firmware and scripts**: [MIT](https://spdx.org/licenses/MIT.html) or [Apache-2.0](https://spdx.org/licenses/Apache-2.0.html).
- **Documentation**: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) or an equivalent permissive documentation license.

Until a license is explicitly selected and added to the repository, all rights are reserved and no redistribution is permitted.

---

## Third-Party Submodules

Third-party repositories referenced as Git submodules under `external/` retain their own licenses. This repository does **not** relicense, modify, or redistribute upstream material. Consumers of this repository must comply with each upstream project's own license terms.

| Submodule path | Repository | License (as of submodule addition — verify current upstream) |
|---|---|---|
| `external/joyned_mt32-evk` | <https://github.com/JOYNED-GmbH/joyned_mt32-evk> | Check upstream repository |
| `external/Teensy8x8AudioBoard` | <https://github.com/palmerr23/Teensy8x8AudioBoard> | Check upstream repository |
| `external/control_TLV320AIC3104` | <https://github.com/palmerr23/control_TLV320AIC3104> | Check upstream repository |
| `external/TeensyAudio` | <https://github.com/PaulStoffregen/Audio> | Check upstream repository (MIT/LGPL mix) |

> **Note:** If you need to redistribute any submodule content, review the upstream license yourself. This table is informational and may not be up to date.

---

## No Datasheet Content

This repository does not reproduce verbatim content from datasheets, application notes, or other copyrighted technical documents. References to pin numbers, register addresses, and functional descriptions are made for interoperability purposes only, consistent with fair use / fair dealing principles. If in doubt, consult the original document from the component manufacturer or module vendor.
