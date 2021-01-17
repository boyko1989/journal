# Прогресс-элементы

Тема про то как простые вещи могут запутать сложной терминологией.

На данный момент мной обнаружено следующее множество названий:

- Level Indicator
- Progress bar
- Preloader
- Progress Dialog

## Нюансы значений

### Level Indicator

Опять же, насколько понял я, Level Indicator — это общее название для всех элементов, описывающих сосотояние процесса.

Но в отличие от *Progress bar*, Level Indicator можно отнести скорее к визуализации статуса. То есть у нас есть какая-то величина, например громкость звука, и мы хотим изменить её значение. Мы можем это сделать введя абсолютное или относительное (напр. " -10 ") значение в виде чисел. Либо можем нажимать на кнопки "больше" или "меньше", а результат визуализировать в виде чисел. Также можем "движком" корректировать положение уровня на шкале. Вот как раз это и будет тот самый Level Indicator. 

Для большей наглядности к нему можно добавить числовое значение, но даже и без последнего он вполне нагляден. 

Как раз таки его можно сделать в виде валкодера: сделать его круглым; или в видеспидометра: сделать Level Indicator полукруглым.

Проходя определённе уровни, вся шкала или часть её может или градиентом или резкими переходами менять цвет (с зелёного до красного). Также, если есть желание "оживить" элемент, то можно добавить гуляющий блик как в стиле Windows 7 либо даже картинку. Однако это больше относится в *Progress bar*.

### Progress bar

На русском можно назвать "Индикатор процесса" или "Индикатор выполнения". Я предполагаю, что этот элемент относится больше к работе программ. [Здесь](https://css-tricks.com/html5-progress-element/) делятся эти элементы на ***неопределённые*** и ***определённые***.

Под **неорпеделёнными** предполагаются такие:

![Картинка из Википедии](https://upload.wikimedia.org/wikipedia/commons/5/53/Loading_bar.gif "Картинка из Википедии")

Такие индикаторы используются, когда доля выполненной работы не может быть определена по сравнению с масштабом  задачи.

В противном случае применяют **определённые** индикаторы, и они выглядят так:

![Картинка из Википедии](https://upload.wikimedia.org/wikipedia/commons/a/af/Progress_bar_gnome_ubuntu.png "Картинка из Википедии")

... или так:

![Картинка из Википедии](https://upload.wikimedia.org/wikipedia/commons/3/32/Progress_bar.png "Картинка из Википедии")

Для отображения этого элемента используется тег \<progress\>. Нужно проработать приёмы стилизации и придания динамимики этим элементам, которые описаны в следующих двух источниках:

- [Cross Browser HTML5 Progress Bars In Depth](http://www.useragentman.com/blog/2012/01/03/cross-browser-html5-progress-bars-in-depth/)
- [The HTML5 progress Element ](https://css-tricks.com/html5-progress-element/)

### Preloader

Классический прелоадер выглядит так:

![](img/preloader.gif)

Ну и понятно он нужен, когда использование Progress Bar будет слишком тяжеловесно, а "скрасить" ожидание пользователя чем-то нужно. Без предварительного загрузчика, особенно на сайтах с большими объемами контента, пользователь может решить, что сайт завис или не работает вообще. При таком решении он, в большинстве случаев, покинет сайт. 

Также, возможно страница сайта загружается не вся и красивая, а по частям, с прыгающими шрифтами, картинками и прочими непривлекательными эффектами, которые могут утомить пользователя и оставить о сайте неприятные впечатления.

На [этой](https://itchief.ru/javascript/how-to-make-preloader-for-site#id-2) странице есть несколько замечательных вариантов Прелоадеров. А [здесь](https://nisnom.com/preloadery-loader/) ещё 50 ссылок на интересные эффекты.

В конце отмечу, что в некоторых случая устанавливают вместо CSS анимации .GIF файлик, как, например в галерее Lightbox. А также привести пример весьма интересной анимации в виде кубиков:

<style>
/* Этот код для примера */
.site-container {
	position: relative;
	height: 300px;
	width: 100%;
}
/* Далее код прелоадера */
.loader {
	position: absolute; /* Заменить на fixed, чтобы прелоадер был на весь экран */
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background-color: #fff;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	z-index: 100;
}
.loader p {
	margin: 1em 0 0 0;
	font-size: 16px;
}
.logo {
	width: 100px;
	height: 100px;
	box-sizing: border-box;
	position: relative;
	background-color: white;
}
.logo::before,
.logo::after {
	z-index: 1;
	box-sizing: border-box;
	content: '';
	position: absolute;
	border: 4px solid transparent;
	width: 0;
	height: 0;
	animation-direction: alternate;
	animation-timing-function: linear;
}
.logo::before {
	top: 0;
	left: 0;
	animation: border-before 1.5s infinite;
	animation-direction: alternate;
}
.logo::after {
	bottom: 0;
	right: 0;
	animation: border-after 1.5s infinite;
	animation-direction: alternate;
}
.logo > div {
	position: absolute;
	opacity: 0;
}
.white {
	border-left: 4px solid #333;
	top: 0;
	bottom: 0;
	right: 0;
	width: 0;
	animation: white 1.5s infinite;
	animation-direction: alternate;
}
.orange {
	border-top: 4px solid #333;
	left: 0;
	bottom: 0;
	right: 0;
	height: 0;
	background-color: #1967c3;
	animation: orange 1.5s infinite;
	animation-direction: alternate;
}
.red {
	border-right: 4px solid #333;
	top: 0;
	bottom: 0;
	left: 0;
	width: 0;
	background-color: #EA5664;
	animation: red 1.5s infinite;
	animation-direction: alternate;
}
@keyframes border-before {
	0% {
		width: 0;
		height: 0;
		border-top-color: #333;
		border-right-color: transparent;
	}
	12.49% {
		border-right-color: transparent;
	}
	12.5% {
		height: 0;
		width: 100%;
		border-top-color: #333;
		border-right-color: #333;
	}
	25%,
	100% {
		width: 100%;
		height: 100%;
		border-top-color: #333;
		border-right-color: #333;
	}
}
@keyframes border-after {
	0%,
	24.99% {
		width: 0;
		height: 0;
		border-left-color: transparent;
		border-bottom-color: transparent;
	}
	25% {
		width: 0;
		height: 0;
		border-left-color: transparent;
		border-bottom-color: #333;
	}
	37.49% {
		border-left-color: transparent;
		border-bottom-color: #333;
	}
	37.5% {
		height: 0;
		width: 100%;
		border-left-color: #333;
		border-bottom-color: #333;
	}
	50%,
	100% {
		width: 100%;
		height: 100%;
		border-left-color: #333;
		border-bottom-color: #333;
	}
}
@keyframes red {
	0%,
	50% {
		width: 0;
		opacity: 0;
	}
	50.01% {
		opacity: 1;
	}
	65%,
	100% {
		opacity: 1;
		width: 27%;
	}
}
@keyframes orange {
	0%,
	65% {
		height: 0;
		opacity: 0;
	}
	65.01% {
		opacity: 1;
	}
	80%,
	100% {
		opacity: 1;
		height: 50%;
	}
}
@keyframes white {
	0%,
	75% {
		width: 0;
		opacity: 0;
	}
	75.01% {
		opacity: 1;
	}
	90%,
	100% {
		opacity: 1;
		width: 23%;
	}
}
</style>
<div class="site-container">
	<div class="loader">
		<div class="logo">
			<div class="white"></div>
			<div class="orange"></div>
			<div class="red"></div>
		</div>
		<p>Loading</p>
	</div>
</div>

### Progress Dialog

По большей части этот элемент относится к программам на Android. Это боло такое окошечко, в котором "крутился" Прелоадер и была надпись "Please wait..." Сейчас от этого отошли на мобильной технике. Однако значение этого термина в рамках рассмотрения Прогресс-элементов полезно.