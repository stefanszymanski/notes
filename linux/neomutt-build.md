# Build neomutt from sources

Either repositories contain old versions or the latest bugfixes and
improvements I want aren't released yet.
Therefore I prefer to build neomutt from the master branch.

```sh
# checkout the project
if [ -d ~/build/neomutt ]; then
    mkdir -p ~/build && cd ~/build || exit 1
    git clone git@github.com:neomutt/neomutt.git
    cd neomutt || exit 1
fi

# install dependencies
sudo xbps-install -S gpgme-devel libsasl-devel lua-devel libnotmuch-devel libidn-devel tokyocabinet-devel mit-krb5-devel

# build neomutt
./configure \
    --build=x86_64-linux-gnu \
    --prefix=/usr \
    {--includedir=${prefix}/include} \
    {--mandir=${prefix}/share/man} \
    {--infodir=${prefix}/share/info} \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --disable-silent-rules \
    {--libdir=${prefix}/lib/x86_64-linux-gnu} \
    {--libexecdir=${prefix}/lib/x86_64-linux-gnu} \
    --disable-maintainer-mode \
    --disable-dependency-tracking \
    --libexecdir=/usr/lib \
    --with-mailpath=/var/mail \
    --gpgme \
    --lua \
    --notmuch \
    --with-ui \
    --gnutls \
    --gss \
    --idn \
    --mixmaster \
    --sasl \
    --tokyocabinet
make
sudo make install

# create a directory for the header cache
mkdir -p ~/.cache/neomutt/headers
```
