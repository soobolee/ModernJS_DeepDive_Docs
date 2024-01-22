## 자바스크립트 실행 환경

모든 브라우저는 JS를 해석하고 실행할 수 있는 엔진을 내장하고 있다.
Node.js도 크롬의 V8엔진을 내장하고 있기 때문에 브라우저 외의 환경에서 사용할 수 있게해준다.
* 그러나 Node.js는 브라우저 환경이 아닌 외부에서 사용되기 때문에, 브라우저에서 지원하는 DOM, BOM, Canvas, XMLHTTPRequest, fetch, SVG, Web Stroage 등을 사용할 수 없고, Node고유의 API를 사용해야 한다.

<hr />

## Node.js

간단한 웹은 브라우저만으로도 개발이 가능하지만 규모가 커짐에 따라 React, Angular등과 같은 프레임워크 또는 라이브러리 사용 시에 Babel, WebPack같은 도구를 사용할 필요가 있을 때 사용한다. <br />
**npm**은 node package manager의 약자로 node와 함께 설치되고, node에서 사용할 수 있는 모듈들의 저장소이다. 살펴보면 많은 편리한 모듈이 존재하지만 left-pad 사건과 마찬가지로 모듈의 위험성을 잘 살펴보고 사용해야한다.



