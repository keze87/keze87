## Hi there 👋

I'm **César Ezequiel Herrera** (`keze87`).

Welcome to my GitHub profile. Below you can find my public cryptographic details and instructions on how to verify my digital signatures for documents, files, and Git commits.

---

## How to Verify My Signatures

### PDF Signing Certificate

- **Signer:** `CN=César Ezequiel Herrera` (`cesar.ezequiel.herrera@gmail.com`)
- **Valid:** `2026-11-19` to `2031-11-18`
- **SHA-256 Fingerprint:** `E5:4C:66:A7:D5:C1:67:FE:DE:27:64:53:8B:39:67:11:C6:EC:D1:71:12:27:44:37:58:86:1A:A8:1F:BE:B1:4B`

#### Verification Steps (PDF)

1. **In a PDF reader** (such as Adobe Acrobat Reader, Okular, or LibreOffice):
    - Open the **Signatures** / **Signature Panel** view.
    - Inspect the certificate details and verify that:
        - The signer is `CN=César Ezequiel Herrera` (`cesar.ezequiel.herrera@gmail.com`).
        - The signing date falls within `2026-11-19` to `2031-11-18`.
        - The **SHA-256 fingerprint** matches `E5:4C:66:A7:D5:C1:67:FE:DE:27:64:53:8B:39:67:11:C6:EC:D1:71:12:27:44:37:58:86:1A:A8:1F:BE:B1:4B`.
2. **From the command line with Python**:
   Install [`pyHanko`](https://pypi.org/project/pyHanko/):

    ```bash
    pip install pyHanko
    ```

    Then set `PDF_PATH` to your file and run:

    ```python
    import hashlib
    from pyhanko.pdf_utils.reader import PdfFileReader
    from pyhanko.sign.validation import validate_pdf_signature

    PDF_PATH = "path/to/document.pdf"
    EXPECTED_FP = "E5:4C:66:A7:D5:C1:67:FE:DE:27:64:53:8B:39:67:11:C6:EC:D1:71:12:27:44:37:58:86:1A:A8:1F:BE:B1:4B"
    expected_hex = EXPECTED_FP.replace(":", "").lower()

    with open(PDF_PATH, "rb") as f:
        reader = PdfFileReader(f)
        if not reader.embedded_signatures:
            print("FAILED: No signatures found in the PDF.")
        for sig in reader.embedded_signatures:
            status = validate_pdf_signature(sig)
            fp = hashlib.sha256(status.signing_cert.dump()).hexdigest()
            if status.intact and status.valid and fp == expected_hex:
                print(f"VALID ({sig.field_name}): Intact signature matching César Ezequiel Herrera ({EXPECTED_FP})")
            else:
                print(f"FAILED ({sig.field_name}): intact={status.intact}, valid={status.valid}, fp={fp}")
    ```

---

### PGP Public Key

- **User ID:** `Cesar Ezequiel Herrera <cesar.ezequiel.herrera@gmail.com>`
- **Fingerprint:** `2654 8ABC 59B5 5BC4 F492 BD96 7428 A10A 4669 1632`
- **Algorithm:** `ed25519` (signing) / `cv25519` (encryption)
- **Public Key URL:** <https://github.com/keze87.gpg>

<details>
<summary>PGP Public Key Block</summary>

```text
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDMEaXDp/RYJKwYBBAHaRw8BAQdAy0t2nQ8Pbebzc0pMftH+K5VMVSZfbpMd1DN1
0UAFb960OUNlc2FyIEV6ZXF1aWVsIEhlcnJlcmEgPGNlc2FyLmV6ZXF1aWVsLmhl
cnJlcmFAZ21haWwuY29tPoiQBBMWCgA4FiEEJlSKvFm1W8T0kr2WdCihCkZpFjIF
Amlw6f0CGwMFCwkIBwIGFQoJCAsCBBYCAwECHgECF4AACgkQdCihCkZpFjKcYQEA
ntnA5t7f3O9qvOacQX1QpMXUn37glzfVqGU4mVQczQsA/3XUc5ncIG8zTI1EGadv
AvGqrTYjMX5yeCyI6xP5aEoAuDgEaXDp/RIKKwYBBAGXVQEFAQEHQH+Ukwwor8AO
1NxQxwEtBcYqJH2SDEWLFAVSyRLhlbVeAwEIB4h4BBgWCgAgFiEEJlSKvFm1W8T0
kr2WdCihCkZpFjIFAmlw6f0CGwwACgkQdCihCkZpFjKTzAD+KR/jRlogNudoPUg5
Yo/ATTBLScoUjkLkG+e5ua1qqnUA/jXEof6JBvcw4SiHh4naKtipFXRjxmHWstkM
x2gJ36MD
=GMyy
-----END PGP PUBLIC KEY BLOCK-----
```

</details>

#### Verification Steps (PGP)

1. **Import the public key** directly from GitHub or by saving the block above to `keze87.asc` (requires GnuPG / [Gpg4win](https://www.gpg4win.org/) on Windows):

    ```bash
    # Linux / macOS
    curl -sL https://github.com/keze87.gpg | gpg --import

    # Windows: curl.exe -sL https://github.com/keze87.gpg | gpg --import

    # Or, from a saved local file:  gpg --import keze87.asc
    ```

2. **Confirm the fingerprint**:
    ```bash
    gpg --fingerprint 26548ABC59B55BC4F492BD967428A10A46691632
    ```
3. **Verify a signed file or Git commit/tag** (confirm the output shows `Good signature` and `Primary key fingerprint: 2654 8ABC 59B5 5BC4 F492 BD96 7428 A10A 4669 1632`):

    ```bash
    # Detached signature
    gpg --verify file.sig file

    # Clearsigned / inline message
    gpg --verify message.asc

    # Git commit or tag
    git verify-commit <commit-hash>
    git verify-tag <tag-name>
    ```
