---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-15"

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

# Release notes

The following sections document the new features and changes that were included for each release of the {{site.data.keyword.toneanalyzershort}} service. The changes do not break existing code.
{: shortdesc}

> **Note:** The release notes now document the *service version* and *interface version* for each update. You specify the *interface version* with the `version` query parameter to use new features and functionality made available with that update. The service returns both versions with the `X-Service-Api-Version` response header.

## New API authentication process
{: #new-authentication}

The {{site.data.keyword.toneanalyzershort}} service has a new API authentication process for service instances that are hosted in the following regions as of the indicated dates:

-   Washington, DC (US East) as of June 11, 2018
-   Sydney and AP North (**au-syd**) as of May 25, 2018

{{site.data.keyword.Bluemix}} is migrating to token-based Identity and Access Management (IAM) authentication. With some service instances, you authenticate to the API by using IAM.

-   *For new service instances that you create after the date indicated previously*, you use IAM for authentication. You can pass either a bearer token or an API key. Tokens support authenticated requests without embedding service credentials in every call. API keys use basic authentication.

    When you use any of the {{site.data.keyword.watson}} SDKs, you can pass the API key and let the SDK manage the lifecycle of the tokens. For more information and examples, see [Authentication ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#authentication){: new_window} in the API reference.
-   *For existing service instances that you created before the indicated date*, you continue to authenticate by providing the username and password for the service instance. Eventually, you will need to migrate these service instances to IAM authentication. Updates will be provided about migration process and dates. For more information about migration, see [Migrating Cloud Foundry service instances to a resource group](https://console.{DomainName}/docs/resources/instance_migration.html).

To learn which authentication process to use with your service instance, view the service credentials by clicking the instance on the {{site.data.keyword.Bluemix_notm}} [Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/dashboard/apps?watson){: new_window}.

All new and existing service instances in other regions continue to use service credentials (`{username}:{password}`) for authentication. IAM access tokens will be enabled for applications that are hosted in other regions soon.

## 11 June 2018
{: #June2018}

**Service version** - `3.5.4`<br/> **Interface version** - `2017-09-21`

For service instances and applications that are hosted in Washington, DC (US East), the service now supports a new API authentication process. For more information, see [New API authentication process](#new-authentication).

## 25 May 2018
{: #May2018}

**Service version** - `3.5.4`<br/> **Interface version** - `2017-09-21`

For service instances and applications that are hosted in Sydney and AP North (**au-syd**), the service now supports a new API authentication process. For more information, see [New API authentication process](#new-authentication).

## 13 March 2018
{: #March2018}

**Service version** - `3.5.3`<br/> **Interface version** - `2017-09-21`

The service was updated to add French (`fr`) input content in addition to English for the customer-engagement endpoint, `/v3/tone_chat`. You use the `Content-Language` request header to specify a language; the default is US English.

## Older releases

-   [28 September 2017](#September2017b)
-   [25 September 2017](#September2017a)
-   [6 July 2017](#July2017b)
-   [1 July 2017](#July2017a)
-   [8 May 2017](#May2017)
-   [17 April 2017](#April2017)
-   [15 March 2017](#March2017)
-   [1 December 2016](#December2016)
-   [18 October 2016](#October2016b)
-   [3 October 2016](#October2016a)
-   [19 May 2016](#May2016)

### 28 September 2017
{: #September2017b}

**Service version** - `3.4.1`<br/> **Interface version** - `2017-09-21`

The throttling limit on the maximum number of requests that an individual {{site.data.keyword.Bluemix_notm}} username can submit increased to 1200 requests per minute. The service returns HTTP response code 429 *Too many requests* if a user exceeds that limit.

### 25 September 2017
{: #September2017a}

**Service version** - `3.4.1`<br/> **Interface version** - `2017-09-21`

-   The general-purpose endpoint (the `/v3/tone` method) changed.

    -   Supports French (`fr`) input content in addition to English.
    -   No longer returns social tones.
    -   No longer returns `disgust` with the emotion tones.
    -   Omits any tone whose score is less than `0.5`. This qualification matches the response of the `/v3/tone_chat` method.
    - No longer returns tone categories (the `tone_categories` field) as part of its output. All tones whose values meet the qualifying threshold are returned as part of a single `tones` object. As before, tones are always returned for the full document and for each individual sentence by default.
    - No longer accepts a `tones` query parameter to limit the output to specified tones. All qualifying tones are returned for every request. Including the parameter with a request does not cause an error, but it has no effect on the response.
    - No longer returns `input_from` and `input_to` fields for individual sentences. In addition to the results of its analysis, the method now returns only an ID and the text of each sentence.

-   The service now returns HTTP response code 200 instead of 400 for partially correct input in the following cases:

    -   For the `/v3/tone` method, if you submit more than 128 KB or 1000 sentences of input content, the service returns a `warning` field as part of its response. The service analyzes the first 1000 sentences for document-level analysis. It analyzes only the first 100 sentences for sentence-level analysis. Earlier versions of the service returned response code 400 for the request if you exceeded either limit. Also, the service now analyzes sentences that have fewer than three words.
    -   For the `/v3/tone_chat` method, if you submit more than 50 utterances, the service returns a `warning` field for the overall content at the `utterances_tone` level of the response; it analyzes only the first 50 utterances. If you submit a single utterance that contains more than 500 characters, the service returns an `error` field for that utterance and does not analyze the utterance. Earlier versions of the service returned response code 400 if you exceeded either limit. If all utterances of the input have more than 500 characters, the service still returns response code 400 for the request.

-   The service now throttles the number of requests that it accepts from a single user. The service returns HTTP response code 429 *Too many requests* if it receives more than 600 requests per minute from an individual {{site.data.keyword.Bluemix_notm}} username.

-   The following changes apply to both the general-purpose and customer-engagement endpoints:

    -   The interface version that is specified with the `version` parameter is `2017-09-21` to use the latest version of the service.
    -   The documentation was updated to note that the service can produce localized output in various languages. Use the `Accept-Language` request header to specify the language.

### 6 July 2017
{: #July2017b}

**Service version** - `3.3.6`<br/> **Interface version** - `2016-05-19`

The service was updated for a small defect fix to the customer-engagement endpoint.

### 1 July 2017
{: #July2017a}

**Service version** - `3.3.5`<br/> **Interface version** - `2016-05-19`

The customer-engagement endpoint of the {{site.data.keyword.toneanalyzershort}} service is now generally available (GA). All calls to the endpoint are now charged at the same rate as calls to the general-purpose endpoint.

### 8 May 2017
{: #May2017}

**Service version** - `3.3.4`<br/> **Interface version** - `2016-05-19`

IBM updated the emotion tone and customer-care engagement tone score models by further expanding the training data set. The models now have greater precision on the benchmark data set.

### 17 April 2017
{: #April2017}

-   A new set of tones specific to the customer-engagement domain is now available: *frustrated*, *satisfied*, *excited*, *polite*, *impolite*, *sad*, and *sympathetic*. The service detects these tones from the text of a conversation between a customer and an agent. The tones are currently beta functionality.
-   IBM released updates to the emotion tone score model that improve the emotion results.

### 15 March 2017
{: #March2017}

IBM updated the emotion tone score model. The training data set was expanded. As a result, the models now have greater precision on the benchmark data set.

### 1 December 2016
{: #December2016}

IBM updated the document emotion tone scores. The new model considers the emotion profile of sentences to aggregate the document score.

### 18 October 2016
{: #October2016b}

IBM enhanced the social tone. The service now uses an open-source word-embedding technique that is called [GloVe ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://nlp.stanford.edu/projects/glove/){: new_window} to infer social tone scores. This change allows the service to cover a larger vocabulary of words when it calculates social tones. For more information about how social tones are inferred, see [The science behind the service](http://www.ibm.com/watson/developercloud/doc/personality-insights/science.html) for the {{site.data.keyword.personalityinsightsshort}} service.

### 3 October 2016
{: #October2016a}

IBM enhanced sentence-level emotion detection. The service uses new training data, a new feature-selection process, and augmented lexicons for words, emojis, and slang. These changes produce different but significantly improved emotion scores at the sentence level. The service's API did not change.

### 19 May 2016
{: #May2016}

The generally available (GA) release of the {{site.data.keyword.toneanalyzershort}} service includes these new features:

-   The service's models use improved context sensitivity to interpret tone. The models now use more than lexical tokens. They now consider features such as punctuation, emoticons, language parameters such as sentence structure, and sentence complexity.
-   The writing tone model was renamed to language tone.
-   The language and emotion tone models now handle negations.
-   The service no longer returns a response for sentences with fewer than three words.
-   The service now has a simplified and improved [demo ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tone-analyzer-demo.ng.bluemix.net){: new_window}.
