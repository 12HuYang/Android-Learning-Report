# AlertDialog 点击后不让消失的方法

```
        AlertDialog mdDialog = new AlertDialog.Builder(getActivity())
                .setTitle("测试")
                .setMessage("-------------------------------------")
                .setView(actProgressLayout)
                .setPositiveButton("允许", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        actProgressLayout.expand();
//                        actProgressLayout.big();
                        Log.d(TAG, "onClick: 1");
                    }
                })
                .setNegativeButton("拒绝", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.dismiss();
                        Log.d(TAG, "onClick: 2");
                    }
                })
                .setCancelable(false).create();

        try {

            Field field = mdDialog.getClass().getDeclaredField("mAlert");
            field.setAccessible(true);
            //   获得mAlert变量的值
            Object obj = field.get(mdDialog);
            field = obj.getClass().getDeclaredField("mHandler");
            field.setAccessible(true);
            //   修改mHandler变量的值，使用新的ButtonHandler类
            field.set(obj, new AlertDialogButtonHandler(mdDialog));
        } catch (Exception e) {
            Log.d(TAG, "startFlow: error");
        }
        mdDialog.show();
```