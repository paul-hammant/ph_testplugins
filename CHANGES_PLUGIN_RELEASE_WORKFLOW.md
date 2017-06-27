# Advice for 'maven-changes-plugin' releasing

## To make the branch

```
mvn clean
git branch maven-changes-plugin-only
git checkout maven-changes-plugin-only
find . -mindepth 1 -maxdepth 1 -type d | sed '/maven-changes-plugin/d' | sed '/.git/d' | xargs git rm -r
git commit -am "changes plugin only"
git mv maven-changes-plugin/src .
git mv -f maven-changes-plugin/pom.xml .
git commit -am "changes plugin only" --amend
git push --all
```


# Where should developers do regular dev work?

On the master branch, which is guarded by CI

```
git checkout master
# edit, build, commit, push as normal
```

# Release workflow

```
mvn clean
git checkout maven-changes-plugin-only
git merge master
git status --porcelain | grep '^DU' | cut -d' ' -f2 | xargs git rm -rf
git commit -m "merge in advance of release"
```

Now, from here do the release plugin operations as normal

Note that changes to the versions in the POM that the release plugin makes are
not pushed back to the master branch by any process.

TODO: decide workflow for versions
