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

## Fedora package update

```sh
export PKG_NAME=apypie
export GH_OWNER=Apipie
export NEW_VER=0.3.2
export VER_PREFIX=v

cat << EOF
# Rawhide
fedpkg switch-branch rawhide
fedpkg pull --rebase
wget https://github.com/$GH_OWNER/$PKG_NAME/archive/$VER_PREFIX$NEW_VER.tar.gz \
    -O /tmp/$PKG_NAME-$NEW_VER.tar.gz
fedpkg new-sources /tmp/$PKG_NAME-$NEW_VER.tar.gz
rpmdev-bumpspec *.spec --comment "Update to $PKG_NAME $NEW_VER" --new $NEW_VER
fedpkg commit -m "Update to $PKG_NAME $NEW_VER"
fedpkg push
fedpkg build

# Fedora 34
fedpkg switch-branch f34
git merge rawhide
fedpkg push
fedpkg build
fedpkg update --notes "Update to $PKG_NAME $NEW_VER" --bugs <bug-id> \
    --request testing --stable-karma 2 --unstable-karma '-2' --type enhancement
EOF
```
