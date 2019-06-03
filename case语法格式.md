```bash
case "$1" in
1)
    echo 'one'
    ;;
[2-9])
    echo 'one more';;
*)
    echo 'other'
    ;;
esac
```

- 最后一个分支可以省略;;   , 其他分支不可以省略
- ;;可以单独写在一行, 但是两个;中间不能有空格