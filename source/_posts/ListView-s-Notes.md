title: "ListView's Notes"
date: 2015-04-23 22:23:48
tags: ListView
---
###焦点

Item 中的子控件抢走了 Item 的焦点：

    //在初始化Item的时候
    setDescendantFocusabilityViewGroup.FOCUS_BLOCK_DESCENDANTS);
###添加 headview footview

- addHeaderView(headView, null, false) (false 设置header不可被选择)
- addHeaderView(headView)
>添加header时调用的addHeaderView方法必须放在listview.setadapter前面*(KITKAT之前的要求)*，意思很明确就是如果想给listview添加头部则必须在给其绑定adapter前添加，否则会报错。原因是当我们在调用setAdapter方法时会android会判断当前listview是否已经添加header，如果已经添加则会生成一个新的tempadapter，这个新的tempadapter包含我们设置的adapter所有内容以及listview的header和footer。所以当我们在给listview添加了header后在程序中调用listview.getadapter时返回的是tempadapter而不是我们通过setadapter传进去的adapter。如果没有设置adapter则tempadapter与我们自己的adapter是一样的。listview.getadapter().getcount()方法返回值会比我们预期的要大，原因是添加了header。

>接着上面的tempadapter说，我们自定义adapter里面的getitem方法里面返回的position是不包括header的，是我们自定义adapter中数据position编号从0开始，也>就是说与我们传进去的list的位置是一样的。

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {  
    // TODO Auto-generated method stub  
    Log.i("adapter", "position:"+position);   //这个position就是我们数据的真实位置  
    }

而listview的onitemclick方法中：

    public void onItemClick(AdapterView<?> arg0, View arg1, int arg2,long arg3)
arg2是当前click的位置，这个位置是指在tempadapter中的位置，从0开始如果listview中添加了header则0代表header。

