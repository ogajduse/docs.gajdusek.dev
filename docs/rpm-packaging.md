# RPM packaging, Fedora packaging

Parse/evaluate specfile

```sh
rpmspec -P python-apypie.spec
```

Download sources from specfile

```sh
spectool --all python-apypie.spec --get-files
```

Mock build from srpm

```sh
mock -r fedora-30-x86_64 rebuild python-apypie-0.2.1-2.fc32.src.rpm
```
