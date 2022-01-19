# Introduction

Access OPTIGA™ TPM 2.0 [[1]](#1) on your Windows machine using TPM Base Services (TBS). The TBS feature is a system service that allows transparent sharing of the Trusted Platform Module (TPM) resources.

# Disclaimer

The project will interact with your Windows' TPM. Be cautious or risk losing your precious data!

# Table of Contents

- **[Prerequisites](#prerequisites)**
- **[Setup on Windows 10](#setup-on-windows-10)**
- **[Operation](#operation)**
- **[References](#references)**
- **[License](#license)**

# Prerequisites

- Tested on a Windows 10 machine with the following specifications:
    <br><img src="https://github.com/wxleong/tpm2-tbs/blob/develop-genesis/media/windows-10.png" width="60%">
- Using the Windows TPM utility (tpm.msc) to check if your machine has an Infineon (IFX) TPM 2.0:
    <br><img src="https://github.com/wxleong/tpm2-tbs/blob/develop-genesis/media/tpm-msc.png">
- Install IntelliJ IDEA [[2]](#2), community version will do

# Setup on Windows 10

Download Microsoft TSS project (forked from [[3]](#3)):
```
$ git clone https://github.com/wxleong/TSS.MSR
$ git checkout develop-tbs-optiga-tpm
```

Start IntelliJ IDEA with "Run as Administrator" and open the Maven project `TSS.MSR/TSS.Java`. The IDEA will resolve Maven dependencies, wait for it to complete.

# Operation

Start IntelliJ IDEA with "Run as Administrator", build and run the JUnit tests in `TSS.MSR/TSS.Java/src/test/TSSMainTests.java`:
- `main()`: The main test campaign
- `clean()`: To clean up any persistent keys, transient keys, and open sessions from running `main()`

Addressed failures:
| Test case | Error | Info | Workaround |
| --- | --- | --- | --- |
| `DrsClient.runProvisioningSequence()` | AES encryption/decryption | Not supported by TPM | Switched to software AES, check commit [4667abb](https://github.com/wxleong/TSS.MSR/commit/4667abb43e051f95f968eae480dc39666de94309) |
| `DrsClient.runProvisioningSequence()` | ActivateCredential | Need administrator permission | Run as Administrator |
| `hash()` | Hashing algorithm SHA384 | Not supported by TPM | Excluded from test |
| `pcr1()` | TPM2_PCR_Extend/TPM2_PCR_Event | Need administrator permission | Run as Administrator |
| `primaryKeys()` | `rsaPrimary.outPublic.validateSignature(dataToSign, rsaSigPss)` | Expected to fail, check the [note](https://github.com/microsoft/TSS.MSR/blob/870c929f4669c4c931f12179c2ab23649d565666/TSS.Java/src/samples/Samples.java#L175) | Fixed in commit [f413909](https://github.com/wxleong/TSS.MSR/commit/f413909e43a0bf58ecf1bd7071aa126ef4f8f13c) |
| `primaryKeys()` | `eccPrimary.outPublic.validateSignature()` | Expected to fail, check the [note](https://github.com/microsoft/TSS.MSR/blob/8368ff0f64f66d85e8e4a6abbcccb7f90da00159/TSS.Java/src/tss/Crypto.java#L171) | Fixed in commit [4337653](https://github.com/wxleong/TSS.MSR/commit/4337653b7e5c7ea5a398489bcb7f0009cce7562b) |
| `softwareKeys()` | `tpm.LoadExternal()` | Encountered intermittent error `TPM ERROR: {BINDING}`, due to the quality of software generated RSA keys | Fixed in commit [7eca140](https://github.com/wxleong/TSS.MSR/commit/7eca140ddb48ffdabf4d32d877b5556cd0398ec4) |
| `softwareKeys()` | `tpm.Sign()` | Encountered error `TPM ERROR: {HANDLE}` | Fixed in commit [3575aed](https://github.com/wxleong/TSS.MSR/commit/3575aed12b782ff20021d788656cb498c98cc0b6) |

# References

<a id="1">[1] https://www.infineon.com/cms/en/product/security-smart-card-solutions/optiga-embedded-security-solutions/optiga-tpm/</a><br>
<a id="2">[2] https://www.jetbrains.com/idea/</a><br>
<a id="3">[3] https://github.com/microsoft/TSS.MSR</a><br>

<!--
https://docs.microsoft.com/en-us/powershell/module/trustedplatformmodule/?view=windowsserver2019-ps
https://docs.microsoft.com/en-us/windows/security/information-protection/tpm/trusted-platform-module-overview
https://docs.microsoft.com/en-us/windows/win32/tbs/command-blocking

Practical guide to TPM 2.0:
In Windows 8, commands are grouped into three sets:
- No Access: Including TPM2_ContextSave and TPM2_ContextLoad
- Administrative-token processes only: Including TPM2_PCR_Extend and privacy-sensitive operations
- Standard-use access: Creation and use of keys, and so on
-->

# License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.