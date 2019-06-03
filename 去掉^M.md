- 方法1:  
> ==dos2unix== filename     #用于文件处理  

- 方法2：  
>==sed== -i 's/^M//g' filename


- 方法3: ==vim==  
```bash
#vim filename  
:1,$ s/^M//g  
```

- 方法4:   
>cat filename |==tr== -d '==\r==' > newfile



**ps: 方法2、3中^M的输入方式是 Ctrl + v ，然后Ctrl + M**