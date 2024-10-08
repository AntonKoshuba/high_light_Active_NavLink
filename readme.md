Этот скрипт на JavaScript используется для реализации активного состояния ссылки в навигации, в зависимости от того, какой раздел страницы в данный момент виден пользователю.

# Подробное объяснение скрипта:

1. Событие DOMContentLoaded:

<pre>document.addEventListener('DOMContentLoaded', function () {</pre>

Этот код выполняется, когда вся структура HTML загружена и обработана, но до загрузки внешних ресурсов (например, изображений). Это гарантирует, что скрипт будет работать с элементами, которые уже доступны на странице.

2. Поиск ссылок и секций:

<pre>const links = document.querySelectorAll('nav ul li a'); 
const sections = document.querySelectorAll('section');</pre>

Здесь скрипт находит все ссылки в меню навигации (предполагается, что они находятся в структуре nav > ul > li > a) и все секции (section) на странице. Эти элементы будут использоваться для определения текущего активного раздела.

3. Функция setActiveLink:

<pre><code>function setActiveLink() {
    let currentSection = '';
    sections.forEach(section => {
        const sectionTop = section.offsetTop;
        const sectionHeight = section.clientHeight;
        if (pageYOffset >= sectionTop - sectionHeight / 3) {
            currentSection = section.getAttribute('id');
        }
    });
</code></pre>

Эта функция вычисляет, какая секция в данный момент видна пользователю:

- Перебираются все секции страницы.
- Для каждой секции вычисляется её верхняя граница (sectionTop) и высота (sectionHeight).
- Если текущая позиция прокрутки страницы (pageYOffset) находится выше, чем 1/3 высоты текущей секции, то эта секция считается активной, и её ID сохраняется в переменной currentSection.

4. Обновление активной ссылки:

<pre>links.forEach(link => {
    link.classList.remove('active');
    if (link.getAttribute('href') === `#${currentSection}`) {
        link.classList.add('active');
    }
});
</pre>

Этот код проходит по всем ссылкам навигации:

- Сначала удаляет класс active у всех ссылок.
- Затем добавляет класс active той ссылке, которая соответствует текущей активной секции (сравнивается атрибут href ссылки с ID активной секции).

5. Добавление слушателя события прокрутки:

<pre>window.addEventListener('scroll', setActiveLink);
setActiveLink();</pre>

- На событие прокрутки (scroll) добавляется обработчик, который вызывает функцию setActiveLink, чтобы обновлять активную ссылку при изменении положения страницы.
- Функция setActiveLink также вызывается один раз при загрузке страницы, чтобы правильно отметить активную ссылку на старте.

**Итог:**

Этот скрипт позволяет автоматически выделять активную ссылку в навигации, когда пользователь прокручивает страницу. Это обеспечивает удобство для пользователя, так как он всегда видит, в каком разделе сайта находится.

Скрипт целиком:
<pre>

        document.addEventListener('DOMContentLoaded', function () {
            const links = document.querySelectorAll('nav ul li a');
            const sections = document.querySelectorAll('section');

            function setActiveLink() {
                let currentSection = '';
                sections.forEach(section => {
                    const sectionTop = section.offsetTop;
                    const sectionHeight = section.clientHeight;
                    if (pageYOffset >= sectionTop - sectionHeight / 3) {
                        currentSection = section.getAttribute('id');
                    }
                });

                links.forEach(link => {
                    link.classList.remove('active');
                    if (link.getAttribute('href') === `#${currentSection}`) {
                        link.classList.add('active');
                    }
                });
            }

            window.addEventListener('scroll', setActiveLink);
            setActiveLink(); // Initial call to highlight the correct link on page load
        });
</pre>