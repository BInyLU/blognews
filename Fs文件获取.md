```js
const fs = require('fs')
// depth为递归的深度，可根据递归的深度输出文件名称前面的格式
function readDirAll(path, depth){
    // 获取字符串
    var getLastCode = function(str){
        return str.substr(str.length-1, 1);
    }

    depth = depth || 0; // 默认为0
    var fir_code = '';

    // 计算文件名称前面的字符，4个空格为1组
    for(var j=0; j<depth; j++){
        fir_code += '    ';
    }
    depth && (fir_code += '|---');

    var stats = fs.statSync(path);
    if( stats.isFile() ){
        console.log( fir_code+path );
    }else if( stats.isDirectory() ){
        var files = fs.readdirSync(path);
        for(var i=0, len=files.length; i<len; i++){
            var item = files[i],
                itempath = getLastCode(path)=='/'  ? path+item : path+'/'+item,
                st = fs.statSync(itempath);

          console.log( fir_code+item );
          if( st.isDirectory() ){
              var s = readDirAll( itempath, depth+1 );
          }
      }
  }js
}
console.log( readDirAll('./') ); 
```

