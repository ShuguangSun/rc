# Passwords

<https://wiki.gentoo.org/wiki/Password_management_tools>

# pwgen

<https://wiki.gentoo.org/wiki/Password_management_tools#pwgen>

## Range from 5 to 9 length

    pwgen $(( 5+(`od -An -N2 -i /dev/random` )%(9-5+1) )) 1

## To file

    pwgen --numerals --secure --no-capitalize 10 80000 \
      | tr '[:lower:]' '[:upper:]' > 80k_upper_201210091031

## Special characters

    pwgen --symbols

## Test for duplicates

<http://stackoverflow.com/questions/6447473/linux-command-or-script-counting-duplicated-lines-in-a-text-file#6447515>

    cat 80k_upper_20120723 20k_upper_201210091031  \
      | sort | uniq -c | grep ' 1 ' --invert-match

# makepasswd

<https://wiki.gentoo.org/wiki/Password_management_tools#makepasswd>

## Range from 5 to 9 length

    makepasswd --maxchars 5 --minchars 9

## Numerical-only exactly 4 digitis

    makepasswd --chars 4 --string 1234567890

# Other

    date +%s | sha256sum | base64 | head -c 32
