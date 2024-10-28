# PGP Certificates

## Generate

[Generating a new PGP key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
```bash
gpg --full-generate-key
```

## List
```bash
gpg --list-keys
gpg --list-secret-keys
```


## Export
```bash
gpg --export-secret-keys -a <keyid> > private.asc
gpg --export -a <keyid> > public.asc
```

## Import
```bash
gpg --import private.asc
gpg --import public.asc
```
