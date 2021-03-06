# Отзывчивые изображения: примеры использования и документированные снипеты, чтобы вы могли начать их использовать

## Вступление

Наконец-то по-настоящему отзывчивые изображения стают реальностью веба -- в чистом HTML, без всяких замысловатых хаков. Элемент `<picture>` и парочка новых атрибутов элемента `<img>` уже поддерживаются в Chromium 37 и идут в комплекте в Chromium 38 (скоро ожидается то же в Opera), в [Firefox Nightly][1] и имплементируются в [WebKit][2] (нужно будет ещё посмотреть, добавит ли их Apple в следующую версию Safari).

Новый элемент `<picture>` может быть длинным и запутанным потому что он решает множество случаев использования. Для того, чтобы помочь удовлетворить ваши нужды в синтаксисе отзывчивых изображений, мы подготовили эту полную примеров статью.

## Четыре вопроса

Прежде чем вы начали использовать отзывчивые изображения в вашей разработке, вам нужно ответить на следующие четыре вопроса:

* Хочу ли я, чтобы **размеры** моих изображений изменялись в зависимости от правил моего отзывчивого дизайна?   
* Хочу ли я оптимизироваться под экраны с высоким **dpi**?
* Хочу ли я отдавать изображения с различными **mime** типами для поддерживающих их браузеров?   
* Хочу ли я отдавать различное **содержимое** в зависимости от определённых контекстовых факторов?
   

В приведенных ниже примерах мы осветим эти вопросы используя ключевые слова **размер**, **dpi**, **mime** и **содержимое** соответственно и далее для каждой комбинации ответов мы покажем кусок примерного кода с коротким описанием. При создании этих примеров, я держал в голове
[этот ночной снимок оперного зала Осло][3] —- он может оказаться полезным для вас.

<figure class="figure">![Ночной снимок оперного зала Осло][4]<figcaption class="figure__caption">Ночной снимок оперного зала Осло</figcaption></figure>

## Вещи, которые нужно запомнить

Перед тем, как вы взглянете на различные примеры ниже, нужно запомнить несколько следующих вещей:

* `<picture>` *требует* `<img>` в качестве своего последнего потомка. Без этого ничего не выведется. Это хорошо для совместимости, так как есть только одно традиционное место для вашего альтернативного текста и это хорошо для поддержки содержимого в старых браузерах, которые показывают только `<img>` элемент.
* Думайте об атрибутах `sizes` и `srcset` элемента `<picture>`,  как о перезаписи `src` в `<img>`. Проверяйте `currentSrc` в JavaScript для того, чтобы узнать что выбирает браузер. Старые браузеры конечно же будут использовать только `<img src>`, поэтому вам нужно будет использовать что-то наподобие `image.currentSrc || image.src`, чтобы обработать все случаи.
* В `srcset` и `sizes` список -- это подсказка для браузеров, а не команда. Например, устройство с device-pixel-ratio в 1.5 будет свободно использовать 1x или 2x изображение в зависимости от того, что оно знает о своих возможностях, сети и т.д.   
* `<img sizes="(max-width: 30em) 100vw …">` сообщает: если это медиавыражение верно, показать изображение с `100vw` шириной. Первое медиавыражение выигрывает, поэтому порядок источников важен.

## Выдача разного содержимого

размеры dpi mime *содержимое*

    <picture>
    	<source
    		media="(min-width: 1024px)"
    		srcset="opera-fullshot.jpg">
    	<img
    		src="opera-closeup.jpg" alt="The Oslo Opera House">
    </picture>
    

Для браузеров с шириной окна в 1024 CSS пиксела и шире используется полноэкранное фото, меньшие окна браузера получат приближенное фото.

## Использование различных типов изображений

размеры dpi *mime* содержимое

    <picture>
    	<source
    		srcset="opera.webp"
    		type="image/webp">
    	<img
    		src="opera.jpg" alt="The Oslo Opera House">
    </picture>
    

Браузеры, которые поддерживают WebP получают WebP изображения; остальные получают JPG.

## Различные типы изображений и разное содержимое

размеры dpi *mime* *содержимое*

    <picture>
    	<source
    		media="(min-width: 1024px)"
    		srcset="opera-fullshot.webp"
    		type="image/webp">
    	<source
    		media="(min-width: 1024px)"
    		srcset="opera-fullshot.jpg">
    	<source
    		srcset="opera-closeup.webp"
    		type="image/webp">
    	<img
    		src="opera-closeup.jpg" alt="The Oslo Opera House">
    </picture>
    

Для окон браузеров с шириной в 1024 CSS пиксела и шире будет использоваться полноэкранное фото; меньшие окна браузера получат приближённое фото. Это фото будет выдаваться в WebP формате для браузеров, которые его поддерживают; другие будут получать в формате JPG.

## Изображения с высоким разрешением

размеры *dpi* mime содержимое

    <img
    	src="opera-1x.jpg" alt="The Oslo Opera House"
    	srcset="opera-2x.jpg 2x, opera-3x.jpg 3x">
    

Браузеры на устройствах с высоким разрешением получают изображения высокого разрешения; другие браузеры получают обычное изображение.

## Изображения с высоким разрешением и разным содержимым

размеры *dpi* mime *содержимое*

    <picture>
    	<source
    		media="(min-width: 1024px)"
    		srcset="opera-fullshot-1x.jpg 1x,
    				opera-fullshot-2x.jpg 2x,
    				opera-fullshot-3x.jpg 3x">
    	<img
    		src="opera-closeup-1x.jpg" alt="The Oslo Opera House"
    		srcset="opera-closeup-2x.jpg 2x,
    				opera-closeup-3x.jpg 3x">
    </picture>
    

Для окон браузеров с шириной 1024 CSS пикселов и шире используется полноэкранное изображение; меньшие окна браузера используют приближенные фото. В дополнение, эти фото поставляются как изображения с высоким разрешением для браузеров на устройствах с экранами высокого разрешения; другие браузеры получат обычное изображение.

## Изображения высокого разрешения и различные типы изображений

размеры *dpi* *mime* содержимое

    <picture>
    	<source
    		srcset="opera-1x.webp 1x,
    				opera-2x.webp 2x,
    				opera-3x.webp 3x"
    		type="image/webp">
    	<img
    		src="opera-1x.jpg" alt="The Oslo Opera House"
    		srcset="opera-2x.jpg 2x,
    				opera-3x.jpg 3x">
    </picture>
    

Браузеры на устройствах с высоким разрешением получают в два или даже в три раза большее количество пикселей; другие браузеры получают обычное изображения. Эти фото выдаются в WebP формате для поддерживающих его браузеров; остальные браузеры получают JPG.

## Изображения с высоким разрешением, различные типы изображений и различное содержимое

размеры *dpi* *mime* *содержимое*

    <picture>
    	<source
    		media="(min-width: 1024px)"
    		srcset="opera-fullshot-1x.webp 1x,
    				opera-fullshot-2x.webp 2x,
    				opera-fullshot-3x.webp 3x"
    		type="image/webp">
    	<source
    		media="(min-width: 1024px)"
    		srcset="opera-fullshot-1x.jpg 1x,
    				opera-fullshot-2x.jpg 2x,
    				opera-fullshot-3x.jpg 3x">
    	<source
    		srcset="opera-closeup-1x.webp 1x,
    				opera-closeup-2x.webp 2x,
    				opera-closeup-3x.webp 3x"
    		type="image/webp">
    	<img
    		src="opera-closeup-1x.jpg" alt="The Oslo Opera House"
    		srcset="opera-closeup-2x.jpg 2x,
    				opera-closeup-3x.jpg 3x">
    </picture>
    

Для окон браузеров с шириной в 1024 CSS пиксела и шире используется полноэкранное изображение; меньшие окна браузера получают приближенное фото. В дополнение, эти фото выдаются в высоком разрешении для браузеров на устройствах с экранами высокого разрешения; остальные браузеры получают обычное изображение. Эти фото выдаются в WebP формате для поддерживающих его браузеров; остальные браузеры получают JPG.

## Смена размеров изображений

*размеры* dpi mime содержимое

    <img
    	src="opera-fallback.jpg" alt="The Oslo Opera House"
    	sizes="(min-width: 640px) 60vw, 100vw"
    	srcset="opera-200.jpg 200w,
    			opera-400.jpg 400w,
    			opera-800.jpg 800w,
    			opera-1200.jpg 1200w">
    

Для окон браузеров с шириной в 640 CSS пикселов и шире используется фото с шириной 60% области просмотра; для браузеров с меньшей шириной окна используется фото равное полной ширине области просмотра. Браузер выбирает оптимальное изображение из выборки изображений с шириной в 200px, 400px, 800px и 1200px, принимая во внимание ширину изображения и разрешение экрана.

## Смена размеров изображения и разное содержимое

*размеры* dpi mime *содержимое*

    <picture>
    	<source
    		media="(min-width: 1280px)"
    		sizes="50vw"
    		srcset="opera-fullshot-200.jpg 200w,
    				opera-fullshot-400.jpg 400w,
    				opera-fullshot-800.jpg 800w,
    				opera-fullshot-1200.jpg 1200w">
    	<img
    	 	src="opera-fallback.jpg" alt="The Oslo Opera House"
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-closeup-200.jpg 200w,
    				opera-closeup-400.jpg 400w,
    				opera-closeup-800.jpg 800w,
    				opera-closeup-1200.jpg 1200w">
    </picture>
    

Для окон браузеров с шириной в 1280 CSS пикселов и шире используется полноэкранное изображение с шириной просмотра в 50%; для браузеров с шириной в 640-1279 CSS пикселов используется фото с 60% ширины области просмотра; для меньших окон браузера используется фото равное полной ширине области просмотра. В каждом случае браузер берёт оптимальное изображение из выборки с шириной в 200px, 400px, 800px и 1200px, принимая во внимание ширину и разрешение экрана устройства.

## Смена размеров изображения и различные типы изображения

*размеры* dpi mime *содержимое*

    <picture>
    	<source
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-200.webp 200w,
    				opera-400.webp 400w,
    				opera-800.webp 800w,
    				opera-1200.webp 1200w"
    		type="image/webp">
    	<img
    		src="opera-fallback.jpg" alt="The Oslo Opera House"
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-200.jpg 200w,
    				opera-400.jpg 400w,
    				opera-800.jpg 800w,
    				opera-1200.jpg 1200w">
    </picture>
    

Для окон браузеров с шириной в 640 CSS пикселов и шире используется полноэкранное изображение с шириной 60% от ширины области просмотра; для меньших окон браузера используется фото равное полной ширине области просмотра. Браузер берёт оптимальное изображение из выборки изображений с шириной 200px, 400px, 800px and 1200px, принимая во внимание ширину и разрешение экрана устройства. Эти фото выдаются в WebP формате для поддерживающих его браузеров; остальные браузеры получают JPG.

## Смена размеров изображения, различные типы изображения и разное содержимое

*размеры* dpi *mime* *содержимое*

    <picture>
    	<source
    		media="(min-width: 1280px)"
    		sizes="50vw"
    		srcset="opera-fullshot-200.webp 200w,
    				opera-fullshot-400.webp 400w,
    				opera-fullshot-800.webp 800w,
    				opera-fullshot-1200.webp 1200w"
    		type="image/webp">
    	<source
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-closeup-200.webp 200w,
    				opera-closeup-400.webp 400w,
    				opera-closeup-800.webp 800w,
    				opera-closeup-1200.webp 1200w"
    		type="image/webp">
    	<source
    		media="(min-width: 1280px)"
    		sizes="50vw"
    		srcset="opera-fullshot-200.jpg 200w,
    				opera-fullshot-400.jpg 400w,
    				opera-fullshot-800.jpg 800w,
    				opera-fullshot-1200.jpg 1200w">
    	<img
    		src="opera-fallback.jpg" alt="The Oslo Opera House"
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-closeup-200.jpg 200w,
    				opera-closeup-400.jpg 400w,
    				opera-closeup-800.jpg 800w,
    				opera-closeup-1200.jpg 1200w">
    </picture>
    

Для окон браузеров с шириной в 1280 CSS пикселов и шире используется полноэкранное фото с шириной 50% области просмотра; для окон браузеров с шириной в 640-1279 CSS пикселов используется фото с шириной в 60% области просмотра; для менее широких окон браузеров используется фото с шириной равной полной области просмотра. В каждом случае браузер выбирает оптимальное изображение из набора изображений с шириной в 200px, 400px, 800px и 1200px, принимая во внимание ширину и разрешение экрана. Эти фото выдаются в WebP формате для поддерживающих его браузеров; остальные браузеры получают JPG.

## Смена размеров изображения и изображения высокого разрешения

*размеры* *dpi* mime содержимое

    <img
    	src="opera-fallback.jpg" alt="The Oslo Opera House"
    	sizes="(min-width: 640px) 60vw, 100vw"
    	srcset="opera-200.jpg 200w,
    			opera-400.jpg 400w,
    			opera-800.jpg 800w,
    			opera-1200.jpg 1200w,
    			opera-1600.jpg 1600w,
    			opera-2000.jpg 2000w">
    

Для окон браузеров с шириной в 640 CSS пикселов и шире используется полноэкранное фото с шириной 60% области просмотра; для менее широких окон браузеров используется фото с шириной равной полной области просмотра. Браузер выбирает оптимальное изображение из набора изображений с шириной в 200px, 400px, 800px, 1200px, 1600px и 2000px, принимая во внимание ширину и разрешение экрана.

## Смена размеров изображения, изображения высокого разрешения и разное содержимое

*размеры* *dpi* mime *содержимое*

    <picture>
    	<source
    		media="(min-width: 1280px)"
    		sizes="50vw"
    		srcset="opera-fullshot-200.jpg 200w,
    				opera-fullshot-400.jpg 400w,
    				opera-fullshot-800.jpg 800w,
    				opera-fullshot-1200.jpg 1200w,
    				opera-fullshot-1600.jpg 1600w,
    				opera-fullshot-2000.jpg 2000w">
    	<img
    		src="opera-fallback.jpg" alt="The Oslo Opera House"
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-closeup-200.jpg 200w,
    				opera-closeup-400.jpg 400w,
    				opera-closeup-800.jpg 800w,
    				opera-closeup-1200.jpg 1200w,
    				opera-closeup-1600.jpg 1600w,
    				opera-closeup-2000.jpg 2000w">
    </picture>
    
Для окон браузеров с шириной в 1280 CSS пикселов и шире используется полноэкранное фото с шириной 50% области просмотра; для окон браузеров с шириной в 640-1279 CSS пикселов используется фото с шириной в 60% области просмотра; для менее широких окон браузеров используется фото с шириной равной полной области просмотра. В каждом случае браузер выбирает оптимальное изображение из набора изображений с шириной в 200px, 400px, 800px, 1200px, 1600px и 2000px, принимая во внимание ширину и разрешение экрана.

## Смена размеров изображения, изображения высокого разрешения и различные типы изображения

*размеры* *dpi* *mime* содержимое

    <picture>
    	<source
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-200.webp 200w,
    				opera-400.webp 400w,
    				opera-800.webp 800w,
    				opera-1200.webp 1200w,
    				opera-1600.webp 1600w,
    				opera-2000.webp 2000w"
    		type="image/webp">
    	<img
    		src="opera-fallback.jpg" alt="The Oslo Opera House"
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-200.jpg 200w,
    				opera-400.jpg 400w,
    				opera-800.jpg 800w,
    				opera-1200.jpg 1200w,
    				opera-1600.jpg 1600w,
    				opera-2000.jpg 2000w">
    </picture>
    

Для окон браузеров с шириной в 640 CSS пикселов и шире используется полноэкранное фото с шириной 60% области просмотра; для менее широких окон браузеров используется фото с шириной равной полной области просмотра. В каждом случае браузер выбирает оптимальное изображение из набора изображений с шириной в 200px, 400px, 800px, 1200px, 1600px и 2000px, принимая во внимание ширину и разрешение экрана. Эти фото выдаются в WebP формате для поддерживающих его браузеров; остальные браузеры получают JPG.

## Смена размеров изображения, изображения высокого разрешения, различные типы изображения и разное содержимое

*размеры* *dpi* *mime* *содержимое*

    <picture>
    	<source
    		media="(min-width: 1280px)"
    		sizes="50vw"
    		srcset="opera-fullshot-200.webp 200w,
    				opera-fullshot-400.webp 400w,
    				opera-fullshot-800.webp 800w,
    				opera-fullshot-1200.webp 1200w,
    				opera-fullshot-1600.webp 1600w,
    				opera-fullshot-2000.webp 2000w"
    		type="image/webp">
    	<source
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-closeup-200.webp 200w,
    				opera-closeup-400.webp 400w,
    				opera-closeup-800.webp 800w,
    				opera-closeup-1200.webp 1200w,
    				opera-closeup-1600.webp 1600w,
    				opera-closeup-2000.webp 2000w"
    		type="image/webp">
    	<source
    		media="(min-width: 1280px)"
    		sizes="50vw"
    		srcset="opera-fullshot-200.jpg 200w,
    				opera-fullshot-400.jpg 400w,
    				opera-fullshot-800.jpg 800w,
    				opera-fullshot-1200.jpg 1200w,
    				opera-fullshot-1600.jpg 1800w,
    				opera-fullshot-2000.jpg 2000w">
    	<img
    		src="opera-fallback.jpg" alt="The Oslo Opera House"
    		sizes="(min-width: 640px) 60vw, 100vw"
    		srcset="opera-closeup-200.jpg 200w,
    				opera-closeup-400.jpg 400w,
    				opera-closeup-800.jpg 800w,
    				opera-closeup-1200.jpg 1200w,
    				opera-closeup-1600.jpg 1600w,
    				opera-closeup-2000.jpg 2000w">
    </picture>
    
Для окон браузеров с шириной в 1280 CSS пикселов и шире используется полноэкранное фото с шириной 50% области просмотра; для окон браузеров с шириной в 640-1279 CSS пикселов используется фото с шириной в 60% области просмотра; для менее широких окон браузеров используется фото с шириной равной полной области просмотра. В каждом случае браузер выбирает оптимальное изображение из набора изображений с шириной в 200px, 400px, 800px, 1200px, 1600px и 2000px, принимая во внимание ширину и разрешение экрана. Эти фото выдаются в WebP формате для поддерживающих его браузеров; остальные браузеры получают JPG.

## Есть о чём задуматься!

Не переживайте, если вы не поняли это полностью! Скоро мы опубликуем полноценный урок по `<picture>` и отзывчивым изображениям. В это время наслаждайтесь подготовкой к улучшению линии пропуска ваших босса и пользователей и повышением производительности сайта.

 [1]: https://bugzilla.mozilla.org/show_bug.cgi?id=870022
 [2]: https://bugs.webkit.org/show_bug.cgi?id=134634
 [3]: http://commons.wikimedia.org/wiki/File:Full_Opera_by_night.jpg
 [4]: img/opera-house.jpg
