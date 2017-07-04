# myGulpExample
	gulp 的一些用法 css、js、img打包合并压缩
# gulp的使用
	
	1.进入目录文件夹 npm install 安装依赖
	2.命令 
	命令行cmd中执行
	(1).gulp  命令 进行css，js，img的打包base64，合并压缩
	(2).gulp clean命令 是清除目录下面的build目录和rev目录
	(3).详情请看gulpfile.js     	
# 详细的教程说明
>码字不易，可以的话给个star。谢谢。
>#### 1.基本的安装设置install
[gulp官网](http://gulpjs.com/)http://gulpjs.com/		

![](https://github.com/well-monkey/myGulpExample/blob/master/screenshot/gulp.png)

	这个是gulp的官方网站，进入后可以看到gulp-cli的基本命令。
	npm install gulp-cli -g 	全局安装gulp
	npm install gulp -D     	  安装依赖
	gulpfile.js					目录下面新建 文件在gulp.js文件里面设置	
```
var gulp = require('gulp');
	 gulp.task('default', function() {
	  // 将你的默认的任务代码放在这
 });
 ```
 	gulp走的是任务流，一个线程线程的走	
> #### 2.同步测试工具Browsersync
[browsersync官网](http://www.browsersync.cn/)http://www.browsersync.cn/
[gulp和browsersync结合](http://www.browsersync.cn/docs/gulp/)http://www.browsersync.cn/docs/gulp/	

	npm install -g browser-sync	
	npm install  browser-sync --save-dev 		
	//安装完依赖后在gulpfile.js中添加
	var browserSync = require('browser-sync').create();
	browserSync.init({
		server: {
		    baseDir: "./build/"
		},
		port: 9999
	});
> #### 3.自动刷新
[gulp和browsersync结合](http://www.browsersync.cn/docs/gulp/)http://www.browsersync.cn/docs/gulp/		

	var gulp        = require('gulp');
	var browserSync = require('browser-sync').create();
	var sass        = require('gulp-sass');
	var reload      = browserSync.reload;
```
	// 静态服务器 + 监听 scss/html 文件
	gulp.task('serve', ['sass'], function() {

	    browserSync.init({
		server: "./app"
	    });

	    gulp.watch("app/scss/*.scss", ['sass']);
	    gulp.watch("app/*.html").on('change', reload);
	});
	// scss编译后的css将注入到浏览器里实现更新
	gulp.task('sass', function() {
	    return gulp.src("app/scss/*.scss")
		.pipe(sass())
		.pipe(gulp.dest("app/css"))
		.pipe(reload({stream: true}));
	});

	gulp.task('default', ['serve']);
```

>#### 4.gulp-notify通知

[gulp-notify](https://www.npmjs.com/package/gulp-notify)https://www.npmjs.com/package/gulp-notify

	npm install --save-dev gulp-notify	//安装gulp-notify依赖	
```
	var notify = require("gulp-notify");
	gulp.src("./src/*.*")
  		.pipe(notify("Hello Gulp!"));
```	

>#### 5.gulp-concat合并
[gulp-concat](https://www.npmjs.com/package/gulp-concat)https://www.npmjs.com/package/gulp-concat

	npm install gulp-concat --save-dev
```
	//js合并	
	var concat = require('gulp-concat');
	
	gulp.task('scripts', function() {
	  return gulp.src('./lib/*.js')
	    .pipe(concat('all.js'))
	    .pipe(gulp.dest('./dist/'));
	});
```

>#### 6.gulp-clean-css压缩
[gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css)https://www.npmjs.com/package/gulp-clean-css	

	npm install gulp-clean-css --save-dev
```
	var gulp = require('gulp');
	var cleanCSS = require('gulp-clean-css');

	gulp.task('minify-css', function() {
	  return gulp.src('styles/*.css')
	    .pipe(cleanCSS({compatibility: 'ie8'}))
	    .pipe(gulp.dest('dist'));
	});
```

>#### 7.gulp-rev 版本控制
[gulp-rev](https://www.npmjs.com/package/gulp-rev)https://www.npmjs.com/package/gulp-rev
	
	npm install --save-dev gulp-rev
```
	var gulp = require('gulp');
	var rev = require('gulp-rev');

	gulp.task('default', function () {
	    return gulp.src('src/*.css')
		.pipe(rev())
		.pipe(gulp.dest('dist'));
	});
```

>#### 8.gulp-rev-collector 路径修改器
[gulp-rev-collector](https://www.npmjs.com/package/gulp-rev-collector)https://www.npmjs.com/package/gulp-rev-collector
	
	npm install --save gulp-rev-collector
```
	var gulp         = require('gulp');
	var rev = require('gulp-rev');

	gulp.task('css', function () {
	    return gulp.src('src/css/*.css')
		.pipe(rev())
		.pipe(gulp.dest('dist/css'))
		.pipe( rev.manifest() )
		.pipe( gulp.dest( 'rev/css' ) );
	});

	gulp.task('scripts', function () {
	    return gulp.src('src/js/*.js')
		.pipe(rev())
		.pipe(gulp.dest('dist/js'))
		.pipe( rev.manifest() )
		.pipe( gulp.dest( 'rev/js' ) );
	});
```

>#### 9.run-sequence同步执行
[run-sequence](https://www.npmjs.com/package/run-sequence)https://www.npmjs.com/package/run-sequence
	
	npm install --save-dev run-sequence
```
	var gulp = require('gulp');
	var runSequence = require('run-sequence');
	var del = require('del');
	var fs = require('fs');
	gulp.task('build', function(callback) {
	  runSequence('build-clean',
		      ['build-scripts', 'build-styles'],
		      'build-html',
		      callback);
	});
```

>#### 10.del 删除模块  vinyl-paths管道删除
[del](https://www.npmjs.com/package/del)https://www.npmjs.com/package/del
[vinyl-paths](https://www.npmjs.com/package/vinyl-paths)https://www.npmjs.com/package/vinyl-paths

	npm install del --save-dev
	npm install vinyl-paths --save-dev 
	
>#### 11.gulp-base64 base64图片
[gulp-base64](https://www.npmjs.com/package/gulp-base64)https://www.npmjs.com/package/gulp-base64

	npm install gulp-base64 --save-dev
```
	var gulp = require('gulp');
	var base64 = require('./build/gulp-base64');

	//basic example 
	gulp.task('build', function () {
	    return gulp.src('./css/*.css')
		.pipe(base64())
		.pipe(concat('main.css'))
		.pipe(gulp.dest('./public/css'));
	});
```

>#### 12.gulp-imagemin 图片压缩
[gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)https://www.npmjs.com/package/gulp-imagemin
	
	npm install gulp-imagemin --save-dev
```
	//es6写法	
	const gulp = require('gulp');
	const imagemin = require('gulp-imagemin');

	gulp.task('default', () =>
	    gulp.src('src/images/*')
		.pipe(imagemin())
		.pipe(gulp.dest('dist/images'))
	);
```	

>#### 13.gulp-css-spriter 雪碧图
[gulp-css-spriter](https://www.npmjs.com/package/gulp-css-spriter)https://www.npmjs.com/package/gulp-css-spriter
	
	npm install gulp-css-spriter --save-dev
	
>#### 14.gulp-babel es6=>es5 
[gulp-babel](https://www.npmjs.com/package/gulp-babel)https://www.npmjs.com/package/gulp-babel

	npm install --save-dev gulp-babel babel-preset-es2015
```
	//es6写法	
	const gulp = require('gulp');
	const babel = require('gulp-babel');

	gulp.task('default', () => {
	    return gulp.src('src/app.js')
		.pipe(babel({
		    presets: ['es2015']
		}))
		.pipe(gulp.dest('dist'));
	});
```

>#### 15.gulp-rename
[gulp-rename](https://www.npmjs.com/package/gulp-rename)https://www.npmjs.com/package/gulp-rename

	npm install gulp-rename --save-dev
```
	
```

>#### 16.gulp-changed
[gulp-changed](https://www.npmjs.com/package/del)https://www.npmjs.com/package/gulp-changed
	
	npm install gulp-changed --save-dev
```
	const gulp = require('gulp');
	const changed = require('gulp-changed');
	const ngAnnotate = require('gulp-ng-annotate'); // just as an example 

	const SRC = 'src/*.js';
	const DEST = 'dist';

	gulp.task('default', () => {
	    return gulp.src(SRC)
		.pipe(changed(DEST))
		// ngAnnotate will only get the files that 
		// changed since the last time it was run 
		.pipe(ngAnnotate())
		.pipe(gulp.dest(DEST));
	});
```

# gulpfile.js

	var gulp 	= require('gulp');
	var less 	= require('gulp-less');						//less插件
	var browserSync = require('browser-sync').create(); 	//同步
	var reload      = browserSync.reload;					//重新加载
	var notify 	= require("gulp-notify");					//通知
	var concat 	= require('gulp-concat');					//文件合并
	var cleanCSS 	= require('gulp-clean-css');			//文件压缩
	var rev 	= require('gulp-rev');						//版本控制
	var revCollector= require('gulp-rev-collector');		//路径修改器
	var runSequence = require('run-sequence');				//同步执行 (重要，有些报错就是因为这个没弄好)
	var del         = require('del')						//删除模块	不建议写在流程里
	var vinylPaths  = require('vinyl-paths')				//管道删除 gulp.src先定义一个位置 然后.pipe(vinylPaths(del))不建议写】
	var base64 	= require('gulp-base64');					//base64
	var fs          = require('fs')							//因为首次加载时候css没有加载进去 所以利用node 判断
	var imagemin 	= require('gulp-imagemin');				//图片压缩
	var spriter 	= require('gulp-css-spriter');			//雪碧图 现在不流行这个流行base64和
	var   babel 	= require('gulp-babel');				//babel es6->es5
	var uglify 	= require('gulp-uglify');					//js压缩
	var rename     	= require('gulp-rename');				//改名字
	var changed  	= require('gulp-changed');
	
	var build ={
		images:'build/images/',
		js:'build/js/'
	}
	var src ={
		images:'./src/images/',
		js:'./src/js/'
	}
	
	//建议手动删除 gulp clean 命令
	gulp.task('clean',function(){
		del([
			'./build/',
			'./rev'
			])	
	})
	gulp.task('js',function() {
		gulp.src([src.js+'a.js',src.js+'b.js'])
			.pipe(concat("index.js"))  
			.pipe(gulp.dest('./src/js'))
		gulp.src(src.js+'index.js')
			.pipe(babel({
		    presets: ['es2015']
		}))
		.pipe(uglify())									//压缩
		.pipe(rename('./index.min.js'))					//改名字
		.pipe(gulp.dest('./build/js'))					//输出
	})
	
	//图片的打包
	gulp.task('images',function() {
		gulp.src(src.images+'*.*')
			.pipe(imagemin())
			.pipe(gulp.dest('./build/images'))
	})
	
	//html生成
	gulp.task('html',function() {
		//因为首次加载时候css没有加载进去 所以利用node 轮寻判断
		fs.exists('./rev/rev-manifest.json',function(aaaa) {

			if(aaaa === true) {
				gulp.src(['./rev/*.json', './src/*.html']) //也可以是src目录下目录下的html文件 './src/*/*.html' 也可以是php
					// .pipe(changed('./src/*.html'))
					.pipe(revCollector())
					.pipe(gulp.dest('./build'))
					.pipe(reload({stream: true}))	
			}else {
				 runSequence('html') 
			}

		})	
	})
	
	//less css base64 打包 压缩 版本限制
	gulp.task('css', function() {
		gulp.src('./src/css/*.less')
			.pipe(less())
			.pipe(gulp.dest('./src/css'))
			.pipe(reload({stream: true}))	
			//.pipe(notify("完成了 less-> css [<%= file.relative %>]"))      //提示信息
		gulp.src(['./src/css/main.css', './src/css/header.css'])
		    .pipe(concat("index.css"))   					//合并完名字
			// .pipe(spriter({			       
			//   	'spriteSheet': './build/images/spritesheet.png',
			//   	'pathToSpriteSheetFromCSS': '../images/spritesheet.png'
			//  }))								//雪碧图只支持jpg和png两个模式建议png 雪碧图不流行现在都用base64和iconfont
		    // .pipe(base64())							//合并完 base64 然后压缩
		    .pipe(base64({
		    maxImageSize: 8*1024, 						// base64限制版 
		    debug: true
		 }))
		    .pipe(cleanCSS())							//压缩
		    .pipe(rev())								//版本控制 随机的css文件
		    .pipe(gulp.dest('./build/css')) 			//输出存储
			.pipe(rev.manifest())  						//生成一个rev-mainfest.json
			.pipe(gulp.dest('./rev'))					//添加到一个新的文件里
			.pipe(reload({stream: true}))
			//.pipe(notify("完成了 css-> css [<%= file.relative %>]"))	     //提示信息
	})
	
	//命令
	gulp.task('default',function() {
	    runSequence('images','css','html','js')
	    browserSync.init({
		server: {
		    baseDir: "./build/"
		},
		port: 9999
	    });
		gulp.watch("src/css/*.less", ['images','css','html','js']);		//名称和数组任务的说明
		gulp.watch("src/*.html",['html']);
	});
   
