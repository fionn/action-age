# Encrypt or Decrypt Files in CI

A simple action to asymmetrically encrypt or decrypt a file.

Primary usecase: sharing data between jobs (as e.g. an artifact or cached file) and needing to keep the content confidential.

Under the hood, it uses [`age`](https://github.com/FiloSottile/age). This is hopefully only an implementation detail, except for key generation. It requires an `apt`-compatible runner for this.

## Usage

Generate a keypair with `age-keygen`.

### Inputs

* `path`
  * The path to the plaintext file, either as the input file to encrypt, or as the output file to decrypt, where in the latter case we expect the ciphertext file to exist under `path` suffixed by `.age`.
* `verb`
  * The action, either `encrypt` or `decrypt`.
* `key`
  * The public or private key, depending on `verb`.

### Outputs

* `path`
  * The relative path to the output file.

### Example

```yaml
- name: Encrypt test file
  id: encrypt
  uses: fionn/action-age@master
  with:
    path: example.txt
    verb: encrypt
    key: ${{ vars.public_key }}

- name: Decrypt test file
  id: decrypt
  uses: fionn/action-age@master
  with:
    path: example.txt
    verb: decrypt
    key: ${{ secrets.private_key }}
```
