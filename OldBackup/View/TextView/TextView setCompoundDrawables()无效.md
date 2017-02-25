## TextView setCompoundDrawables()无效

[TOC]

#### 还需要设置 Bound
通常代码:
```
mUserName.setCompoundDrawables(null,null,rankDrawable,null);
```

实际还需要设置 `Drawable`的`Bound`完整代码如下:

```
	Drawable rankDrawable=null;
        if ("1".equals(fx_level)) {
            rankDrawable=getResources().getDrawable(R.drawable.ic_rank_3);
        } else if ("2".equals(fx_level)) {
            rankDrawable=getResources().getDrawable(R.drawable.ic_rank_2);
        } else if ("3".equals(fx_level)) {
            rankDrawable=getResources().getDrawable(R.drawable.ic_rank_1);
        }
        if (rankDrawable!=null){
            rankDrawable.setBounds(0, 0, rankDrawable.getMinimumWidth(), rankDrawable.getMinimumHeight());//需要配置
            mUserName.setCompoundDrawables(null,null,rankDrawable,null);
        }
```