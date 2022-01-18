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

- Tested with Windows 10 machine:
    <br><img src="https://github.com/wxleong/tpm2-tbs/blob/develop-genesis/media/windows-10.png" width="60%">
- Using the Windows TPM utility (tpm.msc) to check if your machine has an Infineon (IFX) TPM 2.0:
    <br><img src="https://github.com/wxleong/tpm2-tbs/blob/develop-genesis/media/tpm-msc.png">
- Install IntelliJ IDEA [[2]](#2), community version will do

# Setup on Windows 10

Download Microsoft TSS project (forked from [[3]](#3)):
```
$ git clone https://github.com/wxleong/TSS.MSR
$ git checkout develop-optiga-tpm
```

Start IntelliJ IDEA with "Run as Administrator" and open the Maven project `TSS.MSR/TSS.Java`. The IDEA will resolve Maven dependencies, wait for it to complete.

# Operation

Start IntelliJ IDEA with "Run as Administrator", run the JUnit tests in `TSS.MSR/TSS.Java/src/test/TSSMainTests.java`:
- main(): The main test campaign
- clean(): To clean up any persistent keys, transient keys, and open sessions after running `main()`

Observed failures:
- Test case DrsClient.runProvisioningSequence():
    - AES encryption/decryption (reason: not supported by TPM)
    - ActivateCredential (reason: need administrator permission)
- Test case hash():
    - Hashing algorithm SHA384 (reason: not supported by TPM)
- Test case pcr1():
    - TPM2_PCR_Extend/TPM2_PCR_Event (reason: need administrator permission)
- Test case primaryKeys():
    - rsaPrimary.outPublic.validateSignature() expected to fail, check implementation `src/tss/Crypto.java: validateSignature()`
    - eccPrimary.outPublic.validateSignature() similar issue
- Test case softwareKeys():
    - Intermittent tpm.LoadExternal() TPM ERROR: {BINDING}, due to the use of poor quality random value in software key generation
    - tpm.Sign() TPM ERROR: {HANDLE}, unknown yet...

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