---
title: Node.innerText
slug: Web/API/HTMLElement/innerText
translation_of: Web/API/HTMLElement/innerText
original_slug: Web/API/Node/innerText
---
{{APIRef("DOM")}}

**`Node.innerText`** - это свойство, позволяющее задавать или получать текстовое содержимое элемента и его потомков. В качестве геттера, свойство приближается к тексту, который пользователь получит, если он выделит содержимое элемента курсором, затем копирует его в буфер обмена.

Изначально, данное поведение было представлено Internet Explorer, и было формально специализированно в стандарте HTML в 2016 после того, как было адаптировано всеми ведущими браузерами.

{{domxref("Node.textContent")}} - это альтернативное свойство, которое имеет ряд отличий:

- `textContent` получает содержимое _всех_ элементов, включая [`<script>`](/ru/docs/Web/HTML/Element/script "This article hasn't been written yet. Please consider contributing!") и [`<style>`](/ru/docs/Web/HTML/Element/style "HTML-элемент <style> содержит стилевую информацию для документа или его части. По умолчанию стилевые инструкции внутри этого элемента считаются написанными на CSS."), тогда как `innerText` этого не делает.
- `innerText` умеет считывать стили и не возвращает содержимое скрытых элементов, тогда как `textContent` этого не делает.
- Метод `innerText` позволяет получить CSS, а `textContent` — нет.

## Спецификация

{{Specifications}}

## Поддержка браузерами

{{Compat}}

## Смотрите также

- {{domxref("HTMLElement.outerText")}}
- {{domxref("Element.innerHTML")}}
