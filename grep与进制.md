

command | 输出 | 注释
---     | ---  | --- 
echo 'a&&b'   \|grep -P --color -o '\x26+'        | ==> &&  | #16进制
echo 'a&&b'   \|grep -P --color -o '\046+'        | ==> &&  | #8进制 
echo '黑白'   \|grep -P --color -o "\xe9\xbb\x91" | ==> 黑  | #汉字(utf8)
echo '黑白'   \|grep -P --color -o $'\xe9\xbb\x91'   | ==> 黑
echo '中国aa' \|grep -P --color -o "[\x80-\xFF]+" | ==> 中国|(汉字编码编码范围)

- 按编码搜索, 只支持16、8进制
- 使用了 PCRE grep
- 汉字编码范围[\x80-\xFF]

最常用的范围是  [\u4e00-\u9fa5]  ( CJK Unified Ideographs )

java:   String regex = "[\\p{InCJK Unified Ideographs}&&\\P{Cn}]]";  #没有写死