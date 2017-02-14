# myGulpExample
	gulp 的一些用法 css、js、img打包合并压缩

#gulp的使用。

	1.进入目录文件夹 npm install 安装依赖
	2.命令 

	 命令行cmd中执行
	 1.gulp  命令 进行css，js，img的打包base64，合并压缩
	 2.gulp clean命令 是清除目录下面的build目录和rev目录
	 3.详情请看gulpfile.js      
      
#gulpfile.js

var gulp 	= require('gulp');

var less 	= require('gulp-less');					//less插件

var browserSync = require('browser-sync').create(); 			//同步

var reload      = browserSync.reload;					//重新加载

var notify 	= require("gulp-notify");				//通知

var concat 	= require('gulp-concat');				//文件合并

var cleanCSS 	= require('gulp-clean-css');				//文件压缩

var rev 	= require('gulp-rev');					//版本控制

var revCollector= require('gulp-rev-collector');			//路径修改器

var runSequence = require('run-sequence');				//同步执行 (重要，有些报错就是因为这个没弄好)

var del         = require('del')					//删除模块	不建议写在流程里

var vinylPaths  = require('vinyl-paths')				//管道删除 gulp.src先定义一个位置 然后.pipe(vinylPaths(del))不建议写】
var base64 	= require('gulp-base64');				//base64

var fs          = require('fs')						//因为首次加载时候css没有加载进去 所以利用node 判断

var imagemin 	= require('gulp-imagemin');				//图片压缩

var spriter 	= require('gulp-css-spriter');				//雪碧图 现在不流行这个流行base64和

var   babel 	= require('gulp-babel');				//babel es6->es5

var uglify 	= require('gulp-uglify');				//js压缩

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
      	.pipe(uglify())							//压缩
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
	    .pipe(rev())							//版本控制 随机的css文件
	    .pipe(gulp.dest('./build/css')) 					//输出存储
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
   
