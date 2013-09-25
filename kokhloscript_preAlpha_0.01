// ==UserScript==
// @name Kokhloscript
// @description Replaces 'MOVA'. Currently supported by Chrome with Dollchan Ext. Tools
// @author pipenza
// @license MIT
// @version 0.01
// @include http://2ch.*
// ==/UserScript==

// Десять лет назад я изучал Borland Turbo Pascal. С тех пор это первая моя попытка изучить что-то еще.

// Оборачиваем скрипт в замыкание, для кроссбраузерности (opera, ie)
// Кроссбраузерность пока не реализована в полном объеме.
(function (window, undefined) { // нормализуем window
var w;
if (typeof unsafeWindow != undefined) {
	w = unsafeWindow
} else {
	w = window;
}
// не запускаем скрипт во фреймах
// без этого условия скрипт будет запускаться несколько раз на странице с фреймами
if (w.self != w.top) {return;}
// дополнительная проверка наряду с @include
if (~w.location.href.indexOf("2ch.")) {
	
		//Ниже идёт непосредственно код скрипта
		
/*********** IMPLEMENTATION ***********/

		function childrenList(children) { // вывод списка дочерних узлов объекта, вспомогательная функция, которая нужна для отладки.
			var list = '';
			for(var i=0; i<children.length; i++) {
				list = list + i + ". type:" + children[i].nodeType + ", tag: " + children[i].tagName + ", name: " + children[i].nodeName + ", id: " + children[i].id + "\n";
			}
		return(list);
		}

/**********/

		function oink(txt, symbol) { // соль скрипта

			function replaceWord(txt, symbol) { // функция определяет начало и конец слова, содержащего symbol, в строке txt, и заменяет его на "хрю".
				var k = 0; //k - курсор, текущая позиция в нашем тексте
				var first, last; //первый и последний символы в слове
				while (~txt.indexOf(symbol)) {
					k = txt.indexOf(symbol);
					while (!((txt.charCodeAt(k) > 31) && (txt.charCodeAt(k) < 65)) && (k > -1)) {k--;} // отматываем по тексту назад, пока не встретим любой знак препинания или пробел, вплоть до начала строки
					first = ++k;
					while (!((txt.charCodeAt(k) > 31) && (txt.charCodeAt(k) < 65)) && (k < txt.length)) {k++;} // теперь то же самое в другую сторону
					last = k;
					txt = txt.substring(0, first) + "хрю" + txt.substring(last, txt.length);
				}
				return(txt);
			}
			
			/*begin*/
			
			if ((symbol == 'i') || (symbol == 'I') || (symbol == 'і')) { // это не ошибка, см. ремарку в основном теле скрипта
				var tempText = '';
				var k = 0;
				while(~txt.indexOf(symbol)) {
					k = txt.indexOf(symbol);
					if (((txt.charCodeAt(k-1) > 1039) && (txt.charCodeAt(k-1) < 1112)) || ((txt.charCodeAt(k+1) > 1039) && (txt.charCodeAt(k+1) < 1112))) { // хотя бы один соседний символ - кириллица
						txt = replaceWord(txt, symbol);
					} else {
						// убираем из txt html-тэги и латинские слова, содержащие 'i', чтобы искать дальше
						tempText = tempText + txt.substring(0, k+1);
						txt = txt.substring(k+1, txt.length);
					}
				} 
				txt = tempText + txt; // а теперь возвращаем их на место
			} else {
				txt = replaceWord(txt, symbol); // это для всех символов хохлятского алфавита, кроме 'i'
			}	
			return(txt);
		}

/*********** BEGIN ***********/
//	var exec = confirm('ДОСМОТ?'); // отладочный вопрос
	var exec = true;
	if (exec) {
		var thread = document.body.children;
		for(var i=0; i<thread.length; i++) { // ищем форму в корне BODY, т.к. document.forms содержит вообще все формы на всех уровнях вложенности, и они выглядят одинаково.
			if (thread[i].nodeName == "FORM") {
				thread = thread[i].firstChild.children; //записываем в thread содержимое формы, минуя уровень, содержащий лишнюю для нас инфу
				break; // гарантирует работу с самой первой встреченной здесь формой и прерывает цикл, не допуская лишних итераций. Похоже, здесь уместнее while.
			}
		}
		var post, txt;
		for(i=1; i<thread.length; i++) { // перебираем посты, кроме оп-поста
			post = thread[i].firstChild.firstChild.firstChild.children; //прыгаем сразу через несколько уровней, это особенность форматирования треда в хроме текущей версии (с работающим куклоскриптом, если это важно)
			for(var j=0; j<post.length; j++) { // ищем в посте элемент с собственно текстом поста
				if ((post[j].nodeName == "BLOCKQUOTE") && (post[j].firstChild != null)) { //(пропускаем посты без текста)
					txt = post[j].firstChild.innerHTML; //вот с этим мы будем работать
					txt = oink(txt, 'ї');
					txt = oink(txt, 'є');
					txt = oink(txt, 'ґ');
					txt = oink(txt, 'Ï');
					txt = oink(txt, 'Є');
					txt = oink(txt, 'Ґ');
					txt = oink(txt, 'i');
					txt = oink(txt, 'і'); // такое ощущение, что у этого символа не такой код, как у предыдущего, хотя выглядят они одинаково.
					txt = oink(txt, 'I');
					post[j].firstChild.innerHTML = txt; // пишем исправленный текст обратно в документ.
				}
			}
		}
	}
}
})(window);
