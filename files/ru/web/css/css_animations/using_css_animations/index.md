---
title: Использование CSS-анимации
slug: Web/CSS/CSS_Animations/Using_CSS_animations
tags:
  - Advanced
  - CSS
  - CSS Animations
  - Example
  - Experimental
  - Guide
translation_of: Web/CSS/CSS_Animations/Using_CSS_animations
original_slug: Web/CSS/CSS_Animations/Ispolzovanie_CSS_animatciy
---
{{SeeCompatTable}}{{CSSRef}}

CSS-анимации позволяют анимировать переходы от одной конфигурации CSS стилей к другой. CSS-анимации состоят из двух компонентов: стилевое описание анимации и набор ключевых кадров, определяющих начальное, конечное и, возможно, промежуточное состояние анимируемых стилей.

Есть три преимущества CSS-анимации перед традиционными способами:

1. Простота использования для простых анимаций; вы можете создать анимацию, не зная JavaScript.
2. Анимации будут хорошо работать даже при умеренных нагрузках системы. Простые анимации на JavaScript, если они плохо написаны, часто выполняются плохо. Движок может использовать frame-skipping и другие техники, чтобы сохранить производительность на таком высоком уровне .
3. Позволяет браузеру контролировать последовательность анимации, тем самым оптимизируя производительность и эффективность браузера. Например, уменьшая частоту обновления кадров анимации в непросматриваемых в данный момент вкладках.

## Конфигурирование анимации

Чтобы создать CSS-анимацию вы должны добавить в стиль элемента, который хотите анимировать, свойство {{ cssxref("animation") }} или его подсвойства. Это позволит вам настроить ускорение и продолжительность анимации, а также другие детали того, как анимация должна протекать. Это не поможет вам настроить внешний вид анимации, который настраивается с помощью {{ cssxref("@keyframes") }}, рассматриваемой далее в [Определение последовательности анимации с помощью ключевых кадров](#определение_последовательности_анимации_с_помощью_ключевых_кадров).

Свойство {{ cssxref("animation") }} имеет следующие подсвойства:

- {{ cssxref("animation-name") }}
  - : Определяет имя {{ cssxref("@keyframes") }}, настраивающего кадры анимации.
- {{ cssxref("animation-duration") }}
  - : Определяет время, в течение которого должен пройти один цикл анимации.
- {{ cssxref("animation-timing-function") }}
  - : Настраивает ускорение анимации.
- {{ cssxref("animation-delay") }}
  - : Настраивает задержку между временем загрузки элемента и временем начала анимации.
- {{ cssxref("animation-iteration-count") }}
  - : Определяет количество повторений анимации; вы можете использовать значение `infinite` для бесконечного повторения анимации.
- {{ cssxref("animation-direction") }}
  - : Даёт возможность при каждом повторе анимации идти по альтернативному пути, либо сбросить все значения и повторить анимацию.
- {{ cssxref("animation-fill-mode") }}
  - : Настраивает значения, используемые анимацией, до и после исполнения.
- {{ cssxref("animation-play-state") }}
  - : Позволяет приостановить и возобновить анимацию.

## Определение последовательности анимации с помощью ключевых кадров

После того, как вы настроили временные свойства (продолжительность, ускорение) анимации, вы должны определить внешний вид анимации. Это делается с помощью двух и более ключевых кадров после {{ cssxref("@keyframes") }}. Каждый кадр описывает, как должен выглядеть анимированный элемент в текущий момент.

В то время, как временные характеристики (продолжительность анимации) указываются в стилях для анимируемого элемента, ключевые кадры используют {{ cssxref("percentage") }}, чтобы определить стадию протекания анимации. 0% означает начало анимации, а 100% её конец. Так как эти значения очень важны, то для них придумали специальные слова: `from` и `to`.

Вы также можете добавить ключевые кадры, характеризующие промежуточное состояние анимации.

## Примеры

> **Примечание:** **Внимание:** Примеры ниже не используют префиксов для CSS стилей . Webkit-браузеры и старые версии других браузеров нуждаются в указании префиксов в CSS стилях. Примеры, на которые вы можете кликнуть в своём браузере, также содержат префиксы -webkit-.

### Скольжение текста

Этот простой пример анимирует скольжение текста в элементе {{ HTMLElement("p") }} от правого края окна браузера.

Обратите внимание на то, что анимация может сделать страницу шире, чем окно браузера. Этого можно избежать, поместив элемент, который будет анимироваться, в контейнер и установив ему свойство {{cssxref("overflow")}}`: hidden`.

```css
p {
  animation-duration: 3s;
  animation-name: slidein;
}

@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
```

В стиле для элемента {{ HTMLElement("p") }} с помощью свойства {{ cssxref("animation-duration") }} указано, что исполнение анимации от начала до конца должно занять 3 с , и что имя для {{ cssxref("@keyframes") }}, описывающей саму анимацию, определено как "slidein".

В элемент {{ HTMLElement("p") }} можно добавлять и другие пользовательские стили, чтобы как-то украсить его, однако здесь мы хотели продемонстрировать только эффект анимации.

Ключевые кадры определяются с помощью правила {{ cssxref("@keyframes") }}. В данном случае мы имеем только два ключевых кадра. Первый при 0% анимации (`from`). Здесь мы придаём элементу левый отступ в 100% и ширину в 300% (в три раза больше ширины родительского элемента). Это становится причиной того, что при первом кадре анимации заголовок {{ HTMLElement("p") }} находится за пределами правого края окна браузера .

Второй ключевой кадр (to) определяет конец анимации, т.е (100%). Левый отступ устанавливается равным 0, а ширина 100%. Все выглядит так, будто заголовок {{ HTMLElement("p") }} приплывает к левому краю окна браузера.

```html
<p>The Caterpillar and Alice looked at each other for some time in silence:
at last the Caterpillar took the hookah out of its mouth, and addressed
her in a languid, sleepy voice.</p>
```

(Обновите страницу, чтобы увидеть анимацию, или щёлкните по кнопке CodePen, чтобы воспроизвести её в окне CodePen)

{{EmbedLiveSample("Скольжение_текста","100%","250")}}

### Добавление других ключевых кадров

Давайте добавим другие ключевые кадры в предыдущий пример. Скажем, мы хотим чтобы размер шрифта заголовка временно увеличивался по мере продвижения влево, а потом возвращался к первоначальному значению . Это легко реализовать с помощью следующего ключевого кадра:

```css
75% {
  font-size: 300%;
  margin-left: 25%;
  width: 150%;
}
```

```css
p {
  animation-duration: 3s;
  animation-name: slidein;
}

@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }

  75% {
    font-size: 300%;
    margin-left: 25%;
    width: 150%;
  }
}
```

```html
<p>The Caterpillar and Alice looked at each other for some time in silence:
at last the Caterpillar took the hookah out of its mouth, and addressed
her in a languid, sleepy voice.</p>
```

Это говорит браузеру о том, что при 75% выполнения анимации, шрифт должен быть 300%, а ширина 150%.

(Обновите страницу, чтобы увидеть анимацию, или щёлкните по кнопке CodePen, чтобы воспроизвести её в окне CodePen)

{{ EmbedLiveSample('Добавление_других_ключевых_кадров', '100%', '250', '', 'Web/CSS/CSS_Animations/Ispolzovanie_CSS_animatciy') }}

### Настройка повторения

Чтобы настроить повторение, нужно добавить свойство {{ cssxref("animation-iteration-count") }} и задать ему значение, равное нужному количеству повторений анимаций . В данном случае давайте установим значение `infinite` для бесконечного повторения:

```css
p {
  animation-duration: 3s;
  animation-name: slidein;
  animation-iteration-count: infinite;
}
```

```css
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
```

```html
<p>The Caterpillar and Alice looked at each other for some time in silence:
at last the Caterpillar took the hookah out of its mouth, and addressed
her in a languid, sleepy voice.</p>
```

{{EmbedLiveSample("Настройка_повторения","100%","250")}}

### Движение текста вправо и влево

Итак, мы настроили повторение, но получили нечто странное: текст при каждом повторении снова "запрыгивает" за край окна браузера. То, чего мы хотим, так это чтобы текст двигался влево и вправо. Этого легко достичь с помощью установки свойству {{ cssxref("animation-direction") }} значения `alternate`:

```css
p {
  animation-duration: 3s;
  animation-name: slidein;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
```

```css
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
```

```html
<p>The Caterpillar and Alice looked at each other for some time in silence:
at last the Caterpillar took the hookah out of its mouth, and addressed
her in a languid, sleepy voice.</p>
```

{{ EmbedLiveSample('Движение_текста_вправо_и_влево', '100%', '250', '', 'Web/CSS/CSS_Animations/Ispolzovanie_CSS_animatciy') }}

### Использование шорткодов

Шорткод {{cssxref("animation")}} полезен для экономии места в коде. Например, правило, которое мы используем в этой статье:

```css
p {
  animation-duration: 3s;
  animation-name: slidein;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
```

можно заменить на:

```css
p {
  animation: 3s infinite alternate slidein;
}
```

> **Примечание:** **Внимание**: подробнее об этом на странице раздела {{cssxref("animation")}}

### Установка нескольких значений свойствам анимации

CSS-свойство анимации может иметь несколько значений, разделённых запятыми. Это используется, чтобы указать несколько значений анимации в одном правиле и установить разную продолжительность, число повторений и т.д., для различных анимаций. Рассмотрим несколько примеров, чтобы увидеть разницу.

В первом примере у свойства имени анимации установлены три значения, у свойств продолжительности и количества повторений — по одному. В этом случае у всех трёх анимаций одинаковая продолжительность и число повторений:

```css
animation-name: fadeInOut, moveLeft300px, bounce;
animation-duration: 3s;
animation-iteration-count: 1;
```

Во втором примере установлены три значения для каждого из свойств. В этом случае каждая анимация выполняется с соответствующими по порядку значениями в каждом свойстве, так, например, `fadeInOut` имеет продолжительность 2.5 с и количество повторений 2, и т.д.

```css
animation-name: fadeInOut, moveLeft300px, bounce;
animation-duration: 2.5s, 5s, 1s;
animation-iteration-count: 2, 1, 5;
```

В третьем примере определены три значения имени анимации, но два значения продолжительности и количества повторений. В случае, когда количества значений недостаточно для каждой анимации, значения берутся циклически от начала до конца. Например, у `fadeInOut` длительность будет 2.5s, а `moveLeft300px` — 5s. Значения продолжительности закончились, теперь они берутся сначала — `bounce` получит продолжительность 2.5s. Значение количества повторений (а также другие указанные свойства) будет определено таким же образом.

```css
animation-name: fadeInOut, moveLeft300px, bounce;
animation-duration: 2.5s, 5s;
animation-iteration-count: 2, 1;
```

### Использование событий анимации

Вы можете получить дополнительный контроль над анимацией, а также полезную информацию о ней, с помощью _событий анимации_. Эти события, представленные объектом {{ domxref("event/AnimationEvent", "AnimationEvent") }}, можно использовать, чтобы определить, когда начинается и заканчивается анимация или начинается новая итерация. Каждое событие содержит момент времени, когда оно произошло, а также имя анимации, которая вызвала событие.

Мы будем модифицировать текст, чтобы выводить некоторую информацию о каждом событии анимации. Так мы сможем увидеть, как она работает.

#### Добавление CSS

Начнём с добавления CSS. Анимация будет длиться 3 секунды, будет называться "slidein", будет повторяться 3 раза, а также значение animation-direction установлено alternate. В ключевых кадрах {{ cssxref("@keyframes") }} установлены такие значения ширины и левого отступа, что элемент будет скользить по экрану.

```css
.slidein {
  -moz-animation-duration: 3s;
  -webkit-animation-duration: 3s;
  animation-duration: 3s;
  -moz-animation-name: slidein;
  -webkit-animation-name: slidein;
  animation-name: slidein;
  -moz-animation-iteration-count: 3;
  -webkit-animation-iteration-count: 3;
  animation-iteration-count: 3;
  -moz-animation-direction: alternate;
  -webkit-animation-direction: alternate;
  animation-direction: alternate;
}

@-moz-keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}

@-webkit-keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%
  }

  to {
   margin-left: 0%;
   width: 100%;
 }
}

@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%
  }

  to {
   margin-left: 0%;
   width: 100%;
 }
}
```

#### Добавление обработчика события анимации

Будем использовать JavaScript для отслеживания всех трёх возможных событий анимации. Следующий код конфигурирует обработчик; мы вызываем его при первой загрузке документа.

```js
var e = document.getElementById("watchme");
e.addEventListener("animationstart", listener, false);
e.addEventListener("animationend", listener, false);
e.addEventListener("animationiteration", listener, false);

e.className = "slidein";
```

Это довольно стандартный код; вы можете получить дополнительную информацию в документации {{ domxref("element.addEventListener()") }}. Последнее, что делает этот код - это установка класса "slidein" для анимируемого элемента; мы делаем это, чтобы запустить анимацию.

Почему? Потому что в нашем случае событие `animationstart` происходит как только анимация стартует, и это происходит раньше, чем исполняется наш сценарий. Так мы сможем контролировать начало анимации самостоятельно посредством вставки класса "slidein" для анимируемого элемента.

#### Регистрация событий

События будут передаваться функции `listener()`, показанной ниже.

```js
function listener(e) {
  var l = document.createElement("li");
  switch(e.type) {
    case "animationstart":
      l.innerHTML = "Started: elapsed time is " + e.elapsedTime;
      break;
    case "animationend":
      l.innerHTML = "Ended: elapsed time is " + e.elapsedTime;
      break;
    case "animationiteration":
      l.innerHTML = "New loop started at time " + e.elapsedTime;
      break;
  }
  document.getElementById("output").appendChild(l);
}
```

Этот код также очень прост. Этот код следит за {{ domxref("event.type") }}, чтобы определить тип события, и добавляет элемент {{ HTMLElement("ul") }}, чтобы залогировать произошедшее событие.

Вывод, когда анимация закончится, будет выглядеть примерно следующим образом:

- Started: elapsed time is 0
- New loop started at time 3.01200008392334
- New loop started at time 6.00600004196167
- Ended: elapsed time is 9.234000205993652

Обратите внимание, что время, указанное в выводе, и время, которое мы указали в стилях, не совпадают. Также обратите внимание, что после окончания итерации не посылается событие `animationiteration` ; вместо него посылается событие `animationend`.

#### HTML

Ради полноты картины приведём код разметки HTML. В разметке имеется тег _ul,_ в который и выводится вся информация:

```html
<body>
  <h1 id="watchme">Watch me move</h1>
  <p>This example shows how to use CSS animations to make <code>p</code> elements
  move across the page.</p>
  <p>In addition, we output some text each time an animation event fires, so you can see them in action.</p>
  <ul id="output">
  </ul>
</body>
```

{{ EmbedLiveSample('Использование_событий_анимации', '600', '300')}}

## Смотрите также

- {{ domxref("AnimationEvent", "AnimationEvent") }}
- [Определение поддержки CSS-анимации](/ru/docs/CSS/CSS_animations/Detecting_CSS_animation_support)
- [Использование CSS-переходов](/ru/docs/Web/Guide/CSS/Using_CSS_transitions)
