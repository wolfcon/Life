---
title: Xcode 在代码中为注释额外增加⚠️
date: 2021-9-23
---

# Xcode 在代码中为注释额外增加⚠️

## Add a Run Script build phase

```sh
# TAGS is comment keywords. Here is "TODO: and FIXME:"
TAGS="TODO:|FIXME:"
echo "searching ${SRCROOT} for ${TAGS}"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TAGS).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/"

```

Use `//TODO:` or `//FIXME:` in code 🎉

