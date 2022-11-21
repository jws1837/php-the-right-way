---
title:   보조적인 테스트 도구
isChild: true
anchor:  complementary_testing_tools
---

## 보조적인 테스트 도구 {#complementary_testing_tools_title}

테스트 주도 개발이나 행위 주도 개발 프레임워크들 외에도, 어떤 기법을 사용하는 프로젝트에든지 일반적으로 사용될 수 있는
프레임워크나 헬퍼 라이브러리들이 있습니다.

### 도구들 링크

* [Selenium]은 [PHPUnit과 연동하여 사용할 수도 있는][integrated with PHPUnit] 웹브라우저 자동화 도구입니다.
* [Mockery]는 [PHPUnit]이나 [PHPSpec]과 연동하여 사용할 수 있는 Mock Object 프레임워크입니다.
* [Prophecy]는 강력하고 유연한 PHP용 mock 프레임워크입니다. [PHPSpec]과 연동하거나 [PHPUnit]과 함께 사용할 수 있습니다.
* [php-mock]는 자체 함수를 mock 할 수 있게 도와주는 라이브러리입니다.
* [Infection]는 여러분의 테스트 효과성을 측정하는데 도움을 주는 [Mutation Testing]의 PHP 구현입니다.
* [PHPUnit Polyfills]는 테스트 스위트가 다양한 PHPUnit 버전에 대해 실행되어야 할 때 PHPUnit 교차 버전 호환 테스트를 생성할 수 있는 라이브러리입니다.


[Selenium]: https://www.selenium.dev/
[integrated with PHPUnit]: https://github.com/giorgiosironi/phpunit-selenium/
[Mockery]: https://github.com/padraic/mockery
[PHPUnit]: https://phpunit.de/
[PHPSpec]: https://www.phpspec.net/
[Prophecy]: https://github.com/phpspec/prophecy
[php-mock]: https://github.com/php-mock/php-mock
[Infection]: https://github.com/infection/infection
[Mutation Testing]: https://en.wikipedia.org/wiki/Mutation_testing
[PHPUnit Polyfills]: https://github.com/Yoast/PHPUnit-Polyfills
