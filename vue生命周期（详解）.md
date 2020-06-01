### vue生命周期（详解）

- beforeCreate：初始化了部分参数，如果有相同的参数，做了参数合并，执行beforeCreate；el和数据对象都为undefined，还未初始化；
- created：初始化了 Inject、Provide 、props、methods、data、computed和watch，执行created ；data有了，el还没有；
- beforeMount：检查是否存在el属性，存在的话进行渲染dom操作，执行beforeMount；$el和data都初始化了，但是dom还是虚拟节点，dom中对应的数据还没有替换；
- mounted：实例化 Watcher,渲染dom，执行mounted；vue实例挂载完成，dom中对应的数据成功渲染；
- beforeUpdate：在渲染dom 后，执行了mounted 钩子后，在数据更新的时候，执行 beforeUpdate；
- updated：检查当前的watcher列表中，是否存在当前要更新数据的watcher，如果存在就执行updated；
- beforeDestroy：检查是否已经被卸载,如果已经被卸载,就直接return出去,否则执行beforeDestroy；
- destroyed：把所有有关自己痕迹的地方，都给删除掉；

![psc](D:\前端\demo\最新个人文章\psc.jpg)