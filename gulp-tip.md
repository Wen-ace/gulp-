### gulp 的简单使用
- 安装 gulp
    - 安装全局 gulp
    ```
    npm install --global gulp
    ```
    - 安装工程项目 gulp
    ```
    npm init
    npm install --save-dev gulp
    ```
- 创建配置 gulpfile.js 文件
    - 引入 plugins 包
    ```
    const gulp = require('gulp');
    const uglify = require('gulp-uglify');
    const cleanCss = require('gulp-clean-css');
    // 之前安装 gulp-csso 实现 css 压缩，动态更新后出现未知 bug
    const scss = require('gulp-sass');
    // 注意是gulp-sass 而不是 gulp-scss
    // 很容易安装失败， 可以先安装node-sass 安装成功之后再安装 gulp-sass
    const autoprefixer = require('gulp-autoprefixer');
    const babel = require('gulp-babel');
    // 之前安装 gulp-es6-compeliter 不好用
    // 安装 gulp-babel 需要安装一些其他依赖: babel-core babel-preset-es2015
    ```
    - 定义匹配规则
    ```
    const jsPath = './src/**/*.js';
    const cssPath = './src/**/*.css';
    const scssPath = './src/**/*.scss';
    const htmlPath = './src/index.html';
    ```
    - css scss 文件打包任务
    ```
    gulp.task('csstask', () => {
        gulp.src([cssPath, scssPath])
            .pipe(scss())
            .pipe(autoprefixer({
                browsers: ['last 2 versions']
            }))
            .pipe(cleanCss())
            .pipe(gulp.dest('dist/'))
        });
    ```
    - js 打包任务
    ```
    gulp.task('jstask', () => {
        gulp.src(jsPath)
            .pipe(babel({
                presets: ['es2015'] // es6 -> es5
            }))
            .pipe(uglify())
            .on('error', (err) => {
                console.log(err);
            })
            .pipe(gulp.dest('dist/'))
    })
    ```
    - html 打包任务
    ```
    gulp.task('htmltask', () => {
        gulp.src(htmlPath)
            // .pipe(rev())   // 生成hash 码插件
            .pipe(gulp.dest('dist/'))
    })
    ```
    - 添加监听器，实现自动构建
    ```
    gulp.task('watch', () => {
        gulp.watch(scssPath, ['csstask'])
        gulp.watch(jsPath, ['jstask'])
        gulp.watch(htmlPath, ['htmltask'])
    })
    ```
    - 整合打包任务， 方便命令输入
    ```
    gulp.task('default', ['csstask', 'jstask', 'htmltask', 'watch'])
    ```
    - 开始打包
    ```
    cmd> gulp
    ```
    根据项目实际需求（图片gulp-imagemin...）配置相应gulp插件（安装主流插件，有些插件容易出bug）
