## 千位分隔符

   #方法1
   String(123456789).replace(/(\d)(?=(\d{3})+$)/g, "$1,");
   #方法2
   (123456789).toLocaleString('en-US');
