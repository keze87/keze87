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
2. **From the command line** (Linux with `poppler-utils` and `openssl`):
    Set `PDF` to the path of the file you want to verify (`pdfsig -dump` runs inside a temporary directory so existing files in your working directory are untouched):
    ```bash
    PDF="/path/to/document.pdf"
    EXPECTED_FP="E5:4C:66:A7:D5:C1:67:FE:DE:27:64:53:8B:39:67:11:C6:EC:D1:71:12:27:44:37:58:86:1A:A8:1F:BE:B1:4B"

    # 1. Verify signature integrity and signer name
    out=$(LC_ALL=C pdfsig -nocert "$PDF")
    if grep -q "Signature is Valid." <<<"$out" && grep -q "César Ezequiel Herrera" <<<"$out"; then
      echo "VALID: Signature is intact and signed by César Ezequiel Herrera"
    else
      echo "FAILED: Invalid signature or signer mismatch"
    fi

    # 2. Verify the embedded certificate's SHA-256 fingerprint
    PDF_ABS=$(realpath "$PDF")
    tmp=$(mktemp -d)
    (
      cd "$tmp" || exit 1
      LC_ALL=C pdfsig -dump "$PDF_ABS" >/dev/null
      if openssl pkcs7 -inform DER -in * -print_certs \
        | openssl x509 -noout -fingerprint -sha256 \
        | grep -qi "$EXPECTED_FP"; then
        echo "VALID: Certificate SHA-256 fingerprint matches"
      else
        echo "FAILED: Certificate SHA-256 fingerprint mismatch"
      fi
    )
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

1. **Import the public key** directly from GitHub or by saving the block above to `keze87.asc`:
    ```bash
    curl -sL https://github.com/keze87.gpg | gpg --import

    # or, from a local file: gpg --import keze87.asc
    ```
2. **Confirm the fingerprint**:
    ```bash
    gpg --fingerprint 26548ABC59B55BC4F492BD967428A10A46691632
    ```
3. **Verify a signed file or Git commit/tag**:

    ```bash
    # Detached signature
    gpg --verify file.sig file

    # Clearsigned / inline message
    gpg --verify message.asc

    # Git commit or tag
    git verify-commit <commit-hash>
    git verify-tag <tag-name>
    ```
