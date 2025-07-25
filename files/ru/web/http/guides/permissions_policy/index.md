---
title: Feature Policy
slug: Web/HTTP/Guides/Permissions_Policy
---

{{SeeCompatTable}}

Feature Policy позволяет веб-разработчику выборочно включать, отключать и изменять поведение определённых функций и API в браузере. Это похоже на {{Glossary("CSP", "Content Security Policy")}}, но контролирует функции вместо политик безопасности.

## Краткое описание

Заголовок Feature Policy предоставляет механизм для ясного указания функций, используемых или не используемых вашим веб-сайтом. Это позволяет закрепить лучшие практики, даже если кодовая база развивается с течением времени, а также более безопасно включать сторонний контент, ограничивая доступные функции.

С помощью заголовка Feature Policy вы можете включить набор "политик" для браузера, чтобы использовать определённые функции, необходимые веб-сайту. Эти политики определяют какие API сайта могут получать доступ или изменять поведение по умолчанию для определённых функций.

Примеры того, что можно сделать с заголовком Feature Policy:

- Изменить поведение автозапуска видео на мобильных устройствах.
- Ограничить доступ сайта к камере и микрофону.
- Разрешить использование API полноэкранного режима в iframe.
- Блокировать использование устаревших API, например [synchronous XHR](/ru/docs/Web/API/XMLHttpRequest_API/Using_XMLHttpRequest) and {{domxref("document.write()")}}.
- Проверять соответствие размера изображений размерам области просмотра.

## Concepts and usage

Feature Policy allows you to control which origins can use which features, both in the top-level page and in embedded frames. Essentially, you write a policy, which is an allowed list of origins for each feature. For every feature controlled by Feature Policy, the feature is only enabled in the current document or frame if its origin matches the allowed list of origins.

For each policy-controlled feature, the browser maintains a list of origins for which the feature is enabled, known as an allowlist. If you do not specify a policy for a feature, then a default allowlist will be used. The default allowlist is specific to each feature.

### Writing a policy

A policy is described using a set of individual policy directives. A policy directive is a combination of a defined feature name, and an allowlist of origins that can use the feature.

### Specifying your policy

Feature Policy provides two ways to specify policies to control features:

- The {{httpheader('Feature-Policy')}} HTTP header.
- The {{HTMLElement('iframe','<code>allow</code>','#Attributes')}} attribute on iframes.

The primary difference between the HTTP header and the `allow` attribute is that the allow attribute only controls features within an iframe. The header controls features in the response and any embedded content within the page.

## Types of policy-controlled features

Though Feature Policy provides control of multiple features using a consistent syntax, the behavior of policy controlled features varies and depends on several factors.

The general principle is that there should be an intuitive or non-breaking way for web developers to detect or handle the case when the feature is disabled. Newly introduced features may have an explicit API to signal the state. Existing features that later integrate with Feature Policy will typically use existing mechanisms. Some approaches include:

- Return "permission denied" for JavaScript APIs that require user permission grants.
- Return `false` or error from an existing JavaScript API that provides access to feature.
- Change the default values or options that control the feature behavior.

The current set of policy-controlled features fall into two broad categories:

- Enforcing best practices for good user experiences.
- Providing granular control over sensitive or powerful features.

### Best practices for good user experiences

There are several policy-controlled features to help enforce best practices for providing good performance and user experiences.

In most cases, the policy-controlled features represent functionality that when used will negatively impact the user experience. To avoid breaking existing web content, the default for such policy-controlled features is to allow the functionality to be used by all origins. Best practices are then enforced by using policies that disable the policy-controlled features. For more details see "Enforcing best practices for good user experiences".

The features include:

- Layout-inducing animations
- Legacy image formats
- Oversized images
- Synchronous scripts
- Synchronous XMLHTTPRequest
- Unoptimized images
- Unsized media

### Granular control over certain features

The web provides functionality and APIs that may have privacy or security risks if abused. In some cases, you may wish to strictly limit how such functionality is used on a website. There are policy-controlled features to allow functionality to be enabled/disabled for specific origins or frames within a website. Where available, the feature integrates with the Permissions API, or feature-specific mechanisms to check if the feature is available.

The features include:

- Accelerometer
- Ambient light sensor
- Autoplay
- Camera
- Encrypted media
- Fullscreen
- Geolocation
- Gyroscope
- Lazyload
- Microphone
- Midi
- PaymentRequest
- Picture-in-picture
- Speaker
- USB
- VR / XR

## Examples

- See [Feature Policy Demos](https://feature-policy-demos.appspot.com/) for example usage of many policies.

## Спецификации

{{Specifications}}

## Совместимость с браузерами

{{Compat}}

## Смотрите также

- {{HTTPHeader("Feature-Policy")}} HTTP header
- {{HTMLElement('iframe','<code>allow</code>','#Attributes')}} attribute on iframes
- [Introduction to Feature Policy](https://developers.google.com/web/updates/2018/06/feature-policy)
- [Feature policies on www.chromestatus.com](https://www.chromestatus.com/features#component%3A%20Blink%3EFeaturePolicy)
- [Feature-Policy Tester (Chrome Developer Tools extension)](https://chrome.google.com/webstore/detail/feature-policy-tester-dev/pchamnkhkeokbpahnocjaeednpbpacop)
