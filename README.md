get the repos, take whats avail for the specified dist
```
curl https://keyserver.opensuse.org/repositories/ | grep tr | tr '>' '\n'  | grep href | tr '"' ' ' | awk '{print $3}' | grep ":" | sort | uniq | tr -d ':' | tr -d '.' | tr -d '/' | awk '{print "https://keyserver.opensuse.org/repositories/"$1"/openSUSE_Leap_15.0/"$1".repo"}' | parallel curl --fail {} -O -J
```
delete old repos

`zypper rr -a `


add repos 
`ls -1 | xargs -i -t zypper ar -r {} -c -f -K --gpgcheck-strict `

keys (export)
```
for x in $(sudo find /var/tmp/zypp.*  | grep ".gpg" | xargs -i -t sudo dirname {}); do for y in $(sudo gpg --no-default-keyring --trustdb $x/trustdb.gpg 
--primary-keyring $x/pubring.kbx --list-keys --with-colons | awk -F: '/^pub:/ { print $5 }'); do
sudo gpg --no-default-keyring --trustdb $x/trustdb.gpg --primary-keyring $x/pubring.kbx --armor --export $y > $y.asc ; sudo gpg --no-default-keyring --trustdb 
$x/trustdb.gpg --primary-keyring $x/pubring.kbx --export-ownertrust > $y.otrust.txt;
done
done

```

updating (not a trivial thing)

```
find . -type f | xargs -i -t sed -i 's/13.2/15.0/g' {}
```


package list

```
rpm -q -a --qf '%{NAME} ' > xps_9360.package.list   
```
