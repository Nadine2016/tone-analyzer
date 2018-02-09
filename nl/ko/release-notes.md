---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 릴리스 정보

다음 절에는 {{site.data.keyword.toneanalyzershort}} 서비스의 각 릴리스에서 포함되었던 새 기능 및 변경사항이 기록되어 있습니다. 이러한 변경사항은 기존 코드에 문제를 일으키지 않습니다.
{: shortdesc}

> **참고:** 이제 릴리스 정보에는 각 업데이트에 대한 *서비스 버전* 및 *인터페이스 버전*이 문서화되어 있습니다. 사용자는 `version` 조회 매개변수로 *인터페이스 버전*을 지정하여 해당 업데이트에서 사용 가능해진 새 기능을 사용합니다. 서비스는 `X-Service-Api-Version` 응답 헤더에서 두 버전을 모두 리턴합니다. 

## 2017년 9월 28일
{: #September2017b}

**서비스 버전:** `3.4.1`<br/> **인터페이스 버전:** `2017-09-21`

-   개별 {{site.data.keyword.Bluemix_notm}} 사용자가 제출할 수 있는 최대 요청 수에 대한 제한 한계가 분당 1200개 요청으로 늘어났습니다. 서비스는 사용자가 이 한계를 초과하는 경우 HTTP 응답 코드 429 *Too many requests*를 리턴합니다. 

## 2017년 9월 25일
{: #September2017a}

**서비스 버전:** `3.4.1`<br/> **인터페이스 버전:** `2017-09-21`

-   일반 목적 엔드포인트(`/v3/tone` 메소드)가 다음과 같이 변경되었습니다. 

    -   영어 외에 프랑스어(`fr`) 입력 컨텐츠를 지원합니다. 
    -   더 이상 사회적 톤을 리턴하지 않습니다. 
    -   더 이상 감정적 톤에 `disgust`를 리턴하지 않습니다. 
    -   점수가 `0.5` 미만인 어조를 생략합니다. 이 조건은 `/v3/tone_chat` 메소드의 응답과 일치합니다. 
    - 더 이상 출력의 일부로서 어조 카테고리(`tone_categories` 필드)를 리턴하지 않습니다. 값이 조건 임계값을 만족하는 모든 어조는 단일 `tones` 오브젝트의 일부로서 리턴됩니다. 이전과 마찬가지로, 어조는 기본적으로 항상 전체 문서 및 각 개별 문장에 대해 리턴됩니다. 
    - 더 이상 지정된 어조의 출력을 제한하기 위해 `tones` 조회 매개변수를 수락하지 않습니다. 모든 요청에서 조건을 만족하는 모든 어조가 리턴됩니다. 요청에 이 매개변수를 포함시키는 것으로 오류가 발생하지는 않으나, 응답에 영향을 미치지 않습니다. 
    - 더 이상 개별 문장에 대해 `input_from` 및 `input_to` 필드를 리턴하지 않습니다. 이제 이 메소드는 분석 결과 외에 ID 및 각 문장의 텍스트만 리턴합니다. 

-   이제 이 서비스는 다음 경우에 부분적으로 올바른 입력에 대해 HTTP 응답 코드 400 대신 200을 리턴합니다. 

    -   `/v3/tone` 메소드의 경우, 입력 컨텐츠로 128KB 또는 1000개 문장 이상을 제출하면 서비스가 응답의 일부로서 `warning` 필드를 리턴합니다. 서비스는 문서 레벨 분석의 경우 처음 1000개 문장, 문장 레벨 분석의 경우 현재와 마찬가지로 처음 100개 문장만 분석합니다. 이전 서비스 버전에서는 두 한계 중 하나를 초과하는 경우 서비스가 요청에 대해 응답 코드 400을 리턴했습니다. 또한 이제는 서비스가 세 단어보다 적은 단어를 포함하는 문장도 분석한다는 점을 참고하십시오. 
    -   `/v3/tone_chat` 메소드의 경우, 50개 이상의 말을 제출하면 서비스가 응답의 `utterances_tone` 레벨에서 전체 컨텐츠에 대해 `warning` 필드를 리턴합니다. 서비스는 처음 50개의 말만 분석합니다. 500자 이상을 포함하는 하나의 말을 제출하는 경우, 서비스는 해당 말에 대해 `error` 필드를 리턴하고 해당 말을 분석하지 않습니다. 이전 서비스 버전에서는 두 한계 중 하나를 초과하는 경우 서비스가 응답 코드 400을 리턴했습니다. 입력의 모든 말이 500자를 초과하는 경우에는 서비스가 여전히 요청에 대해 응답 코드 400을 리턴한다는 점을 참고하십시오. 

-   이제 서비스는 단일 사용자로부터 수락하는 요청의 수를 제한합니다. 서비스는 개별 {{site.data.keyword.Bluemix_notm}} 사용자로부터 분당 600개 이상의 요청을 수신하는 경우 HTTP 응답 코드 429 *Too many requests*를 리턴합니다. 

-   다음 변경사항은 일반 목적 엔드포인트 및 고객 참여 엔드포인트에 모두 적용됩니다. 

    -   서비스의 최신 버전을 사용하려는 경우 `version` 매개변수로 지정되는 인터페이스 버전은 `2017-09-21`입니다. 
    -   서비스가 다양한 언어로 현지화된 출력을 생성할 수 있음을 표시하기 위해 문서가 업데이트되었습니다. 원하는 언어를 지정하려면 `Accept-Language` 요청 헤더를 사용하십시오. 

## 2017년 7월 6일
{: #July2017b}

**서비스 버전:** `3.3.6`<br/> **인터페이스 버전:** `2016-05-19`

서비스가 고객 참여 엔드포인트의 몇 가지 결함 수정을 위해 업데이트되었습니다. 

## 이전 릴리스

-   [2017년 7월 1일](#July2017a)
-   [2017년 5월 8일](#May2017)
-   [2017년 4월 17일](#April2017)
-   [2017년 3월 15일](#March2017)
-   [2016년 12월 1일](#December2016)
-   [2016년 10월 18일](#October2016b)
-   [2016년 10월 3일](#October2016a)
-   [2016년 5월 19일](#May2016)

### 2017년 7월 1일
{: #July2017a}

**서비스 버전:** `3.3.5`<br/> **인터페이스 버전:** `2016-05-19`

이제 {{site.data.keyword.toneanalyzershort}} 서비스의 고객 참여 엔드포인트가 정식 출시(GA)되었습니다. 이제 이 엔드포인트에 대한 모든 호출은 일반 목적 엔드포인트에 대한 호출과 동일한 요금으로 청구됩니다. 

### 2017년 5월 8일
{: #May2017}

**서비스 버전:** `3.3.4`<br/> **인터페이스 버전:** `2016-05-19`

IBM이 훈련 데이터 세트를 추가로 확장하여 감정적 톤 및 고객 서비스 응대 어조 점수 모델을 업데이트했습니다. 이러한 모델의 벤치마크 데이터 세트에 대한 정확도가 향상되었습니다. 

### 2017년 4월 17일
{: #April2017}

-   고객 참여 영역 고유의 새 어조 세트(*실망*, *만족*, *흥분*, *정중함*, *무례함*, *슬픔* 및 *공감*)가 사용 가능해졌습니다. 서비스는 고객과 직원 간의 대화 텍스트로부터 이러한 어조를 감지합니다. 이러한 어조는 현재 베타 기능입니다. 
-   IBM이 감정 결과를 개선하는 감정적 톤 점수 모델에 대한 업데이트를 릴리스했습니다. 

### 2017년 3월 15일
{: #March2017}

IBM은 감정적 톤 점수 모델을 업데이트했습니다. 훈련 데이터 세트가 확장되었습니다. 이로 인해, 이러한 모델의 벤치마크 데이터 세트에 대한 정확도가 향상되었습니다. 

### 2016년 12월 1일
{: #December2016}

IBM이 문서의 감정적 톤 점수를 업데이트했습니다. 새 모델은 문서 점수를 집계하는 데 문장의 감정 프로파일을 고려합니다. 

### 2016년 10월 18일
{: #October2016b}

IBM은 사회적 톤을 개선했습니다. 이제 서비스는 [GloVe ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://nlp.stanford.edu/projects/glove/){: new_window}라는 오픈 소스 단어 임베딩 기술을 사용하여 사회적 톤 점수를 추측합니다. 이 변경사항을 통해 서비스는 사회적 톤을 계산할 때 더 많은 어휘를 고려할 수 있습니다. 사회적 톤을 추측하는 방법에 대한 자세한 정보는 {{site.data.keyword.personalityinsightsshort}} 서비스의 [서비스의 기반이 된 과학](http://www.ibm.com/watson/developercloud/doc/personality-insights/science.html)을 참조하십시오. 

### 2016년 10월 3일
{: #October2016a}

IBM이 단어 레벨 감정 감지를 개선했습니다. 이제 서비스는 새 훈련 데이터와 새 특성 선택 프로세스를 사용하며 단어, 이모티콘 및 속어에 대해 보강된 사전을 사용합니다. 이러한 변경사항은 문장 레벨에서 이제까지와 다르지만 크게 개선된 감정 점수를 생성합니다. 서비스의 API는 변경되지 않았습니다. 

### 2016년 5월 19일
{: #May2016}

{{site.data.keyword.toneanalyzershort}} 서비스의 정식 출시(GA) 릴리스는 다음 새 기능을 포함하고 있습니다. 

-   서비스의 모델이 어조를 해석하는 데 있어서 문맥 감지 기능이 개선되었습니다. 이제 모델은 사전 토큰만을 사용하지 않습니다. 구두점, 이모티콘과 문장 구조, 문장 복잡도와 같은 언어 매개변수 등의 추가 특성도 고려합니다. 
-   글 어조 모델의 이름이 언어적 톤으로 변경되었습니다. 
-   언어적 및 감정적 톤 모델이 이제 부정을 처리합니다. 
-   서비스가 더 이상 세 단어보다 적은 단어를 포함하는 문장에 대한 응답을 리턴하지 않습니다. 
-   이제 서비스에 대한 단순화되고 개선된 [데모 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://tone-analyzer-demo.mybluemix.net){: new_window}가 있습니다. 