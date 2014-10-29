title: Gulp的简单配置
date: 2014-10-29 23:44:21
tags: [javascript,Gulp]
---
###第一步安装node###
首先，也是最基本的，我们需要搭建node环境。访问[nodejs.org](http://nodejs.org)，然后点击大大的install按钮，就可以下载安装了。
###第二步安装Gulp###
在安装node之后，我们开始使用命令行安装Gulp，在命令行总输入
<pre><code>sudo npm install -g gulp</code></pre>
1. sodo 是以管理员的身份执行命令，一般会要求输入电脑密码。
2. npm 是安装node模块的工具，执行install命令
3. -g 表示在全局环境安装，以便任何项目都能使用它
4. 最后Gulp是将要安装的node模块的名字

运行时注意查看命令行是否有出错的信息，安装完成后，你可以使用下面的命令查看gulp的版本号以确保gulp已经被正确安装
<pre>gulp -v</pre>
接下来，我们需要将gulp安装到项目本地
<pre>npm install gulp --save-dev</pre>
ps:如果失败，尝试下前面加上 <code>sudo</code>,这里我们使用<code>--save-dev</code>来更新package.json文件，更新devDependencies值，以表明项目需要依赖gulp.
###第三步新建Gulpfile文件，运行Gulp###
<p>安装好Gulp之后我们需要告诉它要为我们执行哪些任务，首先我们要清楚的知道项目需要哪些任务，自己都不清楚，代码怎么会知道呢，不要像PM一样，自己都不知道项目要做哪些功能.</p>
*   检查Javascript
*   编译sass或者less之类的预处理文件
*   合并javascript
*   压缩并重命名合并后的javascript
####安装依赖####
<pre>npm install gulp-jshint gulp-sass gulp-concat gulp-uglify gulp-rename --save-dev</pre>
>提醒下，如果以上命令提示权限错误，需要加上sudo再次尝试

####新建gulpfile文件####
<p>现在组件都安装完毕，我们需要新建gulpfile文件以指定gulp需要为我们完成什么任务。</p>
gulp只有5个方法：`task`,`run`,`watch`,`src`和`dest`,在项目根目录新建一个js文件并命名为gulpfile.js把下面的代码贴进去:
<pre>// 引入 gulp
var gulp = require('gulp'); 

// 引入组件
var jshint = require('gulp-jshint');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

// 检查脚本
gulp.task('lint', function() {
    gulp.src('./js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// 编译Sass
gulp.task('sass', function() {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});

// 合并，压缩文件
gulp.task('scripts', function() {
    gulp.src('./js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./dist'));
});

// 默认任务
gulp.task('default', function(){
    gulp.run('lint', 'sass', 'scripts');

    // 监听文件变化
    gulp.watch('./js/*.js', function(){
        gulp.run('lint', 'sass', 'scripts');
    });
});</pre>
