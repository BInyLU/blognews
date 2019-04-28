某小伙伴发了个doc文件给我，说要修改，但被加密，因紧急，需要直接修改，问我有没有办法。

现象是doc文档可以正常打开，但无法编辑，显示受保护，解除要密码。

谷歌一番后，结合实际记录如下：

把doc后缀名改成rar并解压缩，找到/word/settings.xml，用记事本或者sublime打开，找到`<w:DocumentProtection>ReadOnly`，字串符，将ReadOnly改成Forms，保存后将该settings.xml文件存入压缩包内，覆盖修改后，改回后缀名为doc，再次打开，已经可以编辑文档了。