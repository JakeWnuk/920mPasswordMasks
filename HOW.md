Dumped 958m passwords from repository
```
$ wc -l plain-passwords.lst
958104442 plain-passwords.lst
```

Remove low quality entries
```
$ cat plain-passwords.lst | grep -vE 'http:\/\/https:\/\/|\@\.com|\@\.ru|\@\.cn|\@\.org|\@*\.net|<tr>|<div>|<a href|<p>|<img src|\$HEX\[|fbobh_|\@mail|\@msn|\@aol|\@go|\@yahoo|\@gmail|\@hotmail' | grep -v '[^[:print:]]' > prepped-passwords.lst
```
```
$ wc -l prepped-passwords.lst
924073743 prepped-passwords.lst
```

Put through [maskcat](https://github.com/jakewnuk/maskcat) to make password masks
```
$ cat prepped-passwords.lst | maskcat > mask-passwords.lst
```
```
$ wc -l mask-passwords.lst
920747753 mask-passwords.lst
```

Aggregate masks
```
$ cat mask-passwords.lst | sort -T ./ | uniq -c | sort -T ./ -rn > sorted-mask-passwords.lst
```

Filter masks by complexity
```
$ cat clean-sorted-mask-passwords.lst | grep -E ':3$|:4$' > clean-sorted-3to4-complexity-mask-passwords.lst

$ cat clean-sorted-mask-passwords.lst | grep -vE ':3$|:4$' > clean-sorted-1to2-complexity-mask-passwords.lst
```

Filter masks by length
```
$ cat clean-sorted-3to4-complexity-mask-passwords.lst | grep -vE ':1|:2|:5|:6|:7' > clean-sorted-3to4-complexity-ge8-len-mask-passwords.lst

$ cat clean-sorted-1to2-complexity-mask-passwords.lst | grep -vE ':3|:4|:5|:6|:7' > clean-sorted-1to2-complexity-ge8-len-mask-passwords.lst
```
