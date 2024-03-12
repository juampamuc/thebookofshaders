## Hello World

Зазвичай програма "Hello world!" — це перший крок до вивчення нової мови. Це проста програма, яка виводить вітальне повідомлення та заявляє про майбутні можливості.

У GPU-світі рендеринг тексту є надскладним завданням для першого кроку. Натомість щоб висловити свій ентузіазм, ми виберемо та намалюємо яскравий колір!

<div class="codeAndCanvas" data="hello_world.frag"></div>

Якщо ви читаєте цю книгу в браузері, попередній блок коду є інтерактивним. Це означає, що ви можете змінити будь-яку частину коду, яку захочете. Зміни будуть оновлені негайно завдяки архітектурі GPU, яка компілює та замінює шейдери *на льоту*. Спробуйте змінити значення в рядку 8.

Хоча ці прості рядки коду не виглядають складними, ми можемо вивести з них суттєві знання:

1. Мова шейдерів має одну функцію `main`, яка повертає колір у кінці. Це схоже на мову C.

2. Остаточний колір пікселя записується у зарезервовану глобальну змінну `gl_FragColor`.

3. У цій C-подібній мові є вбудовані *змінні* (наприклад, `gl_FragColor`), *функції* і *типи*. У цьому випадку ми щойно познайомилися з `vec4`, який означає чотиривимірний вектор числових значень з рухомою крапкою. Пізніше ми побачимо більше типів, таких як `vec3` і `vec2` разом із популярними `float`, `int` і `bool`.

4. Якщо ми уважно подивимося на тип `vec4`, то можемо зробити висновок, що його чотири аргументи відповідають ЧЕРВОНОМУ, ЗЕЛЕНОМУ, СИНЬОМУ та АЛЬФА-каналам. Також ми бачимо, що ці значення *нормалізовані* - це означає, що вони знаходяться в діапазоні від `0.0` до `1.0`. Пізніше ми дізнаємося, як нормалізація полегшує *зіставлення* значень між змінними.

5. Інша важлива *C-особливість*, яку ми можемо побачити в цьому прикладі, вказує на наявність макросів препроцесора. Макроси є частиною етапу попередньої компіляції. З їх допомогою можна визначити (`#define`) глобальні змінні та виконати деякі базові умовні операції, використовуючи `#ifdef` і `#endif`. Усі макрокоманди починаються з символу решітки `#`. Пре-компіляція відбувається безпосередньо перед компіляцією, підставляє всі визначення із директив `#defines` і перевіряє умови `#ifdef` (якщо визначено) і `#ifndef` (якщо не визначено). У наведеному вище прикладі "hello world!", ми вставляємо рядок 2, лише якщо визначено `GL_ES`, що здебільшого відбувається, коли код компілюється на мобільних пристроях і в браузерах.

6. Типи чисел з рухомою крапкою життєво важливі в шейдерах, тому рівень *точності* є вирішальним. Нижча точність означає швидший рендеринг, але ціною якості. Ви можете бути вибагливими та вказати точність кожної такої змінної, яку використовує. У другому рядку (`precision mediump float;`) ми встановлюємо середню точність для всіх значень з рухомою крапкою. Також можна вибрати низьку (`precision lowp float;`) або високу (`precision highp float;`) точність.

7. Остання і, мабуть, найважливіша деталь полягає в тому, що специфікації GLSL не гарантують автоматичного приведення типів. Що це означає? Виробники мають різні підходи до прискорення роботи відеокарт, але вони змушені гарантувати мінімальні вимоги. Автоматичне приведення типів не відноситься до них. У нашому прикладі `vec4` має містити число з рухомою крапкою на яке й очікує. Якщо ви хочете писати якісний узгоджений код і не витрачати години на відладку білих екранів, звикніть ставити крапку (`.`) для значень з рухомою крапкою. Бо наступний код працюватиме не завжди та не скрізь:

```glsl
void main() {
    gl_FragColor = vec4(1, 0, 0, 1);	// ПОМИЛКА
}
```

Тепер, коли ми описали найбільш релевантні елементи нашої програми "hello world!", настав час повернутися до блоку з кодом та почати застосовувати отримані знання. Ви помітите, що в разі помилок програма не компілюється, показуючи білий екран. Є кілька речей, які можна спробувати, наприклад:

* Спробуйте замінити числа з рухомою крапкою на цілі числа. Ваша графічна карта може підтримувати або не підтримувати таку поведінку.

* Спробуйте закоментувати рядок 8 і не призначати пікселю жодного значення.

* Спробуйте створити окрему функцію, яка повертає певний колір, і використайте її всередині `main()`. Нижче приклад функції, яка повертає червоний колір:

```glsl
vec4 red() {
    return vec4(1.0, 0.0, 0.0, 1.0);
}
```

* Існує кілька способів конструювання типу `vec4`, спробуйте знайти інші способи. Ось один з них:

```glsl
vec4 color = vec4(vec3(1.0, 0.0, 1.0), 1.0);
```

Це не дуже захопливий, але найпростіший приклад - ми змінюємо всі пікселі всередині полотна на той самий однаковий колір. У наступному розділі ми побачимо, як змінити кольори пікселів за допомогою двох типів вхідних даних: просторового (положення пікселя на екрані) і часового (кількість секунд після завантаження сторінки).