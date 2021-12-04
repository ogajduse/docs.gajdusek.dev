# How to get the latest packages from YUM repository

Once I was working on an overengineered automation, I came across a problem of
downloading the latest packages from a YUM repository. Of course, users can
achieve that by defining the repository in `/etc/yum.repos.d` and then run
`yum download the-desired-package`. Sure, that is a way to go, but I decided to
go the harder way and play with the repository metadata.

Every YUM repository has a _repodata_ directory that contains multiple files.
Two of them I was interested in were _repomd.xml_ and _\<hash\>-primary.xml.gz_.
The first one - _repomd_ is basically an index of the repodata directory. It
contains checksums, location, timestamp, and size of every file in the repomd
directory. Then the _primary_ is a list of packages contained in the repository.
Every package in the list contains multiple attributes including their name and
version. Knowing that we are able to programmatically get the latest package
from the repository.

The only thing we need to know to be able to do it is a _baseurl_ of the
repository. Below is the example that is taken from the
[Gremlin's documentation](https://www.gremlin.com/docs/infrastructure-layer/updating-gremlin/).
It is a small Bash script that does open the repomd and searches for the name of
the primary file. Then it opens the primary file and looks for the exact version
of the package.

<!-- markdownlint-disable MD013 -->

```bash
#!/bin/bash
## Download a Gremlin RPM without using yum
##
## requires: curl, xpath, gzip
##
VERSION=$(curl https://rpm.gremlin.com/noarch/latest | cut -d- -f1)
## Or optionally set a specific version
# VERSION=2.16.3
curl -fsSL https://rpm.gremlin.com/noarch/$(\
    curl -fsSL https://rpm.gremlin.com/noarch/$(\
        curl -fsSL https://rpm.gremlin.com/noarch/repodata/repomd.xml \
            | xpath -e 'string(/repomd/data[@type="primary"]/location/@href)' 2>/dev/null) \
        | gunzip \
        | xpath -e 'string(/metadata/package[version/@ver="${VERSION}" and name="gremlin"][last()]/location/@href)' 2>/dev/null) \
    -o gremlin-${VERSION}.x86_64.rpm
curl -fsSL https://rpm.gremlin.com/noarch/$(\
    curl -fsSL https://rpm.gremlin.com/noarch/$(\
        curl -fsSL https://rpm.gremlin.com/noarch/repodata/repomd.xml \
            | xpath -e 'string(/repomd/data[@type="primary"]/location/@href)' 2>/dev/null) \
        | gunzip \
        | xpath -e 'string(/metadata/package[version/@ver="${VERSION}" and name="gremlind"][last()]/location/@href)' 2>/dev/null) \
    -o gremlind-${VERSION}.x86_64.rpm
```

<!-- markdownlint-enable MD013 -->

However, the aim of my work was to get the latest package programmatically.
Luckily, the XPath contains a `last()` function that returns the last item in
the list. Here is how I used it in Ansible.

```yaml
- name: Get the package location from the primary file
 xml:
   path: "{{ get_primary.dest.split('.')[:-1] | join('.') }}"
   namespaces:
     rpm: http://linux.duke.edu/metadata/repo
     common: http://linux.duke.edu/metadata/common
   content: attribute
   xpath: /common:metadata/common:package[common:arch="noarch"
    and common:name="{{ item }}"][last()]/common:location
 register: primary_parse
 delegate_to: 127.0.0.1
 run_once: true
 loop: "{{ rh_internal_packages }}"
```

In the end, I get the location of the packages in the repository. If `baseurl`
and `location` of the package are joined together, you will get the URL from
which you can download the desired package.

Other sources that helped me to accomplish my work:

<https://lago.readthedocs.io/en/0.15/_modules/ovirtlago/repoverify.html>

<https://devhints.io/xpath>
