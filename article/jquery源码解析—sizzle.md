# jQuery 源码解析 -- sizzle选择器

### sizzle 为jQuery自带的选择器引擎，也是jQuery中的重要组成。


##### 空格符
	whitespace = "[\\x20\\t\\r\\n\\f]"

1. \x20 为空格符
2. \t 为制表符 (tabs)
3. \r 为回车符 (Carriage Return)
4. \n 为换行符 (Line Feed)
5. \f 为换页符 (Form feed)