# Introduction

OPTIGA™ TPM 2.0 [[1]](#1) on Windows using TPM Base Services (TBS). The TBS feature is a system service that allows transparent sharing of the Trusted Platform Module (TPM) resources.

# Disclaimer

The project will interact with your Windows' TPM. Know what you are doing or risk bricking your Windows machine!

# Table of Contents

- **[Prerequisites](#prerequisites)**
- **[Setup on Windows 10](#setup-on-windows-10)**
- **[Operation](#operation)**
- **[References](#references)**
- **[License](#license)**

# Prerequisites

- Windows 10 machine with TPM 2.0. Check if your machine has TPM 2.0 using the Windows TPM utility (input "tpm.msc" in the Windows search bar):
    <img src="https://github.com/wxleong/tpm2-tbs/blob/develop-genesis/media/tpm-msc.png">
- Install IntelliJ IDEA [[2]](#2), community version will do

# Setup on Windows 10

Download Microsoft TSS project [[3]](#3):
```
$ git clone https://github.com/microsoft/TSS.MSR
$ git checkout d715b5959f0ff211eef59669240af914bedd3dee
```

Apply patch:
```
$ git clone https://github.com/wxleong/tpm2-tbs
$ git apply tpm2-tbs/patch/upgrade-Maven-dependencies-unit-test.patch
```

Launch IntelliJ IDEA and open the Maven project `TSS.MSR/TSS.Java`. The IDEA will resolve Maven dependencies, wait for it to complete.

# Operation

In IntelliJ IDEA, run the JUnit test in `TSSMainTests.java`.

Expected failures:
- AES operation will fail
- Hashing algorithm SHA384 will fail

# References

<a id="1">[1] https://www.infineon.com/cms/en/product/security-smart-card-solutions/optiga-embedded-security-solutions/optiga-tpm/</a><br>
<a id="2">[2] https://www.jetbrains.com/idea/</a><br>
<a id="3">[3] https://github.com/microsoft/TSS.MSR</a><br>

# License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.