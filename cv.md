# Nastassia Demchenko

## Contacts:
 - email: <vikimikirc@gmail.com>
 - telegram: Viki_Miki
 - phone: +375298704052

## Summary:
I chose front-end development because you see the result of my work and know how it works inside. Having studied a little information has become more interesting and I want to know more and more. There is still a huge amount of information to be studied, but I really want to do my best and will be an excellent specialist.

## Skills:
 - HTML5
 - CSS3
 - JavaScript
 - Delphi
 - SQL
 - SVN, VSS
 - FastReport

## Code examples
- [GitHub](https://github.com/VikiMiki)
- Implementation of the functional “slide-show” as part of the educational process:

```
funSlSh(document.querySelectorAll("#slt_blc")[0], 1);

function funSlSh(container, AllImgCount, imgsURL)
{
var timerID = null,
containerImg = container.children[1],
buttonLeft = container.children[2],
buttonRight = container.children[3],
containerImgLeft = -400,
containerImgStyle = containerImg.style,
numVisImg = 1,
activeButton;

var animationComplete = true;

var _imgHtml = '';
for (var i = 1; i < 8; ++i) {
	_imgHtml += '<img src="img/for_slt_'+i+'.jpg" class="image-'+i+'" >\r\n';
}
document.querySelector('#slt_blc #ssh').innerHTML = _imgHtml;

var sliderBlock = document.querySelector('#slt_blc');
var imagesBlockContainer = sliderBlock.querySelector('#ssh');
imagesBlockContainer.style.width = '280px';
imagesBlockContainer.style.height = '210px';
imagesBlockContainer.style.margin = '35% auto 0';

// получим все картинки
var images = imagesBlockContainer.querySelectorAll('img');

// пройдемся по всем картинкам и проставим зиндекс=20, (все-таки 30!!! исправила, еще думать!!!)
//так же попутно определим первый и предыдущий слайд, наверно и след тоже не помешает
	images.forEach(function(e, i){
		e.style.position = 'absolute';
		e.style.zIndex = 20;
		if( images.length == i+1 ){
			e.classList.add('previous');
			e.style.zIndex = 21;
		}
		if( i == 0 ){
			e.classList.add('current');
			e.style.zIndex = 30;			
		}
	});
		
	container.addEventListener("click", initClick, false);
	container.addEventListener("mouseover", initClick, false);
	container.addEventListener("mouseout", initClick, false);
	
	startAutoSlide();	
	function initClick(e) {
		activeButton = e.target;
		if (activeButton !== buttonLeft && activeButton !== buttonRight) return;
		switch (e.type) {
			case "click":
				if (!parseInt(containerImgStyle.left)) { // защита от "девиантных кликов"
					
					if (activeButton === buttonLeft) {
						PrevSlide();
					} else {
						NextSlide();
					}
				}
				break;
			case "mouseover":
				stopAutoSlide();
				TweenLite.to(activeButton, 0.3, {
					css: {
						opacity: 0.5
					}
				});
				break;
			case "mouseout":
				startAutoSlide();
				TweenLite.to(activeButton, 0.3, {
					css: {
						opacity: 0
					}
				});
				break;
		}
	}	
	// для текущего слайда зададим стили, повысим зиндекс а у всех остальных уберем его
	function PositionCurrent (){
	  	var currentSlide = imagesBlockContainer.querySelector('.current').style.zIndex = 30;
		var previousSlide = imagesBlockContainer.querySelector('.previous').style.zIndex = 21;
		imagesBlockContainer.querySelectorAll('img:not(.current):not(.previous)').forEach(function(e){
			e.style.zIndex = 20;
		});	
	}
    // обработка для показа след слайда
	function NextSlide (){
		if( !animationComplete ){
			return;
		}
		var currentSlide = imagesBlockContainer.querySelector('.current');
		var previewSlide = imagesBlockContainer.querySelector('.previous');
		// повысим зиндекс на след слайд, что бы при отезде показывался он
		var nextSlide = currentSlide.nextElementSibling;
		if (nextSlide===null)
		{
			nextSlide=imagesBlockContainer.firstElementChild;
		}
		//console.log(nextSlide);
		nextSlide.style.zIndex = 22;

		// получим просчитанные браузером значения левого отступа
		var marginLeftDefault = window.getComputedStyle(imagesBlockContainer).getPropertyValue("margin-left").match(/\d+/)[0];
		var leftOffset = marginLeftDefault - nextSlide.width;

		animationComplete = false;
		// запустим обработку анимации
		TweenLite.to(currentSlide, 1, {
			css: {
				left: leftOffset+'px'
			},
			// коллбек выполнения ф-ии, после того как закончилась 1 анимация(отезда влево), пересчитаем все и запустим след шаг
			onComplete: function() {
				currentSlide.classList.remove('current');
				nextSlide.classList.add('current');
				previewSlide.classList.remove('previous');
				currentSlide.classList.add('previous');
				PositionCurrent();

				TweenLite.to(currentSlide, 1, {css: {left: marginLeftDefault+'px'}, 
				onComplete: function(){
					animationComplete = true;
				}});
			}
		});
	}
	
		function PrevSlide(){
		if( !animationComplete ){
			return;
		}
		var currentSlide = imagesBlockContainer.querySelector('.current');
		var previewSlide = imagesBlockContainer.querySelector('.previous');
		var nextSlide = previewSlide.previousElementSibling;
		if (nextSlide===null)
		{
			nextSlide=imagesBlockContainer.lastElementChild;
		}

		var marginLeftDefault = window.getComputedStyle(imagesBlockContainer).getPropertyValue("margin-left").match(/\d+/)[0];
		var leftOffset = marginLeftDefault - nextSlide.width;

		animationComplete = false;
		TweenLite.to(previewSlide, 1, {
			css: {
				left: leftOffset+'px'
			},
			onComplete: function() {
				currentSlide.classList.remove('current');
				previewSlide.classList.remove('previous');
				previewSlide.classList.add('current');
				currentSlide.classList.add('previous');
				PositionCurrent();
				
				TweenLite.to(previewSlide, 1, {css: {left: marginLeftDefault+'px'}, onComplete: function(){
					currentSlide.classList.remove('previous');
					nextSlide.classList.add('previous');
					PositionCurrent();
					animationComplete = true;
				}});
			}
		});
	}
	
	// автоматическая смена картинок
	function startAutoSlide() {
		if (!timerID) {
			timerID = setInterval(function() {
			NextSlide();			
			}, 3000);
		}
	}
	
	// выключение автоматической смены картинок
	function stopAutoSlide() {
		if (timerID) {
			clearInterval(timerID);
			timerID = null;
		}
	}	
}
```

## Education:
 - **IIT BSUIR**, retraining course in "Web-designer"
 - Start training in **"Rolling Scopes School"**

## English:
A2.
I read various articles and watch videos in order to improve my English. I took lessons from a tutor and use BBC Learning English.


