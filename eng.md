## Вступление {#introduction}

Наконец-то по настоящему отзывчивые изображения стают реальностью веба -- в чистом HTML, без всяких замысловатых хаков. Элемент `<picture>` и парочка новых аттрибутов элемента `<img>` уже поддерживаются в Chromium 37 и идут в комплекте в Chromium 38 (скоро ожидается то же в Opera), в [Firefox Nightly][1]
и имплементрируются в [implemented in WebKit][2] (нужно будет ещё посмотреть будет ли 
Apple иметь их в своей следующей версии Safari).

Новый элемент `<picture>` может быть длинным и запутанным потому что он решает множество случаев использования. Для того, чтобы помочь удовлетворить ваши нужды в синтаксисе отзывчивых изображений, мы подготовили эту полную примеров статью.

## Четыре вопроса {#four-questions}

Прежде чем вы начали использовать отзывчивые изображения в вашей разработке, вам нужно ответить на следующие четыре вопроса:

* Хочу ли я чтобы **размеры** моих изображений изменялись в зависимости от правил моего отзывчивого дизайна?   
* Хочу ли я оптимизироваться под экраны с высоким **dpi**?
* Хочу ли я отдавать изображения з различными **mime** типами для поддерживающих их браузеров?   
* Хочу ли я отдавать различное **содержимое** в зависимости от определённых контекстовых факторов?
   

В приведенных ниже примерах мы от the examples below, мы осветим эти вопросы используя ключевые слова **размер**, **dpi**, **mime** and **содержимое** соответственно и далее для каждой комбинации ответов мы покажем кусок примерного кода с коротким описанием. При создании этих примеров, я держал в голове
[этот ночной снимок оперного зала Осло][3] —- он может оказаться полезным для вас.<figure class="figure">

![Ночной снимок оперного зала Осло][4]<figcaption class="figure__caption">Ночной снимок оперного зала Осло</figcaption></figure>
## Вещи, которые нужно запомнить {#things-to-keep-in-mind}

Перед тем, как вы взглянете на различные примеры ниже, нужно запомнить несколько следующих вещей:

* `<picture>` *требует* `<img>` как свой последний предок. Без этого ничего не выведется. Это хорошо для совместимости, так как есть только одно традиционное место для вашего альтернативного текста и это хорошо для поддержки содержимого в старых браузерах, которые показывают только `<img>` элемент.
* Думайте о аттрибутах `<picture>`, `sizes` и `srcset` как о перезаписи `src` в `<img>`. Проверяйте `currentSrc` в JavaScript для того, чтобы узнать что выбирает браузер. Старые браузеры конечно же будут использовать только `<img src>`, поэтому вам нужно будет использовать что-то наподобие `image.currentSrc || image.src`, чтобы обработать все случаи.
* В `srcset` и `sizes` список -- это подсказка для браузеров, а не команда. Например, устройство с соотношением устройство-пиксель в 1.5 будет свободно использовать 1x или 2x изображение в зависимости от того, что оно знает о своих возможностях, сети и т.д.   
* `<img sizes="(max-width: 30em) 100vw …">` сообщает: если этот “медиазапрос” верен, показать изображение с `100vw` шириной. Первый “медиазапрос” выигрывает, поэтому порядок источников важен.

## Выдача разного содержимого {#art-direction-use-case}

размеры dpi mime *содержимое*

    <picture>
    	<source
    		media="(min-width: 1024px)"
    		srcset="opera-fullshot.jpg">
    	<img
    		src="opera-closeup.jpg" alt="The Oslo Opera House">
    </picture>
    

Для браузеров с шириной окна в 1024 CSS пиксела и шире, используется полноэкранное фото, меньшие окна браузера получат приближённое фото.

## Использование различных типов изображений{#different-image-types-use-case}

размеры dpi *mime* содержимое

    <picture>
    	<source
    		srcset="opera.webp"
    		type="image/webp">
    	<img
    		src="opera.jpg" alt="The Oslo Opera House">
    </picture>
    

Браузеры, которые поддерживают WebP получают WebP изображения; остальные получают JPG.

## Различные типы изображений и разное содержимое{#different-image-types--art-direction-use-case}

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
    

Для окон браузеров с шириной в 1024 CSS пиксела и шире, будет использоваться полноэкранное фото; меньшие окна браузера получат приближённое фото. Это фото будет выдаваться в WebP формате для браузеров, которые его поддерживают; другие будут получать в формате JPG.

## Изображения с высоким разрешением{#high-dpi-images-use-case}

размеры *dpi* mime содержимое

    <img
    	src="opera-1x.jpg" alt="The Oslo Opera House"
    	srcset="opera-2x.jpg 2x, opera-3x.jpg 3x">
    

Браузеры на устройствах с высоким разрешением получают изображения высокого разрешения; другие браузеры получают обычное изображение.

## Изображения с высоким разрешением и разным содержимым{#high-dpi-images--art-direction-
use-case
}

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
    

Для окон браузеров с шириной 1024 CSS пикселов и шире используется полноэкранное изображение; меньшие окна браузера используют приближённые фото. В дополнение, эти фото поставляются как изображения с высоким разрешёнием для браузеров на устройствах с экранами высокого разрешения; другие браузеры получат обычное изображение.

## Изображения высокого разрешения и различные типы изображений{#high-dpi-images--different-image-types-use-case}

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

## Изображения с высоким разрешением, различные типы изображений и различное содержимое{#high-dpi-images-different-image-types--art-direction-use-case}

размеры dpi mime содержимое

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
    

Для окон браузеров с шириной в 1024 CSS пикселя и шире используется полноэкранное изображение; меньшие окна браузера получают приближённое фото. В дополнение, эти фото выдаются в высоком разрешения для браузеров на устройствах с экранами высокого разрешения; остальные браузеры получают обычное изображение. Эти фото выдаются в WebP формате для поддерживающих его браузеров; остальные браузеры получают JPG.

## Смена размеров изображений {#changing-image-sizes-use-case}

размеры dpi mime содержимое

    <img
    	src="opera-fallback.jpg" alt="The Oslo Opera House"
    	sizes="(min-width: 640px) 60vw, 100vw"
    	srcset="opera-200.jpg 200w,
    			opera-400.jpg 400w,
    			opera-800.jpg 800w,
    			opera-1200.jpg 1200w">
    

Для окон браузеров с шириной в 640 CSS пикселов и шире используется фото с шириной 60% области просмотра; для браузров с меньшей шириной, width of 60% of the viewport width is used; for less wide browser windows, a 
photo with a width that is equal to the full viewport width is used. The browser
picks the optional image from a selection of images with widths of 200px, 400px,
800px and 1200px, keeping in mind image width and screen DPI.

## Changing image sizes & art direction use case {#changing-image-sizes--art-
direction-use-case
}

размеры dpi mime содержимое

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
    

For browser windows with a width of 1280 CSS pixels and wider, a full-shot
photo with a width of 50% of the viewport width is used; for browser windows 
with a width of 640-1279 CSS pixels, a photo with a width of 60% of the viewport
width is used; for less wide browser windows, a photo with a width that is equal
to the full viewport width is used. In each case, the browser picks the optional
image from a selection of images with widths of 200px, 400px, 800px and 1200px, 
keeping in mind image width and screen DPI.

## Changing image sizes & different image types use case {#changing-image-sizes
--different-image-types-use-case
}

размеры dpi mime содержимое

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
    

For browser windows with a width of 640 CSS pixels and wider, a photo with a
width of 60% of the viewport width is used; for less wide browser windows, a 
photo with a width that is equal to the full viewport width is used. The browser
picks the optional image from a selection of images with widths of 200px, 400px,
800px and 1200px, keeping in mind image width and screen DPI. These photos are 
served as WebP to browsers that support it; other browsers get JPG.

## Changing image sizes, different image types & art direction use case {#
changing-image-sizes-different-image-types--art-direction-use-case
}

размеры dpi mime содержимое

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
    

For browser windows with a width of 1280 CSS pixels and wider, a full-shot
photo with a width of 50% of the viewport width is used; for browser windows 
with a width of 640-1279 CSS pixels, a photo with a width of 60% of the viewport
width is used; for less wide browser windows, a photo with a width that is equal
to the full viewport width is used. In each case, the browser picks the optional
image from a selection of images with widths of 200px, 400px, 800px and 1200px, 
keeping in mind image width and screen DPI. These photos are served as WebP to 
browsers that support it; other browsers get JPG.

## Changing image sizes & high-DPI images use case {#changing-image-sizes--high
-dpi-images-use-case
}

размеры dpi mime содержимое

    <img
    	src="opera-fallback.jpg" alt="The Oslo Opera House"
    	sizes="(min-width: 640px) 60vw, 100vw"
    	srcset="opera-200.jpg 200w,
    			opera-400.jpg 400w,
    			opera-800.jpg 800w,
    			opera-1200.jpg 1200w,
    			opera-1600.jpg 1600w,
    			opera-2000.jpg 2000w">
    

For browser windows with a width of 640 CSS pixels and wider, a photo with a
width of 60% of the viewport width is used; for less wide browser windows, a 
photo with a width that is equal to the full viewport width is used. The browser
picks the optional image from a selection of images with widths of 200px, 400px,
800px, 1200px, 1600px and 2000px, keeping in mind image width and screen DPI.

## Changing image sizes, high-DPI images & art direction use case {#changing-
image-sizes-high-dpi-images--art-direction-use-case
}

размеры dpi mime содержимое

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
    

For browser windows with a width of 1280 CSS pixels and wider, a full-shot
photo with a width of 50% of the viewport width is used; for browser windows 
with a width of 640-1279 CSS pixels, a photo with a width of 60% of the viewport
width is used; for less wide browser windows, a photo with a width that is equal
to the full viewport width is used. In each case, the browser picks the optional
image from a selection of images with widths of 200px, 400px, 800px, 1200px, 
1600px and 2000px, keeping in mind image width and screen DPI.

## Changing image sizes, high-DPI images & different image types use case {#
changing-image-sizes-high-dpi-images--different-image-types-use-case
}

размеры dpi mime содержимое

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
    

For browser windows with a width of 640 CSS pixels and wider, a photo with a
width of 60% of the viewport width is used; for less wide browser windows, a 
photo with a width that is equal to the full viewport width is used. The browser
picks the optional image from a selection of images with widths of 200px, 400px,
800px, 1200px, 1600px and 2000px, keeping in mind image width and screen DPI. 
These photos are served as WebP to browsers that support it; other browsers get 
JPG.

## Changing image sizes, high-DPI images, different image types & art direction
use case {#changing-image-sizes-high-dpi-images-different-image-types--art-
direction-use-case
}

размеры dpi mime содержимое

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
    

For browser windows with a width of 1280 CSS pixels and wider, a full-shot
photo with a width of 50% of the viewport width is used; for browser windows 
with a width of 640-1279 CSS pixels, a photo with a width of 60% of the viewport
width is used; for less wide browser windows, a photo with a width that is equal
to the full viewport width is used. In each case, the browser picks the optional
image from a selection of images with widths of 200px, 400px, 800px, 1200px, 
1600px and 2000px, keeping in mind image width and screen DPI. These photos are 
served as WebP to browsers that support it; other browsers get JPG.

## There is more! {#there-is-more}

Don’t worry if you haven’t understood this fully! Soon, we’ll publish an
in-depth tutorial article on`<picture>` and responsive images. In the
meantime, enjoy preparing to save your boss’ and customers’ bandwidth, and 
making your site even more performant.

 [1]: https://bugzilla.mozilla.org/show_bug.cgi?id=870022
 [2]: https://bugs.webkit.org/show_bug.cgi?id=134634
 [3]: http://commons.wikimedia.org/wiki/File:Full_Opera_by_night.jpg
 [4]: img/opera-house.jpg