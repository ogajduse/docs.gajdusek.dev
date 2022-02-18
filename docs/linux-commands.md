# Linux commands

This document contains mainly brain dumps created at the moment I struggled to
find the right command or set of commands to perform the desired action.
Description is mainly in in Czech language, however the goal is to rewrite it to
English.

## YUM/DNF, RPM

Downgrades the specified packages to the highest of all known lower versions if
possible. When version is given and is lower than version of installed package
then it downgrades to target version

```sh
yum distribution-synchronization
```

Zobrazeni dostupnych repozitaru, repozitare zobrazeni

```sh
yum repolist all
```

Ktery balik nabizi prikaz gcc

```sh
dnf provides '*/gcc'
```

Downgradovat na starsi verze balicku

```sh
dnf downgrade jmock-1.2.0
```

Hledani nainstalovaneho baliku

```sh
rpm -q jmock
```

zobrazi starsi verze balicku, package old version

```sh
dnf list jmock --showduplicates
```

Zapnout repozitar aktivovat, enable repository

```sh
dnf config-manager --set-enabled fedora-debuginfo
```

Install missing dependencies for building an RPM package

```sh
yum-builddep spacewalk-java-2.6.25-1.git.76.aede25c.fc24.src.rpm
```

Instalace baliku

```sh
rpm -ivh http://yum.spacewalkproject.org/nightly/Fedora/23/x86_64/spacewalk-repo-2.5-3.fc23.noarch.rpm
```

Instalovat specifickou verzi balicku
<http://unix.stackexchange.com/questions/151689/how-can-i-instruct-yum-to-install-a-specific-version-of-package-x>

```sh
yum --showduplicates list httpd
yum install <package name>-<version info> #nekdy pozaduje i verzi
```

Porovnani obsahu repozitaru, compare content repository...more in man

```sh
repoclosure -l beaker-Client -l epel -r spacewalk-client-nightly
```

Najit dependence k rpm, find dependencies for specific rpm

```sh
rpm -qpR checkpolicy-2.0.22-1.el6.x86_64.rpm
```

Zobrazit requires

```sh
rpm -qR python3-hwdata
```

Ziskat informace o nenainstalovanem baliku

```sh
rpm -qip foo.rpm
```

Ziskat informace o nenainstalovanem baliku

```sh
rpm -qi python3-dbus
```

Zobrazit seznam nainstalovanych baliku

```sh
rpm -qa
yum list installed
```

Smazat balik bez dependenci, erase, delete package without dependencies

```sh
rpm -e --nodeps "php-sqlite2-5.1.6-200705230937"
```

Zjistit ktery balik vlastni dany soubor, query package owning specified file

```sh
rpm -qf /sbin/rhnreg_ks
```

Ukazat requires pro nenainstalovany balik

```sh
rpm -qp --requires python3-hwdata-1.10.1-6.fc24.noarch.rpm
rpm -qpR python3-hwdata-1.10.1-6.fc24.noarch.rpm
```

Ukazat requires pro nainstalovany balik

```sh
rpm -qR <package>
dnf repoquery --requires vim-enhanced
yum deplist <package>
```

Co dany balik poskytuje

```sh
repoquery --list rhnlib
```

To see which additional RPM packages are needed to satisfy the dependencies

```sh
repoquery --requires --resolve <package>
```

Historie instalaci daneho baliku

```sh
yum history package-list rhnlib*
```

yum log, hledani v logu instalaci

```sh
cat /var/log/yum.log |grep rhnlib
```

Zobrazi dodatecne informace k baliku, additional info about package

```sh
yumdb info package_name
yum info package_name…
```

Rozbalit rpm bez instalace

```sh
rpm2cpio checkpolicy-1.33.1-6.el5.x86_64.rpm | cpio -idmv
```

Instalace epel repa, installation of epel repository

```sh
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
rpm -Uvh epel-release-6*.rpm
```

Zapnuti repa subscription-manager

```sh
subscription-manager repos --enable=rhel-$(rpm -E '%{rhel}')-server-optional-rpms
```

## VIM

Hledani v man manpage, manual, search (stisknout "/" a psat hledany vyraz) Dalsi
vysledek -> stisknout "n", predchozi vysledek "N"

replace, nahrazeni textu `:%s/\<foo\>/bar/gc` - Change only whole words exactly
matching 'foo' to 'bar'; ask for confirmation. `:%s/foo/bar/g` - Find each
occurrence of 'foo' (in all lines), and replace it with 'bar'.

## GIT

Zobrazeni rozdilu

```sh
git diff
```

Pridani verze v interaktivnim modu

```sh
git add -i spacecmd/src/lib/kickstart.py
```

Rozdily v pripravenem ke commitu

```sh
git diff --cached #--cached == --staged
```

Potvrzeni zmen

```sh
git commit
```

Zmena jmena v global configu

```sh
git config --global user.name 'Ondrej Gajdusek'
```

Modifikovani autora posledniho commitu

```sh
git commit --amend --author='Ondrej Gajdusek <ogajduse@redhat.com>'
```

Modifikovani posledniho commitu - vsech souboru

```sh
git commit --amend -a
```

Ziskani obsahu remote repozitare

```sh
git pull --rebase
```

Pushnuti vetve na github??

```sh
git push --set-upstream origin setsoftwaredetails
```

master==origin, revert posledniho commitu, --hard param. Zpusobi uplnou ztratu
commitu/snimku

```sh
git reset --hard HEAD~1
```

Revert posledniho commitu, commit ponecha obsah v casti non staged

```sh
git reset HEAD~1
```

Reset vetve na commit 0d1d

```sh
git reset --hard 0d1d7fc32
```

Situace, kdy je remote branch napred oproti lokalni, provedu hard reset na
starsi commit, remote je ted ahead, musim zmenit FETCH_HEAD

```sh
git reset FETCH_HEAD --hard
```

Hledani v gitu, git hledani, git search,

```sh
git grep kickstart_
```

Hledani retezec obsahujici RHN v souborech s priponou .java

```sh
git grep '*.* RHN' -- '*.java'
```

Pushnuti vetve na GitHub, ignoruje konflikty

```sh
git push --force
```

Zobrazi remote repozitare, URL, kterou má Git uloženou pro zkrácený název, který
si přejete rozepsat

```sh
git remote -v
```

Cherry pick commitu

```sh
git cherry-pick
```

Vetev neni na stejne urovni jako master, udelej ->

```sh
git rebase master
```

<http://stackoverflow.com/questions/52704/how-do-you-discard-unstaged-changes-in-git>
How do I discard changes in my working copy that are not in the index? For a
specific file use:

```sh
git checkout path/to/file/to/revert
```

For all unstaged files use:

```sh
git checkout -- .
```

Make sure to include the period (dot) at the end.

Revert specific file, revert one file
<https://stackoverflow.com/questions/215718/reset-or-revert-a-specific-file-to-a-specific-revision-using-git>

```sh
git checkout c5f567 -- file1/to/restore file2/to/restore
```

Delete untracked files, smazani netrackovanych/neverzovanych souboru

```sh
git clean -n # zobrazi, co se smaze
git clean -f # smaze
```

Smazani tagu

```sh
git tag -d spacecmd-2.6.7-1
```

Smazani tagu z remote, delete remote tag

```sh
git push origin :refs/tags/spacecmd-2.6.7-1
```

gitk refresh - `Shift+F5`

Commit pri kterem se pri editovani souboru vypise nejen status ale i diff

```sh
git commit -v
```

Git do revize automaticky zahrne každý soubor, který je sledován Zcela tak
odpadá potřeba zadávat příkaz git add

```sh
git commit -a
```

Zobrazí rozdíly diff provedené v každé revizi

```sh
git log -p
```

Zobrazi posledni 2 commity

```sh
git log -2
```

Zobrazí některé stručné statistiky pro každou revizi

```sh
git log --stat
```

Zobrazi logy od nebo do urciteho data

```sh
git log --until #--since
```

Hledani v logu

```sh
git log --all-match #--author, --grep, --commiter
```

Stylovani logu, vypis pouze commit message -
<https://www.kernel.org/pub/software/scm/git/docs/git-log.html#_pretty_formats>

```sh
git log --pretty=format:"%s"
```

Odstrani referenci na vzdaleny repozitar

```sh
git remote rm <název_repozitare>
```

Zobrazeni urcite mnoziny tagu

```sh
git tag -l 'spacecmd*'
```

Pridani anotovane znacky

```sh
git tag -a v0.1 -m 'alpha verze'
```

Dodatecne pridani tagu k existujicimu commitu

```sh
git tag -a v1.2 9fceb02
```

Delete tag from remote, delete remote tag, smazat vzdaleny tag -
<https://nathanhoad.net/how-to-delete-a-remote-git-tag>

```sh
git tag -d 12345
git push origin :refs/tags/12345
```

Pushnuti znacky na remote repo

```sh
git push origin [název značky]
```

Pushnuti vsech znacek na remote repo

```sh
git push origin --tags
```

Pridani aliasu pro prikazy git xxx

```sh
git config --global alias.co checkout
git config --global alias.ci commit
```

Zobrazi posledni commit/revizi na kazde vetvi

```sh
git branch -v
```

Které větve už byly začleněny do větve, na níž se nacházím

```sh
git branch --merged
```

Zobrazit větve, které obsahují dosud nezačleněnou práci

```sh
git branch --no-merged
```

Prejmenovat vetev If you want to rename a branch while pointed to any branch,
do:

```sh
git branch -m <oldname> <newname>
```

If you want to rename the current branch, you can do:

```sh
git branch -m <newname>
```

Smazat vetev na remote repozitari

```sh
git push origin :the_remote_branch
```

Pushnuti zmen, konkretne tagu

```sh
git push origin :refs/tags/spacewalk-java-2.7.12-1
```

Forked repo, forknute repo, jak udrzet updated s upstreamem -
<https://gist.github.com/CristinaSolana/1885435>

How to merge two branches in different repositories, local repositories -
<http://stackoverflow.com/questions/3402599/how-do-you-merge-two-git-branches-that-are-in-different-local-repos-folders>

```sh
# switch to repo A

cd folder_a

# Add repo B as a remote repository
git remote add folderb /path/to/folder_b

# Pull B's master branch into a local branch named 'b'
git pull folderb master:b

# Merge b into A's master branch
git merge b

# Switch to repo B
cd ../folder_b

# Add repo A as a remote repository
git remote add foldera /path/to/folder_a

# Pull the resulting branch into repo B's master branch
git pull foldera master:master
```

Reference first commit, initial commit, prvni commit

```sh
git rev-list --max-parents=0 HEAD
```

Moving one directory from repoA to repoB with history -
<http://gbayer.com/development/moving-files-from-one-git-repository-to-another-preserving-history/>

Move tag, presunout tag na jiny commit -
<http://stackoverflow.com/questions/8044583/how-can-i-move-a-tag-on-a-git-branch-to-a-different-commit>

Find all repositories with uncommited changes.

```bash
locate -r "\.git$" | sed 's/\.git$//' | grep -v cache | xargs -I _ sh -c \
'if [ -n "$(git -C _ status --porcelain)" ]; then echo Changes in _ ;fi;'
```

## Koji

regenerace repa

```sh
koji regen-repo <buid tag>
```

Build baliku pro specificky build target

```sh
koji build spacewalk-nightly-fedora25 git://git@github.com/spacewalkproject/spacewalk.git/#spacewalk-branding-2.7.2-1
```

## BEAKER

Prodlouzeni casu u beaker masiny

```sh
# posledni cislo je cislo tasku u daneho jobu
bkr watchdog-extend --by $( expr 3600 \* 24 \* 9 ) 43476098
```

Extend test time

```sh
rhts-test-checkin 127.0.0.1:7086 bkrsystemfqdn.com 1400356 \
distribution/reservesys 120h 42970190
```

## SED

Replace text in file sed, replace link sed -
<http://stackoverflow.com/a/1583282/6239929>

Strip package version from NEVRA to get only package name

```sh
sed -e 's/\([^.]*\).*/\1/' -e 's/\(.*\)-.*/\1/'
```

Smazani radku se specifickym obsahem, remove line with specific contents -
<http://stackoverflow.com/questions/5410757/delete-lines-in-a-text-file-that-containing-a-specific-string>

To remove the line and print the output to standard out:

```sh
sed '/pattern to match/d' ./infile
```

To directly modify the file:

```sh
sed -i '/pattern to match/d' ./infile
```

To directly modify the file (and create a backup):

```sh
sed -i.bak '/pattern to match/d' ./infile
```

For Mac OS X users:

```sh
sed -i '' '/pattern/d' ./infile
```

## Other

Odstraneni slozky i s obsahem It deletes all files and folders contained in the
lampp directory.

```sh
rm -rf lampp
```

Zobrazit vypis prikazu , ktere bezi ve scriptu, command script

```sh
set -i
```

Ukonci script skript, kdyz neco failne

```sh
set -e
```

Zmena barvy fontu promptu -
<http://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/>

Barvy v bashi - <http://misc.flogisoft.com/bash/tip_colors_and_formatting>

NetworkManager text user interface (TUI)

```sh
nmtui
```

Zobrazi posledni cast souboru. Output the last part of files

```sh
tail
```

Silence user input, heslo password zadani hesla

```sh
read -s
```

Hledani souboru, search file

```sh
locate
```

Updatovani databaze pro prikaz locate, update a database for locate

```sh
updatedb
```

Zjisteni rozdilnosti mezi dvemi soubory

```sh
diff s1 s2
```

Zobrazeni prvniho radku souboru

```sh
head -1 file
```

Zabit proces v cmd - ctrl+D

Prenos souboru pres ssh -
<https://docs.fedoraproject.org/en-US/Fedora/12/html/Deployment_Guide/s2-openssh-using-scp.html>

```sh
scp testscr.sh root@your.awesome.system.com:~/
```

Prenos vice souboru z local stanice do remote

```sh
scp /home/ogajduse/spacewalk/spacecmd/src/lib/{utils,misc,system}.py root@your.awesome.system.com:/usr/lib/python2.6/site-packages/spacecmd/
```

Jak zjistit ssh klic Pokud ho mas vygenerovany, tak:

```sh
$ cat ~/.ssh/id_rsa.pub
# Pokud ho nemas vygenerovany, tak:
$ ssh-keygen
```

SSH permissions for id\_\* files -
<http://stackoverflow.com/questions/9270734/ssh-permissions-are-too-open-error>

SSH client config file - <https://linux.die.net/man/5/ssh_config>

Zmenit heslo soukromeho klice ssh, change ssh key password -
<http://www.cyberciti.biz/faq/howto-ssh-changing-passphrase/>

Which distribution am I using, linux version, release -
<https://superuser.com/questions/80251/how-to-know-which-linux-distribution-im-using?newreg=c51cdaabc57b48dc86a555101d0f6471>

Zjisteni verze RHELu, verzi rhel zjistit

```sh
lsb_release -i -r
```

Sekla sessiona ssh

> enter tilda tecka

Kopirovani id na remote

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub root@ts.unitedclans.tk
```

Zobraz volne misto na disku, free disk space

```sh
df -h
```

Reloadovat bashrc config bez odhlaseni

```sh
source ~/.bashrc
```

Zjistit typ souboru, filetype, file type

```sh
file ./file
```

Konverze timestamp conversion to time

```sh
date -d @<timestamp>
```

Zjistit plnou cestu prikazu (shell), show the full path of (shell) commands

```sh
which spacewalk-service
type -p spacewalk-service
```

List block devices, vylistovat blokova zarizeni

```sh
lsblk
```

Package cache cleanup (PackageKit, packagekit cache removal) -
<https://unix.stackexchange.com/questions/265755/fedora-23-can-i-safely-delete-files-in-var-cache-packagekit-metadata-updates>

```sh
pkcon refresh force -c -1
```

Run tar, gzip on all cores, pigz -
<https://stackoverflow.com/questions/12313242/utilizing-multi-core-for-targzip-bzip-compression-decompression>

```sh
tar -c --use-compress-program=pigz -f tar.file dir_to_zip
```

Decompress via pigz

```sh
pigz -dc target.tar.gz | tar xf -
```
