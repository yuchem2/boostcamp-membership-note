# Web Rendering

## 개념 및 용어

### Rendering
- SSR: 클라이언트측 혹은 범용 앱의 요청을 서버에서 HTML로 렌더링
- CSR: DOM을 수정하기 위해 JavaScript를 통해 브라우저에서 직접 렌더링
- Rehydration: 클라이언트에서 JavaScript View를 실행시켜 서버에서 렌더링한 HTML의 DOM 트리와 데이터를 재사용
- Hydration: JS가 정적 HTML을 브라우저의 동적 및 대화형 요소로 변환하는 기술
- Prerendering: 빌드할 때 초기 상태를 static HTML로 생성하여 클라이언트 측 앱을 실행
### Performance
- Time to First Byte(TTFB): 링크를 클릭한 이후 콘텐츠의 첫 비트가 도착하는 시간
- First Contentful Paint(FCP): 요청된 콘텐츠가 보이기 시작하는 시간
- Interaction to Next Paint(INP): 페이지가 사용자 입력에 일관되게 빠르게 반응하는지를 평가하는 대표 측정 항목
- Total Blocking Time(TBT): 페이지 로드 중 기본 thread가 차단된 시간을 계산하는 INP의 프록시 측정 항목
### 기타
- First-party JS: 개발 중인 웹사이트/애플리케이션의 자체 도메인에서 제공되는 JS 코드
- Third-party JS: 개발 중인 웹사이트/애플리케이션이 아닌 외부 도메인에서 제공되는 JS 코드
## SSR
**SSR은 서버에서 요청마다 페이지에 대한 전체 HTML을 생성해 응답으로 전달합니다.** 이를 통해 추가적인 데이터 전달과 클라이언트에서 템플릿을 생성하는 작업을 피할 수 있습니다.

SSR은 일반적으로 빠른 FCP를 제공합니다. 서버에서 전체 페이지를 렌더링하기 때문에 클라이언트에 많은 JS를 전송하지 않아도 됩니다. 그래서 페이지의 TBT를 줄임으로써 INP를 낮출 수 있습니다. (기본 thread가 페이지가 로드되는 동안 차단되지 않습니다.) 메인 thread가 비교적 덜 차단되기 때문에 사용자 상호작용이 더 빨리 실행될 수 있는 기회가 증가하기 때문입니다.

![500](https://velog.velcdn.com/images/yuchem2/post/ede95418-e64a-4c61-a2da-77b697244c7c/image.png)

SSR을 사용하면 사용자 측에서 CPU-bound JS가 실행될 때까지 기다리지 않을 수 있습니다. Third-part JS 사용을 하는 경우에도 first-party JS 비용이 줄어 더 많은 웹 예산을 사용할 수 있습니다. **하지만, 서버에서 직접 페이지를 생성해 시간이 소요되므로 TTFB가 높아질 수 있습니다.** 이 문제를 해결하기 위해 여러 사이트에서는 hybrid rendering 기술을 사용합니다.
### 장점
- SEO 중심 웹사이트에 적합합니다.
- 요청 시 모든 콘텐츠를 제공해 사용자에게 뛰어난 사용자 경험을 제공합니다.
### 단점
- 비용이 비쌀 수 있습니다.
- 서버가 다운되면 홈페이지도 다운됩니다.
## Static Rendering
Static Rendering은 build할 때 발생하는 방식입니다. **서버에서 미리 생성해놓고, 요청이 올 때 페이지 HTML을 전달하는 방식입니다.** 이 방식은 클라이언트 측 JS의 양이 제한적이라는 가정하에 빠른 FCP와 더 낮은 TBT 및 INP를 제공합니다. 또한, 동적으로 HTML를 생성하지 않기 떄문에 지속적으로 빠른 TTFB를 달성할 수 있습니다.

일반적인 방식으로 Static Rendering은 각 URL에 대해 별도의 HTML 파일을 미리 생성하는 것을 의미합니다. 이를 통해 여러 CDN에 배포하여 edge caching을 활용할 수 있습니다.

![500](https://velog.velcdn.com/images/yuchem2/post/774252e6-d213-417e-8b5f-a302c180e486/image.png)

이 방식의 단점 중 하나는 **가능한 모든 URL에 대해 개별 HTML 파일을 생성해야 한다는 것입니다.** 이러한 URL을 예측할 수 없거나(동적 라우팅을 사용하는 경우 등) 고유 페이지가 많은 사이트의 경우 까다롭거나 실현하지 못할 수 있습니다.

Static Rendering과 prerendering 방식의 차이를 이해하는 것이 중요합니다. Static Rendering되어 생성된 페이지는 많은 클라이언트 측 JS를 실행하지 않고도 상호작용이 가능한 반면, prerendering된 페이지는 사용자가 모든 기능을 사용하기 위해서는 클라이언트 측에서 JS 실행을 해야 합니다.

### 장점
- SEO 중심 웹사이트에 적합합니다.
- 신뢰성, 탁월한 성능, 보안성, 확장성
- 저렴한 비용, CDN에서 서비스를 제공할 수 있습니다.
- 호스팅 제공업체 간 이동이 용이합니다.
### 단점
- 변경사항이 발생한 경우 전체 웹 사이트를 다시 배포해야 합니다.
## CSR
**CSR은 브라우저에서 JS로 페이지를 직접 렌더링한다는 의미입니다.** 모든 로직(API를 통해 데이터를 검색하고, 논리를 관리하고, 브라우저에서 직접 페이지간 라우팅을 수행, .. 등)이 서버가 아닌 클라이언트에서 처리됩니다. 결과적으로 더 많은 데이터가 서버에서 사용자 기기로 전달되게 됩니다. 컴퓨팅 파워가 낮은 기기에서 CSR은 비교적 낮은 속도를 제공할 수 있습니다.

![500](https://velog.velcdn.com/images/yuchem2/post/81a4cd32-0c21-4733-9ff0-6fe3560fd603/image.png)

CSR의 가장 큰 단점은 앱이 커질수록 필요한 JS의 양이 증가하는 경향이 있어 페이지의 INP에 부정적인 영향을 미칠 수 있다는 점입니다. 대규모 JS 번들을 사용하는 CSR 환경에서는 페이지 로드 중의 TBT 및 INP를 낮추기 위해 코드 분할을 고려해야 하며, JS를 지연 로드해야 합니다. 즉, **성능 향상을 위해 필요할 때 필요한 항목만 게시해야 합니다.**

### 장점
- 사용자에게 빠른 피드백과 원활한 사용자 경험을 제공합니다.
- 모든 콘텐츠가 클라이언트/사용자 브라우저에서 처리되어 서버 부하가 낮습니다.
- SSR 보다 비용이 저렴합니다.
### 단점
- 사용자 경험이 저하될 수 있습니다.
- 클라이언트 측 앱으로 인해 검색 엔진 로봇이 웹 사이트 컨텐츠를 보기 어려워지고 색인이 생성되지 않거나 불량해질 수 있습니다.
- 사용자 브라우저에서 JS가 비활성화되어 있으면 JS 페이지가 표시되지 않습니다.
## Rehydration
Rehydration을 통해 CSR과 SSR을 결합할 수 있습니다. **이 방식은 CSR과 SSR을 모두 실행하여 두 방식의 장단점을 완화하려는 시도입니다.** 전체 페이지 로드 혹은 새로고침과 같은 탐색 요청은 HTML을 렌더링하는 서버에서 처리되며, 그러면 렌더링에 사용되는 JS 및 데이터의 결과가 문서에 삽입됩니다. 이를 통해 SSR과 같은 빠른 FCP를 이뤄낼 수 있고, (re)hydration 기술을 통해 선택적으로 rendering을 할 수 있습니다.

![500](https://velog.velcdn.com/images/yuchem2/post/a0a94ace-b9ea-462e-9fff-a2fe81d86c60/image.png)

효과적인 방식이지만 상당한 성능 단점이 수반될 수 있습니다. FCP가 개선되더라도 TBT 및 INP에 부정적인 영향을 줄 수 있다는 점입니다. SSR로 생성된 페이지는 로드되고 즉시 상호작용이 가능한 것처럼 보일 수 있으나 클라이언트 측 스크립트가 실행되고 이벤트 핸들러가 연결될 때까지 실제로 작동할 수 없습니다.
## 요약

결국 위에서 설명한 방식을 한 줄로 설명하자면 다음과 같습니다.
- SSR: 요청 때마다 동적으로 HTML 전체를 생성하여 응답으로 전달
- Static Rendering(Static SSR): 빌드 시간에 HTML을 미리 성생해 놓고 전달
- SSR with (Re)hydration: 동적으로 HTML을 생성하지만 전체 HTML을 생성하는 것이 아니라 일부는 JS/DOM으로 전달
- CSR: 클라이언트에게 JS/DOM을 전달하여 클라이언트에서 전체 HTML을 생성
- CSR with Prerendering: 페이지의 일부는 정적 HTML을 통해 생성해 놓고, 응답에 따라 생성되지 않은 부분의 JS/DOM과 정적 HTML을 함께 전달하여 클라이언트 측에서 생성

![500](https://velog.velcdn.com/images/yuchem2/post/0a5b3075-b435-49a6-8fc0-70d81066dc77/image.png)

## 출처
- [https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps/Introduction](https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps/Introduction)  
- [https://medium.com/compendium/3-ways-of-rendering-on-the-web-436j864c859e](https://medium.com/compendium/3-ways-of-rendering-on-the-web-436j864c859e)