##各种定位

####一 绝对定位元素垂直水平居中

1 在知道元素具体大小时，此方法也可用于水平居中

	position:absolute;
	left:50%;
	top:50%;
	margin-left:-50px;
	margin-top:-50px;

2 不知道具体元素大小

	position:absolute;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
	margin:auto;
