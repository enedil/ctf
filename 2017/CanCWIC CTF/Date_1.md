We are given a website that lets us add a format string that is passed to `date` command.

First what we see is a banner:

> Proudly coded with Vim editor

We check address `http://159.203.38.169:5675/.index.php.swp` and get some version of `index.php` file. Here we see that there is a file `varflag.php` which holds a flag.

Back to the form. The command we send is escaped, but as a whole, so if we could interfere with outside world with `date` arguments only, we could probably do something. Reading manual tells that option `-f` allows to read format strings from file. We send ` -f varflag.php` (space is to separate format string from the `-f` argument), view source (so that `<?php` doesn't get rendered) and get flag:

```
date: invalid date '<?php'
date: invalid date '$date = "Sunday April 1st 2018. 2018-04-01";'
date: invalid date '$easy_flag = "FLAG{Want Love? Don\'t Date!}";'

date: invalid date '?>'
```

