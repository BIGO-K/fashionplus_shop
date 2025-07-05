# 패션플러스 리뉴얼 2020 (pc/mobile/admin)
+ 엠몬스타 전체 일정
	https://docs.google.com/spreadsheets/d/1ExM1T59YjsDJhZJajegziMwlqQb6MLMfI_Mo6ejRIHE/edit#gid=425404578
+ 스토리보드 구글드라이브 경로
	https://drive.google.com/drive/u/0/folders/16g7QnXLWtqricFatBIgcJ0WizZVxSlp5
+ 퍼블리싱 작업 리스트
	- pc : http://publish.mmonstar.co.kr/fashionplus/shop_2020/pc/
	- mobile : http://publish.mmonstar.co.kr/fashionplus/shop_2020/mobile/
	- admin : http://publish.mmonstar.co.kr/fashionplus/shop_2020/admin/
+ vscode에서 작업할 경우 **SFTP** 를 이용하면 FTP에 바로 업로드됩니다.
	- SFTP의 **sftp.json** 파일설정
		```
		"name": "ftp",
		"host": "1.xxx.xx.x2",
		"protocol": "sftp",
		"port": 8023,
		"username": "mmon_publish",
		"password": "비밀번호",
		"remotePath": "/home/mmon_publish/html/fashionplus/shop_2020/",
		"remotePath": "/home/mmon_publish/작업자/html/fashionplus/shop_2020/",(서브 작업자 설정방식)
		"uploadOnSave": true,
		...
		"watcher": {
			"files": "**",
			"autoUpload": true,
			"autoDelete": true
		},
		"ignore": [".vscode", ".git", ".DS_Store"]
		...
		```
	- css, fonts, html, js, scss, image 등 모든 폴더에 watch가 걸려있어 변화가 생기면 자동으로 업로드됩니다.
	- images 폴더는 포토샵에서 save for web을 할 때 파일이 생성되는 시점이 달라 오류 발생할 수 있습니다. (이슈 발생 시 ignore에 추가 테스트 필요)
	- 동기화가 되어 따로 파일을 삭제하지 않아도 됩니다.
<br><br>
- - -

## DESIGN
+ 디자인 구글드라이브 경로
	https://drive.google.com/drive/u/0/folders/1VerrH1UDXEaNry3KSnTID7HSzcj-FEW9
<br><br>
- - -

## HTML
+ 참고 사이트
	- [w3schools](https://www.w3schools.com/)
	- [caniuse](https://caniuse.com/)
+ 웹 표준 검사 필수
	- [kldp validator (한글)](http://validator.kldp.org/)
	- [kldp가 오류날 경우 해외사이트 이용](https://validator.w3.org/)
	- [간단한 웹 접근성 검사](https://accessibility.kr/nia/check.php)
	- kldp validator를 필수로 하고, 오류 0/경고 최소화 할 수 있도록 작업
	- 최소한의 웹 접근성 적용 : alt, 대체 텍스트, 탭 이동
+ 크로스브라우징
 	- PC : **IE11**
	- 모바일 : **android 5** (android 4.4 G3에서 알아볼 수 있는 정도, 노트2는 버그가 많아 제외)
+ 시멘틱 마크업
+ 주석 사용
	- \<!-- 영역시작 -->, \<!--// 영역끝 -->
	- \<!-- (D) 개발 전달사항 및 설명 -->
	- \<!-- !: 삭제예정, 위험, 오류 -->
	- \<!-- ?: 변수설명 -->
	- \<!-- *: 설명의 제목 (있을 시 사용) -->
	- \<!-- ~: 추가예정 및 현재 적용 안됨 -->
	- \<!-- todo: 할 일 -->
+ id는 앵커 이동이 필요한 영역 (팝업 등) 또는 레이아웃 영역에만 사용 (개발에서 사용할 수 있도록 최소화)
+ 속성 사용 순서
	```
	type id class name rel href/src/resource target accept data tabindex maxlength (그 외) value
	checked/selected readonly disabled auto*** alt title style onscript
	```
+ class
	- 동일 단어 중복 지양
	- UI (공통영역)
		- 기본 : mm_ui-하위-하위
		- 뎁스가 많을 때 : mm_ui__상위-하위
		- ui에서 동적으로 생성되는 가상의 연결요소 : mm__ui-하위-하위
		- 가상요소의 뎁스가 많을 때 : mm__ui__상위-하위
	- 개별 페이지 컨텐츠
		- 기본 : m_페이지-컨텐츠-하위
		- 뎁스가 많을 때 : m__상위-하위-하위
	- 팝업 페이지
		- 공통팝업 : m_popup-페이지 / m_popup-페이지-하위 / m__상위-하위-하위
		- 페이지팝업 : m_popup-뎁스-페이지 / m__페이지-하위-하위 / m__상위-하위-하위
	- 추가 속성
		- 기본 : \_\_이름_추가_추가_추가\_\_ (예, mm_btn \_\_btn_sm_line_primary\_\_)
		- UI의 상태 변경 : \_\_이름-상태 (**스크립트에서 추가되는 요소에만 사용** / 예, \_\_layout-popup)
		속성 프리픽스 : 속성_이름-상태 (btn, bg 제외 풀네임으로 적용 / btn_, bg_, text_, image_, ico_ 등)
	- 여러 요소들의 집합 : **mm_formmix-이름** (예, mm_formmix-linked / mm_formmix-period / mm_formmix-address)
	- 뎁스 네이밍 고정사항
		```
		<div class="mm_UIpack">
		  <div class="mm_UIpack-하위></div>
		  <div class="mm_UIbox>
			<div class="mm_UIbox-하위"></div>
			<div class="mm_UI"></div> // 단독사용 가능
			<div class="mm_UIbox-하위"></div>
		  </div>
		</div>
		```
+ a태그 속성 사용 (스크립트 연동)
	- 링크이동 : **data-href="{ 'type': 'link' }"** (로딩 노출)
		mm.link(url); 스크립트 실행
	- 앵커이동 : **data-href="anchor"**
		mm.anchor(target, options); 스크립트 실행
	- 히스토리이동 : **data-href="back"**, **data-href="forward"**
		mm.history.back(number);, mm.history.forward(number); 스크립트 실행
	- 팝업 : **data-href="{ 'type': 'popup' }"**
		mm.popup.open(url, options); 스크립트 실행
+ ~~팝업 페이지는 mm_app 클래스에 \__app_popup\_\_을 추가 (**class="mm_app \__app_popup\_\_"**)~~
+ 자주쓰는 유니코드 (entity code)
	- **\<** : \&#60; \&lt;
	- **\>** : \&#62; \&gt;
	- **\&** : \&#38; \&amp;
	- **빈칸** : \&#160; \&nbsp;
	- **©** : \&#169; \&copy;

### head
+ title을 뎁스에 맞게 넣어주세요.(SEO, 웹 접근성 준수)
	- 기본 : **사이트**
	- 1뎁스 : **1뎁스 - 사이트**
	- 2뎁스 이상 : **3뎁스 < 2뎁스 < 1뎁스 - 사이트**
	- 검색 : **"ㅇㅇ"으로 검색한 결과 - 사이트**
	- 상품/글 상세 : **"ㅇㅇ" 상세보기 - 사이트**, **"ㅇㅇ" 상품정보 - 사이트**
	- 팝업 : **(팝업) 페이지 이름 - 사이트**
+ 공유에 사용하는 소셜태그는 기본 항목만 있습니다.
	필요에 따라 추가 삭제하시면 됩니다.
	```
	<meta property="og:title" content="">
	<meta property="og:description" content="">
	<meta property="og:image" content="">
	<meta property="og:url" content="">
	<meta property="og:site_name" content="">
	```
+ 웹폰트는 동기식으로 스크립트에서 로드합니다.
	- *app_page-common.js* 에서 로드
	- 속도를 위해 페이지 레디 후에 전체를 로드하도록 적용
	- CDN 사용 시 스크립트 없이 바로 적용해도 됨
+ js파일은 **폴리필** 과 **라이브러리** 만 head에 넣고, 나머지는 body하단에 추가합니다.

### body
+ 페이지를 로드할 때 **app_common.js** 에서 **팝업, 모달, BOM** 관련 html태그를 추가합니다.

### tag
+ 태그 사용
	- ie11부터 대응함에 따라 \<header>, \<footer> 사용 가능합니다.
	- section, article과 같이 제목영역이 들어가는 태그안에 header를 사용합니다.
+ 텍스트에 사용하는 b와 i요소는 inline요소의 구조화를 위해 변형해서 사용합니다.
	- b는 div처럼 활용하여 block, inline-block 속성을 적용하여 요소를 묶어주는 역할을 합니다.
	- i는 image, background, icon 등 이미지 요소에 적용합니다.
+ 텍스트 요소
	- p : 문장
	- span : 일반 문자열
	- small : 작은 문자열
	- strong : 강조가 필요한 문자열 (b대신 사용)
	- em : strong보다는 약하지만 강조가 필요한 문자열 (i대신 사용하여 italic 적용)
	- sub, sup : sub는 하단, sup는 상단 작은 문자열에 사용하지만 small로 대신 사용 가능
<br><br>
- - -

## CSS/SCSS
+ SCSS를 사용하여 작업합니다.
+ CSS는 SCSS watch를 이용하여 자동변환합니다.
	- dart sass 사용 (ruby sass보다 속도가 월등히 빨라서 즉시 확인 가능)
	- 주의 : 문법 오류가 나면 watch가 멈추는 경우가 있어, 이때는 커멘드창에서 ctrl+c(복사아님)로 sass 종료 후 재실행
	```
	sass scss:css --watch --style=compressed
	인코딩은 기본이 utf-8이어서 따로 설정하지 않아도 되고,
	파일이 아닌 scss:css를 사용하여 폴더에 watch를 연결합니다.
	--style=compressed로 최대압축을 적용합니다.
	```
+ 주석사용
	- //< 영역시작, //> 영역끝
	- // 한 줄 설명, /* 여러 줄 설명 \*/
	- // !: 삭제, 위험, 오류
	- // ?: 변수설명
	- // *: 설명의 제목 (있을 시 사용)
	- // ~: 추가예정 및 현재 적용 안됨
	- // todo: 할일
+ SCSS 작성 규칙
	```
								.m_a- {
	<div class="m_a-b"></div>		==>		  &b { ... }
	<div class="m_a-c"></div>				  &c { ... }
								}

	<div class="m_a">					.m_a {
	  <div class="m_a-b"></div> 	        ==>               &-b { ... }
	  <div class="m_a-c"></div>				  &-c { ... }
	</div>							}
	```
+ CSS3 속성 사용
	- [caniuse](https://caniuse.com/)에서 크로스브라우징 확인 필수
	- transition 모션은 퍼포먼스를 위해 **opacity, transform** 에만 적용 (그 외 모션은 **tweenmax** 스크립트 모션으로 사용)
	- font-weight : normal, bold 대신 **400, 700 등** 숫자로 사용
		- Noto webfont : **thin 100, light 300, demilight/regular 400, medium 500, bold 700, black 900**
		- Gothic A1 webfont : **thin 100, extra light 200, light 300, regular 400, medium 500, semi bold 600, bold 700, extra bold 800, black 900**
	- 모바일에서 **position: fixed** 속성은 안드로이드 낮은 버전관 ios에서 오류가 많아 사용에 주의
	- 값을 지정할 때 사용하는 **calc()** 와 수치에 사용하는 **vmax, vmin** 등은 안드로이드 4.4에서 지원하지 않으므로 사용에 주의
+ **CSS 사용 순서**
	```
	display, visibility, overflow(x, y), box(sizing, shadow),
	flex(wrap, direction, grow, shrink, basis), justify-content,
	align(content, items, self),
	clear, float,
	position, z-index, top, right, bottom, left,
	margin(top, right, bottom, left), padding(top, right, bottom, left),
	width(min, max), height(min, max), border(size, style, color, radius), outline
	background(image, color, repeat, position, size, origin),
	vertical-align, list-style,
	font(style, weight, size, family), line-height,
	text(align, indent, overflow, shadow), letter-spacing, white-space, word(wrap, break, spacing),
	counter(reset, increment), content,
	opacity, transform(origin, style),
	transition(property, duration, timing, delay),
	pointer-events, 그 외 속성
	```
+ **extend.scss**
	- 다른 페이지에서 공통으로 사용되는 컨텐츠에 관한 스타일 작성
	- extend.scss에 **%변수명**으로 작성 후
		해당 컨텐츠를 사용해야하는 scss파일에서 @use '../path/extend'로 연결하여 **@extend %변수명**으로 사용
	- 같은 sass 파일이 아닌 다른 sass 파일에 있는 스타일을 가져올 때는 **@extend %변수명 !optional;** 으로 사용
+ 그 외 참고사항
	- **:before, :after** : inline-block 속성이 적용되어 있습니다.
	적용할 때 content: ""만 설정하면 width와 height 등을 바로 설정할 수 있습니다.
	- **i, b** : 태그를 변형해서 사용하기위해 inline-block 속성이 적용되어 있습니다.
<br><br>
- - -

## JAVASCRIPT
+ 라이브러리 (플러그인) 파일은 아래 경로를 통해 **CDN, git URL** 을 확인할 수 있습니다.
	- [cdnjs](https://cdnjs.com/)
	- [jsdelivr](https://www.jsdelivr.com/)
+ 규칙
	- 일반 변수명 **앞에 \_언더스코어** 추가 (\_var)
	- 엘리먼트 변수명은 \_없이 **문자 그대로** 사용 (element)
	- 오브젝트와 배열의 변수명은 \_없이 **뒤에 s를** 추가 (objects, arrays)
	- jQuery 셀렉터는 **앞에 $** 추가 ($selector)
	- 함수 (function)의 파라미터에는 **앞에 \__언더스코어 2개** 추가 (\__args)
	- 변수명과 함수명은 **카멜타입** 으로 사용 (aaBbCc)
	- 문자열은 "더블퀏"이 아닌 **'싱글퀏'** 으로 사용
+ 라이브러리 (플러그인)
	- [jQuery](https://github.com/jquery/jquery)
		* slim버전을 사용합니다.
		* slim버전에서는 animate와 ajax 등 일부 기능이 제거됐습니다.
	- [axios](https://github.com/axios/axios)
		* jQuery ajax 대신 사용하는 ajax 플러그인 입니다.
		* promise 기반으로 IE10 이하 지원을 위해 폴리필 ***es6-promise.auto-4.2.8.min.js*** 을 추가합니다.
		* 요청 취소 등 디테일한 설정이 가능합니다.
	- [lodash](https://github.com/lodash/lodash)
		* object, array 등을 손쉽게 변환/사용할 수 있도록 지원하는 라이브러리입니다.
		* underscore랑 _.function 사용하는 방식은 같지만 lodash가 기능이 더 많고 속도도 더 빠릅니다.
		* ie하위버전에서 지원 안되는 부분들이 있습니다. (현 프로젝트 ie11이상 지원하여 사용 가능)
	- [load-image](https://github.com/blueimp/JavaScript-Load-Image)
		* 이미지의 EXIF 속성을 확인할 수 있습니다.
		* 이미지를 로드할 때 img, canvas 등으로 설정할 수 있습니다.
		* EXIF 속성으로 카메라로 찍은 사진의 회전 값을 확인할 수 있습니다.
	- [swiper](https://github.com/nolimits4web/Swiper)
		* 컨텐츠 슬라이드 플러그인입니다.
	- [gsap](https://github.com/greensock/GSAP)
		* DOM 모션 플러그인입니다.
		* CSS transition보다 퍼포먼스가 좋습니다. (opacity, transform 제외)
	- [trumbowyg](https://github.com/Alex-D/Trumbowyg)
		* jQuery 기반 WYSIWYG 텍스트 에디터입니다.
	- [sortable](https://github.com/RubaXa/Sortable)
		* jQuery UI의 sortable보다 퍼포먼스가 좋습니다.
		* 컨텐츠의 순서를 변경할 때 사용합니다.
		* 드래그하는 요소에 ```touch-action: none``` CSS를 적용해야 브라우저 console창에 오류가 표시되지 않습니다.
	- ~~[overlay scrollbars](https://github.com/KingSora/OverlayScrollbars)~~
		* 커스텀 스크롤바 플러그인입니다.
	- ~~[auto numeric](https://github.com/autoNumeric/autoNumeric)~~
		* input 요소의 number type value 컨트롤
		* 자동으로 숫자와 통화단위 타입 적용
		* **필요한 경우 pc에서만 사용**
	- ~~[cleave](https://github.com/nosir/cleave.js)~~
		* input 요소의 number type value 컨트롤
		* 자동으로 숫자와 통화단위, 카드번호, 커스텀 타입 적용
		* **auto numeric** 보다 기능은 적지만 가벼움
	- ~~[moment](https://github.com/moment/moment)~~
		* 다양한 date format을 지원합니다.
	- ~~[chart](https://github.com/chartjs/Chart.js)~~
		* 다양한 차트를 svg가 아닌 canvas로 구현합니다.
		* svg보다 생성하는 DOM개수가 월등히 적습니다.
+ 퍼블리싱 js
	- **[퍼블 PC common.js 설명 보기](PC_JAVASCRIPT_COMMON.md)**
	- **[퍼블 MOBILE common.js 설명 보기](MOBILE_JAVASCRIPT_COMMON.md)**
	- **app_common**
		* 공통으로 사용되는 컴포넌트 모듈입니다.
		* 페이지 로드 시 바인딩됩니다.
		* 노출함수를 이용하여 외부에서 사용할 수 있습니다.
	- **app_page-common**
		* 프로젝트 페이지에서 공통으로 사용되는 스크립트입니다.
		* 폰트 로드, 디바이스 확인, 페이지관련 설정이 포함되어 있습니다.
	- **app_page-페이지이름**
		* 그 외 필요할 경우 js파일 생성하고 body하단에 추가합니다.