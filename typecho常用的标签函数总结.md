网站地址

```
<?php $this->options->siteUrl(); ?>
```

分类缩略名

```
<?php $categorys->slug();?>
```

分类名

```
<?php $categorys->name();?>
```

纯文字分类名称，不带链接

```
<?php $this->category(',', false); ?>
```

自定义字段输出
哪个好使用哪个~

```
<?php echo $this->fields->自定义字段名;?>
<?php echo $posts->fields->自定义字段名;?>
```

截取部份文章简称“摘要”，200是字数限制

```
<?php $this->excerpt(200, '.. .'); ?>
```

评论分页

```
<?php $comments->pageNav('«', '»', 1, '...', array('wrapTag' => 'div', 'wrapClass' => 'layui-laypage layui-laypage-molv', 'itemTag' => '', 'textTag' => 'span', 'currentClass' => 'current', 'prevClass' => 'prev', 'nextClass' => 'next',)); ?>
```

面包屑

```
<div class="layui-fluid map">
<span class="layui-breadcrumb">
<a href="<?php $this->options->siteUrl(); ?>">首页</a>
<?php if ($this->is('index')): ?>
<?php elseif ($this->is('post')): ?>
<?php $this->category(); ?>
<a><cite><?php $this->title() ?></cite></a>
<?php else: ?>
<a><cite><?php $this->archiveTitle(' &raquo; ','',''); ?></cite></a>
<?php endif; ?>
</span>
</div>
```

全部tag标签列表，按照MID排序

```
<?php $this->widget('Widget_Metas_Tag_Cloud')
->to($taglist); ?><?php while($taglist->next()): ?>
<li><a href="<?php $taglist->permalink(); ?>" title="<?php $taglist->name(); ?>"><?php $taglist->name(); ?></a></li>
<?php endwhile; ?>
```

tag调用标签、按照文章数量排序，调用20条

```
<?php $this->widget('Widget_Metas_Tag_Cloud', array('sort' => 'count', 'ignoreZeroCount' => true, 'desc' => true, 'limit' => 20))->to($tags); ?>
<?php while($tags->next()): ?>
<a class="layui-btn layui-btn-xs layui-btn-danger" href="<?php $tags->permalink(); ?>"><?php $tags->name(); ?></a>
<?php endwhile; ?>
```

整站数据统计输出

```
<?php Typecho_Widget::widget('Widget_Stat')->to($stat); ?>
文章总数：<?php $stat->publishedPostsNum() ?>篇
分类总数：<?php $stat->categoriesNum() ?>个
评论总数：<?php $stat->publishedCommentsNum() ?>条
页面总数：<?php $stat->publishedPagesNum() ?>个
```

文章列表或页面，评论数目输出方法

```
<?php $this->commentsNum('No Comments', '1 Comment' , '%d Comments'); ?>
```

登录状态

```
<?php if (!empty($this->options->sidebarBlock) && in_array('ShowOther', $this->options->sidebarBlock)): ?>
<?php if($this->user->hasLogin()): ?>
<!-- 登陆后显示 -->
<a href="<?php $this->options->adminUrl(); ?>"><?php _e('进入后台'); ?> (<?php $this->user->screenName(); ?>)</a>
<a href="<?php $this->options->logoutUrl(); ?>"><?php _e('退出'); ?>
<?php else: ?>
<!-- 未登录显示 -->
<a href="<?php $this->options->adminUrl('login.php'); ?>"><?php _e('登录'); ?></a></li>
<?php endif; ?>
<!-- 一直显示 -->
<a href="<?php $this->options->feedUrl(); ?>"><?php _e('文章 RSS'); ?></a>
<a href="<?php $this->options->commentsFeedUrl(); ?>"><?php _e('评论 RSS'); ?></a>
<?php endif; ?>

```

文章阅读次数含cookie

```
<?php echo get_post_view($this) ?>

function get_post_view($archive)
{
$cid = $archive->cid;
$db = Typecho_Db::get();
$prefix = $db->getPrefix();
if (!array_key_exists('views', $db->fetchRow($db->select()->from('table.contents')))) {
$db->query('ALTER TABLE `' . $prefix . 'contents` ADD `views` INT(10) DEFAULT 0;');
return 0;
}
$row = $db->fetchRow($db->select('views')->from('table.contents')->where('cid = ?', $cid));
if ($archive->is('single')) {
$views = Typecho_Cookie::get('extend_contents_views');
if(empty($views)){
$views = array();
}else{
$views = explode(',', $views);
}
if(!in_array($cid,$views)){
$db->query($db->update('table.contents')->rows(array('views' => (int) $row['views'] + 1))->where('cid = ?', $cid));
array_push($views, $cid);
$views = implode(',', $views);
Typecho_Cookie::set('extend_contents_views', $views); //记录查看cookie
}
}
return $row['views'];
}

```

文章阅读次数不含cookie一直刷新增加次数

```
<?php get_post_view($this) ?>

function get_post_view($archive)
{
$cid = $archive->cid;
$db = Typecho_Db::get();
$prefix = $db->getPrefix();
if (!array_key_exists('views', $db->fetchRow($db->select()->from('table.contents')))) {
$db->query('ALTER TABLE `' . $prefix . 'contents` ADD `views` INT(10) DEFAULT 0;');
echo 0;
return;
}
$row = $db->fetchRow($db->select('views')->from('table.contents')->where('cid = ?', $cid));
if ($archive->is('single')) {
$db->query($db->update('table.contents')->rows(array('views' => (int) $row['views'] + 1))->where('cid = ?', $cid));
}
echo $row['views'];
}

```

循环输出独立页面

```
<?php $this->widget('Widget_Contents_Page_List')->parse('<a href="{permalink}">{title}</a>'); ?>
```

分类链接的几种方式，可不停地换一下试试
在不同的语句，分类链接的标签是不一样的

```
<?php $categorys->permalink();?>
<?php $posts->permalink(); ?>
<?php echo $child['permalink'];?>
```

输出分类

```
<?php $this->widget('Widget_Metas_Category_List')->to($categorys);
while ($categorys->next()) {
if ($categorys->levels === 0) {
$children = $categorys->getAllChildren($categorys->mid);
if (empty($children)) {?>
<li class="layui-nav-item"<?php if ($this->is('category', $categorys->slug)) {?> class="active"<?php }?>>
<a href="<?php $categorys->permalink();?>" title="<?php $categorys->name();?>"> <i class="fa fa-fw <?php $this->options->FuIcon(); ?>"></i> <span> <?php $categorys->name();?></span></a>
</li>
<?php } else {?>
<li class="layui-nav-item">
<a href="javascript:;" class="nav-item-a">
<i class="fa fa-fw <?php $this->options->FuIcon(); ?>"></i>
<span><?php $categorys->name();?></span>
<span class="layui-nav-more"></span>
</a>
<ul class="layui-nav-child" id="scroll2">
<?php foreach ($children as $mid) {$child = $categorys->getCategory($mid);?>
<li>
<a href="<?php echo $child['permalink'];?>" title="<?php echo $child['name'];?>" ><i class="fa <?php $this->options->ZiIcon(); ?>"></i> <?php echo $child['name'];?></a>
</li>
<?php
}
?>
</ul>
</li>
<?php
}
}
}
?>
```