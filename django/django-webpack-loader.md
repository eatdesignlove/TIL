###### 원본: https://github.com/ezhome/django-webpack-loader
##### 2017.9.14

# Django-webpack-loader

이 라이브러리를 이용해 장고에 웹팩을 설정하기위한 단계별 상세한 가이드는 http://owaislone.org/blog/webpack-plus-reactjs-and-django/를 읽어보세요.

여러분의 정적 번들들을 장고의 staticfiles 또는 opaque wrappers  생성하기 위해 웹팩을 사용하세요.

장고 웹팩 로더는 webpack-bundle-tracker에 의해 생성된 결과물을 통해 여러분이 장고에서 생성된 번들들을 사용하게 합니다.

changelog는 사용가능합니다.

## 호환성 Compatibility

테스트 케이스는 파이썬 2.7과 파이썬>=3.3 위에서 장고>=1.6을 커버합니다. 100% 코드 커버리지를 목표로 했고, 언제든 모든 것이 작동하는 것을 확신합니다. 더불어 아마 장고의 예전 버전에서도 작동할 테지만, 테스트를 수반하지는 않습니다.

## 설치 Install
```shell
npm install --save-dev webpack-bundle-tracker
pip install Django-webpack-loader
```

## 설정 Configuration

### 가정들 Assumptions

설정의 `BASE_DIR`이 장고 앱의 루트를 참조하고 있다고 가정합니다.
```python
import sys
import os

BASE_DRI = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
```

다음과 같이 `settings.STATICFILES_DIRS`에 `assets/`가 설정되있다고 가정합니다.

```python
STATICFILES_DIRS = (
	os.path.join(BASE_DIR, 'assets'),
)
```

여러분의 웹팩 설정이 `./webpack.config.js`에 있고 다음과 같다고 가정합니다.

```js
var path = require('path');
var webpack = require('webpack');
var BundleTracker = require('webpack-bundle-tracker');

module.exports = {
	context: __dirname,
	entry: './assets/js/index',
	output: {
		path: path.resolve('./assets/webpack_bundles/'),
		filename: "[name]-[hash].js"
	},

	plugins: [
		new BundleTracker({filename: './webpack-stats.json'})
	]
}
```

### 기본 설정 Default Configuration
```python
WEBPACK_LOADER = {
	'DEFAULT': {
		'CACHE': not DEBUG,
		'BUNDLE_DIR_NAME': 'webpack_bundles/', #슬래시로 끝나야 합니다.
		'STATS_FILE': os.path.join(BASE)_DIR, 'webpack-stats.json'),
		'POLL_INTERVAL': 0.1,
		'TIMEOUT': None,
		'IGNORE': ['.+\.hot-update.js', '.+\.map']
	}
}
```

#### 캐시
```python
WEBPACK_LOADER = {
	'DEFAULT': {
		'CACHE': not DEBUG
	}
}
```
`CACHE`를 True로 설정할 때, webpack-loader는 stat file을 오직 한 번만 읽을 것이고, 결과를 캐시합니다. 이것은 웹워커가 stats 파일들의 변경사항을 반영하기 위해서는 위해서는 재시작이 필요함을 의미합니다.

#### BUNDLE_DIR_NAME
```python
WEBPACK_LOADER = {
	'DEFAULT': {
		'BUNDLE_DIR_NAME': 'bundles/' #슬래시로 끝나야 합니다.
	}
}
```
`BUNDLE_DIR_NAME`은 웹팩이 출력하는 번들들이 있는 dir을 참조합니다. 이것은 전체 경로가 될 수 없습니다. 만약 `./assets`이 여러분의 정적 경로 중 하나이고, 웹팩이 `./assets/output/bundles` 안에 번들들을 생성한다면, `BUNDLE_DIR_NAME`은 `output/bundles`가 되어야 합니다.

만약 번들이 `main-cf4b5fab6e00a404e0c7.js`라 불리는 파일을 생성하고 여러분의 STATIC_URL이 `/static/`이라면, `<script>` 태그는 다음과 같아야 합니다.

```html
<srcript type="text/javascript" src="/static/output/bundles/main-cf4b5fab6e00a404e0c7.js"/>
```


#### STATS_FILE
```python
WEBPACK_LOADER = {
	'DEFAULT': {
		'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.json')
	}
}
```
`STATS_FILE`은 `webpack-bundle-tracker`플러그인에 의해 생성되는 파일을 향하는 파일시스템 경로입니다. 만약 여러분이 다음과 같이 `webpack-bundle-tracker`를 설정한다면

```js
new BundleTracker({filename: './webpack-stats.json'})
```

여러분의 웹팩설정은 `/home/src/webpack.config.js`에 위치하게 되고, `STATS_FILE`의 값은 `/home/src/webpack-stats.json`이 되어야 합니다.

#### IGNORE
`IGNORE`는 정규표현식의 한 목록입니다. 만약 웹팩에 의해 생성된 파일이 표현 중 하나와 매치된다면, 그 파일은 템플릿 안에 포함되지 않을 것입니다.

#### POLL_INTERVAL
`POLL_INTERVAL`은 웹팩 로더가 stats 파일의 폴링하는 사이에 기다려야 하는 시간(초)입니다. Stats 파일은 기본적으로 100밀리초 마다 폴링되고, 웹펙이 번들을 컴파일하는 동안 요청이 차단됩니다. 만약 번들들의 컴파일 시간이 단축되면 이 시간을 더 줄일 수 있습니다.

**NOTE:** Stats 파일은 제품모드에서 폴링되지 않습니다. (DEBUG=False)

#### TIMEOUT
`TIMEOUT`은 웹팩 로더가 예외가 발생하기 전에  컴파일이 완료되기까지 기다리는 시간(초)입니다. `0`, `None` 또는 설정에서 값을 제거하면 시간초과가 비활성화 됩니다.

## 사용 Usage

### Manually run webpack to build assets.
django-webpack-loader의 핵심 원리 중 하나는 여러분이 원하는 방식으로 웹팩을 실행시키기 위한 유연함을 위하여 웹팩 자체를 관리하지 않는 것입니다. 만약 여러분이 웹팩을 처음 사용한다면, 예제들 중 하나를 살펴보고, 제 블로그 포스트나 웹팩문서를 읽어보세요.

### Settings
`webpack_loader`를 `INSTALLED_APPS`에 추가합니다.
```python
INSTALLED_APPS = (
	...
	'webpack_loader',
)
```

### Templates
```python
{% load render_bundle from webpack_loader %}
{% render_bundle 'main' %}
```
`render_bundle`은 템플릿에 필요한 적절한 `<script>`와 `<link>`태그를 렌더링합니다.

render_bundle은 일치시킬 파일 확장자가 될 수있는 두 번째 인수도 사용합니다. 이 기능은 파일의 여러 유형을 개별적으로 렌더링하려는 경우에 유용합니다. 예를 들어 CSS를 맨 아래에 그리고 JS를 아래쪽에 렌더링하려면 다음과 같이하면됩니다.

```html
{% load render_bundle from webpack_loader %}
<html>
	<head>
		{% render_bundle 'main' 'css' %}
	</head>
	<body>
		....
		{% render_bundle 'main' 'js' %}
	</body>
</html>
```

### Multiple webpack projects
버전 2.0 이상의 webpack loader는 여러 webpack 구성을 지원합니다. 다음 구성은 설정에서 2 개의 webpack stats 파일을 정의하고 템플릿 태그의 config 인자를 사용하여 어떤 stats 파일이 번들을 로드하는지 영향을 줍니다.

```python
WEBPACK_LOADER = {
	'DEFAULT': {
		'BUNDLE_DIR_NAME': 'bundles/',
		'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.json'),
	},
	'DASHBOARD': {
		'BUNDLE_DIR_NAME': 'dashboard_bundles/',
		'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats-dashboard.json'),
	}
}
```

```python
{% load render_bundle from webapack_loader %}

<html>
	<body>
		....
		{% render_bundle 'main' 'js' 'DEFAULT' %}
    {% render_bundle 'main' 'js' 'DASHBOARD' %}

    <!-- or render all files from a bundle -->
    {% render_bundle 'main' config='DASHBOARD' %}

    <!-- the following tags do the same thing -->
    {% render_bundle 'main' 'css' 'DASHBOARD' %}
    {% render_bundle 'main' extension='css' config='DASHBOARD' %}
    {% render_bundle 'main' config='DASHBOARD' extension='css' %}

    <!-- add some extra attributes to the tag -->
    {% render_bundle 'main' 'js' 'DEFAULT' attrs='async chatset="UTF-8"'%}
	</body>
</html>
```

### File URLs instead of html tags
HTML 태그없이 에셋의 URL이 필요한 경우 `get_files` 템플릿 태그를 사용할 수 있습니다. 일반적인 사용 사례는 자바스크립트 플러그인의 맞춤 CSS 파일 URL을 지정하는 경우입니다.

`get_files`는 일치하는 파일의 목록을 반환하고 목록을 사용자 지정 템플릿 변수에 할당 할 수 있다는 점을 제외하고는 `render_bundle`과 똑같이 작동합니다. 예를 들어,

```python
{% get_files 'editor' 'css' as editor_css_files %}
CKEDITOR.config.contentsCss = '{{ editor_css_files.0.publicPath }}';

<ul>
{% for css_file in editor_css_files %}
	<li>{{ css_file.name }} : {{ css_file.path }} : {{ css_file.publicPath }}</li>
{% endfor %}
</ul>
```

### Refer other static assets
`webpack_static` 템플릿 태그는 장고 템플릿에서 webpack에 의해 관리되는 정적 자산을 로드하는 기능을 제공합니다. 장고는 `static` 태그에 내장되어 있지만 webpack 자산에는 대신 사용됩니다. 아래의 예에서 `logo.png`는 npm 또는 bower 패키지와 함께 제공되는 정적 자산 일 수 있습니다.

```python
{% load webpack_static from webpack_loader %}

<!-- render full public path of logo.png -->
<img src="{% webpack_static 'logo.png' %}"/>
```

### From Python code
응용 프로그램 코드에서 webpack 에셋 경로 정보에 액세스하려면 `webpack_loader.utils` 모듈에서 이 함수를 사용할 수 있습니다.

```shell
>>> untils.get_files('main')
[{'url': '/static/bundles/main.js', u'path': u'/home/mike/root/projects/django-webpack-loader/tests/assets/bundles/main.js', u'name': u'main.js'},
 {'url': '/static/bundles/styles.css', u'path': u'/home/mike/root/projects/django-webpack-loader/tests/assets/bundles/styles.css', u'name': u'styles.css'}]
>>> utils.get_as_tags('main')
['<script type="text/javascript" src="/static/bundles/main.js" ></script>',
 '<link type="text/css" href="/static/bundles/styles.css" rel="stylesheet" />']
```

## How to use in Production
여러분에게 달려있습니다. 프로덕션에서 이를 처리 할 수있는 몇 가지 방법이 있습니다. 저는 프로덕션과 로컬을 위해 약간 다른 설정을 하는 것을 좋아합니다. git에서 로컬 stat + 번들 파일을 무시하지만 생산을 위해 추적합니다. 새 버전을 프로덕션으로 보내기 전에 프로덕션 구성을 사용하여 새 번들을 생성하고 새 통계 파일과 번들을 커밋합니다. `STATICFILES_DIR`에 추가 된 디렉토리에 통계 파일과 번들을 저장합니다. 이것은 저에게 collectstatic와 자유로운 통합을 제공합니다. 생성 된 번들은 자동으로 대상 디렉토리로 수집되고 S3에 동기화됩니다.

`./webpack_production.config.js`
```js
var config = require('./webpack.config.js');
var BundleTracker = require('webpack-bundle-tracker');

config.output.path = require('path').resolve('./assets/dist');

config.plugins = [
	new BundleTracker({filename: './webpack-stats-prod.json'})
]

// override any other settings here like using Uglify or other things that make sense for production enviroment.

module.exports = config;
```

`settings.py`
```python
if not DEBUG:
	WEBPACK_LOADER.update({
		'BUNDLE_DIR_NAME': 'dist/',
		'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats-prod.json')
	})
```
collectstatic을 실행하기 전에 서버에 번들을 간단히 생성 할 수도 있습니다.

## Extra

## Jinja2 Configuration
jinja 템플릿에서 자산을 출력해야하는 경우 Django Jinja 모듈 및 Django 1.8과 호환되는 Jinja2 확장을 제공합니다.

확장 기능을 설치하려면 [ "OPTIONS"] [ "extension"] 목록의 django_jinja TEMPLATES 설정에 추가하십시오.

```python
TEMPLATES = [
	{
		"BACKEND": "django_jinja.backend.Jinja2",
		"OPTIONS": {
			"extensions": [
			"django_jinja.builtins.extensions.DjangoFiltersExtension",
							"webpack_loader.contrib.jinja2ext.WebpackExtension",
			],
		}
	}
]
```

Then in your base jinja template:
```python
{{ render_bundle('main') }}
```
------
장고 & 웹팩과 함께 즐기세요 :)
