---
title: Привет, Gulp
subtitle: ты, трудолюбивый и надежный помощник
date: 2021-06-21T16:48:05+03:00
Lastmod: 2021-06-21T16:48:05+03:00
draft: false
# description: A short description of this page, «в двух словах»
summary: главной особенностью Gulp является моментальное выполнение рутинных, часто повторяющихся задач, которые неизбежны в веб-разработке и не только. Очень многие из них легко программируются и решаются с помощью многочисленных задач и плагинов.
author:
  given_name: Vladimir
  family_name: Buresh
  display_name: wBuresh
categories: [веб-разработка]
# categories: ['web development']
tags: [Gulp, Hugo]
toc: true
---

До Gulp был и есть прекрасный Grunt. Gulp - его преемник. Он заметно проще, а задачи наглядны и . В этом его преимущество и секрет популярности. Вскоре триумфального взлета у Gulp_а появился «товарищ по жизни». WebPack, называется. Он многое может, еще больше хочет, да и порог вхождения имеет очень приличный. Но стал модным и вызвал заметный отток пользователей Gulp. К тому же, на подходе оказалась 4-я версия Gulp, которую, как это порой случается, часть сообщества встретила без особых восторгов. Это значит, что они остались на прежней версии, либо... В результате Gulp получил более сплоченное и верное ему сообщество. Есть и фанаты. Вот один из них, - Никита Дубко. Здесь вполне уместно привести фрагмент его выступления перед специалистами на тему [`<img>`. Доклад Яндекса](https://habr.com/ru/company/yandex/blog/559442/). Рассматривая вопросы применения графических объектов, он прямо сказал:

Я фанат Gulp. Можете посмотреть у меня в репозитории: [mefody/image-processor](https://github.com/MeFoDy/image-processor).

Использую простые пакеты: `gulp-imagemin`, `gulp-responsive`, `imagemin-guetzli`, `imagemin-pngquant`, `imagemin-svgo`, `imagemin-webp`.

Просто из этого стартера я собираю себе почти под любой проект оптимизатор картинок, который распиливает их по разным размерам. `gulp-imagemin` внутри себя содержит `imagemin`.

<!-- {linenos=table,hl_lines=[1,"4-5"],linenostart=2} -->

``` js {linenos=table,linenostart=1}
gulp.task('convert', gulp.series(
    'clean',
    'convert:retina',
    'convert:webp',
    'convert:avif',
    gulp.parallel(
        'optimize:guetzli',
        'optimize:png',
        'optimize:svg'
    ),
));
```

Чем этот пакет хорош? Тем, что туда можно подключать новые плагины. Но там пока нет поддержки AVIF (на момент подготовки доклада — прим. автора).

Зато там есть поддержка Guetzli. Этот алгоритм тоже разработан в Google. В чем его суть?

![Просто рисунок](https://hsto.org/r/w1560/webt/-n/6_/oj/-n6_ojr5mjcpi1czlx4s3dxy_mo.png)

Что обычно делают оптимизаторы? Они снижают качество у JPG, не меняя информацию о цвете. А Guetzli ещё и меняет цвета картинок, но так, чтобы визуально для глаза это почти не чувствовалось. Ключевое слово — «почти». И оказывается, за счёт такого незаметного изменения цветов можно сделать сжатие значительно круче.

Но происходит страшное. Даже на картинке 1000x1000 у вас начинает жужжать ноутбук, всё это обрабатывается очень долго. Алгоритм банально делает очень много разных переборов и сравнений. При этом сжимает настолько потрясающе, что у меня JPG обычно весят меньше, чем WebP. Поэтому я могу из своих source в принципе выкидывать ненужный мне WebP. Но, кстати, объём у сжатых через Guetzli картинок всё равно больше, чем у AVIF. Я попробовал.

Кстати, «guetzli» — это, я так понимаю, сладость, скандинавская выпечка. Как-то в подкасте мы пытались выяснить, что такое guetzli, brotli, zopfli. Оказалось, всё это — выпечка.

AVIF уже, на самом деле, в gulp засунуть можно, но через костыли.

``` js {linenos=table,linenostart=1}
const gulp = require('gulp');
const exec = require('gulp-exec');

const config = require('./config');

gulp.task('convert:avif', () => {
    let src = `${config.base.dist}/**/*.{jpg,png}`;
    return gulp.src(src)
        .pipe(exec((file) =>
            `avifenc -c aom --min 20 --max 50 \
            ${file.path} \
            ${file.path.replace(/(png|jpg)$/ig, 'avif')}`
        ));
});
```

Потому что под imagemin пакета нет (уже есть — прим. автора), но у вас есть возможность вызвать gulp-exec, который дёргает какую-нибудь команду прямо у вас на локальной машинке. Я так и сделал, и оно работает. Достаточно просто, но при этом меньше контроля из Node.js: вы не можете количество файлов отслеживать в прогрессе, но, наверное, в будущем это будет возможно.

Автор: Никита Дубко - DarkMeFoDy, разработчик интерфейсов ["`<img>`. Доклад Яндекса"](https://habr.com/ru/company/yandex/blog/559442/) Хабр, 2021-06-15.

Вот яркое свидетельство, что Gulp живет и хорошеет в руках таких замечательных и искусных пользователей.

Вот еще одна совсем свежая статья

Автор: Роман Сердюк - @budfy, Markup developer ["Как я сделал свою сборку Gulp для быстрой, лёгкой и приятной вёрстки"](https://habr.com/ru/post/560894/). Хабр, 2021-06-03.

Webpack - бандлер, Gulp - таск раннер.

...Это разные инструменты. Webpack — сборщик JS-проектов. Gulp — автоматизатор для вёрстки. Да, webpack тоже умеет работать с html, но это другой инструмент, предназначенный для других целей.

...советую воспользоваться вашим же советом «Webpack — сборщик JS-проектов. Gulp — автоматизатор для вёрстки» и поставить webpack/rollup рядом, либо в gulp либо запускать их параллельно.
